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
ms.date: 06/20/2017
ms.author: davidmu
ms.openlocfilehash: c9c740a3a1a28a1a9a4f2abf579fe2adb54e4f47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Konfigurace brány firewall webových aplikací na nový nebo existující aplikace brány pomocí rozhraní příkazového řádku Azure

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

Tento článek ukazuje, jak [přidání brány firewall webových aplikací do existující aplikace bránu](#add-web-application-firewall-to-an-existing-application-gateway). Také ukazuje, jak [vytvoření služby application gateway, která používá brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall).

![scénář image][scenario]

## <a name="prerequisite-install-the-azure-cli-20"></a>Předpoklad: Instalace rozhraní příkazového řádku Azure 2.0

Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure (Azure CLI) pro Mac, Linux a Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>Rozdíly konfigurace firewall webových aplikací

Pokud jste si přečetli [vytvoření služby application gateway pomocí Azure CLI](application-gateway-create-gateway-cli.md), pochopit SKU nastavení konfigurace při vytvoření služby application gateway. Firewall webových aplikací poskytuje další nastavení můžete definovat při konfiguraci verze SKU na aplikační brány. Neexistují žádné další změny, které provedete ve službě application gateway sám sebe.

| **Nastavení** | **Podrobnosti**
|---|---|
|**SKU** |Normální aplikační brána bez firewall webových aplikací podporuje **standardní\_malé**, **standardní\_střední**, a **standardní\_velké**velikosti. Se zavedením firewall webových aplikací, jsou dva další SKU, **firewall webových aplikací\_střední** a **firewall webových aplikací\_velké**. Firewall webových aplikací není podporována na malé application Gateway.|
|**Režim** | Toto nastavení je režim firewall webových aplikací. Povolené hodnoty jsou **detekce** a **prevence**. Když je nastavený firewall webových aplikací **detekce** režimu, všechny hrozby jsou uložené v souboru protokolu. V **prevence** režim, jsou protokolovány události, ale útočník obdrží 403 Neautorizováno odpověď ze služby application gateway.|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Přidání brány firewall webových aplikací do existující aplikační brány

Tento příkaz změní stávající standardní aplikační brány firewall webových aplikací s podporou aplikační brány:

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Tento příkaz aktualizuje aplikační bránu firewall webových aplikací. Chcete-li pochopit, jak zobrazit protokoly pro službu application gateway, přečtěte si téma [diagnostics Application Gateway](application-gateway-diagnostics.md). Vzhledem k zabezpečení povaze firewall webových aplikací zkontrolujte protokoly pravidelně zjistit postavení zabezpečení webových aplikací.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Vytvoření služby application gateway pomocí brány firewall webových aplikací

Následující příkaz vytvoří aplikační brány s firewall webových aplikací:

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> Application Gateway vytvoří s základní konfigurací firewall webových aplikací jsou nakonfigurovány s řádku 3.0 pro ochranu.

## <a name="get-an-application-gateway-dns-name"></a>Získat název DNS brány aplikace

Po vytvoření brány je dalším krokem je konfigurace front-endu pro komunikaci. Pokud použijete veřejnou IP adresu, služby application gateway vyžaduje dynamicky přiřazené název DNS, který není popisný. Aby se zajistilo, že uživatelé můžete dosáhl aplikační bránu, použijte záznam CNAME přejděte na veřejný koncový bod služby application gateway. Další informace najdete v tématu [nakonfigurovat vlastní název domény pro cloudové služby Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Pokud chcete nakonfigurovat záznam CNAME, načtěte podrobnosti o aplikační brány a její přidružené název IP a DNS pomocí prvku PublicIPAddress připojit k službě application gateway. Použijte službu application gateway název DNS vytvořit záznam CNAME, který ukazuje dva webové aplikace k tomuto názvu DNS. Nedoporučujeme použití záznamy A, protože VIP může změnit po restartování služby application gateway.

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
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

Další postup přizpůsobení pravidel firewall webových aplikací najdete v tématu [přizpůsobit pravidla brány firewall webových aplikací pomocí Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
