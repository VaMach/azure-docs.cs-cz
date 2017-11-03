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
ms.topic: article
ms.date: 08/17/2016
ms.author: naziml
ms.openlocfilehash: 4c0e2d649f71d7797efbfe2c8e93ea0c844152df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment
## <a name="overview"></a>Přehled
Brány firewall webových aplikací, jako [Barracuda firewall webových aplikací pro Azure](https://www.barracuda.com/programs/azure) která je dostupná na [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) pomáhá zabezpečit zkontrolováním příchozí webové přenosy blokování vložení SQL, webových aplikací Skriptování, malwaru nahrávání & aplikace DDoS a jiným útokům. Je také zkontroluje, zda obsahuje odpovědi ze serveru back endové webové pro prevenci ztráty dat (DLP). V kombinaci s izolace a další škálování poskytované prostředí App Service, toto poskytuje ideální prostředí k hostiteli obchodní kritické webovým aplikacím, které je potřeba odolat škodlivými požadavky a vyšší objemy přenosů.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Nastavení
Pro tento dokument, který bude nakonfigurujeme naše App Service Environment za několik zatížení vyvážit instancí Barracuda firewall webových aplikací, aby pouze provoz z firewall webových aplikací dosáhnout App Service Environment a nebude dostupný z hraniční síti. Azure Traffic Manager jsme bude mít i před naše instancí Barracuda firewall webových aplikací na Vyrovnávání zatížení v rámci datových center Azure a oblastech. Nejvyšší úrovni diagram instalace bude vypadat co jsou uvedeny níže.

![Architektura][Architecture] 

> Poznámka: se zavedením [ILB podporu pro App Service Environment](app-service-environment-with-internal-load-balancer.md), můžete nakonfigurovat App Service Environment pro nepřístupný od DMZ a být k dispozici pro privátní sítě. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Konfigurace prostředí služby App Service
Konfigurace služby App Service Environment najdete v tématu [naší dokumentaci](app-service-web-how-to-create-an-app-service-environment.md) na předmět. Jakmile máte vytvoření služby App Service Environment, můžete vytvořit webové aplikace, aplikace API a [Mobile Apps](../../app-service-mobile/app-service-mobile-value-prop.md) v tomto prostředí, které budou všechny chráněné za firewall webových aplikací nakonfigurujeme v další části.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurace Barracuda firewall webových aplikací cloudové služby
Barracuda má [podrobné článku](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) na nasazení jeho firewall webových aplikací na virtuálním počítači v Azure. Ale vzhledem k tomu, že nám chcete redundance a není způsobit jediný bod selhání, které chcete nasadit alespoň 2 virtuální počítače instance firewall webových aplikací do stejné cloudové služby při těchto pokynů.

### <a name="adding-endpoints-to-cloud-service"></a>Přidání koncové body pro cloudové služby
Jakmile máte 2 nebo více virtuálních počítačů firewall webových aplikací instancí v rámci cloudové služby můžete použít [portál Azure](https://portal.azure.com/) přidat HTTP a HTTPS koncové body, které se používají v aplikaci, jak je znázorněno na obrázku níže.

![Konfigurace koncového bodu][ConfigureEndpoint]

Pokud vaše aplikace používat ostatní koncové body, ujistěte se, že jste přidejte je do tohoto seznamu také. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurace Barracuda firewall webových aplikací prostřednictvím portálu pro správu
Barracuda firewall webových aplikací používá TCP Port 8000 pro konfiguraci prostřednictvím portálu pro správu. Vzhledem k tomu, že máme několik instancí virtuálních počítačů firewall webových aplikací, budete muset sem kroky zopakujte pro každou instanci virtuálního počítače. 

> Poznámka: Po dokončení konfigurace firewall webových aplikací odeberte koncový bod TCP/8000 ze všech firewall webových aplikací virtuálních počítačů k lepšímu zabezpečení vašeho firewall webových aplikací.
> 
> 

Přidáte koncový bod správy, jak je znázorněno na obrázku níže ke konfiguraci vaší Barracuda firewall webových aplikací.

![Přidání koncového bodu správy][AddManagementEndpoint]

Použijte prohlížeč a přejděte ke koncovému bodu správy v cloudové službě. Pokud cloudové služby je volána test.cloudapp.net, by procházením http://test.cloudapp.net:8000 přístup k tomuto koncovému bodu. Měli byste vidět přihlašovací stránky, jako níže, můžete se přihlásit pomocí přihlašovacích údajů zadaných ve fázi instalace virtuálních počítačů firewall webových aplikací.

![Správa přihlašovací stránky][ManagementLoginPage]

Po přihlášení byste měli vidět řídicí panel jako ten, který nabídne základní statistické údaje o ochranu firewall webových aplikací na obrázku níže.

![Řídicí panel správy][ManagementDashboard]

Kliknutím na kartu služby vám umožní nakonfigurovat vaše firewall webových aplikací pro služby, které chrání. Další informace o konfiguraci vašeho Barracuda firewall webových aplikací lze najít [jejich dokumentaci](https://techlib.barracuda.com/waf/getstarted1). V příkladu níže webové aplikace Azure byla nakonfigurována s provozem na protokolu HTTP a HTTPS.

![Přidání služeb správy][ManagementAddServices]

> Poznámka: V závislosti na tom, jak jsou nakonfigurované vašich aplikací a jaké funkce jsou používány ve službě App Service Environment, je nutné pro přenos dat pro TCP jiné porty než 80 a 443, například pokud máte instalaci IP SSL pro webovou aplikaci. Seznam síťové porty používané v prostředí App Service naleznete v [řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) části síťové porty.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurace Microsoft Azure Traffic Manageru (volitelné)
Pokud vaše aplikace je k dispozici v několika oblastech, pak budete chtít načíst vyvážit je za [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md). K tomu můžete přidat koncový bod v [portál Azure classic](https://manage.azure.com) pomocí název cloudové služby pro vaše firewall webových aplikací v profil služby Traffic Manager, jak je znázorněno na obrázku níže. 

![Koncový bod Traffic Manager][TrafficManagerEndpoint]

Pokud vaše aplikace vyžaduje ověřování, zajistěte, že abyste měli některé prostředků, která nevyžaduje žádné ověřování pro správce provozu na příkaz ping dostupnosti vaší aplikace. Adresu URL v části konfigurace můžete nakonfigurovat na [portál Azure classic](https://manage.azure.com) jak je uvedeno níže.

![Konfigurace Traffic Manageru][ConfigureTrafficManager]

Předávání příkazy ping Traffic Manager z vašeho firewall webových aplikací do vaší aplikace, musíte instalační program překlady webu na vaší Barracuda firewall webových aplikací pro přenos dat do vaší aplikace, jak je znázorněno v následujícím příkladu.

![Překlady webu][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Zabezpečení přenosy do služby App Service Environment pomocí skupin zabezpečení sítě (NSG)
Postupujte podle [řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) podrobnosti o provoz směřující do služby App Service Environment z firewall webových aplikací pouze pomocí adresy VIP cloudové služby. Zde je ukázka příkazu prostředí Powershell pro provádění této úlohy pro TCP port 80.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Nahraďte virtuální IP adresa (VIP) vaší firewall webových aplikací cloudové služby SourceAddressPrefix.

> Poznámka: VIP cloudové služby se změní při odstranit a znovu vytvořit Cloudovou službu. Ujistěte se, až to uděláte tak, aktualizujte IP adresu ve skupině prostředků sítě. 
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
