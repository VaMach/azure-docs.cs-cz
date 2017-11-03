---
title: "Konfigurace brány firewall webových aplikací: Azure Application Gateway | Microsoft Docs"
description: "Tento článek obsahuje pokyny o tom, jak začít používat brány firewall webových aplikací na existující nebo nové aplikační brány."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.openlocfilehash: e8106805d21b325e33fb3ab376db75cd783b9042
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurace brány firewall webových aplikací na bránu nový nebo existující aplikace

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Naučte se vytvářet brány firewall webových aplikací (firewall webových aplikací)-Povolit aplikační brány. Také zjistěte, jak přidat do existující aplikace bránu firewall webových aplikací.

Firewall webových aplikací v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace.

 Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, zda jsou v cloudu nebo místně. Application Gateway poskytuje mnoho řadiče (ADC) funkce doručování aplikací:

 * Vyrovnávání zatížení HTTP
 * Spřažení na základě souboru cookie relace
 * Secure Sockets Layer (SSL) snižování zátěže
 * Testy vlastní stavu
 * Podpora pro funkce ve více lokalitách
 
 Úplný seznam podporovaných funkcích naleznete v tématu [Přehled služby Application Gateway](application-gateway-introduction.md).

Tento článek ukazuje, jak [existující aplikační brány přidat firewall webových aplikací](#add-web-application-firewall-to-an-existing-application-gateway). Také ukazuje, jak [vytvoření služby application gateway, která používá firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall).

![scénář image][scenario]

## <a name="waf-configuration-differences"></a>Rozdíly konfigurace firewall webových aplikací

Pokud jste si přečetli [vytvoření služby application gateway pomocí prostředí PowerShell](application-gateway-create-gateway-arm.md), pochopit SKU nastavení konfigurace při vytvoření služby application gateway. Firewall webových aplikací poskytuje další nastavení můžete definovat při konfiguraci SKU na aplikační brány. Neexistují žádné další změny, které provedete ve službě application gateway sám sebe.

| **Nastavení** | **Podrobnosti**
|---|---|
|**SKU** |Normální aplikační brána bez firewall webových aplikací podporuje **standardní\_malé**, **standardní\_střední**, a **standardní\_velké**velikosti. Se zavedením firewall webových aplikací, jsou dva další SKU, **firewall webových aplikací\_střední** a **firewall webových aplikací\_velké**. Firewall webových aplikací není podporována na malé application Gateway.|
|**Vrstvy** | Dostupné hodnoty jsou **standardní** nebo **firewall webových aplikací**. Pokud používáte firewall webových aplikací, musíte vybrat **firewall webových aplikací**.|
|**Režim** | Toto nastavení je režim firewall webových aplikací. Povolené hodnoty jsou **detekce** a **prevence**. Když je nastavený firewall webových aplikací **detekce** režimu, všechny hrozby jsou uložené v souboru protokolu. V **prevence** režim, jsou protokolovány události ale útočník obdrží 403 Neautorizováno odpověď ze služby application gateway.|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Přidání brány firewall webových aplikací do existující aplikační brány

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Další informace najdete v tématu [použijte rozhraní Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

1. Přihlaste se k účtu Azure.

    ```powershell
    Login-AzureRmAccount
    ```

2. Vyberte předplatné, které chcete použít pro tento scénář.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Načtěte brány, ve které chcete přidat firewall webových aplikací.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

4. Nakonfigurujte SKU firewall webových aplikací. Dostupné velikosti jsou **firewall webových aplikací\_velké** a **firewall webových aplikací\_střední**. Pokud použijete firewall webových aplikací, musí být vrstvě **firewall webových aplikací**. Zkontrolujte kapacitu, při nastavení verze SKU.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

5. Nakonfigurujte nastavení firewall webových aplikací, jak jsou definovány v následujícím příkladu. Pro **FirewallMode**, dostupné hodnoty jsou **prevence** a **detekce**.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

6. Aktualizujte aplikační bránu s nastavením, které jste zadali v předchozím kroku.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Tento příkaz aktualizuje aplikační bránu firewall webových aplikací. Chcete-li pochopit, jak zobrazit protokoly pro službu application gateway, přečtěte si téma [diagnostics Application Gateway](application-gateway-diagnostics.md). Vzhledem k zabezpečení povaze firewall webových aplikací zkontrolujte protokoly pravidelně zjistit postavení zabezpečení webových aplikací.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Vytvoření služby application gateway pomocí brány firewall webových aplikací

Následující postup vás provede celý proces vytvoření služby application gateway s firewall webových aplikací.

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Další informace najdete v tématu [použijte rozhraní Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

1. Přihlaste se k Azure spuštěním `Login-AzureRmAccount`. Se zobrazí výzva k ověření pomocí svých přihlašovacích údajů.

2. Zkontrolujte předplatná pro příslušný účet spuštěním `Get-AzureRmSubscription`.

3. Vyberte, jaké předplatné použít.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pro službu application gateway.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem "appgw-RG" umístěním "Západní USA".

> [!NOTE]
> Když potřebujete nakonfigurovat vlastní test paměti svojí aplikační brány, přečtěte si část [Vytvořit bránu s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Další informace najdete v tématu [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).

### <a name="configure-a-virtual-network"></a>Konfigurace virtuální sítě

Služby application gateway vyžaduje vlastní podsíť. V tomto kroku vytvoříte virtuální síť s adresní prostor 10.0.0.0/16 a dvě podsítě, jeden pro službu application gateway a jeden pro členy fondu back-end.

```powershell
# Create a subnet configuration object for the application gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for application gateway instances. With a smaller subnet, you might not be able to add more instances of your application gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the back-end pool members subnet.
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previously created subnets.
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-the-public-ip-address"></a>Nakonfigurujte veřejnou IP adresu

Pro zpracování požadavků na externí, služby application gateway vyžaduje veřejnou IP adresu. Tato veřejná IP adresa nesmí mít `DomainNameLabel` definovaná používat službu application gateway.

```powershell
# Create a public IP address for use with the application gateway. Defining the `DomainNameLabel` during creation is not supported for use with the application gateway.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a>Nakonfigurujte aplikační bránu

```powershell
# Create an IP configuration to configure which subnet the application gateway uses. When the application gateway starts, it picks up an IP address from the configured subnet and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a back-end pool to hold the addresses or NIC handles for the application that the application gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authentication certificate to be used to communicate with the back-end servers.
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Configure the back-end HTTP settings to be used to define how traffic is routed to the back-end pool. The authentication certificate used in the previous step is added to the back-end HTTP settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a front-end port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a front-end IP configuration to associate the public IP address with the application gateway.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the application gateway. This certificate is used to decrypt and re-encrypt the traffic on the application gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the application gateway. Assign the front-end IP configuration, port, and SSL certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

# Create a load-balancer routing rule that configures the load balancer behavior. In this example, a basic round-robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the application gateway.
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define the SSL policy to use.
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

# Configure the WAF configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the application gateway by using all the previously created configuration objects.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Application Gateway vytvoří s základní konfigurací firewall webových aplikací jsou nakonfigurovány s řádku 3.0 pro ochranu.

## <a name="get-an-application-gateway-dns-name"></a>Získat název DNS brány aplikace

Po vytvoření brány je dalším krokem je konfigurace front-endu pro komunikaci. Pokud použijete veřejnou IP adresu, služby application gateway vyžaduje dynamicky přiřazené název DNS, který není popisný. Aby se zajistilo, že uživatelé můžete dosáhl aplikační bránu, použijte záznam CNAME přejděte na veřejný koncový bod služby application gateway. Další informace najdete v tématu [nakonfigurovat vlastní název domény pro cloudové služby Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Pokud chcete konfigurovat alias, načtěte podrobnosti o aplikační brány a její přidružené IP a DNS název pomocí prvku PublicIPAddress připojit k službě application gateway. Použijte službu application gateway název DNS vytvořit záznam CNAME, který ukazuje dva webové aplikace k tomuto názvu DNS. Nedoporučujeme použití záznamy A, protože VIP může změnit po restartování služby application gateway.

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

Další postup konfigurace protokolování diagnostiky do protokolu událostí, které jsou zjištěna nebo zabránit s firewall webových aplikací najdete v tématu [diagnostics Application Gateway](application-gateway-diagnostics.md).

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
