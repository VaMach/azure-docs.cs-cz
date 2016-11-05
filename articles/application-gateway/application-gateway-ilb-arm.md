---
title: Vytvořte a nakonfigurujte aplikační bránu s interním nástrojem pro vyrovnávání zatížení (ILB) pomocí nástroje Azure Resource Manager| Microsoft Docs
description: Tahle stránka poskytuje pokyny pro vytvoření, konfiguraci, spuštění a odstranění služby Azure application gateway s interním nástrojem pro vyrovnávání zatížení (ILB) pro nástroj Azure Resource Manager
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/19/2016
ms.author: gwallace

---
# Vytvořte aplikační bránu s interním nástrojem pro vyrovnávání zatížení (ILB) pomocí nástroje Azure Resource Manager
> [!div class="op_single_selector"]
> * [Klasické kroky ve službě Azure](application-gateway-ilb.md)
> * [Kroky Správce prostředků PowerShell](application-gateway-ilb-arm.md)
> 
> 

Služba Azure Application Gateway se dá nakonfigurovat pomocí virtuální IP adresy s přístupem k Internetu, nebo pomocí interního koncového bodu, který není vystavený v Internetu, známého také jako koncový bod interního nástroje pro vyrovnávání zatížení (ILB). Konfigurace brány pomocí ILB je užitečná pro interní-obchodní aplikace, které nejsou vystaveny v Internetu. To se hodí i pro služby a vrstvy v rámci vícevrstvé aplikace, které se nacházejí v rámci hranice zabezpečení nevystavené k Internetu, ale stále vyžadují distribuci zatížení modelu kruhového dotazování, dlouhodobost relace nebo ukončení SSL (Secure Sockets Layer).

Tenhle článek vás provede kroky konfigurace aplikační brány s ILB.

## Než začnete
1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvořte virtuální síť a podsíť služby Application Gateway. Ujistěte se, že tuto podsíť nepoužívají žádné virtuální počítače ani cloudová nasazení. Aplikační brána musí být sama o sobě v podsíti virtuální sítě.
3. Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.

## Co je potřeba k vytvoření služby Application Gateway?
* **Fond back-end serverů:** seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do virtuální sítě, ale v jiné podsíti pro aplikační bránu, nebo by se mělo jednat o veřejné IP nebo virtuální IP adresy.
* **Nastavení fondu back-end serverů:** každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, s rozlišením malých a velkých písmen) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu. V tuhle chvíli se podporuje jenom *základní* pravidlo. *Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

## Vytvoření služby Application Gateway
Rozdíl mezi použitím nástrojů Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte službu Application Gateway, a v položkách, které konfigurujete.
S Resource Managerem se všechny položky, které tvoří službu Application Gateway, konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.

Toto jsou kroky, které se musí provést k vytvoření služby Application Gateway:

1. Vytvořte skupinu prostředků pro Resource Manager
2. Vytvoření virtuální sítě a podsítě pro službu Application Gateway
3. Vytvoření objektu konfigurace služby Application Gateway
4. Vytvořte prostředek aplikační brány

## Vytvoření skupiny prostředků pro Resource Manager
Ujistěte se, že jste přepnuli režim prostředí PowerShell tak, aby se mohly použít rutiny Azure Resource Manageru. Další informace jsou k dispozici v části [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### Krok 1
    Login-AzureRmAccount

### Krok 2
Zkontrolujte předplatná pro příslušný účet.

    Get-AzureRmSubscription

Zobrazí se výzva k ověření pomocí přihlašovacích údajů.<BR>

### Krok 3
Zvolte předplatné Azure, které chcete použít. <BR>

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### Krok 4
Vytvořte novou skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem „appgw-rg“ a umístěním „Západní USA“.

## Vytvoření virtuální sítě a podsítě pro službu Application Gateway
Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Managera:

### Krok 1
    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

To přiřadí proměnné podsítě rozsah adres 10.0.0.0/24, který se použije k vytvoření virtuální sítě.

### Krok 2
    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Tím se vytvoří virtuální síť s názvem „appgwvnet“ v prostředku skupiny „appgw-rg“ pro oblast Západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.

### Krok 3
    $subnet = $vnet.subnets[0]

Tím se přiřadí objekt podsítě k proměnné $subnet pro další kroky.

## Vytvořte objekt konfigurace aplikační brány
### Krok 1
    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Tím se vytvoří konfigurace IP aplikační brány s názvem „gatewayIP01“. Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

### Krok 2
    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Tím se konfiguruje fond back-end IP adresy s názvem „pool01“ s IP adresami „134.170.185.46, 134.170.188.221,134.170.185.50“. Jsou to IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy. Výše uvedené IP adresy nahradíte vlastními aplikačními koncovými body IP adresy.

### Krok 3
    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Tím se nakonfiguruje nastavení aplikační brány „poolsetting01“ pro síťový provoz s vyrovnáváním zatížení ve fondu back-end.

### Krok 4
    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Tím se nakonfiguruje port front-end IP adresy pro ILB s názvem „frontendport01“.

### Krok 5
    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

To vytvoří konfiguraci front-end IP adresy, nazvanou „fipconfig01“, a přidruží ji s privátní IP adresou z aktuální podsítě virtuální sítě.

### Krok 6
    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

To vytvoří naslouchací proces nazvaný „listener01“ a přiřadí front-end port ke konfiguraci front-end IP adresy.

### Krok 7
    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Tím se vytvoří pravidlo směrování pro vyrovnávání zatížení s názvem „rule01“, které konfiguruje chování nástroje pro vyrovnávání zatížení.

### Krok 8
    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Tím se nakonfiguruje velikost instance aplikační brány.

> [!NOTE]
> Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota *GatewaySize* je Medium (Střední). Můžete vybrat mezi Standard_Small, Standard_Medium a Standard_Large.
> 
> 

## Vytvořte aplikační bránu pomocí New-AzureApplicationGateway
Tím se vytvoří aplikační brána se všemi položkami konfigurace z výše uvedených kroků. V tomto příkladu se aplikační brána nazývá „appgwtest“.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Tímhle se vytvoří aplikační brána se všemi položkami konfigurace z výše uvedených kroků. V příkladu se aplikační brána nazývá „appgwtest“.

## Odstranění aplikační brány
Pokud chcete odstranit aplikační bránu, musíte provést následující kroky v uvedeném pořadí:

1. Pro zastavení brány použijte rutinu **Stop-AzureRmApplicationGateway**.
2. Pro odstranění brány použijte rutinu **Remove-AzureRmApplicationGateway**.
3. Prověřte odstranění brány pomocí rutiny **Get-AzureApplicationGateway**.

### Krok 1
Získejte objekt služby Application Gateway a přidružte ho k proměnné „$getgw“.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### Krok 2
Pro zastavení aplikační brány použijte rutinu **Stop-AzureRmApplicationGateway**. Tenhle příklad ukazuje rutinu **Stop-AzureRmApplicationGateway** v prvním řádku, následovanou výstupem.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Jakmile je aplikační brána v zastaveném stavu, pro odstranění služby použijte rutinu **Remove-AzureRmApplicationGateway**.

    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

> [!NOTE]
> K potlačení zprávy s potvrzením o odstranění se dá použít přepínač **-force**.
> 
> 

Pokud chcete zkontrolovat, jestli se služba odebrala, použijte rutinu **Get-AzureRmApplicationGateway**. Tenhle krok není povinný.

    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## Další kroky
Když chcete konfigurovat přesměrování zpracování SSL, přejděte do části [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Když chcete provést konfiguraci aplikační brány pro použití s ILB, přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!--HONumber=ago16_HO5-->


