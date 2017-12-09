---
title: "Konfigurovat přesměrování zpracování SSL - Azure Application Gateway - Azure CLI 2.0 | Microsoft Docs"
description: "Tento článek poskytuje pokyny pro vytvoření služby application gateway pomocí protokolu SSL, přesměrování zpracování úloh pomocí Azure CLI 2.0"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 4bdca33dae2ce52fdeccdae9a67abb6667593f9d
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-cli-20"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě. Přesměrování zpracování SSL zjednodušuje i správy certifikátů na serveru front-end.

## <a name="prerequisite-install-the-azure-cli-20"></a>Předpoklad: Instalace rozhraní příkazového řádku Azure 2.0

Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="required-components"></a>Požadované součásti

* **Fond back-end serverů**: seznam IP adres back-end serverů. Uvedené IP adresy by měly patřit do podsítě virtuální sítě nebo by měla být veřejné IP adresy nebo virtuální IP adresy (VIP).
* **Nastavení fondu back-end serverů**: každý fond má nastavení, jako je port, protokol a spřažení na základě souboru cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port**: Tento port je veřejný port, který se otevírá ve službě application gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces**: naslouchací proces má front-end port, protokol (Http nebo Https; tato nastavení jsou malá a velká písmena) a název certifikátu SSL (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo**: pravidlo váže naslouchací proces a fond back-end serverů a definuje, kterému fondu back-end serverů směrovat provoz při volání příslušného naslouchacího procesu. V tuhle chvíli se podporuje jenom *základní* pravidlo. *Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

**Další poznámky ke konfiguraci**

Pro konfiguraci certifikátů SSL by se měl změnit protokol v **HttpListener** na *Https* (rozlišování velkých a malých písmen). Přidat **SslCertificate** element **HttpListener** s hodnotou proměnné nakonfigurovanou pro certifikát SSL. Front-end port se musí aktualizovat na **443**.

**Chcete-li povolit spřažení na základě souborů cookie**: můžete nakonfigurovat služby application gateway zajistit, že žádost od klientské relace vždy směrovala na stejný virtuální počítač ve webové farmě. K tomu, vložte soubor cookie relace, který umožňuje bráně řízení provozu odpovídajícím způsobem. Když chcete povolit spřažení na základě souboru cookie, nastavte **CookieBasedAffinity** na *Povoleno* v elementu **BackendHttpSettings**.

## <a name="configure-ssl-offload-on-an-existing-application-gateway"></a>Konfigurovat přesměrování zpracování SSL na existující aplikační brány

Zadejte následující příkazy ke konfiguraci přesměrování zpracování SSL existující aplikační brány:

```azurecli-interactive
#!/bin/bash

# Create a new front end port to be used for SSL
az network application-gateway frontend-port create \
  --name sslport \
  --port 443 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Upload the .pfx certificate for SSL offload
az network application-gateway ssl-cert create \
  --name "newcert" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new listener referencing the port and certificate created earlier
az network application-gateway http-listener create \
  --frontend-ip "appGatewayFrontendIP" \
  --frontend-port sslport  \
  --name sslListener \
  --ssl-cert newcert \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new back-end pool to be used
az network application-gateway address-pool create \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG" \
  --name "appGatewayBackendPool2" \
  --servers 10.0.0.7 10.0.0.8

# Create a new back-end HTTP settings using the new probe
az network application-gateway http-settings create \
  --name "settings2" \
  --port 80 \
  --cookie-based-affinity Enabled \
  --protocol "Http" \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new rule linking the listener to the back-end pool
az network application-gateway rule create \
  --name "rule2" \
  --rule-type Basic \
  --http-settings settings2 \
  --http-listener ssllistener \
  --address-pool temp1 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

```

## <a name="create-an-application-gateway-with-ssl-offload"></a>Vytvoření služby application gateway s přesměrování zpracování SSL

Následující příklad vytvoří aplikační brány s přesměrování zpracování SSL. Certifikát a heslo certifikátu musí být aktualizovány na platný privátní klíč.

```azurecli-interactive
#!/bin/bash

# Creates an application gateway with SSL offload
az network application-gateway create \
  --name "AdatumAppGateway3" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG2" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet" \
  --subnet-address-prefix "10.0.0.0/28" \
  --frontend-port 443 \
  --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 \
  --sku "Standard_Small" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip" \
  --public-ip-address-allocation "dynamic"
```

## <a name="get-an-application-gateway-dns-name"></a>Získat název DNS brány aplikace

Po vytvoření brány je dalším krokem je konfigurace front-endu pro komunikaci.  Aplikační brána vyžaduje dynamicky přiřazené název DNS, při použití veřejnou IP adresu, která není popisný. Zajistěte, aby koncoví uživatelé mohou dosáhl aplikační bránu, můžete záznam CNAME přejděte na veřejný koncový bod služby application gateway. Další informace najdete v tématu [konfigurace vlastního názvu domény pro v Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Pokud chcete konfigurovat alias, načíst podrobnosti o aplikační brány a její přidružené IP a DNS název pomocí **PublicIPAddress** element připojený k službě application gateway. Použijte službu application gateway název DNS vytvořit záznam CNAME, který ukazuje dva webové aplikace k tomuto názvu DNS. Nedoporučujeme použití záznamů A protože VIP můžete změnit na restartovat aplikační brány.


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

## <a name="next-steps"></a>Další kroky

Pokud chcete provést konfiguraci aplikační brány pro použití s nástrojem pro vyrovnávání zatížení pro vnitřní naleznete v části [vytvoření aplikační brány s nástrojem pro vyrovnávání zatížení pro vnitřní](application-gateway-ilb.md).

Další informace o možnostech obecně Vyrovnávání zatížení najdete v tématu:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
