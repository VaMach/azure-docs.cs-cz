---
title: "Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment"
description: "Informace o konfiguraci brány firewall webových aplikací před služby App Service Environment."
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/17/2016
ms.author: naziml
ms.custom: mvc
ms.openlocfilehash: bfe36ee5365e71db4280e8e2ccff6db8e552dd39
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment
## <a name="overview"></a>Přehled
Brány firewall webových aplikací, jako [Barracuda firewall webových aplikací pro Azure](https://www.barracuda.com/programs/azure) která je dostupná na [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) pomáhá zabezpečit zkontrolováním příchozí webové přenosy blokování vložení SQL, webových aplikací Skriptování, malwaru nahrávání & aplikace DDoS a jiným útokům. Je také zkontroluje, zda obsahuje odpovědi ze serveru back endové webové pro prevenci ztráty dat (DLP). V kombinaci s izolace a další škálování poskytované prostředí App Service, toto poskytuje ideální prostředí k hostiteli obchodní kritické webovým aplikacím, které je potřeba odolat škodlivými požadavky a vyšší objemy přenosů.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Nastavení
K tomuto dokumentu jsme nakonfigurovat App Service Environment za několik instancí skupinu s vyrovnáváním zatížení Barracuda firewall webových aplikací, aby jenom přenosy z firewall webových aplikací dosáhnout App Service Environment a není přístupný z hraniční síti. Máme také Azure Traffic Manager před instancí Barracuda firewall webových aplikací na Vyrovnávání zatížení v rámci datových center Azure a oblastech. Vysokoúrovňový diagram instalace by vypadat podobně jako na následujícím obrázku:

![Architektura][Architecture] 

> [!NOTE]
> Se zavedením [ILB podporu pro App Service Environment](app-service-environment-with-internal-load-balancer.md), můžete nakonfigurovat App Service Environment pro nepřístupný od DMZ a být k dispozici pro privátní sítě. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Konfigurace prostředí služby App Service
Konfigurace služby App Service Environment, najdete v tématu [naší dokumentaci](app-service-web-how-to-create-an-app-service-environment.md) na předmět. Jakmile máte vytvoření služby App Service Environment, můžete vytvořit webové aplikace, aplikace API, a [Mobile Apps](../../app-service-mobile/app-service-mobile-value-prop.md) v tomto prostředí, které budou všechny chráněné za firewall webových aplikací nakonfigurujeme v další části.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurace Barracuda firewall webových aplikací cloudové služby
Barracuda má [podrobné článku](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) na nasazení jeho firewall webových aplikací na virtuálním počítači v Azure. Ale vzhledem k tomu, že nám chcete redundance a není způsobit jediný bod selhání, které chcete nasadit alespoň dva virtuální počítače instance firewall webových aplikací do stejné cloudové služby při těchto pokynů.

### <a name="adding-endpoints-to-cloud-service"></a>Přidání koncové body pro cloudové služby
Jakmile máte 2 nebo více virtuálních počítačů firewall webových aplikací instancí v rámci cloudové služby, můžete použít [portál Azure](https://portal.azure.com/) přidat HTTP a HTTPS koncové body, které se používají v aplikaci, jak je znázorněno na následujícím obrázku:

![Konfigurace koncového bodu][ConfigureEndpoint]

Pokud vaše aplikace používat ostatní koncové body, ujistěte se, že jste je přidejte do tohoto seznamu také. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurace Barracuda firewall webových aplikací prostřednictvím portálu pro správu
Barracuda firewall webových aplikací používá TCP Port 8000 pro konfiguraci prostřednictvím portálu pro správu. Pokud máte více instancí virtuálních počítačů firewall webových aplikací, budete muset sem kroky zopakujte pro každou instanci virtuálního počítače. 

> [!NOTE]
> Jakmile jste hotovi s konfigurací firewall webových aplikací, odeberte ze všech firewall webových aplikací virtuálních počítačů k lepšímu zabezpečení vašeho firewall webových aplikací koncový bod TCP/8000.
> 
> 

Přidáte koncový bod správy, jak je znázorněno na následujícím obrázku ke konfiguraci vaší Barracuda firewall webových aplikací.

![Přidání koncového bodu správy][AddManagementEndpoint]

Použijte prohlížeč a přejděte ke koncovému bodu správy v cloudové službě. Pokud cloudové služby je volána test.cloudapp.net, by procházením http://test.cloudapp.net:8000 přístup k tomuto koncovému bodu. Měli byste vidět přihlašovací stránky jako na následujícím obrázku, který vám umožní přihlásit pomocí přihlašovacích údajů, které jste zadali ve fázi instalace virtuálních počítačů firewall webových aplikací.

![Správa přihlašovací stránky][ManagementLoginPage]

Jakmile se přihlásíte, měli byste vidět řídicí panel jako na následujícím obrázku, který představuje základní statistické údaje o ochranu firewall webových aplikací.

![Řídicí panel správy][ManagementDashboard]

Kliknutím na **služby** karta vám umožní nakonfigurovat vaše firewall webových aplikací pro služby, které chrání. Další podrobnosti o konfiguraci vašeho Barracuda firewall webových aplikací najdete v tématu [jejich dokumentaci](https://techlib.barracuda.com/waf/getstarted1). V následujícím příkladu má nakonfigurované s provozem na protokolu HTTP a HTTPS webové aplikace Azure.

![Přidání služeb správy][ManagementAddServices]

> [!NOTE]
> V závislosti na tom, jak jsou nakonfigurované vašich aplikací a jaké funkce jsou používány ve službě App Service Environment budete muset dál provoz pro TCP jiné porty než 80 a 443, například pokud máte instalaci IP SSL pro webovou aplikaci. Seznam síťové porty používané v prostředí App Service najdete v tématu [řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) části síťové porty.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurace Microsoft Azure Traffic Manageru (volitelné)
Pokud vaše aplikace je k dispozici v několika oblastech, pak budete chtít načíst vyvážit je za [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md). Uděláte to tak, můžete přidat koncový bod v [portál Azure](https://portal.azure.com) pomocí název cloudové služby pro vaše firewall webových aplikací v profil služby Traffic Manager, jak je znázorněno na následujícím obrázku. 

![Koncový bod Traffic Manager][TrafficManagerEndpoint]

Pokud vaše aplikace vyžaduje ověřování, zajistěte, že abyste měli některé prostředků, která nevyžaduje žádné ověřování pro správce provozu na příkaz ping dostupnosti vaší aplikace. Můžete nakonfigurovat na adresu URL **konfigurace** stránky v [portál Azure](https://portal.azure.com) jak je znázorněno na následujícím obrázku:

![Konfigurace Traffic Manageru][ConfigureTrafficManager]

Předávání příkazy ping Traffic Manager z vašeho firewall webových aplikací do vaší aplikace, musíte nastavit překlady webu na vaší Barracuda firewall webových aplikací pro přenos dat do vaší aplikace, jak je znázorněno v následujícím příkladu:

![Překlady webu][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Zabezpečení přenosy do služby App Service Environment pomocí skupin zabezpečení sítě (NSG)
Postupujte podle [řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) podrobnosti o provoz směřující do služby App Service Environment z firewall webových aplikací pouze pomocí adresy VIP cloudové služby. Zde je ukázka příkazu prostředí Powershell pro provádění této úlohy pro TCP port 80.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Nahraďte virtuální IP adresa (VIP) vaší firewall webových aplikací cloudové služby SourceAddressPrefix.

> [!NOTE]
> VIP cloudové služby se změní při odstranit a znovu vytvořit Cloudovou službu. Ujistěte se, až to uděláte tak, aktualizujte IP adresu ve skupině prostředků sítě. 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
