---
title: "Vytvoření služby application gateway pro hostování více webů | Microsoft Docs"
description: "Tato stránka obsahuje pokyny k vytvoření, konfiguraci služby Azure application gateway pro hostování několika webových aplikací na stejnou bránu."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: d42efa7d359f5c87c14afbfd138328b37c8ae6c2
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Vytvoření služby application gateway pro hostování několika webových aplikací

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Hostování více lokalitu umožňuje nasadit více než jednu webovou aplikaci ve stejném application gateway. Přitom spoléhá na přítomnost Hlavička hostitele v příchozím požadavku HTTP, chcete-li zjistit, který naslouchací proces by přijímat přenosy. Naslouchací proces pak přesměruje přenosy na příslušné back-end fondu podle konfigurace v definici pravidla brány. V protokolu SSL povoleno webových aplikací Aplikační brána spoléhá na toto rozšíření indikace názvu serveru (SNI) vyberte správné naslouchací proces pro webový provoz. Běžně používá pro více hostování lokality je načíst vyrovnávat požadavky na jiných webových domén na jiný server back endové fondy. Více subdomény stejné kořenové domény podobně také může být hostovaný na stejné aplikační brány.

## <a name="scenario"></a>Scénář

V následujícím příkladu se Aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com s dvěma fondy back-end serverů: contoso fondu serverů a fond serverů fabrikam. Podobně jako instalační program může subdomény hostitele jako app.contoso.com a blog.contoso.com.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Servery přidané do fondu back-end pro použití služby application gateway, musí existovat nebo mít své koncové body vytvořené, buď ve virtuální síti v jiné podsíti, nebo s veřejné nebo virtuálními IP Adresami přiřazené.

## <a name="requirements"></a>Požadavky

* **Fond back-end serverů:** Seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami. Můžete také použít plně kvalifikovaný název domény.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL). Pro aplikace s povolenou více servery brány název hostitele a indikátory SNI jsou také přidat.
* **Pravidlo:** pravidlo váže naslouchací proces fondu back-end serverů a definuje, kterému fondu back-end serverů provoz směrovat při volání příslušného naslouchacího procesu. Pravidla se zpracovávají v pořadí, ve kterém jsou uvedeny a provoz se přesměruje přes první pravidlo, které odpovídá bez ohledu na specifické podobě. Například, pokud máte pravidlo pomocí základní naslouchací proces a pravidla pomocí více lokalit naslouchací proces obou na stejném portu, musí být uvedené pravidlo s několika lokalitami naslouchací proces před pravidlo základní naslouchací proces, aby pravidlo více lokalit a fungovat podle očekávání.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Tady jsou kroky potřebné k vytvoření aplikační brány:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro službu application gateway.
3. Vytvoření objektu konfigurace služby Application Gateway
4. Vytvoření prostředku služby Application Gateway

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Další informace najdete v [pomocí prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

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
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

Případně můžete vytvořit také značky pro skupinu prostředků pro službu application gateway:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem **appgw-RG** s umístění ve **západní USA**.

> [!NOTE]
> Když potřebujete nakonfigurovat vlastní test paměti svojí aplikační brány, přečtěte si část [Vytvořit bránu s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Navštivte [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md) Další informace.

## <a name="create-a-virtual-network-and-subnets"></a>Vytvoření virtuální sítě a podsítě

Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Manageru. Dvě podsítě jsou vytvořené v tomto kroku. První podsíť je pro službu application gateway sám sebe. Aplikační brána vyžaduje vlastní podsíti pro uložení její instance. Pouze jiné službu application Gateway se dá nasadit v této podsíti. Druhou podsíť se používá pro uložení aplikace back-end serverů.

### <a name="step-1"></a>Krok 1

Přiřaďte rozsah adres 10.0.0.0/24 proměnné podsítě, který se má použít pro službu application gateway.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a>Krok 2

Přiřaďte proměnnou Podsíť2 má být použit pro back-endové fondy 10.0.1.0/24 rozsah adres.

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>Krok 3

Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24 a 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a>Krok 4

Přiřaďte proměnnou podsítě pro další kroky, které se vytvoří aplikační brána.

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci

Vytvořte prostředek veřejné IP adresy **publicIP01** ve skupině prostředků **appgw-rg** pro oblast Západní USA.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

IP adresa je ke službě Application Gateway přiřazena při spuštění služby.

## <a name="create-application-gateway-configuration"></a>Vytvoření konfigurace brány aplikace

Před vytvořením aplikační brány musíte nastavit všechny položky konfigurace. Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.

### <a name="step-1"></a>Krok 1

Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**. Při spuštění služby application gateway, vybere IP adresa z nakonfigurované podsítě a směrovat síťový provoz na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a>Krok 2

Nakonfigurujte fond back-end IP adres s názvem **pool01** a **pool2** s IP adresami **134.170.185.46**, **134.170.188.221**, **134.170.185.50** pro **pool1** a **134.170.186.46**, **134.170.189.221**, **134.170.186.50** pro **pool2**.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

V tomto příkladu jsou dva back endové fondy ke směrování síťového provozu založené na požadovaný server. Jeden fond přijímá provoz z lokality "contoso.com" a dalších fondu přijímá provoz z lokality "fabrikam.com". Je třeba nahradit předchozí IP adresy, chcete-li přidat vlastní koncové body aplikace IP adresu. Místo interní IP adresy můžete pro back-end instance také použít veřejné IP adresy, plně kvalifikovaný název domény nebo síťový adaptér Virtuálního počítače. Určete plně kvalifikované názvy domény místo IP adresy v prostředí PowerShell pomocí "-BackendFQDNs" parametr.

### <a name="step-3"></a>Krok 3

Konfigurace nastavení brány aplikace **poolsetting01** a **poolsetting02** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end. V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro back endové fondy. Každý fond back-end může mít vlastní nastavení fondu back-end.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Krok 4

Nakonfigurujte IP adresu front-endu s koncovým bodem s veřejnou IP adresou.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Krok 5

Nakonfigurujte front-end port pro službu Application Gateway.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a>Krok 6

Nakonfigurujte dva certifikáty SSL pro dva weby, že budeme podporovat v tomto příkladu. Jeden certifikát je pro provoz contoso.com a druhá je pro provoz fabrikam.com. Tyto certifikáty musí být z certifikační autority, která vydala certifikáty pro vaše weby. Certifikáty podepsané svým držitelem podporuje, ale nedoporučuje pro produkční provoz.

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a>Krok 7

Nakonfigurujte dva naslouchací procesy pro dva weby v tomto příkladu. Tento krok nakonfiguruje naslouchací procesy pro veřejné IP adresy, portu a hostitele, na které se používá k přijetí příchozích přenosů. Parametr název hostitele je vyžadovaná pro podporu více lokality a musí být nastavena na příslušné web, pro kterou je provoz přijata. RequireServerNameIndication parametr musí být nastavena na hodnotu true pro weby, které potřebují podporu pro protokol SSL ve scénáři více hostitelů. Pokud se vyžaduje podpora protokolu SSL, musíte taky zadat certifikát SSL, který se používá k zabezpečení přenosů pro tuto webovou aplikaci. Kombinace FrontendIPConfiguration, FrontendPort a název hostitele musí být jedinečné pro naslouchací proces. Každý naslouchací proces může podporovat jeden certifikát.

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a>Krok 8

Vytvořte dvě nastavení pravidla pro dva webové aplikace v tomto příkladu. Pravidlo sváže společně naslouchací procesy, back-endové fondy a nastavení protokolu http. Tento krok konfiguruje službu application gateway používat základní pravidlo směrování, jednu pro každý web. Data na každý web přijme jeho nakonfigurované naslouchací proces a je pak přesměrované na jeho nakonfigurované back-end fondu pomocí vlastnosti uvedené na BackendHttpSettings.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a>Krok 9

Nakonfigurujte počet instancí a velikost pro službu Application Gateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Vytvořte aplikační bránu

Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> Zřizování aplikace brány je dlouhotrvající operace a může trvat delší dobu pro dokončení.
> 
> 

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Budete muset načíst podrobnosti o službě Application Gateway a název její přidružené IP adresy nebo DNS, a to pomocí elementu PublicIPAddress připojeného ke službě Application Gateway. Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS. Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.

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

Zjistěte, jak chránit své weby s [Application Gateway - brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)

