---
title: "Vytvoření služby application gateway pomocí pravidel směrování adres URL | Microsoft Docs"
description: "Tato stránka obsahuje pokyny k vytvoření a konfiguraci služby Azure application gateway pomocí pravidel směrování adres URL."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.openlocfilehash: f2797864d7f0bda35d4d84ee78b157879451f889
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-by-using-path-based-routing"></a>Vytvoření služby application gateway pomocí směrování na základě cesty

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Na základě cestu směrování přidruží tras na základě cesty adresy URL požadavku HTTP. Ho ověří, zda je trasu k back-end fondu pro adresu URL v službu application gateway nakonfigurovaná, a poté odešle síťový provoz do definované fond back-end. Běžně používá pro směrování podle adresy URL je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.

Služba Azure Application Gateway má dva typy pravidel: základní směrování a směrování podle cesty. Basic poskytuje kruhového dotazování služby pro back endové fondy. Na základě cestu směrování, kromě distribučních kruhového dotazování, také pomocí vzorek cesty adresy URL žádosti o fond back-end.

## <a name="scenario"></a>Scénář

V následujícím příkladu se Aplikační brána slouží provoz pro doménu contoso.com s dvěma fondy back-end serverů: fondu video serverů a fond bitové kopie serveru.

Požadavky pro http://contoso.com/image * jsou směrovány do fondu serverů bitové kopie (**pool1**), a požadavky pro http://contoso.com/video * jsou směrovány do fondu serverů videa (**pool2**). Pokud cesta vzory neodpovídají, výchozí fond serverů (**pool1**) je vybrána.

![Adresa URL trasy](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvořte virtuální síť a podsíť pro aplikační bránu. Ujistěte se, že žádné virtuální počítače ani Cloudová nasazení použít tuto podsíť. Služba Application Gateway musí být sama o sobě v podsíti virtuální sítě.
3. Zajistěte, aby servery přidané do fondu back-end pro službu application gateway existují, nebo, ke kterým mají své koncové body vytvořené ve virtuální síti nebo s veřejné přiřazené nebo virtuálními IP Adresami.

## <a name="requirements-to-create-an-application-gateway"></a>Požadavky pro vytvoření služby application gateway

* **Fond back-end serverů**: seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo veřejné IP Adrese nebo VIP.
* **Nastavení fondu back-end serverů**: například port, protokol a spřažení na základě souboru cookie. Tyto jsou vážou na fond a použity na všechny servery v rámci fondu.
* **Front-end port**: veřejný port, který se otevírá ve službě application gateway. Provoz volá Tenhle port a pak přesměruje na jeden z back-end serverů.
* **Naslouchací proces**: naslouchací proces má front-end port, protokol (Http nebo Https, které jsou malá a velká písmena) a název certifikátu SSL (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo**: pravidlo váže naslouchací proces a fond back-end serverů a definuje, do které fondu provoz směrovat při volání naslouchací proces.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Rozdíl mezi použitím portálu Azure classic a Azure Resource Manager je pořadí, ve kterém vytvoříte službu application gateway a položky, které musí být nakonfigurované.

S Resource Managerem se všechny položky, které tvoří službu Application Gateway, konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.

Postupujte podle těchto kroků k vytvoření aplikační brány:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro službu Application Gateway
3. Vytvoření objektu konfigurace služby Application Gateway
4. Vytvoření prostředku služby Application Gateway

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Najít další informace v [pomocí prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se k Azure.

```powershell
Login-AzureRmAccount
```

Se zobrazí výzva k ověření pomocí svých přihlašovacích údajů.<BR>

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná pro příslušný účet.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Krok 3

Zvolte předplatné Azure, které chcete použít. <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Krok 4

Vytvořte skupinu prostředků. (Tento krok přeskočte, pokud používáte některou ze stávajících skupin prostředků.)

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

Alternativně můžete vytvořit značky pro skupinu prostředků pro aplikační brány:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Azure Resource Manager vyžaduje, aby skupiny prostředků určily výchozí umístění, které se používá pro všechny prostředky v této skupině. Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem "appgw-RG" a použili umístění "Západní USA".

> [!NOTE]
> Pokud potřebujete nakonfigurovat vlastní test paměti svojí aplikační brány, přejděte na [vytvoření služby application gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). V tématu [ Přehled monitorování stavu Application Gateway](application-gateway-probe-overview.md) Další informace.
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu Application Gateway

Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Manageru. Tento příklad vytvoří virtuální síť pro službu application gateway. Aplikační brána vyžaduje vlastní podsíti. Z tohoto důvodu je menší než adresního prostoru virtuální sítě podsíť vytvořená pro službu application gateway. To umožňuje jiné prostředky, včetně, ale mimo jiné webové servery, nakonfigurované ve stejné virtuální síti.

### <a name="step-1"></a>Krok 1

Proměnné podsítě, která se má použít k vytvoření virtuální podsítě, přiřaďte rozsah adres 10.0.0.0/24.  Tím se vytvoří objekt konfigurace podsítě pro službu application gateway, který se používá v dalším příkladu.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Krok 2

Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24. Dokončení konfigurace virtuální sítě s jednu podsíť pro aplikační brány, aby se nacházejí.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Krok 3

Přiřaďte proměnnou podsítě pro další kroky. To je předán `New-AzureRMApplicationGateway` rutiny v příštím kroku.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci

Vytvořte prostředek veřejné IP adresy **publicIP01** ve skupině prostředků **appgw-rg** pro oblast Západní USA. Aplikační bránu můžete použít veřejnou IP adresu a interní IP adresu pro příjem požadavků pro vyrovnávání zatížení.  Tento příklad používá jenom veřejné IP adresy. V následujícím příkladu žádný název DNS je nakonfigurován pro vytváření veřejnou IP adresu, protože aplikační brána nepodporuje vlastní názvy DNS na veřejné IP adresy.  Pokud název vlastního se vyžaduje pro koncový bod veřejné, vytvořte záznam CNAME tak, aby odkazoval na automaticky generovaný název DNS pro veřejnou IP adresu.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

IP adresa je ke službě Application Gateway přiřazena při spuštění služby.

## <a name="create-the-application-gateway-configuration"></a>Vytvoření konfigurace brány aplikace

Všechny položky konfigurace musíte nastavit před vytvořením služby application gateway. Následující kroky k vytvoření položek konfigurace potřebné pro prostředek aplikační brány.

### <a name="step-1"></a>Krok 1

Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**. Při spuštění služby Application Gateway se vybere IP adresa z nakonfigurované podsítě a trasy síťový provoz na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Krok 2

Nakonfigurujte fond back-end IP adres s názvem **pool1** a **pool2** s IP adresami pro **pool1** a **pool2**. Toto jsou IP adresy prostředky, které hostitele webové aplikace, které byly chráněny aplikační brány. Tito členové fondu back-end všechny ověřuje jako v pořádku základní nebo vlastní testy paměti. Provoz je pak směrován do nich, když služba Application Gateway obdrží požadavky. Back endové fondy mohou využívat více pravidel v rámci služby application gateway. To znamená, že jeden fond back-end lze použít pro více webových aplikací, které jsou umístěny na stejném hostiteli.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

V tomto příkladu dvě back endové fondy směrovat síťový provoz na základě cesty adresy URL. Jeden fond přijímá provoz z cesty URL "/ video," a dalších fondu přijímá provoz z cesty "/ bitové kopie." Nahrazením předchozích IP adres přidejte vlastní koncové body IP adres aplikace. 

### <a name="step-3"></a>Krok 3

Konfigurace nastavení služby application gateway **poolsetting01** a **poolsetting02** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end. V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro back endové fondy. Každý fond back-end může mít svůj vlastní nastavení.  Pravidla používají nastavení HTTP back-end přesměrovat provoz na členy správné fond back-end. Určuje protokol a port, který se používá pro odesílání provozu na členy fondu back-end. Na základě souborů cookie relací jsou určeny také nastavení HTTP back-end. Pokud je povoleno, spřažení relace na základě souborů cookie zasílá provozu jako předchozí požadavky jednotlivých paketů na stejnou back-end.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Krok 4

Konfiguraci front-end IP adresu s veřejné koncové body IP. Naslouchací proces používá objekt konfigurace IP front-endu na IP adresu směřující ven se týkají naslouchací proces.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Krok 5

Nakonfigurujte front-end port pro službu Application Gateway. Naslouchací proces používá objekt konfigurace front-end port pro definování jaké portu brány aplikace nenaslouchala komunikaci na naslouchací proces.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Krok 6

Nakonfigurujte naslouchací proces pro veřejnou IP adresu a port, na které se používá k přijetí příchozích síťových přenosů. Následující příklad trvá dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (Http nebo Https, které jsou malá a velká písmena) a nakonfiguruje naslouchací proces. V tomto příkladu naslouchací proces naslouchá provozu HTTP na portu 80 pro veřejnou IP adresu, kterou jste vytvořili dříve.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Krok 7

Konfigurovat pravidla cesty adresy URL pro back endové fondy. Tento krok nakonfiguruje relativní cesta používaná systémem aplikační brány a definuje mapování mezi cestu adresy URL a fond back-end, který přiřazen zpracovávat příchozí provoz.

> [!IMPORTANT]
> Každá cesta musí začínat "/" a hvězdičku je povoleno pouze na konci. Platnými hodnotami jsou /xyz, /xyz*, nebo /xyz/*. Řetězec dodáni do objekt přiřazení vzorce cesta nezahrnuje jakýkoli text po první "?" nebo "#" a tyto znaky nejsou povoleny. 

Následující příklad vytvoří dvě pravidla: jeden pro "/ image /" cesty směrování provozu na back-end **pool1**a druhý pro "/ video /" cesty směrování přenosem k back-end **pool2**. Tato pravidla se ujistěte, že přenosy dat pro každou sadu adresy URL se směruje na back-end. Například http://contoso.com/image/figure1.jpg přejde k **pool1** a http://contoso.com/video/example.mp4 přejde na **pool2**.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Pokud cesta neodpovídá žádné z pravidel předem definovaná cesta, nakonfiguruje konfiguraci pravidla cesty mapy taky výchozího fondu adres back-end. Například http://contoso.com/shoppingcart/test.html přejde k **pool1** protože je definována jako výchozí fond pro neodpovídající provoz.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>Krok 8

Vytvořte nastavení pravidla. Tento krok konfiguruje službu application gateway používat na základě cestu směrování adres URL. `$urlPathMap` Proměnná definovaná v předchozím kroku se teď používá k vytvoření pravidla na základě cesty. V tomto kroku jsme pravidlo přidružit naslouchací proces a mapování cesty adresy URL vytvořili dříve.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>Krok 9

Nakonfigurujte počet instancí a velikost pro službu Application Gateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-an-application-gateway-dns-name"></a>Získat název DNS brány aplikace

Po vytvoření brány, budete konfigurovat front-endu pro komunikaci. Pokud používáte veřejnou IP adresu, aplikační brána vyžaduje dynamicky přiřazené název DNS, který není popisný. Zajistěte, aby zákazníci, kteří mohou setkají aplikační bránu, můžete záznam CNAME přejděte na veřejný koncový bod služby application gateway. Další informace najdete v tématu [konfigurace vlastního názvu domény pro cloudové služby Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).

Pokud chcete nakonfigurovat záznam IP CNAME front-endu, načtěte podrobnosti o aplikační brány a její přidružené IP a DNS název pomocí prvku PublicIPAddress připojit k službě application gateway. Použijte službu application gateway název DNS vytvořit záznam CNAME. Nedoporučujeme použití záznamy A protože VIP může změnit na restartování Application Gateway.

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

Pokud chcete další informace o přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manager](application-gateway-ssl-arm.md).

