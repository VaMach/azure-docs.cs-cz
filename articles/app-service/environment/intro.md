---
title: "Úvod do služby Azure App Service Environment"
description: "Stručný přehled služby Azure App Service Environment"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 63e2256ca6d392a0cd284269e8c2d059f94e9139
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="introduction-to-app-service-environments"></a>Úvod do služby App Service Environment #
 
## <a name="overview"></a>Přehled ##

Azure App Service Environment je funkce služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací App Service ve velkém rozsahu. Tato funkce může hostování vašich webových aplikací, [mobilní aplikace][mobileapps], aplikace API, a [funkce][Functions].

Služby App Service Environment (ASEs) jsou vhodné pro aplikační procesy, které vyžadují:

- Velmi velký rozsah.
- Izolace a bezpečný přístup.
- Využití velkého množství paměti.

Zákazníci vytvářet více ASEs v rámci jedné oblasti Azure nebo nad několika oblastmi Azure. Díky této flexibilitě ASEs ideální pro bezstavové aplikačními vrstvami podporu vysoké zatížení RPS vodorovně škálování.

ASEs izolují spouštění jenom jednoho zákazníka aplikací a vždy nasazených do virtuální sítě. Zákazníci mají jemně odstupňovanou kontrolu nad aplikace příchozí a odchozí síťový provoz. Aplikace může vytvořit vysokorychlostní zabezpečené připojení prostřednictvím sítě VPN k firemním prostředkům místně.

* ASEs povolit špičkové aplikace hostuje s zabezpečený přístup. Další informace najdete v tématu [podrobné informace AzureCon](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) na ASEs.
* Více ASEs lze škálovat horizontálně. Další informace najdete v tématu [postup nastavte geograficky distribuovaná aplikace nároků](app-service-app-service-environment-geo-distributed-scale.md).
* ASEs slouží ke konfiguraci Architektura zabezpečení, jak je znázorněno v AzureCon podrobné informace. Konfigurace Architektura zabezpečení uvedené v AzureCon podrobné informace najdete v tématu [článek o tom, jak implementovat architektura vrstveného zabezpečení](app-service-app-service-environment-layered-security.md) s služby App Service Environment.
* Aplikace běžící na ASEs může mít svůj přístup ověřované vrácení nadřazeného zařízeními, jako jsou brány firewall systému webové aplikace (WAFs). Další informace najdete v tématu [konfigurace firewall webových aplikací pro služby App Service Environment](app-service-app-service-environment-web-application-firewall.md).

## <a name="dedicated-environment"></a>Vyhrazeném prostředí ##

App Service Environment je vyhrazený výhradně v rámci jednoho předplatného a může být hostitelem 100 instancí. Rozsah může mít rozsah 100 instance jednoho plánu služby App Service na 100 plány služby App Service jedné instance a všechno mezi nimi.

App Service Environment se skládá z front je ukončená a pracovních procesů. Front-end jsou zodpovědní za ukončení protokolu HTTP nebo HTTPS a automatické vyvažování zátěže požadavků aplikace v App Service Environment. Front-end se automaticky přidají jako plány služby App Service v App Service Environment jsou škálovat na více systémů.

Zaměstnanci jsou role, které jsou hostiteli aplikace zákazníka. Pracovníci jsou k dispozici tři pevné velikosti:

* Jeden základní/3.5 GB paměti RAM
* Dvě jádra/7 GB paměti RAM
* Čtyři základní/14 GB paměti RAM

Zákazníci, není potřeba spravovat front-end a pracovních procesů. Všechny infrastruktury je automaticky přidán jako zákazníci škálování jejich plány služby App Service. Jak plány služby App Service jsou vytvořená nebo škálovat App Service Environment, požadované infrastrukturu je přidat nebo odebrat podle potřeby.

Není nestrukturované měsíční rychlost, pro který platí za infrastruktury a nezmění s velikostí App Service Environment App Service Environment. Kromě toho je s náklady za jádra plán služby App Service. Všechny aplikace, které jsou hostované v App Service Environment jsou v izolované ceny SKU. Informace o cenách pro App Service Environment, najdete v článku [služby App Service – ceny] [ Pricing] stránky a projděte si dostupné možnosti pro ASEs.

## <a name="virtual-network-support"></a>Podpora virtuální sítě ##

App Service Environment může vytvořit pouze v virtuální síť Azure Resource Manager. Další informace o virtuálních sítí Azure, najdete v článku [Azure virtuální sítě – nejčastější dotazy](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). App Service Environment vždy existuje ve virtuální síti a přesněji řečeno, v rámci jedné podsítě virtuální sítě. Funkce zabezpečení systému virtuální sítě můžete řídit příchozí a odchozí síťové komunikace pro vaše aplikace.

App Service Environment může být internetové s veřejnou IP adresu nebo interní přístupem jenom adresu (ILB) nástroj pro vyrovnávání zatížení Azure interní.

[Skupin zabezpečení sítě] [ NSGs] omezit příchozí síťovou komunikaci na podsíť, které se nachází App Service Environment. Skupiny Nsg můžete použít ke spuštění aplikace za nadřazeného zařízení a služby, například WAFs a poskytovatelů SaaS sítě.

Aplikace také často potřebují přístup k podnikovým prostředkům, jako jsou třeba interní databáze a webové služby. Pokud nasadíte App Service Environment ve virtuální síti, který má připojení VPN k místní síti, aplikace, které App Service Environment přístup k místním prostředkům. Tato možnost platí bez ohledu na to, jestli je síť VPN [site-to-site](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) nebo [Azure ExpressRoute](http://azure.microsoft.com/services/expressroute/) sítě VPN.

Další informace o tom, jak ASEs práci s virtuálními sítěmi a místními sítěmi najdete v tématu [aspekty sítě služby App Service Environment][ASENetwork].

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud/player]

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##

Služba App Service Environment má dvě verze: ASEv1 a ASEv2. Uvedené informace se podle ASEv2. V této části jsou uvedeny rozdíly mezi ASEv1 a ASEv2. 

V ASEv1 budete muset spravovat všechny prostředky ručně. Který obsahuje front-end, pracovníků a adres IP použitých pro založená na protokolu IP. Předtím, než můžete škálovat plán služby App Service, budete muset první horizontální navýšení kapacity fondu pracovních procesů, kam chcete ji hostovat.

ASEv1 používá jiný model tvorby cen z ASEv2. V ASEv1 platí pro každý jádra přidělené. Používá pro front-end nebo pracovních procesů, které nejsou hostování jakékoliv zátěže jader, který zahrnuje. V ASEv1 je výchozí maximální měřítko velikostí App Service Environment 55 celkový počet hostitelů. Který zahrnuje pracovníků a front-end. Jedna z výhod ASEv1 je, že může být nasazena v klasické virtuální sítě a virtuální sítě Resource Manager. Další informace o ASEv1 najdete v tématu [App Service Environment v1 ÚVOD][ASEv1Intro].

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
