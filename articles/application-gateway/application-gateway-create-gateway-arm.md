---
title: "Vytvoření a správa brány Azure Application Gateway – PowerShell | Dokumentace Microsoftu"
description: "Tato stránka obsahuje pokyny pro vytvoření, konfiguraci, spuštění a odstranění služby Azure Application Gateway pomocí Azure Resource Manageru."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 866e9b5f-0222-4b6a-a95f-77bc3d31d17b
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: 141270c353d3fe7341dfad890162ed74495d48ac
ms.openlocfilehash: 6d38dd6802a25b147fd014b4d26ca432ca87a07d
ms.contentlocale: cs-cz
ms.lasthandoff: 07/25/2017

---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Vytvoření, spuštění nebo odstranění služby Application Gateway pomocí Azure Resource Manageru

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu.
Application Gateway poskytuje mnoho funkcí Application Delivery Controlleru (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních testů stavu, podpory více webů a mnoha dalších.

Úplný seznam podporovaných funkcí najdete v tématu [Přehled služby Application Gateway](application-gateway-introduction.md).

Tenhle článek vás provede kroky k vytvoření, konfiguraci, spuštění a odstranění aplikační brány.

> [!IMPORTANT]
> Než začnete pracovat s prostředky Azure, je důležité uvědomit si, že Azure aktuálně používá dva modely nasazení: Resource Manager a klasický model. Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md) rozumíte. Dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty v horní části tohoto článku. Tento dokument se týká vytvoření služby Application Gateway pomocí Azure Resource Manageru. Pokud chcete použít klasickou verzi, přejděte na téma [Vytvoření klasického nasazení služby Application Gateway pomocí prostředí PowerShell](application-gateway-create-gateway.md).

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
1. Pokud máte existující virtuální sítě, vyberte buď existující prázdnou podsíť, nebo vytvořte podsíť v existující virtuální síti výhradně pro účely služby Application Gateway. Nejde nasadit službu Application Gateway do jiné virtuální sítě, než jsou prostředky, které máte v úmyslu nasadit za službou Application Gateway.
1. Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co je potřeba k vytvoření služby Application Gateway?

* **Fond back-end serverů:** Seznam IP adres, plně kvalifikovaných názvů domén nebo síťových karet back-end serverů. Pokud jsou používány IP adresy, měly by buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Rozdíl mezi použitím nástrojů Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte službu Application Gateway, a v položkách, které konfigurujete.

S Resource Managerem se všechny položky, které tvoří službu Application Gateway, konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.

Následující kroky je třeba provést k vytvoření služby Application Gateway.

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se k Azure.

```powershell
Login-AzureRmAccount
```

Zobrazí se výzva k ověření pomocí přihlašovacích údajů.

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná pro příslušný účet.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Krok 3

Zvolte předplatné Azure, které chcete použít.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Krok 4

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem **appgw-RG** a umístěním **Západní USA**.

> [!NOTE]
> Když potřebujete nakonfigurovat vlastní test paměti služby Application Gateway, přečtěte si článek [Vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).

## <a name="create-a-virtual-network-and-a-subnet"></a>Vytvoření virtuální sítě a podsítě

Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Manageru. Tento příklad vytvoří virtuální síť pro službu Application Gateway. Služba Application Gateway vyžaduje vlastní podsíť. Z toho důvodu je podsíť vytvořená pro službu Application Gateway menší než adresní prostor virtuální sítě. Použití menší podsítě umožňuje konfigurování dalších prostředků (včetně webových serverů a jiných prostředků) v téže virtuální síti.

### <a name="step-1"></a>Krok 1

Proměnné podsítě, která se má použít k vytvoření virtuální podsítě, přiřaďte rozsah adres 10.0.0.0/24. V tomto kroku se vytvoří objekt konfigurace podsítě pro službu Application Gateway, který se používá v dalším příkladu.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Krok 2

Vytvořte virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast Západní USA s použitím předpony 10.0.0.0/16 s podsítí 10.0.0.0/24. V tomto kroku se dokončí konfigurace virtuální sítě s jedinou podsítí pro umístění služby Application Gateway.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Krok 3

Přiřaďte proměnnou podsítě pro další kroky. Tato proměnná bude v budoucím kroku předána rutině `New-AzureRMApplicationGateway`.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy

Vytvořte prostředek veřejné IP adresy **publicIP01** ve skupině prostředků **appgw-rg** pro oblast Západní USA. Služba Application Gateway může pro příjem požadavků na vyrovnávání zatížení používat veřejnou IP adresu, interní IP adresu nebo obojí.  V tomto příkladu se používá jen veřejná IP adresa. V následujícím příkladu není pro vytvoření veřejné IP adresy nakonfigurován žádný název DNS.  Služba Application Gateway nepodporuje u veřejných IP adres vlastní názvy DNS.  Pokud je pro veřejný koncový bod vyžadován vlastní název, měl by být vytvořen záznam CNAME odkazující na automaticky vygenerovaný název DNS pro příslušnou veřejnou IP adresu.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

> [!NOTE]
> IP adresa je ke službě Application Gateway přiřazena při spuštění služby.

## <a name="create-the-application-gateway-configuration-objects"></a>Vytvoření objektů konfigurace aplikační brány

Před vytvořením služby Application Gateway musí být nastaveny všechny položky konfigurace. Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.

### <a name="step-1"></a>Krok 1

Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**. Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Krok 2

Nakonfigurujte fond back-end IP adres s názvem **pool01** s IP adresami pro **pool1**. Tyto IP adresy jsou IP adresy prostředků, které jsou hostiteli webové aplikace, která má být chráněna službou Application Gateway. Funkčnost všech těchto členů fondu back-end se ověřuje základními nebo vlastními testy.  Provoz je pak směrován do nich, když služba Application Gateway obdrží požadavky. Fondy back-end můžou být využívány více pravidly v rámci služby Application Gateway. To znamená, že jeden fond back-end může být využíván pro více webových aplikacích umístěných ve stejném hostiteli.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50
```

V tomto příkladu jsou pro směrování síťového provozu na základě cesty URL použity dva fondy back-end. Jeden fond přijímá provoz z cesty URL „/video“ a druhý fond přijímá provoz z cesty „/images“. Nahrazením předchozích IP adres přidejte vlastní koncové body IP adres aplikace.

### <a name="step-3"></a>Krok 3

Nakonfigurujte nastavení služby Application Gateway **poolsetting** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end. Každý fond back-end může mít vlastní nastavení fondu back-end.  Nastavení back-endu HTTP jsou využívána pravidly pro směrování provozu do správných členů fondu back-end. Nastavení back-endu HTTP určují protokol a port používaný při odesílání provozu do členů fondu back-end. Podle nastavení HTTP back-endu se určují i relace založené na souborech cookie.  Pokud je tato funkce povolena, spřažení relace založené na souborech cookie odesílá provoz do stejného back-endu jako předchozí požadavky pro jednotlivé pakety.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
```

### <a name="step-4"></a>Krok 4

Nakonfigurujte front-end port pro službu Application Gateway. Objekt konfigurace portu front-end je používán naslouchacím procesem k definování portu, na kterém služba Application Gateway v naslouchacím procesu naslouchá provozu.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

### <a name="step-5"></a>Krok 5

Nakonfigurujte IP adresu front-endu s koncovým bodem s veřejnou IP adresou. Objekt konfigurace IP adresy front-endu je používán naslouchacím procesem k předání veřejně viditelné IP adresy pro naslouchací proces.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

### <a name="step-6"></a>Krok 6

Nakonfigurujte naslouchací proces. V tomto kroku je nakonfigurován naslouchací proces pro veřejnou IP adresu a port používaný pro příjem příchozího síťového provozu. V následujícím příkladu se pracuje s dříve nastavenou konfigurací IP adresy front-endu, konfigurací portu front-endu a protokolem (http nebo https) a nakonfiguruje se naslouchací proces. V tomto příkladu naslouchací proces naslouchá provozu HTTP na portu 80 pro veřejnou IP adresu, kterou jste vytvořili dříve.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

### <a name="step-7"></a>Krok 7

Vytvořte pravidlo směrování pro nástroj pro vyrovnávání zatížení s názvem **rule01**, které konfiguruje chování nástroje pro vyrovnávání zatížení. Nastavení fondu back-end, naslouchací proces a fond back-end vytvořený v předchozích krocích společně tvoří pravidlo. Na základě definovaných kritérií se provoz směruje do příslušného back-endu.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-8"></a>Krok 8

Nakonfigurujte počet instancí a velikost pro službu Application Gateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Výchozí hodnota **InstanceCount** je 2, přičemž maximální hodnota je 10. Výchozí hodnota **GatewaySize** je Medium (Střední). Můžete si vybrat mezi hodnotami **Standard_Small** (Standardní_malá), **Standard_Medium** (Standardní_střední) a **Standard_Large** (Standardní_velká).

## <a name="create-the-application-gateway"></a>Vytvoření služby Application Gateway

Vytvořte službu Application Gateway se všemi položkami konfigurace z předchozích kroků. V tomto příkladu má služba Application Gateway název **appgwtest**.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Načtěte podrobnosti o DNS a VIP služby Application Gateway z veřejného IP prostředku připojeného ke službě Application Gateway.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  
```

## <a name="delete-the-application-gateway"></a>Odstranění služby Application Gateway

Pokud chcete službu Application Gateway odstranit, postupujte takto:

### <a name="step-1"></a>Krok 1

Získejte objekt služby Application Gateway a přidružte ho k proměnné `$getgw`.

```powershell
$getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Krok 2

Pomocí rutiny `Stop-AzureRmApplicationGateway` zastavte službu Application Gateway.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw
```

Jakmile je služba Application Gateway v zastaveném stavu, pomocí rutiny `Remove-AzureRmApplicationGateway` službu odstraňte.

```powershell
Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force
```

> [!NOTE]
> K potlačení zprávy s potvrzením o odstranění se dá použít přepínač **-force**.

Pokud chcete zkontrolovat, že se služba odstranila, můžete použít rutinu `Get-AzureRmApplicationGateway`. Tenhle krok není povinný.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Pokud chcete najít dynamicky generovaný název DNS, načtěte podrobnosti o službě Application Gateway a přidružené IP adrese nebo názvu DNS, a to pomocí prvku PublicIPAddress připojeného ke službě Application Gateway. Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS. Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.

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

## <a name="delete-all-resources"></a>Odstranění všech prostředků

Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:

```powershell
Remove-AzureRmResourceGroup -Name appgw-RG
```

## <a name="next-steps"></a>Další kroky

Pokud chcete konfigurovat přesměrování zpracování SSL, přečtěte si článek [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Pokud chcete službu Application Gateway nakonfigurovat pro použití s interním nástrojem pro vyrovnávání zatížení, přečtěte si článek [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete získat další informace o obecných možnostech vyrovnávání zatížení, přečtěte si článek:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)


