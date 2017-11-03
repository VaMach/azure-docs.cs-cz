---
title: "Vytvoření služby application gateway - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Postup vytvoření služby application gateway pomocí Azure CLI 2.0 ve službě Správce prostředků."
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 3f43feaa8866725c5e6d8aa8cb03cd1a945de453
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>Vytvoření služby application gateway pomocí Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Služba Azure Application Gateway je vyhrazené virtuální zařízení, která poskytuje aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 Vyrovnávání zatížení možnosti pro vaši aplikaci.

## <a name="cli-versions"></a>Verze rozhraní příkazového řádku

Vytvořením služby application gateway pomocí jedné z následujících verzí rozhraní příkazového řádku (CLI):

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md): rozhraní příkazového řádku Azure classic a modelech nasazení Azure Resource Manager
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md): generace rozhraní příkazového řádku pro model nasazení Resource Manager

## <a name="prerequisite-install-the-azure-cli-20"></a>Předpoklad: Instalace rozhraní příkazového řádku Azure 2.0

Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat Azure CLI pro systému macOS, Linux a Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Potřebujete účet Azure k vytvoření aplikační brány. Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scénář

V tomto scénáři zjistěte, jak k vytvoření služby application gateway pomocí portálu Azure.

Tento scénář se:

* Vytvoření střední application gateway se dvěma instancemi.
* Vytvořte virtuální síť s názvem AdatumAppGatewayVNET s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.

> [!NOTE]
> Další konfigurace aplikační brány, včetně stavu vlastní testy paměti, fondu back-end adresy a další pravidla, dojde po vytvoření služby application gateway a ne během počátečního nasazení.

## <a name="before-you-begin"></a>Než začnete

Služby application gateway vyžaduje vlastní podsíti. Při vytváření virtuální sítě, ujistěte se, že necháte dostatek adresní prostor pro více podsítí. Po nasazení služby application gateway k podsíti, můžete přidat pouze další application Gateway k této podsíti.

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Otevřete **Microsoft Azure příkazového řádku** a přihlaste se:

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Můžete také použít `az login` bez přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.

Po zadání předchozí příkaz obdržíte kód. Přejděte na https://aka.ms/devicelogin v prohlížeči pokračovat v procesu přihlášení.

![cmd zobrazující zařízení přihlášení][1]

V prohlížeči zadejte kód, který jste dostali. Přesměruje na přihlašovací stránku.

![prohlížeče k zadání kódu][2]

Zadejte kód pro přihlášení a pak zavřete prohlížeč pokračovat.

![Úspěšné přihlášení][3]

## <a name="create-the-resource-group"></a>Vytvoření skupiny prostředků

Než vytvoříte službu application gateway, vytvořte skupinu prostředků, který ji obsahuje. Použijte následující příkaz:

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a>Vytvoření služby Application Gateway

Pomocí back-end IP adres pro IP adres back-end serverů. Tyto hodnoty může být buď soukromé IP adresy ve virtuální síti, veřejné IP adresy nebo plně kvalifikované názvy domény pro back-end serverů. V následujícím příkladu se vytvoří aplikační brána s další konfigurace pro nastavení protokolu HTTP, porty a pravidla:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

V předchozím příkladu zobrazuje několik vlastností, které nejsou nutné během vytváření služby application gateway. Následující příklad kódu se vytvoří aplikační brána se požadované informace:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> Seznam parametrů, které chcete použít během vytváření, spusťte následující příkaz: `az network application-gateway create --help`.

Tento příklad vytvoří základní aplikační brána s výchozím nastavením pro naslouchací proces, fond back-end, nastavení HTTP back-end a pravidla. Můžete upravit toto nastavení tak, aby vyhovovala vašemu nasazení po úspěšné přidělení přístupových práv.

Pokud webová aplikace je definovaný s fond back-end v předchozích krocích, Vyrovnávání zatížení se teď zahájí.

## <a name="get-the-application-gateway-dns-name"></a>Získat název DNS brány aplikace
Po vytvoření brány, nakonfigurujete front-endu pro komunikaci. Pokud používáte veřejnou IP adresu, služby application gateway vyžaduje dynamicky přiřazené název DNS, který není popisný. K zajištění, že uživatelé mohou dosáhl aplikační bránu, použijte záznam CNAME tak, aby odkazoval koncový bod veřejné aplikační brány. Další informace najdete v tématu [Azure DNS používá k poskytování nastavení vlastní domény pro službu Azure](../dns/dns-custom-domain.md).

Pokud chcete konfigurovat alias, načtěte podrobnosti o aplikační brány a její přidružené IP a DNS název pomocí prvku PublicIPAddress připojit k službě application gateway. Použijte službu application gateway název DNS vytvořit záznam CNAME, který ukazuje dva webové aplikace k tomuto názvu DNS. Nemusíte používat A záznamy, protože VIP může změnit na restartování aplikační brány.


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="delete-all-resources"></a>Odstranění všech prostředků

Pokud chcete odstranit všechny prostředky, které jsou vytvořené v tomto článku, spusťte následující příkaz:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Další kroky

Naučte se vytvářet vlastní stavu sondy, přejděte na [vytvořit vlastní test paměti pro službu Application Gateway pomocí portálu](application-gateway-create-probe-portal.md).

Zjistěte, jak nakonfigurovat snižování zátěže protokolu SSL a proveďte nákladná dešifrování SSL vypnout webových serverů, najdete v tématu [konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manager](application-gateway-ssl-arm.md).

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
