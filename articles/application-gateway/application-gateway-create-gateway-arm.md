---
title: Vytvoření, spuštění nebo odstranění služby Application Gateway pomocí Azure Resource Manageru | Microsoft Docs
description: Tato stránka obsahuje pokyny pro vytvoření, konfiguraci, spuštění a odstranění služby Azure Application Gateway pomocí Azure Resource Manageru.
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
ms.date: 09/06/2016
ms.author: gwallace

---
# Vytvoření, spuštění nebo odstranění služby Application Gateway pomocí Azure Resource Manageru
Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu. Služba Application Gateway poskytuje následující funkce doručování aplikací: vyrovnávání zatížení HTTP, spřažení relace na základě souborů cookie a přesměrování zpracování Secure Sockets Layer (SSL).

> [!div class="op_single_selector"]
> * [portál Azure](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)
> 
> 

Tenhle článek vás provede kroky k vytvoření, konfiguraci, spuštění a odstranění aplikační brány.

> [!IMPORTANT]
> Než začnete pracovat s prostředky Azure, je potřeba si uvědomit, že Azure má v současné době dva modely nasazení: Resource Manager a Classic. Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md) rozumíte. Dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty v horní části tohoto článku. Tento dokument se týká vytvoření služby Application Gateway pomocí Azure Resource Manageru. Pokud chcete použít klasickou verzi, přejděte na téma [Vytvoření klasického nasazení služby Application Gateway pomocí prostředí PowerShell](application-gateway-create-gateway.md).
> 
> 

## Než začnete
1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Pokud máte existující virtuální sítě, vyberte buď existující prázdnou podsíť, nebo vytvořte podsíť v existující virtuální síti výhradně pro účely služby Application Gateway. Nejde nasadit službu Application Gateway do jiné virtuální sítě, než jsou prostředky, které máte v úmyslu nasadit za službou Application Gateway. 
3. Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.

## Co je potřeba k vytvoření služby Application Gateway?
* **Fond back-end serverů:** Seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu. 

## Vytvoření služby Application Gateway
Rozdíl mezi použitím nástrojů Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte službu Application Gateway, a v položkách, které konfigurujete.

S Resource Managerem se všechny položky, které tvoří službu Application Gateway, konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.

Následující kroky je třeba provést k vytvoření služby Application Gateway.

## Vytvoření skupiny prostředků pro Resource Manager
Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### Krok 1
Přihlaste se k Azure.

    Login-AzureRmAccount

Zobrazí se výzva k ověření pomocí přihlašovacích údajů.

### Krok 2
Zkontrolujte předplatná pro příslušný účet.

    Get-AzureRmSubscription

### Krok 3
Zvolte předplatné Azure, které chcete použít.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### Krok 4
Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem „appgw-RG“ a umístěním „Západní USA“.

> [!NOTE]
> Pokud pro svoji službu Application Gateway potřebujete nakonfigurovat vlastní test paměti, přečtěte si článek [Vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Další informace najdete v článku [Vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).
> 
> 

## Vytvoření virtuální sítě a podsítě pro službu Application Gateway
Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Manageru.

### Krok 1
Proměnné podsítě, která se má použít k vytvoření virtuální podsítě, přiřaďte rozsah adres 10.0.0.0/24.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### Krok 2
Vytvořte virtuální síť s názvem „appgwvnet“ ve skupině prostředků „appgw-rg“ pro oblast Západní USA s použitím předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### Krok 3
Přiřaďte proměnnou podsítě pro další kroky, ve kterých se bude vytvářet služba Application Gateway.

    $subnet=$vnet.Subnets[0]

## Vytvoření veřejné IP adresy pro front-end konfiguraci
Vytvořte prostředek veřejné IP adresy „publicIP01“ ve skupině prostředků „appgw-rg“ pro oblast Západní USA.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## Vytvořte objekt konfigurace aplikační brány
Před vytvořením služby Application Gateway musíte nastavit všechny položky konfigurace. Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.

### Krok 1
Vytvořte konfiguraci protokolu IP služby Application Gateway s názvem „gatewayIP01“. Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### Krok 2
Nakonfigurujte fond back-end IP adres s názvem „pool01“ s IP adresami „134.170.185.46, 134.170.188.221,134.170.185.50“. Tyto IP adresy jsou IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy. Předchozí IP adresy nahradíte vlastními aplikačními koncovými body IP adresy.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### Krok 3
Nakonfigurujte nastavení služby Application Gateway „poolsetting01“ pro síťový provoz s vyrovnáváním zatížení ve fondu back-end.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### Krok 4
Nakonfigurujte port front-end IP adresy s názvem „frontendport01“ pro koncový bod veřejné IP adresy.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### Krok 5
Vytvořte konfiguraci front-end protokolu IP s názvem „fipconfig01“ a přidružte veřejnou IP adresu s konfigurací front-end IP adresy.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### Krok 6
Vytvořte naslouchací proces nazvaný „listener01“ a přiřaďte front-end port ke konfiguraci front-end IP adresy.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### Krok 7
Vytvořte pravidlo směrování pro vyrovnávání zatížení s názvem „rule01“, které konfiguruje chování nástroje pro vyrovnávání zatížení.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### Krok 8
Nakonfigurujte velikost instance služby Application Gateway.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

> [!NOTE]
> Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota *GatewaySize* je Medium (Střední). Můžete si vybrat mezi hodnotami Standard_Small (Standardní_malá), Standard_Medium (Standardní_střední) a Standard_Large (Standardní_velká).
> 
> 

## Vytvoření služby Application Gateway pomocí New-AzureApplicationGateway
Vytvořte službu Application Gateway se všemi položkami konfigurace z předchozích kroků. V tomto příkladu má služba Application Gateway název „appgwtest“.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### Krok 9
Načtěte podrobnosti o DNS a VIP služby Application Gateway z veřejného IP prostředku připojeného ke službě Application Gateway.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## Odstranění služby Application Gateway
Pokud chcete službu Application Gateway odstranit, postupujte takto:

### Krok 1
Získejte objekt služby Application Gateway a přidružte ho k proměnné „$getgw“.

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### Krok 2
K zastavení služby Application Gateway použijte rutinu **Stop-AzureRmApplicationGateway**.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  


Jakmile je služba Application Gateway v zastaveném stavu, odeberte ji pomocí rutiny **Remove-AzureRmApplicationGateway**.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force



> [!NOTE]
> K potlačení zprávy s potvrzením o odstranění se dá použít přepínač **-force**.
> 
> 

Pokud chcete zkontrolovat, jestli se služba odebrala, použijte rutinu **Get-AzureRmApplicationGateway**. Tenhle krok není povinný.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


## Další kroky
Pokud chcete konfigurovat přesměrování zpracování SSL, přejděte do části [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Pokud chcete provést konfiguraci aplikační brány pro použití s interním nástrojem pro vyrovnávání zatížení, přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!--HONumber=Sep16_HO3-->


