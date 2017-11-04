---
title: "Správa po migraci – Azure SQL Database | Microsoft Docs"
description: "Naučte se spravovat vaše databáze po dokončení migrace do Azure SQL Database."
services: sql-database
documentationcenter: 
author: joesackmsft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 10/14/2016
ms.author: Joe.Sack
ms.suite: SQL
ms.prod_service: sql-database
ms.component: management
ms.openlocfilehash: 663ab4aaf229f8a88d1116b34ccb74450aa66c9d
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="how-should-i-manage-my-azure-sql-database-after-migration"></a>Jak spravovat databáze Azure SQL po migraci?

*Nejčastější dotazy ohledně správy vaší investice do Azure SQL Database*

Takže jste nedávno přesunout databáze systému SQL Server do Azure SQL Database, nebo možná už plánujete na velmi brzy přesunutí. Jakmile jste jste přesunuli, co je další? Vzhledem k tomu, že je databáze SQL *platforma jako služba*, Microsoft zpracovává několika oblastech vaším jménem. Ale jak přesně to mění vaší společnosti postupů klíčové oblasti, například zabezpečení, kontinuity podnikových procesů, správy databáze, optimalizace výkonu, monitorování a další? 

Účelem tohoto článku je stručně uspořádání prostředků a pokyny, které budete potřebovat pro provedení k posunu ke správě vaší investice do databáze SQL. Hlavními oblastmi v tomto článku se věnují kontinuity podnikových procesů, zabezpečení, údržby databáze a monitorování, data výkonu a pohyb. Jsme zaměříme klíčové oblasti, které se liší systému SQL Server a SQL Database a vyvolávající aplikovatelné nejlepší postupy, které vám pomůžou maximalizovat výhody a minimalizovat riziko. 

## <a name="manage-business-continuity-after-migration"></a>Správa kontinuity podnikových procesů po migraci

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Jak vytvořit a spravovat zálohy pro službu SQL Database? 
SQL Database automaticky zálohuje databáze pro vás a také možnost obnovit do libovolného bodu v čase v dobu uchování. Doba uchování je 35 dní pro Standard a Premium databáze a 7 dní pro základní databáze. Funkce dlouhodobé uchovávání navíc umožňuje podržte záložní soubory po delší dobu (až 10 let) a obnovit ze zálohy v libovolném bodě. Kromě toho jsou zálohy databáze geograficky replikované zajistit možnost geografické obnovení v libovolné oblasti v případě havárie nebo regionální katastrofická. V tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-a-regional-catastrophe"></a>Jak zjistím kontinuity podnikových procesů v případě havárie úrovni datacenter nebo regionální katastrofická? 

Zálohování databáze jsou geograficky replikované zajistit možnost geografické obnovení v libovolné oblasti v případě havárie nebo regionální katastrofická. V tématu [obchodní kontinuity přehled](sql-database-business-continuity.md). SQL Database navíc poskytuje možnost Udržovat aktivně geograficky replikované sekundární databáze v jiné oblasti. Konfigurace je ve skupině automatické převzetí služeb při selhání zajistí, že databáze automaticky převzetí služeb při selhání na sekundární ve scénáři po havárii. Pokud není nakonfigurováno skupinu automatické převzetí služeb při selhání, aplikace musí aktivně sledování havárii a zahájit převzetí služeb při selhání na sekundární. 
### <a name="sql-server-provided-me-readable-secondary-replicas-can-i-access-the-secondaries-on-sql-database"></a>SQL Server zadat čitelných místních replikách, můžete přístup sekundární repliky v databázi SQL? 

Ano, funkci, aktivní geografickou replikaci, je použít k vytvoření čitelných místních replikách. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premise-to-sql-database"></a>Jak Moje plánu zotavení po havárii změní z místního k databázi SQL? 
Implementace systému SQL Server je nutné ke aktivně spravovat zálohy pomocí funkce, jako je Clustering převzetí služeb při selhání, zrcadlení databáze, replikace, přesouvání protokolu nebo jen prostý vanilla souboru BACPAC zálohy. V databázi SQL, ale zálohy jsou plně spravované microsoftem a pouze můžou mít plány zálohování a po havárii obnovení nakonfigurované a práce s několika kliknutí na portálu Azure (nebo několik příkazů v prostředí PowerShell). ‌
### <a name="in-the-event-of-disaster-how-do-i-recover-my-databases"></a>Jak se v případě havárie, obnovení databází.? 
SQL Database automaticky umožňuje obnovit své databáze do libovolného bodu v čase za posledních 35 dnů. Toto je možnost, při ztrátě dat nebo čelí havárii týkající se aplikace. 

V případě, že místní po havárii, čelí, pokud jsou nakonfigurované geograficky replikované sekundární databáze, můžete obnovit z vašich geo sekundárních databází v jiné oblasti. Pro přístup k aplikacím v reálném čase můžete selhání do geograficky sekundární v jiné oblasti ručně. Případně pokud máte nakonfigurované skupiny automatické převzetí služeb při selhání, toto převzetí služeb při selhání na sekundární geograficky dojde automaticky ve scénáři po havárii. Pokud nemáte nakonfigurován geograficky replikované sekundární databáze, stále můžete obnovit své databáze z automatické replikované záložní soubory (integrovanou funkci, není potřeba konfigurace), s relativně delší dobu obnovení (12 hodin RTO) a po jednom ztráta dat hodinu. 

### <a name="are-the-failovers-to-secondary-transparent-how-does-my-application-handle-database-failovers"></a>Jsou transparentní převzetí služeb při selhání pro sekundární? Jak Moje aplikace zpracovávat databáze převzetí služeb při selhání? 
Pokud máte nakonfigurované skupiny automatické převzetí služeb při selhání, převzetí služeb při selhání pro sekundární je transparentní. Ale pokud máte není, pak aplikace potřebuje začlenit logiku ke sledování dostupnosti primární a pak ruční převzetí služeb při selhání na sekundární. 
 
## <a name="manage-security-after-migration"></a>Správa zabezpečení po migraci

### <a name="how-can-i-restrict-access-to-my-sql-database"></a>Jak můžete omezit přístup k databázi SQL? 
 
Existuje několik způsobů, jak zablokovat přístup k připojení k databázím SQL. 
1. Limit přenos přes Internet Express trasy přiřadí vyhrazené fiber k síti Azure tak, aby vaše data není přenášet přes internet. Je také možné nakonfigurovat pro různé oblasti připojení pomocí Express route. Následující odkazy popisují Express Route podrobněji: 
 - [Úvod na Express Route](../expressroute/expressroute-introduction.md)
 - [Požadavky](../expressroute/expressroute-prerequisites.md) 
 - [Pracovní postupy](../expressroute/expressroute-workflows.md) 
 
2. Vyberte prostředky, ke kterým se připojit k databázi SQL: 

   Ve výchozím vaše databáze SQL je nakonfigurované tak, aby "Povolit všech služeb Azure" – což znamená, že všechny virtuální počítače v Azure mohou zkuste se připojit k databázi.  Všech přihlášení stále musí dojít k ověření. Pokud nechcete, databáze byla přístupná pro všechny IP adresy Azure, můžete zakázat "Povolit všech služeb Azure" a použijte [koncové body služby virtuální síť](sql-database-vnet-service-endpoint-rule-overview.md) omezení příchozí přístup k databázi ze pouze prostředky Azure, které jsou v rámci danou Podsíť virtuální sítě Azure. 

   ![Koncové body služby virtuální sítě](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

   Alternativní možností je zřídit [vyhrazené IP adresy](../virtual-network/virtual-networks-reserved-public-ip.md) pro virtuální počítače a seznam povolených adres ty konkrétní virtuálních počítačů pro IP adresy na serveru nastavení brány firewall. (Viz na snímku obrazovky níže jako příklad na portálu Azure.) Přiřazením vyhrazené IP adresy uložit potíže toho, že se aktualizovat pravidla brány firewall se při změně IP adres. 

3. Vyhněte se vystavení port 1433 mimo Azure

   Spustit v Azure pomocí aplikace SSMS [Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps). To není nutné otevřít odchozí připojení k portu 1433, tak databázi lze udělit pouze vzdálené aplikace RemoteApp je statická IP adresa, podporuje více Multi-Factor Authentication (MFA) a je více uživatelů. 

### <a name="what-authentication-methods-are-offered-in-sql-database"></a>Jaké metody ověřování jsou k dispozici v databázi SQL?

Metody ověřování hlavní nabízena v SQL Database a SQL Data Warehouse se ověřování Azure Active Directory a ověřování SQL. Azure Active Directory (AD) je centralizovaná služba správy identit a přístupu a SQL je jen jednou z mnoha služby Azure, které jsou integrované s Azure AD. Výhodou centralizované spravované služby je, že přihlašovacích údajů uživatele je sdílen na všech služeb Azure, který použijete pro jednodušší ověřování. To také umožňuje SQL Database a SQL Data Warehouse k nabízení služby Multi-Factor authentication a hosta uživatelské účty v doméně služby Azure AD. 

Pokud již máte Active Directory v místě, můžete vytvořit federaci adresář s Azure Active Directory pro rozšíření adresáře do Azure. 


|||
|---|---|
| Pokud...|Databáze Azure SQL / Azure SQL datového skladu|
| Raději použít Azure Active Directory (AD) ve službě Azure|Použití [ověřování SQL.](sql-database-security-overview.md)|
| Použít AD v systému SQL Server na místě|[Vytvořit federaci s Azure AD AD](../active-directory/connect/active-directory-aadconnect.md)a používání ověřování Azure AD. To můžete pomocí jednotného přihlašování.|
| Potřebujete vynutit vícefaktorové ověřování (MFA)|Vyžadovalo jako zásada prostřednictvím [podmíněného přístupu Microsoft](sql-database-conditional-access.md)a použijte [Azure AD Universal ověřování s podpora vícefaktorového ověřování](sql-database-ssms-mfa-authentication.md).|
| Účty hostů z účty Microsoft (live.com, outlook.com) nebo v jiných doménách (gmail.com)|Použití [Azure AD Universal ověřování](sql-database-ssms-mfa-authentication.md) v SQL databáze nebo datového skladu, která využívá [spolupráce Azure AD B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
| Jsou protokolovány systému Windows z federované domény pomocí přihlašovacích údajů Azure AD|Použití [integrované ověřování služby Azure AD](sql-database-aad-authentication-configure.md).|
| Jsou protokolovány systému Windows pomocí přihlašovacích údajů z domény není sdružených se službou Azure|Použití [ověřování hesla služby Azure AD](sql-database-aad-authentication-configure.md).|
| Máte služby střední vrstvy, které je třeba se připojit k Azure SQL Database nebo datového skladu|Použití [tokenu ověřování Azure AD](sql-database-aad-authentication-configure.md).
|||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-databases-from-the-application-side"></a>Způsob omezení přístupu k citlivým datům v mé databáze ze strany aplikace? 

Chcete-li zabránit neoprávněným uživatelům v schopnost zobrazit citlivá data, existuje několik možností dostupných v databázi SQL: 

- [Funkce Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) je forma šifrování na straně klienta, který šifruje citlivé sloupce v databázi (tak, aby byly v ciphertext neoprávněným uživatelům a správci databází). Klíč pro funkce Always Encrypted je uložen na straně klienta, takže pouze autorizovaným klientům můžete dešifrování citlivých sloupce. Šifrovaný vždy podporuje porovnání rovnosti dnes, takže DBAs můžete pokračovat v dotazování šifrované sloupce jako součást jejich příkazy SQL. Vždy šifrovaný lze použít s celou řadu možností úložiště klíčů, jako například [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md), úložiště certifikátů systému Windows a modulů místní hardwarového zabezpečení.
- [Dynamické maskování dat](sql-database-dynamic-data-masking-get-started.md) je funkce maskování dat, která omezuje zranitelnost citlivá data pomocí maskování uživatelům bez oprávnění na aplikační vrstvu. Můžete definovat pravidla maskování, které můžete vytvořit vzor maskování (například k zobrazení pouze poslední 4 číslice national ID a označit rest jako značkou) a určit, kteří uživatelé mohou být vyloučeny z pravidlo maskování.
- [Zabezpečení na úrovni řádků](/sql/relational-databases/security/row-level-security) umožňuje řídit přístup k řádky v tabulce databáze na základě uživatele provádění dotazu (skupiny členství nebo provádění kontextu). Omezení přístupu se provádí na úrovni databáze místo v aplikační vrstvě, zjednodušit logiky aplikace. 

### <a name="what-encryption-options-do-i-have-in-sql-database-and-what-actors-does-the-encryption-protect-from"></a>Jaké možnosti šifrování je nutné v databázi SQL a jaké aktéři šifrování chránit před?
Existují tři hlavní šifrovací technologie, které jsou k dispozici v databázi SQL: 
- [Funkce Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) (což je zmiňována ve výše uvedené otázku): šifruje citlivé sloupců v tabulce koncová, z neoprávněným klientům fyzického disku. Data správci serveru a nezobrazí citlivých dat, protože šifrovací klíče jsou uložené na straně klienta. 
- [Transparentní šifrování dat](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) (TDE): šifrování v klidovém stavu, který šifruje na úrovni databáze a chrání datové soubory, soubory protokolu a přidružených záloh před odcizením fyzická média. Ve výchozím nastavení pro všechny nově vytvořené databáze je povolené šifrování TDE.
 
  Následující diagram ukazuje přehled šifrování výběr technologie.

   ![Přehled šifrování](./media/sql-database-manage-after-migration/overview-encryption.png)

### <a name="how-should-i-manage-encryption-keys-in-the-cloud"></a>Jak by měla spravovat šifrovací klíče v cloudu? 
Existují možnosti správy klíčů pro vždy šifrovat (šifrování na straně klienta) a transparentní šifrování dat (šifrování v klidovém stavu). Je doporučeno pravidelně obměna šifrovacích klíčů a s frekvencí, zarovnaná s interní předpisy a dodržování předpisů požadavky.

- **Funkce Always Encrypted**: je [dva klíče hierarchie](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) v vždy šifrována - sloupec citlivých dat šifrování AES 256 sloupec šifrovací klíč (CEK), který je zase šifruje pomocí hlavního klíče sloupce (CMK). Klient ovladače dodané pro funkce Always Encrypted mít žádná omezení délky CMKs.

  Zašifrovanou hodnotu CEK je uložené v databázi, a CMK je uložené v důvěryhodné úložiště klíčů, jako je například úložiště certifikátů systému Windows, Azure Key Vault nebo modul hardwarového zabezpečení. 
  
  Obě [CEK a CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) lze otáčet. Otočení CEK může být časově náročný v závislosti na velikosti tabulky, který obsahuje šifrované sloupce. Proto CEK otočení velmi pečlivě naplánujte. Otočení CMK na druhé straně, nebudou v konfliktu s výkonem databáze a lze provést pomocí oddělených role.

  Následující diagram znázorňuje možnosti úložiště klíčů pro hlavního klíče sloupce v vždy šifrována. 

   ![Funkce Always encrypted CMK ukládání zprostředkovatelů](./media/sql-database-manage-after-migration/always-encrypted.png)

- **Transparentní šifrování šifrování dat (TDE)**: je hierarchie dva klíče v TDE – data v každé databázi uživatele jsou zašifrována pomocí symetrického AES 256 databáze jedinečný šifrovací klíč databáze (DEK), který je zase šifruje pomocí serveru jedinečný asymetrické šifrování RSA 2048 hlavní klíč. 

  Ve výchozím nastavení hlavní klíč pro transparentní šifrování dat spravuje služba SQL Database pro usnadnění práce. Pokud si organizace přeje kontrolu nad hlavního klíče, je možnost k použití [Azure Key Vault](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql) jako úložiště klíčů. 

  Pomocí Azure Key Vault, vaše organizace předpokládá kontrolu nad klíče ovládací prvky zřizování, otáčení a oprávnění. [Otočení nebo přepnutí typ šifrování TDE hlavní klíč](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) je rychlé, protože pouze znovu zašifruje klíč DEK. 

  Pro organizace s oddělení rolí mezi zabezpečení a správu dat může správce zabezpečení zřídit materiál klíče pro šifrování TDE hlavního klíče v Azure Key Vault a zajistit identifikátorem klíče Azure Key Vault pro správce databáze pro šifrování v klidovém stavu na serveru. 

## <a name="monitoring-and-compliance-after-migration"></a>Monitorování a dodržování předpisů po migraci

### <a name="how-do-i-monitor-database-activities-in-sql-database"></a>Jak se monitorování aktivity databáze v databázi SQL?
Existuje několik monitorování možnosti do databáze SQL, který sledování zabezpečení a další události v databázi:
- [Auditování SQL](sql-database-auditing.md) umožňuje shromažďovat protokoly auditování událostí databáze v účtu úložiště Azure.
- [Detekce hrozeb SQL](sql-database-threat-detection.md) umožňuje detekovat podezřelé aktivity, které indikují možné zlými úmysly přístup, porušení nebo využívat data v databázi. Detekce hrozeb databáze SQL se spustí více sad algoritmů, které je zjistit potenciální ohrožení zabezpečení a prostřednictvím injektáže SQL a také nezvyklé databázové přístupové vzorce (například přístup z neobvyklého umístění nebo neznámého objektu zabezpečení). Osoby zabezpečení nebo jiné určené správci zobrazit e-mailové oznámení, pokud je zjištěno ohrožení v databázi. Jednotlivá oznámení obsahuje podrobnosti o podezřelé aktivity a doporučení dále zkoumat a zmírnit riziko. 
- [Vyhodnocení ohrožení zabezpečení SQL](sql-vulnerability-assessment.md) je databáze vyhledávání a vytváření sestav služby, která umožňuje sledovat stav zabezpečení vašich databází ve velkém měřítku a zjistit rizika zabezpečení a soubor ze základního zabezpečení, které jste definovali. Po každé prohledávání jsou součástí přizpůsobeného seznamu řešitelné kroky a nápravy skripty a zprávu o hodnocení, které můžete použít pro pomoc ke splnění dodržování předpisů. 
- [Aplikaci synchronizace zabezpečení SQL OMS](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) využívá veřejná rozhraní API Operations Management Suite (OMS) k protokoly auditu SQL OMS pro analýzy protokolů a schopnost definovat vlastní detekce výstrahy, včetně: 
 - Auditování databáze SQL dlaždice & řídicí panel obsahují jasným a logickým sestavu databázové aktivity. 
 - SQL analýzy protokolů pro analýzu databázové aktivitě a prozkoumat nesrovnalostí a anomálií, které by mohly být známkou narušení možného zabezpečení.
 - Pokročilé specifická pravidla výstrahy na zjištěné události, které spouštějí e-mailu, Webhooku a Azure automation runbook výstrahy (tj. změny hesla, after-hours, konkrétní příkazy SQL).
- [Azure Security Center](../security-center/security-center-intro.md) nabízí možnost správy centralizované zabezpečení přes úlohy běžící v Azure, místně a v ostatních cloudů. Můžete zobrazit jestli základní ochranu databáze SQL, jako je například auditování a transparentní šifrování dat jsou nakonfigurované na všechny prostředky a vytvořit zásady v závislosti na své vlastní požadavky. 

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>Je kompatibilní s žádné ze zákonných požadavků v databázi SQL a jak který pomáhá s kontrolou vlastní organizaci? 
Databáze SQL Azure je kompatibilní s rozsahem regulačních compliances. Chcete-li zobrazit nejnovější sadu compliances, které byly splněny, navštivte [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings) a přechod na compliances, které jsou důležité pro vaši organizaci chcete zobrazit, pokud databáze SQL Azure je zahrnuta v části kompatibilní s Azure služby. Je důležité si uvědomit, že i když Azure SQL Database může musí být certifikovaná jako kompatibilní služby, pomáhá při dodržování předpisů služby vaší organizace ale nezaručí automaticky ji. 

## <a name="database-maintenance-and-monitoring-after-migration"></a>Údržby databáze a monitorování po migraci

### <a name="how-do-i-monitor-growth-in-data-size-and-resource-utilization"></a>Jak se monitorování růst v dat velikost a využití prostředků?

- Monitorování metriky lze zobrazit o vaší databáze velikost a využití prostředků v grafu, monitorování, na portálu Azure. 

  ![Graf sledování](./media/sql-database-manage-after-migration/monitoring-chart.png)

- Pokud chcete získat podrobnější přehled a podrobnostem podrobnosti o dotazy, můžete použít "Query Performance Insight, k dispozici na portálu Azure. To bude vyžadovat, že 'úložiště dotazů, je aktivní databáze.

  ![Query Performance Insight](./media/sql-database-manage-after-migration/query-performance-insight.png)

- Alternativně můžete zobrazit metriky pomocí zobrazení dynamické správy (zobrazení dynamické správy) příliš – pomocí [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) a [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database). 

### <a name="how-often-do-i-need-to-run-consistency-checks-like-dbcccheckdb"></a>Jak často je potřeba spusťte kontrolu konzistence jako DBCC_CHECKDB?
DBCC_CHECKDB ověří integritu logické a fyzické všechny objekty v databázi. Už musíte udělat tyto kontroly, protože to je spravováno společností Microsoft v Azure. Další informace najdete v tématu [Integrity dat v databázi SQL Azure](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="monitor-performance-and-resource-utilization-after-migration"></a>Sledování výkonu a využití prostředků po migraci

### <a name="how-do-i-monitor-performance-and-resource-utilization-in-azure-sql-database"></a>Jak monitorování výkonu a využití prostředků v Azure SQL Database?
Můžete sledovat výkon a využití prostředků v Azure SQL Database pomocí následujících metod:

- **Portál Azure**: portál Azure ukazuje využití izolované databáze výběrem databáze a kliknutím na grafu v podokně s přehledem. Můžete upravit v grafu zobrazí více metriky, včetně procento využití procesoru, procentuální DTU, procento vstupů/výstupů dat, procentuální relací a procentech velikosti databáze. 
  ![využití prostředků](./media/sql-database-manage-after-migration/resource-utilization.png)

  Z tohoto grafu můžete také nakonfigurovat výstrahy prostředkem. Tyto výstrahy umožní reagovat na prostředek podmínky s e-mailu, zapisovat do koncový bod HTTPS nebo HTTP nebo provedení akce. Najdete v článku [monitorování výkonu databáze ve službě Azure SQL Database](sql-database-single-database-monitor.md) podrobné pokyny.

- **Zobrazení**: můžete zadat dotaz [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) zobrazení dynamické správy vrácení historie statistiky spotřeby prostředků z za poslední hodinu a [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) zobrazení katalogu systému vrátit historie za posledních 14 dní. 

- **Dotaz na informace o výkonu**: [Query Performance Insight](sql-database-query-performance.md) vám umožní zobrazit historii nejčastějších dotazů využívání prostředků a dlouho běžící dotazy pro konkrétní databázi. Tato funkce vyžaduje [úložiště dotazů](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) být povoleno a aktivní databáze.

- **SQL analýzy Azure (Preview) na portálu analýzy protokolů**: [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md) umožňuje shromažďovat a vizualizovat klíčové metriky výkonu Azure SQL Azure, podporu až 150 000 databází SQL Azure a 5 000 elastické SQL Fondy na pracovní prostor. Můžete ho k monitorování a přijímat oznámení. Můžete sledovat Azure SQL Database a metriky elastického fondu v rámci více předplatných Azure a elastické fondy a slouží k identifikaci problémů v každé vrstvě balíčku aplikace. 

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>Jak zjistím, že je použito příslušnou službu úroveň a úroveň výkonu?
Monitorování [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) a [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) zobrazení dynamické správy k pochopení spotřeby procesoru, vstupně-výstupních operací a paměti. Můžete také použít SQL Database [Query Performance Insight](sql-database-query-performance.md) zobrazíte spotřeby prostředků. Pokud používáte konzistentně na vysoké procento dostupné prostředky, by měla zvažte přechod na vyšší úroveň výkonu v rámci existující vrstvy služeb nebo přesunout do vyšší úrovně služby. Naopak pokud používáte konzistentně nízkou procento dostupné prostředky, můžete zvažte přechod na nižší úroveň výkonu nebo úrovně služby.

### <a name="i-am-seeing-performance-issues-how-does-my-azure-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Problémy s výkonem se zobrazují. Jak se liší Moje metodika pro řešení problémů s Azure SQL Database a SQL Server?
Mnoho aspektů vaší metodologie řešení potíží výkonu zůstane stejný ve službě Azure SQL Database, ale bude určité rozdíly. Například pokud se zobrazí snížení celkového výkonu, sledovat [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) a [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) zobrazení dynamické správy pro pochopení využití procesoru, vstupně-výstupních operací a paměti Spotřeba. Budete muset změnit úroveň výkonu a služeb na základě požadavků zatížení vrstvy.
Komplexní sadu doporučení pro ladění problémů s výkonem, najdete v části [ladění výkonu v Azure SQL Database](sql-database-performance-guidance.md). 

### <a name="do-i-need-to-maintain-indexes-and-statistics"></a>Je potřeba udržovat indexy a statistiky?
Azure SQL Database neudržuje indexy a statistiky automaticky jako součást služby. Jste zodpovědní za plánování údržby indexy a statistiky. V následujícím článku metody automatizace Azure, podrobnosti několik možností plánování úloh údržby na databázi SQL Azure.

## <a name="data-movement-after-migration"></a>Přesun dat po migraci

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-azure-sql-database"></a>Jak exportovat a importovat data jako soubory souboru BACPAC z databáze Azure SQL Database? 

- **Export**: vaší databázi SQL Azure můžete exportovat jako soubor souboru BACPAC z portálu Azure.

  ![Exportovat jako souboru BACPAC](./media/sql-database-export/database-export.png)

- **Import**: můžete importovat jako soubor souboru BACPAC k databázi pomocí portálu Azure.

  ![Import souboru BACPAC](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-azure-sql-database-sql-server-2016--2012"></a>Jak synchronizovat data mezi Azure SQL databáze SQL serveru 2016 / 2012?
[Synchronizaci dat](sql-database-sync-data.md) funkce umožňuje synchronizaci dat obousměrně mezi více místní databáze systému SQL Server a databáze SQL Azure. Vzhledem k tomu, že je aktivační událost na základě, konzistence typu případné záruku, že (bez ztráty dat), ale není zaručena konzistence transakcí. 

## <a name="next-steps"></a>Další kroky
Další informace o [databáze Azure SQL](sql-database-technical-overview.md).
