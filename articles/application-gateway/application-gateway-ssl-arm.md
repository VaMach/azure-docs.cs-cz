---
title: "Konfigurovat přesměrování zpracování SSL - Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Tento článek poskytuje pokyny pro vytvoření služby application gateway pomocí protokolu SSL, přesměrování zpracování úloh pomocí Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.openlocfilehash: ee48ca45ae0d337b5b919dbbb28341caf8af0d45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manageru

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě. Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvořte virtuální síť a podsíť pro aplikační bránu. Ujistěte se, že žádné virtuální počítače nebo cloudová nasazení nepoužívají podsíť. Služba Application Gateway musí být sama o sobě v podsíti virtuální sítě.
3. Servery, které nakonfigurujete pro použití služby application gateway, musí existovat nebo mít své koncové body vytvořené ve virtuální síti nebo s veřejnou IP adresu nebo virtuální adresy IP (VIP) přiřazené.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co je potřeba k vytvoření služby Application Gateway?

* **Fond back-end serverů**: seznam IP adres back-end serverů. Uvedené IP adresy by měly patřit do podsítě virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.
* **Nastavení fondu back-end serverů**: každý fond má nastavení, jako je port, protokol a spřažení na základě souboru cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port**: Tento port je veřejný port, který se otevírá ve službě application gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces**: naslouchací proces má front-end port, protokol (Http nebo Https; tato nastavení jsou malá a velká písmena) a název certifikátu SSL (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo**: pravidlo váže naslouchací proces a fond back-end serverů a definuje, kterému fondu back-end serverů směrovat provoz při volání příslušného naslouchacího procesu. V tuhle chvíli se podporuje jenom *základní* pravidlo. *Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

**Další poznámky ke konfiguraci**

Pro konfiguraci certifikátů SSL by se měl změnit protokol v **HttpListener** na **Https** (rozlišování velkých a malých písmen). Přidat **SslCertificate** element **HttpListener** s hodnotou proměnné nakonfigurovanou pro certifikát SSL. Front-end port se musí aktualizovat na **443**.

**Chcete-li povolit spřažení na základě souborů cookie**: můžete nakonfigurovat služby application gateway zajistit, že žádost od klientské relace vždy směrovala na stejný virtuální počítač ve webové farmě. K tomu, vložte soubor cookie relace, který umožňuje bráně řízení provozu odpovídajícím způsobem. Když chcete povolit spřažení na základě souboru cookie, nastavte **CookieBasedAffinity** na **Povoleno** v elementu **BackendHttpSettings**.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Rozdíl mezi použitím modelu nasazení Azure classic a Azure Resource Manager je pořadí, ve kterém vytvoříte aplikační bránu a položky, které musí být nakonfigurované.

S Resource Managerem se všechny součásti služby application gateway se konfigurovat individuálně a potom se spojí dohromady a vytvoří prostředek aplikační brány.

Tady jsou kroky, které se musí udělat k vytvoření aplikační brány:

1. [Vytvořte skupinu prostředků pro Resource Manager](#create-a-resource-group-for-resource-manager)
2. [Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro službu application gateway](#create-virtual-network-subnet-and-public-IP-for-the-application-gateway)
3. [Vytvoření objektu konfigurace služby application gateway](#create-an-application-gateway-configuration-object)
4. [Vytvořte prostředek aplikační brány](#create-an-application-gateway-resource)

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že jste přepnuli režim prostředí PowerShell tak, aby se mohly použít rutiny Azure Resource Manageru. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

   1. Zadejte následující příkaz:

   ```powershell
   Login-AzureRmAccount
   ```

   2. Zkontrolujte předplatná pro příslušný účet, zadejte následující příkazy:

   ```powershell
   Get-AzureRmSubscription
   ```

   Zobrazí se výzva k ověření pomocí přihlašovacích údajů.

   3. Chcete-li zvolit, která z vašich předplatných Azure používat, zadejte následující příkaz:

   ```powershell
   Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
   ```

   4. Pokud chcete vytvořit skupinu prostředků, zadejte následující příkaz. (Tento krok přeskočte, pokud používáte některou ze stávajících skupin prostředků.)

   ```powershell
   New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
   ```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto nastavení slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem **appgw-RG** a umístění je **západní USA**.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu Application Gateway

Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Managera:

   1. Zadejte následující příkaz:

   ```powershell
   $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
   ```

   Tato ukázka přiřadí rozsah adres **10.0.0.0/24** do proměnné podsítě, který se má použít k vytvoření virtuální sítě.

   2. Zadejte následující příkaz:

   ```powershell
   $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

   Tato ukázka vytvoří virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro **západní USA** oblasti s použitím předponu **10.0.0.0/16** s podsítí **10.0.0.0/24**.

   3. Zadejte následující příkaz:

   ```powershell
   $subnet = $vnet.Subnets[0]
   ```

   Tato ukázka se přiřadí objekt podsítě k proměnné **$subnet** pro další kroky.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci

Pro vytvoření veřejné IP adresy pro front-end konfiguraci, zadejte následující příkaz:

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Tato ukázka vytvoří prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro **západní USA** oblast.

## <a name="create-an-application-gateway-configuration-object"></a>Vytvořte objekt konfigurace aplikační brány 

   1. K vytvoření objektu konfigurace aplikační brány, zadejte následující příkaz:

   ```powershell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
   ```

   Tato ukázka vytvoří konfigurace IP aplikační brány s názvem **gatewayIP01**. Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě a směruje síťový provoz na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

   2. Zadejte následující příkaz:

   ```powershell
   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
   ```

   Tato ukázka konfiguruje fond back-end IP adres s názvem **pool01** s IP adresami **134.170.185.46**, **134.170.188.221**, a **134.170.185.50** . Jsou to IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy. Nahraďte IP adresy z předchozího příkladu IP adresami koncových bodů vaší webové aplikace.

   3. Zadejte následující příkaz:

   ```powershell
   $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
   ```

   Tato ukázka nakonfiguruje nastavení brány aplikace **poolsetting01** síťový provoz Vyrovnávání zatížení ve fondu back-end.

   4. Zadejte následující příkaz:

   ```powershell
   $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
   ```

   Tato ukázka se nakonfiguruje port front-end IP s názvem **frontendport01** pro koncový bod veřejné IP adresy.

   5. Zadejte následující příkaz:

   ```powershell
   $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
   ```

   Tímto vzorovým kódem se nakonfiguruje certifikát používaný pro připojení SSL. Certifikát musí být ve formátu PFX a heslo musí mít délku 4 až 12 znaků.

   6. Zadejte následující příkaz:

   ```powershell
   $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
   ```

   Tato ukázka vytvoří konfiguraci front-end IP adresy s názvem **fipconfig01** a přidruží veřejnou IP adresu s konfigurací front-end IP adresy.

   7. Zadejte následující příkaz:

   ```powershell
   $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
   ```

   Tato ukázka vytvoří naslouchací proces s názvem **listener01** a přiřadí front-end port ke konfiguraci front-end IP adresy a certifikát.

   8. Zadejte následující příkaz:

   ```powershell
   $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

   Tento příklad vytvoří pravidlo směrování pro vyrovnávání zatížení s názvem **rule01** které konfiguruje chování nástroje pro vyrovnávání zatížení.

   9. Zadejte následující příkaz:

   ```powershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
   ```

   Tímto vzorovým kódem se nakonfiguruje velikost instance aplikační brány.

   > [!NOTE]
   > Výchozí hodnota pro **InstanceCount** je **2**, přičemž maximální hodnota je 10. Výchozí hodnota pro **GatewaySize** je **střední**. Můžete si vybrat mezi hodnotami Standard_Small (Standardní_malá), Standard_Medium (Standardní_střední) a Standard_Large (Standardní_velká).

   10. Zadejte následující příkaz:

   ```powershell
   $policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
   ```

   Tento krok definuje zásady protokolu SSL pro použití ve službě application gateway. Další informace najdete v tématu [verze zásad konfigurace protokolu SSL a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md).

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Vytvořte aplikační bránu pomocí New-AzureApplicationGateway

Zadejte následující příkaz:

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Tato ukázka se vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků. V příkladu se Aplikační brána nazývá **appgwtest**.

## <a name="get-the-application-gateway-dns-name"></a>Získat název DNS brány aplikace

Po vytvoření brány je dalším krokem je konfigurace front-endu pro komunikaci. Aplikační brána vyžaduje dynamicky přiřazené název DNS, při použití veřejnou IP adresu, která není popisný. Zajistěte, aby koncoví uživatelé mohou dosáhl aplikační bránu, můžete záznam CNAME přejděte na veřejný koncový bod služby application gateway. Další informace najdete v tématu [konfigurace vlastního názvu domény v Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Název DNS brány aplikace získáte načtení podrobností o aplikační brány a její přidružené název IP a DNS pomocí **PublicIPAddress** element připojený k službě application gateway. Použijte službu application gateway název DNS vytvořit záznam CNAME, který ukazuje dva webové aplikace k tomuto názvu DNS. Jsme nemáte doporučujeme použití záznamů A, protože virtuální IP adresu můžete změnit na restartovat aplikační brány.


```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Další kroky

Pokud chcete provést konfiguraci aplikační brány pro použití s nástrojem pro vyrovnávání zatížení pro vnitřní naleznete v části [vytvoření aplikační brány s nástrojem pro vyrovnávání zatížení pro vnitřní](application-gateway-ilb.md).

Další informace o službě Vyrovnávání zatížení možnosti obecně naleznete v tématu:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
