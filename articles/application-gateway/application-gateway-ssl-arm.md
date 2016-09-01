<properties
   pageTitle="Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manageru | Microsoft Azure"
   description="Tahle stránka obsahuje informace k vytvoření aplikační brány s přesměrováním zpracování SSL pomocí Azure Resource Manageru"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/09/2016"
   ms.author="gwallace"/>

# Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manageru

> [AZURE.SELECTOR]
-[Azure Portal](application-gateway-ssl-portal.md)
-[Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
-[Azure Classic PowerShell](application-gateway-ssl.md)

 Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě. Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.


## Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvoříte virtuální síť a podsíť pro službu Application Gateway. Ujistěte se, že tuto podsíť nepoužívají žádné virtuální počítače ani cloudová nasazení. Aplikační brána musí být sama o sobě v podsíti virtuální sítě.
3. Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.

## Co je potřeba k vytvoření služby Application Gateway?


- **Fond back-end serverů:** Seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.
- **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
- **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
- **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, tato nastavení rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).
- **Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu. V tuhle chvíli se podporuje jenom *základní* pravidlo. *Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

**Další poznámky ke konfiguraci**

Pro konfiguraci certifikátů SSL by se měl změnit protokol v **HttpListener** na *Https* (rozlišování velkých a malých písmen). Element **SslCertificate** se přidá do **HttpListener** s hodnotou proměnné nakonfigurovanou pro certifikát SSL. Front-end port se musí aktualizovat na hodnotu 443.

**Když chcete povolit spřažení na základě souborů cookie**: aplikační brána se může nakonfigurovat tak, aby se žádost od klientské relace vždy směrovala na stejný virtuální počítač v prostředí webové serverové farmy. To se provádí injektáží souboru cookie relace, který umožňuje bráně řízení provozu odpovídajícím způsobem. Když chcete povolit spřažení na základě souboru cookie, nastavte **CookieBasedAffinity** na *Povoleno* v elementu **BackendHttpSettings**.


## Vytvoření služby Application Gateway

Rozdíl mezi použitím modelu nasazení Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte aplikační bránu, a v položkách, které konfigurujete.

S Resource Managerem se všechny položky, které tvoří službu Application Gateway, konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.


Tady jsou kroky, které se musí udělat k vytvoření aplikační brány:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvořte virtuální síť, podsíť a veřejnou IP adresu pro aplikační bránu
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


        PS C:\> Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### Krok 4

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto nastavení slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem „appgw-RG“ a umístěním „Západní USA“.

## Vytvořte virtuální síť a podsíť pro aplikační bránu

Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Managera:

### Krok 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

To přiřadí proměnné podsítě rozsah adres 10.0.0.0/24, který se použije k vytvoření virtuální sítě.

### Krok 2
    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Tím se vytvoří virtuální síť s názvem „appgwvnet“ v prostředku skupiny „appgw-rg“ pro oblast Západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.

### Krok 3

    $subnet=$vnet.Subnets[0]

Tím se přiřadí objekt podsítě k proměnné $subnet pro další kroky.

## Vytvoření veřejné IP adresy pro front-end konfiguraci

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Tím se vytvoří prostředek veřejné IP adresy „publicIP01“ v prostředku skupiny „appgw-rg“ pro oblast Západní USA.


## Vytvoření objektu konfigurace služby Application Gateway

### Krok 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Tím se vytvoří konfigurace IP aplikační brány s názvem „gatewayIP01“. Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

### Krok 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Tím se konfiguruje fond back-end IP adresy s názvem „pool01“ s IP adresami „134.170.185.46, 134.170.188.221,134.170.185.50“. Jsou to IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy. Nahraďte IP adresy z výše uvedeného příkladu IP adresami koncových bodů vaší webové aplikace.

### Krok 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Tím se nakonfiguruje nastavení aplikační brány „poolsetting01“ pro síťový provoz s vyrovnáváním zatížení ve fondu back-end.

### Krok 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

Tím se nakonfiguruje port front-end IP adresy s názvem „frontendport01“ pro koncový bod veřejné IP adresy.

### Krok 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

Tím se nakonfiguruje certifikát používaný pro připojení SSL. Certifikát musí být ve formátu .pfx, heslo musí mít 4 až 12 znaků.

### Krok 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Tím se vytvoří konfigurace front-end IP adresy s názvem „fipconfig01“ a přidruží se veřejná IP adresa s konfigurací front-end IP adresy.

### Krok 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Tím se vytvoří naslouchací proces nazvaný „listener01“ a přiřadí front-end port ke konfiguraci a certifikátu front-end IP adresy.

### Krok 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Tím se vytvoří pravidlo směrování pro vyrovnávání zatížení s názvem „rule01“, které konfiguruje chování nástroje pro vyrovnávání zatížení.

### Krok 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Tím se nakonfiguruje velikost instance aplikační brány.

>[AZURE.NOTE]  Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota *GatewaySize* je Medium (Střední). Můžete vybrat mezi Standard_Small, Standard_Medium a Standard_Large.

## Vytvořte aplikační bránu pomocí New-AzureApplicationGateway

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Tímhle se vytvoří aplikační brána se všemi položkami konfigurace z výše uvedených kroků. V příkladu se aplikační brána nazývá „appgwtest“.

## Další kroky

Pokud chcete provést konfiguraci aplikační brány pro použití s interním nástrojem pro vyrovnávání zatížení (ILB), přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

- [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!--HONumber=Aug16_HO4-->


