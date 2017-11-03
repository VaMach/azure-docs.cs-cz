---
title: "Protokolu zabezpečení dat Analytics | Microsoft Docs"
description: "Další informace o tom, jak analýzy protokolů chrání vaše osobní údaje a zabezpečuje data."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: magoedte
ms.openlocfilehash: 91af774560860b35913e57b49fb7a1dd59f5640f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-data-security"></a>Protokolu zabezpečení analýzy dat
Společnost Microsoft se zavazuje chránit vaše osobní údaje a zabezpečení vašich dat, při dodávání softwaru a služeb, které vám pomohou při správě infrastruktury IT v organizaci. Uvědomujeme si, že pokud jste svěřit vaše data s jinými uživateli, tohoto vztahu důvěryhodnosti vyžaduje přísných zabezpečení. Společnost Microsoft dodržuje přísné pokyny pro dodržování předpisů a zabezpečení – od psaní kódu po provoz služeb.

Zabezpečení a ochrana dat je nejvyšší prioritou ve společnosti Microsoft. Kontaktujte nás s dotazy, návrhy nebo problémy k libovolnému následující informace, včetně naše zásady zabezpečení v [možnosti Azure podpory](http://azure.microsoft.com/support/options/).

Tento článek vysvětluje, jak data jsou shromažďována, zpracování a zabezpečené analýzy protokolů v Operations Management Suite (OMS). Agenti můžete použít pro připojení k webové službě, použít System Center Operations Manager ke shromažďování provozních dat nebo načtení dat z Azure diagnostics pro použití analýzy protokolů. Shromážděná data se odesílají přes Internet pomocí ověřování pomocí certifikátů a 3 protokolu SSL ve službě analýzy protokolů, který je hostován v Microsoft Azure. Data se komprimují agentem před odesláním.

Službu analýzy protokolů bezpečně spravuje vaše data založená na cloudu pomocí následujících metod:

* Oddělení dat
* uchovávání dat
* Fyzické zabezpečení
* Správa incidentů
* Dodržování předpisů
* certifikace standardy zabezpečení

## <a name="data-segregation"></a>Oddělení dat
Zákaznická data se ukládají na jednotlivé komponenty v celé službě OMS logicky samostatné. Všechna data jsou označená podle organizace. Toto značení přetrvává v průběhu celého životního cyklu dat a je vyžadováno na každé úrovni služby. Každý zákazník má vyhrazený Azure blob, kde dlouhodobá data

## <a name="data-retention"></a>Uchovávání dat
Indexovaná data vyhledávání protokolu je uložena a zachována podle cenový plán. Další informace najdete v tématu [Log Analytics ceny](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft odstraní data zákazníků 30 dní, po zavření pracovním prostorem OMS. Microsoft také odstraní účet úložiště Azure, kde se nachází data. Při odebrání data zákazníků žádné fyzické disky zničena.

Následující tabulka uvádí některé z dostupných řešení v OMS a příklady typů dat, které shromažďují.

| **Řešení** | **Datové typy** |
| --- | --- |
| Posouzení konfigurace |Konfigurační data, metadata a data o stavu |
| Plánování kapacit |Údaje o výkonu a metadata |
| Antimalware |Konfigurační data a metadata |
| Posouzení aktualizace systému |Stav metadata a data |
| Správa protokolů |Uživatelem definované protokoly událostí, protokoly událostí systému Windows nebo protokoly služby IIS |
| Sledování změn |Inventář softwaru a metadata služby systému Windows |
| SQL a hodnocení služby Active Directory |Data rozhraní WMI, registru dat, údaje o výkonu a dynamické správy SQL Server zobrazení výsledků |

V následující tabulce jsou uvedeny příklady typů dat:

| **Datový typ** | **Pole** |
| --- | --- |
| Výstrahy |Výstrahy název, popis výstrahy, BaseManagedEntityId, ID problému, IsMonitorAlert, RuleId, ResolutionState, Priority, závažnosti, kategorie, vlastníka, ResolvedBy, TimeRaised, TimeAdded, změněno, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved RepeatCount TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, |
| Konfigurace |CustomerID, ID agenta, EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Událost |ID události, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, číslo, kategorie, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Poznámka:** při zápisu událostí s vlastními poli v protokolu událostí systému Windows shromažďuje OMS je. |
| Metadata |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP adresa, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP Adresa, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Výkon |ObjectName, název_čítače, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Stav |StateChangeEventId, StateId, NewHealthState, OldHealthState, kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, identifikátor MonitorId, stav HealthState, změněno, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fyzické zabezpečení
Analýzy protokolů v OMS služby byla obsazena zaměstnanců společnosti Microsoft a všechny aktivity se protokolují a je možné je auditovat. Služba úplně spuštění v Azure a v souladu s Azure běžné engineering kritéria. Podrobnosti o fyzické zabezpečení prostředků Azure lze zobrazit na stránce 18 [Přehled zabezpečení Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Pro každý, kdo již nemá odpovědnost za OMS služby, včetně přenosu a ukončení jsou fyzické přístupová práva k zabezpečení oblasti změnit do jednoho pracovního dne. Další informace o globální fyzické infrastruktuře používáme v [Microsoft Datacenters](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Správa incidentů
OMS má proces správy incidentů, které splňovat všechny služby společnosti Microsoft. To Shrneme, jsme:

* Použít sdílené odpovědnost model, kde část zabezpečení odpovědnost patří společnosti Microsoft a část patří k zákazníkovi
* Spravovat incidenty zabezpečení Azure
  * Spustit šetření při zjištění incidentu
  * Hodnotit dopad a závažnost incident podle reakcí na incidenty na volání člen týmu. Na základě na důkaz, hodnocení může nebo nemusí mít za následek další eskalaci zabezpečení týmu odpovědi.
  * Diagnostika incident odborníky zabezpečení odpovědi provede technické nebo forenzního vyšetřování, identifikovat strategie omezení, omezení rizik a alternativní řešení. Pokud týmem zabezpečení dochází k závěru, že data zákazníků může stát vystavené až k jednotlivcům nezákonné nebo neoprávněným, paralelní provádění proces oznámení Incident zákazník zahájí paralelně.  
  * Stabilizovat a obnovit z incidentu. Reakce na incidenty týmu vytvoří plán obnovení pro zmírnění problém. Krizové členství ve skupině kroky jako je například umístění do karantény ovlivněné systémy může dojít, okamžitě a souběžně s diagnostiku. Způsoby zmírnění rizik delší období může plánované kterých se provádějí až po uplynutí okamžitou riziko.  
  * Uzavřít incident a provedení po porážce. Reakce na incidenty týmu vytvoří postmortální, který popisuje podrobnosti o incidentu, s úmyslem zkontrolovat, jestli zásady, postupy a procesy, aby se zabránilo opakování události.
* Upozorněte na bezpečnostní incidenty v oblasti
  * Určení oboru dopad odběratelů a pro poskytování kdokoliv, kdo je ovlivněno jako podrobné oznámení o nejdříve
  * Vytvoření oznámení zajistit, že zákazníkům podrobné dostatek informací, takže můžete provést šetření na jejich end a splňovat žádné závazky, která byla pro své koncové uživatele při zpozdit přílišný proces oznámení.
  * Potvrďte a deklarovat incidentu, podle potřeby.
  * Upozorněte zákazníkům incidentu oznámení bez prodlení nepřiměřený a v souladu s žádné právní či smluvními závazků. Oznámení bezpečnostních incidentů se doručují na jeden nebo více správců zákazníka a to jakýmkoli způsobem, který vybere Microsoft, včetně e-mailem.
* Chování team připravenosti a školení
  * Microsoft pracovníky jsou nutné k dokončení zabezpečení a školení sledování, která pomáhá identifikovat a hlášení problémů možného zabezpečení.  
  * Operátory práce ve službě Microsoft Azure mají povinnosti školení přidání kolem svůj přístup k citlivé systémy hostování zákaznická data.
  * Microsoft security odpovědi pracovníky přijímat specializované školení pro své role

Pokud dojde ke ztrátě všech datech zákazníků, upozorníme každého zákazníka a to během jednoho dne. Ale zákazníka došlo ke ztrátě dat. nikdy s OMS. Kromě toho jsme udržovat kopie dat, který byl vytvořen a je geograficky distribuovanou.

Další informace o tom, jak Microsoft odpoví na bezpečnostní incidenty v najdete v tématu [odpověď zabezpečení společnosti Microsoft Azure v cloudu](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Dodržování předpisů
OMS softwaru vývoj a služby týmu informace o zabezpečení a zásad správného řízení programu podporuje požadavky na jeho firmy a dodržuje zákonů a nařízení, jak je popsáno v [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) a [ Centrum zabezpečení Microsoft dodržování předpisů](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Jak vytváří požadavky na zabezpečení OMS, identifikuje ovládací prvky zabezpečení, spravovat a sledovat rizika jsou také popsány existuje. Ročně, jsme zkontrolujte zásady, standardy, postupy a pokyny.

Každý člen týmu vývoj OMS obdrží formální aplikace bezpečnostního školení. Interně používáme systém správy verzí pro vývoj softwaru. Každý projekt softwaru je chráněn systém správy verzí.

Společnost Microsoft nemá tým zabezpečení a dodržování předpisů, který dohlíží a vyhodnocuje všechny služby ve službě Microsoft. Informace o zabezpečení osob tvoří tým a nejsou přidruženy engineering oddělení, které vyvíjet OMS. Zabezpečení osoby mít vlastní řetězec správy a provedení nezávislé posuzování produktů a služeb k zajištění zabezpečení a dodržování předpisů.

Společnosti Microsoft správní rady je oznámení roční zprávu o všechny programy pro zabezpečení informací společnosti Microsoft.

Tým vývoj a služby softwaru OMS aktivně ve spolupráci s týmy Microsoft Legal a dodržování předpisů a dalšími partnery odvětví získat různé certifikáty.

## <a name="certifications-and-attestations"></a>Certifikace a atestace podle
Analýzy protokolů OMS splňuje následující požadavky:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/en-us/blog/iso22301/)
* [Platebních karet (PCI kompatibilní) Data zabezpečení Standardní (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) Radou standardy PCI zabezpečení.
* [Typ služby organizace ovládacích prvků (SOC) 1 1 a 1 typ SOC 2](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) kompatibilní
* [HIPAA a HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) pro společnosti, které mají HIPAA obchodní přidružit smlouvu
* Společná kritéria technikům Windows
* Microsoft Trustworthy Computing
* Jako služba Azure součásti, které používá OMS splňovat požadavky na Azure dodržování předpisů. Další informace v [dodržování předpisů Center důvěřovat Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> V některých osvědčení služby nebo atestace podle, analýzy protokolů je uveden v části jeho starší název *Statistika provozu*.
>
>


## <a name="cloud-computing-security-data-flow"></a>Cloud computing tok dat zabezpečení
Následující diagram znázorňuje Architektura zabezpečení cloud jako toku informací z vaší společnosti a jak jsou zabezpečená, jako je přesune do služby analýzy protokolů, nakonec kontaktu s vámi na portálu OMS. Další informace o jednotlivých kroků následuje diagramu.

![Obrázek OMS shromažďování dat a zabezpečení](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Zaregistrujte si analýzy protokolů a shromažďování dat
Pro vaši organizaci k odesílání dat k analýze protokolů nakonfigurujte agenty se systémem Windows, agentů spuštěných na virtuálních počítačích Azure nebo OMS agentů pro Linux. Pokud používáte agenty nástroje Operations Manager, pak použijete Průvodce konfigurací v konzoli Operations console jejich konfigurace. Uživatelé (které mohou být můžete, ostatní jednotlivé uživatele nebo skupinu uživatelů) vytvořte jeden nebo více účtů OMS (OMS pracovní prostory) a zaregistrujte agenty pomocí jedné z těchto účtů:

* [ID organizace](../active-directory/sign-up-organization.md)
* [Účet Microsoft - Outlook, Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Pracovní prostor služby OMS je, kde se data shromažďují, agregován, analyzovat a zobrazovat. Pracovní prostor slouží především jako prostředek k oddílu dat a každý pracovního prostoru není jedinečný. Například můžete chtít mít vaše produkční data spravovaná s jeden pracovní prostor OMS a testovací data spravovaná pomocí jiného pracovního prostoru. Pracovní prostory také pomoci správci řídit uživatel přístup k datům. Každém pracovním prostoru může mít víc uživatelských účtů s ním spojená, a všechny uživatelské účty, můžete přístup k více OMS pracovních prostorů. Můžete vytvořit na základě datacenter oblasti pracovních prostorů. Každý pracovní prostor se replikují do jiných datových centrech v oblasti, především pro dostupnost služeb OMS.

Pro nástroj Operations Manager po dokončení Průvodce konfigurací každou skupinu pro správu nástroje Operations Manager naváže připojení se službou analýzy protokolů. Pak můžete použít Průvodce přidáním počítače zvolit, které počítače ve skupině pro správu jsou povoleny pro odesílání dat do služby. Pro ostatní typy agenta každý připojuje bezpečně ke službě OMS.

Veškerá komunikace mezi systémy připojené a službu analýzy protokolů je zašifrovaná.  Protokol TLS (HTTPS) se používá pro šifrování.  V rámci procesu Microsoft SDL je následovaný zajistit, že je aktuální pomocí nejnovější pokroky v kryptografické protokoly analýzy protokolů.

Každý typ agent shromažďuje data pro analýzu protokolu. Typ dat, která se shromažďují je závisí na typech řešení použít. Zobrazí souhrn shromažďování dat v [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md). Kromě toho je k dispozici pro většinu řešení podrobnější informace o kolekci. Řešení je sady předdefinovaných zobrazení, protokolu vyhledávací dotazy, pravidla shromažďování dat a zpracování logiky. Analýzy protokolů pro import řešení mohou používat pouze správci. Po importu řešení, se přesune do serverů pro správu nástroje Operations Manager (Pokud se používá) a pro všechny agenty, které jste vybrali. Potom agenty shromažďovat data.

## <a name="2-send-data-from-agents"></a>2. Odesílání dat z agentů
Zaregistrujte všechny typy agenta s klíčem registrace a je navázat zabezpečené připojení mezi agentem a analýzy protokolů služby pomocí ověřování pomocí certifikátů a protokol SSL s portem 443. OMS používá tajný úložiště pro vygenerování a udržovat klíče. Privátní klíče otáčejí každých 90 dní a jsou uložené v Azure a spravuje činnosti Azure, kteří podle striktní regulačních a dodržování předpisů.

S nástrojem Operations Manager zaregistrujte pracovního prostoru se službou analýzy protokolů a navázání zabezpečeného připojení HTTPS mezi serverem pro správu nástroje Operations Manager.

U agentů Windows spuštěných na virtuálních počítačích Azure se použije klíč k úložišti jen pro čtení ke čtení diagnostických událostí do tabulek Azure.

Pokud není schopen komunikovat se službou service z jakéhokoli důvodu každého agenta, shromážděná data uložena do mezipaměti pro dočasné a server pro správu se pokusí znovu odeslat data každých 8 minut po dobu dvou hodin. Data uložená v mezipaměti agenta je chráněn úložiště přihlašovacích údajů operačního systému. Pokud služba nemůže zpracovat data po dvou hodin, budou data fronty agentů. Pokud zaplnění fronty, OMS spustí vyřazení datové typy, počínaje údaje o výkonu. Limit fronty agenta je klíč registru, můžete upravit, v případě potřeby. Shromážděná data komprimovaná a odešle do služby, bez místní databáze, tak nepřidá žádné zatížení na ně. Po odeslání shromážděná data, odebere se z mezipaměti.

Jak je popsáno výše, data z agenty se odešlou přes SSL datová centra služby Microsoft Azure. Volitelně můžete ExpressRoute pro další zabezpečení pro data. ExpressRoute je způsob, jak připojovat přímo k Azure z existující sítě WAN, například více protokol popisku přepínání sítě VPN (MPLS) poskytované poskytovatelem síťové služby. Další informace najdete v tématu [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Službu analýzy protokolů přijímá a zpracovává data
Službu analýzy protokolů zajišťuje, že příchozích dat z důvěryhodného zdroje pomocí ověřování certifikátů a integrity dat pomocí ověřování Azure. Nezpracované nezpracovaná data jsou pak uloženy jako objekt blob v [Microsoft Azure Storage](../storage/common/storage-introduction.md) a nejsou šifrována. Každý objekt blob úložiště Azure má však sadu jedinečnou sadu klíčů, které je přístupné pouze pro tohoto uživatele. Typ dat uložených závisí na typech řešení, které byly naimportovány a používá ke shromažďování dat. Potom služba analýzy protokolů zpracovává nezpracovaná data pro tento objekt blob úložiště Azure.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Pomocí analýzy protokolů pro přístup k datům
Přihlášením k analýze protokolů na portálu OMS pomocí účtu organizace nebo účtu Microsoft, kterou jste vytvořili dříve. Všechny přenosy mezi portálu OMS a analýzy protokolů v OMS se odesílají přes zabezpečený kanál protokolu HTTPS. Když používáte portál OMS, se vygeneruje ID relace na straně klienta uživatele (webový prohlížeč) a data jsou uložena v místní mezipaměti, dokud relace je ukončena. Pokud byla ukončena, mezipaměti je odstranit. Soubory cookie na straně klienta, které neobsahují identifikovatelné osobní údaje, se automaticky neodeberou. Soubory cookie relací jsou označeny HTTPOnly, která jsou zabezpečená. Po předem určené době nečinnosti je ukončen relace portálu OMS.

Pomocí portálu OMS, můžete exportovat data do souboru CSV a dostanete dat pomocí rozhraní API pro vyhledávání. Exportovat ve formátu CSV je omezený na 50 000 řádků na jeden exportu a rozhraní API dat je omezen na 5 000 řádků na jeden vyhledávání.

## <a name="next-steps"></a>Další kroky
* [Začínáme s analýzy protokolů](log-analytics-get-started.md) získáte další informace o analýzy protokolů a zprovoznění v minutách.
