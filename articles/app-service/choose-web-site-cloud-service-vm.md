---
title: "Porovnání Azure App Service, virtuální počítače, Service Fabric a Cloud Services | Microsoft Docs"
description: "Zjistěte, jak si vybrat mezi Azure App Service, virtuální počítače, Service Fabric a Cloud Services pro hostování webových aplikací."
services: app-service\web, virtual-machines, cloud-services
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 7d346a23-532a-42a9-98a8-23b7286d32a8
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/07/2016
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: edd5099d2804fdb5867b4be5b11a361004db1665
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Porovnání Azure App Service, virtuální počítače, Service Fabric a cloudové služby
## <a name="overview"></a>Přehled
Azure nabízí několik způsobů, jak hostitele webů: [Azure App Service][Azure App Service], [virtuální počítače][Virtual Machines], [Service Fabric] [ Service Fabric], a [cloudových služeb][Cloud Services]. Tento článek vám pomůže porozumět možnostem a ten správný pro vaši webovou aplikaci.

Aplikační služba Azure je nejlepší volbou pro většinu webové aplikace. Nasazení a správy se integrují do platformy, lokalit můžete rychle škálovat pro zpracování vysoké přenosové zatížení a poskytovat vysokou dostupnost, Správce vyrovnávání a provoz integrované zatížení. Existující lokality můžete přesunout do služby Azure App Service snadno s [nástroj pro migraci online](https://www.migratetoazure.net/), použít open-source aplikace z Galerie webových aplikací, nebo vytvořit nové lokality pomocí framework a nástrojů dle vašeho výběru. [WebJobs] [ WebJobs] funkci umožňuje snadno přidat úloha zpracování na pozadí do webové aplikace služby App Service.

Service Fabric je vhodný, pokud vytváříte novou aplikaci nebo přepisovat stávající aplikace pro použití architektury mikroslužby. Aplikace, které se spouštějí na sdílenému fondu počítačů, můžete začněte v malém rozsahu a dosáhnout masivním měřítku se stovkami nebo tisíci počítačů podle potřeby. Stavové služby usnadňují konzistentně a spolehlivě uložení stavu aplikace a Service Fabric automaticky spravuje služba rozdělení do oddílů, škálování a dostupnost za vás.  Service Fabric podporuje také WebAPI s Open Web Interface pro .NET (OWIN) a ASP.NET Core.  Ve srovnání s App Service, Service Fabric taky poskytuje další kontrolu nad nebo přímý přístup k podkladové infrastruktury. Můžete vzdálené do vašich serverů nebo můžete nakonfigurovat úlohy spuštění serveru. Cloudové služby je podobná Service Fabric v stupeň kontroly a snadné použití, ale nyní je starší verze služby a Service Fabric se doporučuje pro nový vývoj.

Pokud máte existující aplikace, které by vyžadovaly významné změny ke spuštění v App Service nebo Service Fabric, může zvolit virtuální počítače za účelem zjednodušení migrace do cloudu. Ale správně konfigurace, zabezpečení a údržba virtuální počítače vyžaduje mnohem víc času a IT odborných znalosti ve srovnání s Azure App Service a Service Fabric. Pokud uvažujete o virtuálních počítačích Azure, ujistěte se, že byste vzít v úvahu následné údržbě náročnost opravy, aktualizovat a spravovat prostředí virtuálních počítačů. Virtuální počítače Azure je infrastruktury jako služba (IaaS), zatímco služba App Service a Service Fabric jsou platformy jako služba (Paas). 

## <a name="features"></a>Porovnání funkcí
Následující tabulka porovnává funkce služby App Service, cloudové služby, virtuální počítače a Service Fabric a vám pomůže zajistit nejlepší volbou. Aktuální informace o smlouvě SLA pro jednotlivé možnosti najdete v tématu [smlouvy o úrovni služeb Azure](https://azure.microsoft.com/support/legal/sla/).

| Funkce | Služby App Service (webové aplikace) | Cloudové služby (webové role) | Virtuální počítače | Service Fabric | Poznámky |
| --- | --- | --- | --- | --- | --- |
| Téměř rychlých nasazení |X | | |X |Nasazení aplikace nebo aktualizace aplikace v cloudové službě nebo vytvoření virtuálního počítače, trvá několik minut minimálně; nasazení aplikace do webové aplikace trvá sekund. |
| Škálování na větší počítače bez znovu ho zaveďte |X | | |X | |
| Instancemi webového serveru sdílet obsah a konfiguraci, což znamená, že nemáte znovu nasaďte, nebo znovu nastavit při změně měřítka. |X | | |X | |
| Prostředí s více nasazení (provozní prostředí a Fázování) |X |X | |X |Service Fabric můžete mít více prostředí pro vaše aplikace a nasadit různé verze vaší aplikace-souběžného. |
| Automatická správa aktualizací operačního systému |X |X | | |Částečně prostřednictvím opravy Orchestration aplikace (druhů) a plně v budoucnu. |
| Přepínání bezproblémové platformy (snadný přesun mezi 32bitová a 64bitová verze) |X |X | | | |
| Nasaďte kód prostřednictvím GIT, FTP |X | |X | | |
| Nasazení kódu pomocí nástroje nasazení webu |X | |X | |Cloudové služby podporuje použití nasazení k nasazení aktualizací do instance jednotlivých rolí webu. Ale nelze ho použít pro počáteční nasazení role, a pokud použijete nasazení webu pro aktualizaci musíte nasadit samostatně pro každou instanci role. Více instancí jsou požadovány k platí Smlouva SLA cloudové služby pro provozní prostředí. |
| Podpora služby WebMatrix |X | |X | | |
| Přístup ke službám, jako je Service Bus, úložiště, databáze SQL |X |X |X |X | |
| Hostitele web nebo webovou vrstvu služby vícevrstvé architektury |X |X |X |X | |
| Střední vrstvy hostitele vícevrstvé architektury |X |X |X |X |Webové aplikace aplikační služby můžou snadno hostovat rozhraní REST API střední vrstvy a [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) funkce může hostovat úloh zpracování na pozadí. Webové úlohy můžete spustit na vyhrazené webové stránce k dosažení nezávislé škálovatelnost pro vrstvu. |
| Integrovaná podpora MySQL jako služby |X |X |X | |Cloudové služby můžete integrovat MySQL jako službu prostřednictvím nabídky na ClearDB, ale ne jako součást pracovního postupu portálu Azure. |
| Podpora pro technologii ASP.NET, classic ASP, Node.js, PHP, Python |X |X |X |X |Service Fabric podporuje vytvoření front-endu webové pomocí [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) nebo jakéhokoli typu aplikace (Node.js, Java atd.) můžete nasadit jako [spustitelný soubor hosta](../service-fabric/service-fabric-deploy-existing-app.md). |
| Škálování na více instancí bez znovu ho zaveďte |X |X |X |X |Virtuální počítače lze škálovat na více instancí, ale se služby spuštěné na nich musí být napsané pro zpracování této Škálováním na více systémů. Budete muset nakonfigurovat Vyrovnávání zatížení směrování žádostí mezi počítači a vytvořit skupinu vztahů, aby se zabránilo souběžných restartování všech instancí z důvodu údržby nebo k selhání hardwaru. |
| Podpora pro protokol SSL |X |X |X |X |Pro webové aplikace služby App Service SSL u vlastních názvů domén je podporována pouze pro režim Basic a Standard. Informace o použití protokolu SSL s webovými aplikacemi najdete v tématu [konfigurace certifikát protokolu SSL pro web Azure](app-service-web-tutorial-custom-ssl.md). |
| Integrace aplikace Visual Studio |X |X |X |X | |
| Vzdálené ladění |X |X |X | | |
| Nasazení kódu do sady TFS |X |X |X |X | |
| Izolace pomocí sítě [Azure Virtual Network](/azure/virtual-network/) |X |X |X |X |Viz také [weby Azure virtuální sítě integrace](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| Podpora pro [Azure Traffic Manager](/azure/traffic-manager/) |X |X |X |X | |
| Monitorování integrované koncového bodu |X |X |X | | |
| Přístup ke vzdálené ploše na servery | |X |X |X | |
| Nainstalujte všechny vlastní MSI | |X |X |X |Service Fabric umožňuje hostování žádné spustitelného souboru jako [spustitelný soubor hosta](../service-fabric/service-fabric-deploy-existing-app.md) nebo jakékoli aplikaci, můžete nainstalovat na virtuálních počítačích. |
| Umožňuje definovat, spouštění spuštění úlohy | |X |X |X | |
| Můžete poslouchat události trasování událostí pro Windows | |X |X |X | |

## <a name="scenarios"></a>Scénáře a doporučení
Tady jsou některé běžné scénáře s doporučení, které Azure webového hostingu možnost může být nejvhodnější pro jednotlivé aplikace.

* [Potřebuji front-end webové s pozadí zpracování a databáze back-end ke spouštění aplikací obchodní integraci s místní prostředky.](#onprem)
* [Potřebuji spolehlivý způsob, jak hostovat Moje podnikové weby, které dobře Škáluje a poskytuje globální přístup.](#corp)
* [Je nutné aplikaci služby IIS 6 v systému Windows Server 2003.](#iis6)
* [Jsem vlastníka malé firmy, je potřeba levný způsob, jak hostovat mé lokality, ale s budoucí růst v paměti.](#smallbusiness)
* [Jsem web nebo grafický Návrhář a chcete návrh a vytváření webů jménem zákazníka.](#designer)
* [Moje vícevrstvé aplikace s webového front-endu I mě migraci do cloudu.](#multitier)
* [Moje aplikace závisí na vysoce přizpůsobenou Windows nebo Linux prostředí a I chcete přesunout do cloudu.](#custom)
* [Mé lokality používá software s otevřeným zdrojem, a chcete ji hostovat v Azure.](#oss)
* [Je nutné-obchodní aplikace, která potřebuje připojit k podnikové síti.](#lob)
* [Chcete hostovat rozhraní API REST nebo webové služby pro mobilních klientů.](#mobile)

### <a id="onprem"></a>Potřebuji front-end webové s pozadí zpracování a databáze back-end ke spouštění aplikací obchodní integraci s místní prostředky.
Azure App Service je vynikající řešení pro komplexní podnikové aplikace. Umožňuje vyvíjet aplikace, které automaticky škálovat na platformě vyrovnáváním zatížení, jsou zabezpečené službou Active Directory a připojení k místním prostředkům. Usnadňuje správu tyto aplikace snadno prostřednictvím špičkových portál a rozhraní API a umožňuje vám získat přehled o tom, jak zákazníci používají je přehled nástroje pro aplikaci. [Webjobs] [ Webjobs] funkce umožňuje spouštět procesy na pozadí a úkoly v rámci vaší webové vrstvy, zatímco hybridní připojení a funkce virtuální sítě můžete snadno připojit zpět k místním prostředkům. Aplikační služba Azure poskytuje tři 9 SLA pro webové aplikace a umožňuje:

* Spolehlivě spuštění aplikací na cloudové platformy samoopravitelné, automatické opravy.
* Automaticky škálovat prostřednictvím globální sítě datových center.
* Zálohování a obnovení pro zotavení po havárii.
* Vyhovovat ISO, SOC2 a PCI.
* Integrace s Active Directory

### <a id="corp"></a>Potřebuji spolehlivý způsob, jak hostovat Moje podnikové weby, které dobře Škáluje a poskytuje globální přístup.
Azure App Service je vynikající řešení pro hostování podnikové weby. Umožňuje webové aplikace škálovat rychle a snadno potřeby prostřednictvím globální sítě datových center. Nabízí místní reach, odolnost proti chybám a inteligentní provoz správy. Všechny na platformu, která poskytuje nástroje pro správu špičkových což umožňuje získat přehled o stavu lokality a provoz lokality snadno a rychle. Aplikační služba Azure poskytuje tři 9 SLA pro webové aplikace a umožňuje:

* Spolehlivě spuštění své weby na cloudové platformy samoopravitelné, automatické opravy.
* Automaticky škálovat prostřednictvím globální sítě datových center.
* Zálohování a obnovení pro zotavení po havárii.
* Správa protokolů a provozu pomocí integrovaných nástrojů.
* Vyhovovat ISO, SOC2 a PCI.
* Integrace s Active Directory

### <a id="iis6"></a>Je nutné aplikaci služby IIS 6 v systému Windows Server 2003.
Aplikační služba Azure je snadné se vyhnout infrastruktury nákladů spojených se migrace starší aplikací služby IIS 6. Společnost Microsoft vytvořila [nástrojů pro migraci snadno použitelný a migrace podrobné pokyny](https://www.movemetowebsites.net/) které umožňují zkontrolovat kompatibilitu a identifikovat změny, které je potřeba provést. Integrace s Visual Studio, sady TFS a běžné nástroje pro systém CMS umožňuje snadno nasadit IIS6 aplikace přímo do cloudu. Po nasazení portálu Azure poskytuje robustní správu nástrojů, které vám umožní snižovat náklady na správu a až splňují potřebují podle potřeby. Nástroj pro migraci můžete:

* Snadno a rychle migrujte starší verze systému Windows Server 2003 webové aplikace do cloudu.
* OPT nechte připojené SQL databáze místní vytvořit hybridní aplikace.
* Automaticky přesunout databáze SQL společně s starší verzi aplikace.

### <a id="smallbusiness"></a>Jsem vlastníka malé firmy, je potřeba levný způsob, jak hostovat mé lokality, ale s budoucí růst v paměti.
Azure App Service je vynikající řešení pro tento scénář, protože můžete jej začít používat bezplatné a následně přidat další možnosti, když je potřebujete. Každý bezplatných webových aplikací se dodává s doménou poskytovaný platformou Azure (*your_company*. azurewebsites.net), a platforma obsahuje integrované nasazení a nástroje pro správu, jakož i galerii aplikací, který snadné začít pracovat. Existuje mnoho dalších služeb a škálování možnosti, které umožnily lokalitě momentální uživatele zvýšené poptávky. Azure App Service můžete:

* Začínat úroveň free a pak škálovat podle potřeby.
* Pomocí aplikace Galerie tak rychlé nastavení těchto oblíbených webových aplikací, jako je WordPress.
* Podle potřeby přidejte další služby Azure a funkcí do vaší aplikace.
* Zabezpečení webové aplikace s protokolem HTTPS.

### <a id="designer"></a>Jsem web nebo grafický Návrhář a chcete návrh a vytváření webů jménem zákazníka
Pro webových vývojářů a návrhářů snadno se integruje s celou řadu architektur a nástroje Azure App Service, zahrnuje podporu nasazení Git a FTP a nabízí úzkou integraci s nástroje a služby, jako je například Visual Studio a SQL Database. App Service umožňuje:

* Pomocí nástroje příkazového řádku pro [automatizovaných úloh,][scripting].
* Práce s oblíbených jazyků, jako [.Net][dotnet], [PHP][PHP], [Node.js] [ nodejs], a [Python][Python].
* Vyberte tři různé úrovně škálování pro škálování na velmi vysoké kapacity.
* Integrace s jinými službami Azure, jako například [SQL Database][sqldatabase], [Service Bus] [ servicebus] a [úložiště] [ Storage], nebo nabídky z partnerských [úložiště Azure][azurestore], jako jsou například MySQL a MongoDB.
* Integrate nástroje, jako je Visual Studio, Git, služba WebMatrix, Web Deploy, sady TFS a FTP.

### <a id="multitier"></a>Moje vícevrstvé aplikace s webového front-endu I mě migraci do cloudu
Pokud používáte vícevrstvé aplikace, například webový server, který se připojuje k databázi, Azure App Service je vhodný, která nabízí úzkou integraci s Azure SQL Database. A můžete použít funkci WebJobs pro spuštěné procesy back-end.

Zvolte Service Fabric pro jednu nebo více vaší vrstev, pokud potřebujete více řízení celého prostředí serveru, jako je například možnost vzdáleného do vašeho serveru nebo konfigurace serveru spuštění úloh.

Vyberte virtuální počítače pro jeden nebo více vaší vrstev, pokud chcete používat vlastní image počítače nebo spouštět serverového softwaru nebo služby, které nelze nakonfigurovat v Service Fabric.

### <a id="custom"></a>Moje aplikace závisí na vysoce přizpůsobenou Windows nebo Linux prostředí a I chcete přesunout do cloudu.
Pokud vaše aplikace vyžaduje komplexní instalace nebo konfigurace softwaru a operačního systému, je nejlepším řešením pravděpodobně virtuálních počítačů. Virtuální počítače můžete:

* Ke spuštění s operačním systémem, jako je například systému Windows nebo Linux, použijte Galerii virtuálních počítačů a upravit ho pro požadavky vaší aplikace.
* Vytvořit a nahrát vlastní image existujícího serveru na místní spuštění ve virtuálním počítači v Azure.

### <a id="oss"></a>Mé lokality používá software s otevřeným zdrojem, a chcete ji hostovat v Azure
Pokud v App Service se nepodporuje požadované rozhraní s otevřeným zdrojem, jazyků a rozhraní, které jsou vyžadované vaší aplikace jsou konfigurovány pro vás automaticky. Služby App Service umožňuje:

* Pomocí mnoha oblíbených open zdroj jazyků, jako například [.NET][dotnet], [PHP][PHP], [Node.js] [ nodejs], a [Python][Python].
* Nastavte WordPress, Drupal, Umbraco, DNN a mnoho dalších aplikací, webových třetích stran.
* Migrovat existující aplikaci nebo vytvořte novou z Galerie aplikace.

Pokud se v App Service nepodporuje požadované rozhraní s otevřeným zdrojem, ho můžete spustit na jednom z jiné službě Azure web hostování možnosti. S virtuálními počítači, instalaci a konfiguraci softwaru do bitové kopie počítače, které může být Windows nebo linuxu.

### <a id="lob"></a>Je nutné-obchodní aplikace, která potřebuje připojit k podnikové síti
Pokud chcete vytvořit-obchodní aplikace, může váš web vyžadovat přímý přístup na služby nebo data v podnikové síti. To je možné služby App Service, Service Fabric a virtuálních počítačů pomocí [služby Azure Virtual Network](/azure/virtual-network/). V App Service můžete použít [funkce integrace virtuální sítě](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), což umožňuje aplikací Azure spustit jako by byly ve vaší podnikové síti.

### <a id="mobile"></a>Chcete hostovat rozhraní API REST nebo webové služby pro mobilní klienty
Založené na protokolu HTTP webové služby umožňují podporu širokou škálu klientů, včetně mobilních klientů. Integrace architektury, jako je ASP.NET Web API pomocí sady Visual Studio, aby bylo snazší vytvářet a využívat služby REST.  Tyto služby se zveřejňují z koncový bod webové, takže je možné použít webhosting technika v Azure pro podporu tohoto scénáře. Služby App Service je však je služba skvělou volbou pro hostování rozhraní REST API. App Service umožňuje:

* Rychle vytvořit [mobilní aplikace](../app-service-mobile/app-service-mobile-value-prop.md) nebo aplikace API k hostování webové služby HTTP v jednom z Azure globálně distribuované datových centrech.
* Migrovat stávající služby nebo vytvořit nové.
* Dosáhnout SLA pro dostupnost pomocí jedné instance nebo škálovat na více vyhrazených počítače.
* Pomocí webu publikované lze zadat rozhraní REST API pro všechny klienty HTTP, včetně mobilních klientů.

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací k účtu, přejděte na <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci v Azure App Service zdarma. Nepožaduje se žádná platební karta a bez jakýchkoli závazků.
> 
> 

## <a id="nextsteps"></a>Další kroky
Další informace o jsou tři možnosti hostování webových najdete v tématu [představení Azure](../fundamentals-introduction-to-azure.md).

Chcete-li začít se vybrané možnosti pro vaši aplikaci, najdete v následujících materiálech:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Virtuální počítače Azure](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: app-service-web-tutorial-custom-ssl.md
[azurestore]: https://azuremarketplace.microsoft.com/en-us/marketplace/apps
[scripting]: https://azure.microsoft.com/documentation/scripts/?services=web-sites
[dotnet]: https://azure.microsoft.com/develop/net/
[nodejs]: https://azure.microsoft.com/develop/nodejs/
[PHP]: https://azure.microsoft.com/develop/php/
[Python]: https://azure.microsoft.com/develop/python/
[servicebus]: /azure/service-bus/
[sqldatabase]: /azure/sql-database/
[Storage]: /azure/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
