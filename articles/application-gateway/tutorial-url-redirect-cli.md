---
title: "Vytvoření služby application gateway pomocí adresy URL na základě cestu přesměrování - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Postup vytvoření služby application gateway pomocí adresy URL na základě cesty přesměrovaných přenosů dat pomocí rozhraní příkazového řádku Azure."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: davidmu
ms.openlocfilehash: 73fc20cf738f584008e7d8a019b8f7012dcacab1
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-application-gateway-with-url-path-based-redirection-using-the-azure-cli"></a>Vytvoření služby application gateway pomocí adresy URL na základě cestu přesměrování pomocí rozhraní příkazového řádku Azure

Rozhraní příkazového řádku Azure můžete použít ke konfiguraci [pravidla směrování na základě cesty adresy URL](application-gateway-url-route-overview.md) při vytváření [Aplikační brána](application-gateway-introduction.md). V tomto kurzu vytvoříte pomocí back-endové fondy [sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Pak vytvoříte pravidla směrování URL, která Ujistěte se, že webový provoz bude přesměrován na příslušné back-endový fond.

V tomto článku se dozvíte, jak:

> [!div class="checklist"]
> * Nastavení sítě
> * Vytvoření služby Application Gateway
> * Přidejte naslouchací procesy a pravidla směrování
> * Vytvoření sady škálování virtuálního počítače pro back-endové fondy

Následující příklad ukazuje webový provoz přicházející ze porty 8080 a 8081 a směrovat do stejné back-endové fondy:

![Příklad směrování URL](./media/tutorial-url-redirect-cli/scenario.png)

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Vytvoření skupiny prostředků pomocí [vytvořit skupinu az](/cli/azure/group#create).

Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAG* v *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Vytvoření síťové prostředky 

Vytvořit virtuální síť s názvem *myVNet* a podsíť s názvem *myAGSubnet* pomocí [vytvoření sítě vnet az](/cli/azure/network/vnet#az_net). Poté můžete přidat podsíť s názvem *myBackendSubnet* to vyžaduje back-end serverů pomocí [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Vytvoření veřejné IP adresy s názvem *myAGPublicIPAddress* pomocí [vytvoření veřejné sítě az-ip](/cli/azure/public-ip#az_network_public_ip_create).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Můžete použít [az brány aplikace-vytvořit](/cli/azure/application-gateway#create) k vytvoření aplikační brány s názvem myAppGateway. Při vytváření služby application gateway pomocí Azure CLI, je třeba zadat informace o konfiguraci, například kapacitu, sku a nastavení HTTP. Službu application gateway je přiřazena k *myAGSubnet* a *myPublicIPSddress* kterou jste vytvořili.  

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

 Ho může trvat několik minut pro službu application gateway, který se má vytvořit. Po vytvoření služby application gateway se zobrazí tyto nové funkce:

- *appGatewayBackendPool* -aplikační brány musí mít alespoň jeden fond adres back-end.
- *appGatewayBackendHttpSettings* -Určuje, že port 80 a protokol HTTP se používá pro komunikaci.
- *appGatewayHttpListener* -přidružený naslouchací proces výchozí *appGatewayBackendPool*.
- *appGatewayFrontendIP* -přiřadí *myAGPublicIPAddress* k *appGatewayHttpListener*.
- *rule1 New* – výchozí směrování pravidlo, které souvisí s *appGatewayHttpListener*.


### <a name="add-backend-pools-and-ports"></a>Přidat back-endové fondy a porty

Můžete přidat fondy adres back-end s názvem *imagesBackendPool* a *videoBackendPool* vaší aplikační brány pomocí [az Aplikační brána fond síťových adres-vytvořit](/cli/azure/application-gateway#az_network_application_gateway_address-pool_create). Přidat portů front-endu pro fondy pomocí [vytvoření aplikační brány sítě az front-endu port](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). 

```azurecli-interactive
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name imagesBackendPool
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name videoBackendPool
az network application-gateway frontend-port create \
  --port 8080 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name bport
az network application-gateway frontend-port create \
  --port 8081 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name rport
```

## <a name="add-listeners-and-rules"></a>Přidejte naslouchací procesy a pravidla

### <a name="add-listeners"></a>Přidejte naslouchací procesy

Přidejte naslouchací procesy back-end, s názvem *backendListener* a *redirectedListener* , jsou potřeba ke směrování provozu pomocí [az sítě Aplikační brána-naslouchací proces protokolu http vytvořit](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port bport \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway http-listener create \
  --name redirectedListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port rport \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-the-default-url-path-map"></a>Přidejte výchozí cestu mapování adresy URL

Adresa URL cesty mapy Ujistěte se, že konkrétní adresy URL jsou směrované na konkrétní back-endové fondy. Můžete vytvořit mapy cestu adresy URL s názvem *imagePathRule* a *videoPathRule* pomocí [az Aplikační brána url cesta mapy sítě vytvořit](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_create) a [az sítě Vytvořit pravidlo adresy url. cesta mapy aplikační brány](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_rule_create)

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name urlpathmap \
  --paths /images/* \
  --resource-group myResourceGroupAG \
  --address-pool imagesBackendPool \
  --default-address-pool appGatewayBackendPool \
  --default-http-settings appGatewayBackendHttpSettings \
  --http-settings appGatewayBackendHttpSettings \
  --rule-name imagePathRule
az network application-gateway url-path-map rule create \
  --gateway-name myAppGateway \
  --name videoPathRule \
  --resource-group myResourceGroupAG \
  --path-map-name urlpathmap \
  --paths /video/* \
  --address-pool videoBackendPool
```

### <a name="add-redirection-configuration"></a>Přidat konfiguraci přesměrování

Můžete konfigurovat přesměrování pro naslouchací proces pomocí [vytvoření aplikační brány sítě az přesměrování config](/cli/azure/application-gateway#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --gateway-name myAppGateway \
  --name redirectConfig \
  --resource-group myResourceGroupAG \
  --type Found \
  --include-path true \
  --include-query-string true \
  --target-listener backendListener
```

### <a name="add-the-redirection-url-path-map"></a>Přidat mapu cestu adresy URL přesměrování

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name redirectpathmap \
  --paths /images/* \
  --resource-group myResourceGroupAG \
  --redirect-config redirectConfig \
  --rule-name redirectPathRule
```

### <a name="add-routing-rules"></a>Přidat pravidla směrování

Pravidla směrování přidružit mapy cesta URL naslouchací procesy, které jste vytvořili. Můžete přidat pravidla s názvem *defaultRule* a *redirectedRule* pomocí [vytvořit pravidlo Aplikační brána sítě az](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name defaultRule \
  --resource-group myResourceGroupAG \
  --http-listener backendListener \
  --rule-type PathBasedRouting \
  --url-path-map urlpathmap \
  --address-pool appGatewayBackendPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectedRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectedListener \
  --rule-type PathBasedRouting \
  --url-path-map redirectpathmap \
  --address-pool appGatewayBackendPool
```

## <a name="create-virtual-machine-scale-sets"></a>Vytvoření sady škálování virtuálního počítače

V tomto příkladu můžete vytvořit tři sady škálování virtuálního počítače, které podporují tři back-endové fondy, které jste vytvořili. Jsou pojmenované sady škálování, které vytvoříte *myvmss1*, *myvmss2*, a *myvmss3*. Každá sada škálování obsahuje dvě instance virtuálního počítače, na kterých instalujete NGINX.

```azurecli-interactive
for i in `seq 1 3`; do
  if [ $i -eq 1 ]
  then
    poolName="appGatewayBackendPool" 
  fi
  if [ $i -eq 2 ]
  then
    poolName="imagesBackendPool"
  fi
  if [ $i -eq 3 ]
  then
    poolName="videoBackendPool"
  fi
  az vmss create \
    --name myvmss$i \
    --resource-group myResourceGroupAG \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password Azure123456! \
    --instance-count 2 \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
    --vm-sku Standard_DS2 \
    --upgrade-policy-mode Automatic \
    --app-gateway myAppGateway \
    --backend-pool-name $poolName
done
```

### <a name="install-nginx"></a>Instalace serveru NGINX

```azurecli-interactive
for i in `seq 1 3`; do
  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="test-the-application-gateway"></a>Testování služby application gateway

Chcete-li získat veřejnou IP adresu brány, aplikace, můžete použít [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#az_network_public_ip_show). Zkopírujte veřejnou IP adresu a pak ji vložit do panelu Adresa prohlížeče. Například *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm*, *http://40.121.222.19:8080/video/test.htm*, nebo *http:// 40.121.222.19:8081/Images/test.htm*.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Otestovat základní adresu URL v aplikační brány](./media/tutorial-url-redirect-cli/application-gateway-nginx.png)

Změňte adresu URL k http://&lt;ip adresu&gt;: 8080/video/test.html, nahraďte IP adres pro &lt;ip adresu&gt;, a měli byste vidět něco podobného jako v následujícím příkladu:

![Testovací adresu URL bitové kopie v aplikační brány](./media/tutorial-url-redirect-cli/application-gateway-nginx-images.png)

Změňte adresu URL k http://&lt;ip adresu&gt;: 8080/video/test.html, nahraďte IP adres pro &lt;ip adresu&gt;, a měli byste vidět něco podobného jako v následujícím příkladu.

![Adresa URL videa testu v aplikační brány](./media/tutorial-url-redirect-cli/application-gateway-nginx-video.png)

Nyní, změňte adresu URL k http://&lt;ip adresu&gt;: 8081/images/test.htm, nahraďte IP adres pro &lt;ip adresu&gt;, a měli byste vidět provoz přesměrován zpět do fondu back-end bitové kopie na http://&lt;ip adresu&gt;: 8080/bitové kopie.

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Nastavení sítě
> * Vytvoření služby Application Gateway
> * Přidejte naslouchací procesy a pravidla směrování
> * Vytvoření sady škálování virtuálního počítače pro back-endové fondy

> [!div class="nextstepaction"]
> [Další informace o co můžete dělat s aplikační brány](application-gateway-introduction.md)