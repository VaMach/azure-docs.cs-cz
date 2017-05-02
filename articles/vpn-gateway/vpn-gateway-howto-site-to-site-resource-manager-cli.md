---
title: "Připojení místní sítě k virtuální síti Azure: Síť VPN typu Site-to-Site: Rozhraní příkazového řádku | Dokumentace Microsoftu"
description: "Postup vytvoření připojení IPsec z vaší místní sítě k virtuální síti Azure přes veřejný internet. Tyto kroky vám pomůžou vytvořit připojení VPN Gateway typu Site-to-Site mezi různými místy pomocí rozhraní příkazového řádku."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c3563f3a3fa46d40ba02fe97b3b0ebe3c45caddd
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Vytvoření virtuální sítě s připojením VPN typu Site-to-Site pomocí rozhraní příkazového řádku

Tento článek ukazuje, jak pomocí Azure CLI vytvořit připojení brány VPN typu Site-to-Site z místní sítě k virtuální síti. Postupy v tomto článku se týkají modelu nasazení Resource Manager. Tuto konfiguraci můžete vytvořit také pomocí jiného nástroje nasazení nebo pro jiný model nasazení, a to výběrem jiné možnosti z následujícího seznamu:

> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager – Rozhraní příkazového řádku](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Classic – Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Classic – portál Classic](vpn-gateway-site-to-site-create.md)
> 
>


![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-connection-diagram.png)

Připojení brány VPN typu Site-to-Site slouží k připojení místní sítě k virtuální síti Azure přes tunel VPN IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje místní zařízení VPN, které má přiřazenou veřejnou IP adresu. Další informace o bránách VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Než začnete

Před zahájením konfigurace ověřte, že splňujete následující kritéria:

* Ujistěte se, že chcete pracovat s modelem nasazení Resource Manager. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Další informace o kompatibilních zařízeních VPN a konfiguraci zařízení najdete v tématu [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).
* Máte veřejnou IPv4 adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Pokud neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže. Při vytváření této konfigurace musíte zadat předpony rozsahu IP adres, které bude Azure směrovat do vašeho místního umístění. Žádná z podsítí vaší místní sítě se nesmí překrývat s podsítěmi virtuální sítě, ke kterým se chcete připojit. 
* Máte nejnovější verzi příkazů rozhraní příkazového řádku (2.0 nebo novější). Informace o instalaci příkazů rozhraní příkazového řádku najdete v tématu [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="example-values"></a>Příklady hodnot

Tyto hodnoty můžete použít k vytvoření testovacího prostředí nebo můžou sloužit k lepšímu pochopení příkladů v tomto článku:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.12.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.12.0.0/24 
GatewaySubnet           = 10.12.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24 
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli
az login
```

Pokud máte více než jedno předplatné Azure, vypište předplatná pro daný účet.

```azurecli
Az account list --all
```

Určete předplatné, které chcete použít.

```azurecli
Az account set --subscription <replace_with_your_subscription_id>
```

## <a name="2-create-a-resource-group"></a>2. Vytvoření skupiny prostředků

Následující příklad vytvoří skupinu prostředků TestRG1 v umístění eastus. Pokud v oblasti, ve které chcete vytvořit virtuální síť, již máte skupinu prostředků, můžete ji použít.

```azurecli
az group create -n TestRG1 -l eastus
```

## <a name="VNet"></a>3. Vytvoření virtuální sítě

Pokud ještě nemáte virtuální síť, vytvořte si ji. Při vytváření virtuální sítě ověřte, že se zadané adresní prostory nepřekrývají s adresními prostory ve vaší místní síti. 

Následující příklad vytvoří virtuální síť TestVNet1 a podsíť Subnet1.

```azurecli
az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.12.0.0/16 -l eastus --subnet-name Subnet1 --subnet-prefix 10.12.0.0/24
```

## 4. <a name="gwsub"></a>Vytvoření podsítě brány

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Pro tuto konfiguraci potřebujete také podsíť brány. Brána virtuální sítě používá podsíť brány, která obsahuje IP adresy, které používají služby brány VPN. Při vytváření podsítě brány je nutné ji pojmenovat GatewaySubnet. Pokud zadáte jiný název, vytvoříte sice podsíť, ale Azure ji nebude považovat za podsíť brány.

Velikost podsítě brány, kterou zadáte, závisí na konfiguraci brány VPN, kterou chcete vytvořit. I když je možné vytvořit podsíť brány s minimální velikostí /29, doporučujeme vytvořit větší podsíť, která pojme více adres, tzn. vybrat velikost /27 nebo /28. Použitím větší podsítě brány zajistíte dostatek IP adres pro případné další konfigurace.


```azurecli
az network vnet subnet create --address-prefix 10.12.255.0/27 -n GatewaySubnet -g TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Vytvoření brány místní sítě

Brána místní sítě obvykle odkazuje na vaše místní umístění. Pro toto umístění určíte název, podle kterého na něj bude Azure odkazovat, a pak zadáte IP adresu místního zařízení VPN, ke kterému vytvoříte připojení. Zadáte také předpony IP adres, které se budou přes bránu VPN směrovat do zařízení VPN. Předpony adres, které zadáte, jsou předpony ve vaší místní síti. V případě změny vaší místní sítě můžete tyto předpony snadno aktualizovat.

Použijte následující hodnoty:

* *--gateway-ip-address* je IP adresa vašeho místního zařízení VPN. Zařízení VPN nesmí být umístěné za překladem adres (NAT).
* *--local-address-prefixes* jsou vaše místní adresní prostory.

Následující příklad ukazuje, jak přidat bránu místní sítě s více předponami adres:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Vyžádání veřejné IP adresy

Vyžádejte si veřejnou IP adresu, která bude přidělena pro bránu VPN vaší virtuální sítě. K této IP adrese se po konfiguraci bude připojovat vaše zařízení VPN.

Brána virtuální sítě pro model nasazení Resource Manager v současné době podporuje pouze veřejné IP adresy pomocí metody dynamického přidělení. To ale neznamená, že se IP adresa změní. Změna IP adresy brány VPN proběhne pouze v případě odstranění a nového vytvoření brány. V případě změny velikosti, resetování nebo jiné operace interní údržby/upgradu vaší brány VPN se veřejná IP adresa brány virtuální sítě nezmění. 

```azurecli
az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. Vytvoření brány VPN

Vytvořte bránu VPN virtuální sítě. Vytvoření brány VPN může trvat až 45 minut nebo ještě déle.

Použijte následující hodnoty:

* Hodnota *--gateway-type* pro konfiguraci typu Site-to-Site je *Vpn*. Typ brány je vždy specifický pro konfiguraci, kterou implementujete. Další informace najdete v části [Typy bran](vpn-gateway-about-vpn-gateway-settings.md#gwtype).
* Hodnota *--vpn-type* může být *RouteBased* (v některé dokumentaci nazývaná Dynamická brána), nebo *PolicyBased* (v některé dokumentaci nazývaná Statická brána). Toto nastavení závisí na konkrétních požadavcích zařízení, ke kterému se připojujete. Další informace o typech bran VPN najdete v tématu [Informace o nastavení konfigurace služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* Hodnota *--sku* může být Basic, Standard, nebo HighPerformance. Pro určité skladové jednotky (SKU) platí omezení konfigurace. Další informace najdete v části [Skladové jednotky (SKU) brány](vpn-gateway-about-vpngateways.md#gateway-skus).

Po spuštění tohoto příkazu se nezobrazí žádná zpětná vazba ani výstup. Vytvoření brány trvá přibližně 45 minut.

```azurecli
az network vnet-gateway create -n VNet1GW --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku Standard --no-wait 
```

## <a name="VPNDevice"></a>8. Konfigurace zařízení VPN

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]
Pokud chcete zjistit veřejnou IP adresu brány virtuální sítě, použijte následující příklad, ve kterém nahradíte hodnoty vlastními. Pro snadné čtení je výstup formátovaný, aby zobrazil seznam veřejných IP adres ve formátu tabulky.

  ```azurecli
  az network public-ip list -g TestRG1 -o table
  ```

## <a name="CreateConnection"></a>9. Vytvoření připojení VPN

Vytvořte připojení VPN typu Site-to-Site mezi bránou virtuální sítě a místním zařízením VPN. Věnujte zvláštní pozornost hodnotě sdíleného klíče, která se musí shodovat s hodnotou sdíleného klíče nakonfigurovanou pro zařízení VPN.

```azurecli
az network vpn-connection create -n VNet1toSite2 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Za malou chvíli dojde k vytvoření připojení.

## <a name="toverify"></a>10. Ověření připojení VPN

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)] 

Pokud chcete k ověření připojení použít jinou metodu, přečtěte si téma [Ověření připojení VPN Gateway](vpn-gateway-verify-connection-resource-manager.md).

## <a name="common-tasks"></a>Běžné úkoly

### <a name="to-view-local-network-gateways"></a>Zobrazení bran místní sítě

```azurecli
az network local-gateway list --resource-group TestRG1
```

### <a name="modify"></a>Úprava předpony IP adresy pro místní síťovou bránu
Pokud potřebujete změnit předpony pro bránu místní sítě, postupujte podle následujících pokynů. Při každé změně je nutné zadat celý seznam předpon, ne jenom předpony, které chcete změnit.

- **Pokud jste zadali připojení**, postupujte podle následujícího příkladu. Zadejte celý seznam předpon, který se skládá ze stávajících předpony a předpon, které chcete přidat. V tomto příkladu jsou již přítomny předpony 10.0.0.0/24 a 20.0.0.0/24. Přidáme předpony 30.0.0.0/24 a 40.0.0.0/24.

  ```azurecli
  az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 -n VNet1toSite2 -g TestRG1
  ```

- **Pokud jste nezadali připojení**, použijte stejný příkaz, který používáte k vytvoření brány místní sítě. Tento příkaz můžete použít také k aktualizaci IP adresy brány pro zařízení VPN. Používejte tento příkaz, pouze pokud ještě nemáte připojení. V tomto příkladu jsou přítomny předpony 10.0.0.0/24, 20.0.0.0/24, 30.0.0.0/24 a 40.0.0.0/24. Zadáme pouze předpony, které chceme ponechat. V tomto případě 10.0.0.0/24 a 20.0.0.0/24.

  ```azurecli
  az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
  ```

### <a name="modifygwipaddress"></a>Změna IP adresy místní síťové brány

IP adresa v této konfiguraci je IP adresa zařízení VPN, ke kterému vytváříte připojení. Pokud se IP adresa zařízení VPN změní, můžete tuto hodnotu upravit. IP adresu můžete změnit dokonce i v případě, že existuje připojení brány.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 -n Site2 -g TestRG1
```

Při prohlížení výsledků ověřte, že zahrnují předpony IP adres.

  ```azurecli
  "localNetworkAddressSpace": { 
    "addressPrefixes": [ 
      "10.0.0.0/24", 
      "20.0.0.0/24", 
      "30.0.0.0/24" 
    ] 
  }, 
  "location": "eastus", 
  "name": "Site2", 
  "provisioningState": "Succeeded",  
  ```

### <a name="to-view-the-virtual-network-gateway-public-ip-address"></a>Zobrazení veřejné IP adresy brány virtuální sítě

Pokud chcete zjistit veřejnou IP adresu brány virtuální sítě, použijte následující příklad. Pro snadné čtení je výstup formátovaný, aby zobrazil seznam veřejných IP adres ve formátu tabulky.

```azurecli
az network public-ip list -g TestRG1 -o table
```

### <a name="to-verify-the-shared-key-values"></a>Ověření hodnot sdíleného klíče

Ověřte, že hodnota sdíleného klíče je stejná jako hodnota, kterou jste použili pro konfiguraci zařízení VPN. Pokud není, buď spusťte připojení znovu s použitím hodnoty ze zařízení, nebo hodnotu v zařízení aktualizujte na vrácenou hodnotu. Tyto hodnoty se musí shodovat.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 -g TestRG1
```

## <a name="next-steps"></a>Další kroky

*  Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Informace o protokolu BGP najdete v tématech [Přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci protokolu BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Informace o vynuceném tunelování najdete v tématu [Konfigurace vynuceného tunelování](vpn-gateway-forced-tunneling-rm.md).
* Seznam příkazů Azure CLI pro práci se sítěmi najdete v tématu věnovaném [Azure CLI](https://docs.microsoft.com/cli/azure/network).
