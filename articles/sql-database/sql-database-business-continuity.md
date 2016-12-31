---
title: "Provozní kontinuita v cloudu – obnovení databází – SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak Azure SQL Database podporuje provozní kontinuitu v cloudu a obnovení databází a jak pomáhá udržovat klíčové cloudové aplikace v chodu."
keywords: "provozní kontinuita, provozní kontinuita v cloudu, zotavení databáze po havárii, obnovení databáze"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2016
ms.author: carlrab;sashan
translationtype: Human Translation
ms.sourcegitcommit: 747f6ca642a33c4ce9bcaacad4976e8eaed8fa44
ms.openlocfilehash: f642cfade2369f5c758ab45994c7cf3f37b6d4c5


---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Přehled provozní kontinuity se službou Azure SQL Database
Tento přehled popisuje možnosti, které služba Azure SQL Database nabízí pro zajištění provozní kontinuity a zotavení po havárii. Poskytuje možnosti, doporučení a kurzy pro zotavení z ničivých událostí, které mohou způsobit ztrátu dat nebo nedostupnost databáze a aplikace. Zabývá se i tím, co dělat v případě, kdy chyba uživatele nebo aplikace ovlivňuje integritu dat, když dojde k výpadku oblasti Azure nebo když aplikace vyžaduje údržbu. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Funkce služby SQL Database, pomocí kterých můžete zajistit provozní kontinuitu
SQL Database poskytuje řadu funkcí provozní kontinuity, včetně automatizovaných záloh a volitelné replikace databáze. Každá má jiné vlastnosti ohledně odhadovaného času obnovení (ERT) a potenciální ztráty dat posledních transakcí. Jakmile tyto možnosti pochopíte, můžete si mezi nimi vybírat a ve většině scénářů je spolu kombinovat a používat pro různé scénáře. Při vývoji plánu provozní kontinuity musíte pochopit maximální přijatelnou dobu úplného obnovení aplikace po ničivé události – to je vaše plánovaná doba obnovení (RTO). Musíte také porozumět maximálnímu množství posledních aktualizací dat (časový interval), jejichž ztrátu může aplikace tolerovat při obnovení po ničivé události – cíl bodu obnovení (RPO). 

Následující tabulka porovnává ERT a RPO pro tři nejběžnější scénáře.

| Schopnost | Úroveň Basic | Úroveň Standard | Úroveň Premium |
| --- | --- | --- | --- |
| Obnovení k určitému bodu v čase ze zálohy |Libovolný bod obnovení do 7 dní |Libovolný bod obnovení do 35 dní |Libovolný bod obnovení do 35 dní |
| Geografické obnovení z geograficky replikovaných záloh |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h |
| Obnovení z úložiště záloh Azure Backup Vault |ERT < 12 h, RPO < 1 týden |ERT < 12 h, RPO < 1 týden |ERT < 12 h, RPO < 1 týden |
| Aktivní geografická replikace |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |

### <a name="use-database-backups-to-recover-a-database"></a>Obnovení databáze pomocí záloh databáze
SQL Database automaticky provádí kombinaci zálohování úplné databáze každý týden, rozdílového zálohování databáze každou hodinu a zálohování protokolů transakcí každých pět minut, čímž pomáhá chránit vaši firmu před ztrátou dat. Tyto zálohy se uchovávají v místně redundantním úložišti po dobu 35 dní pro databáze v úrovních služby Standard a Premium, a po dobu 7 dní pro databáze v úrovni služby Basic – další podrobnosti o úrovních služeb najdete v článku o [úrovních služeb](sql-database-service-tiers.md). Pokud doba uchovávání vaší úrovně služby nevyhovuje požadavkům vaší organizace, můžete dobu uchovávání prodloužit [změnou úrovně služby](sql-database-scale-up.md). Pro zajištění ochrany před výpadkem datového centra se úplné a rozdílové zálohy databáze také replikují do [spárovaného datového centra](../best-practices-availability-paired-regions.md). Další podrobnosti najdete v článku o [automatickém zálohování databáze](sql-database-automated-backups.md).

Pokud předdefinovaná doba uchovávání není pro vaši aplikaci dostatečná, můžete ji prodloužit konfigurací zásad dlouhodobého uchovávání pro vaše databáze. Další informace najdete v tématu [Dlouhodobé uchovávání](sql-database-long-term-retention.md). 

Tyto automatické zálohy databáze můžete použít k obnovení databáze po různých ničivých událostech, a to jak v rámci vašeho datového centra, tak do jiného datového centra. Při použití automatických záloh databáze závisí odhadovaný čas obnovení na několika faktorech. Patří mezi ně celkový počet obnovovaných databází ve stejném regionu a ve stejnou dobu, velikost databáze, velikost protokolu transakcí a šířka pásma sítě. Ve většině případů čas obnovení nepřekročí 12 hodin. Při obnovování do jiné oblasti dat je potenciální ztráta dat omezena na 1 hodinu díky geograficky redundantnímu úložišti s rozdílovými zálohami prováděnými každou hodinu. 

> [!IMPORTANT]
> Pokud chcete provést obnovení pomocí automatizovaného zálohování, musíte být členem role Přispěvatel SQL Serveru nebo vlastník předplatného – viz [RBAC: Předdefinované role](../active-directory/role-based-access-built-in-roles.md). Obnovení můžete provést pomocí webu Azure Portal, prostředí PowerShell nebo rozhraní REST API. Nelze použít jazyk Transact-SQL.
> 
> 

Automatizované zálohování použijte pro zajištění provozní kontinuity a jako mechanismus obnovení, pokud vaše aplikace:

* Není považována za klíčovou.
* Nemá zavazující smlouvu SLA, a proto 24hodinový nebo delší výpadek nebude mít za následek finanční závazky.
* Pracuje s nízkou mírou změn dat (málo transakcí za hodinu) a ztráta změn provedených během až jedné hodiny je přijatelnou ztrátou dat. 
* Je citlivá na změny nákladů. 

Pokud potřebujete rychlejší obnovení, použijte [aktivní geografickou replikaci](sql-database-geo-replication-overview.md) (věnujeme se jí níže). Pokud potřebujete mít možnost obnovit data z období staršího než 35 dní, zvažte pravidelné zálohování databáze do souboru bacpac (komprimovaný soubor, který obsahuje schéma databáze a příslušná data) uloženého v Azure Blob Storage nebo v jiném umístění, které si zvolíte. Další informace o tom, jak vytvořit transakčně konzistentní archiv databáze, najdete v tématech [Vytvoření kopie databáze](sql-database-copy.md) a [export kopie databáze](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Zkrácení času obnovení a omezení ztráty dat spojené s obnovením pomocí aktivní geografické replikace
Kromě používání záloh databáze k obnovení databáze v případě narušení provozu můžete pomocí [aktivní geografické replikace](sql-database-geo-replication-overview.md) nakonfigurovat databázi, aby měla až čtyři sekundární databáze pro čtení v oblastech podle vašeho výběru. Tyto sekundární databáze jsou synchronizovány s primární databází pomocí mechanismu asynchronní replikace. Tato funkce slouží k ochraně před narušením provozu v případě výpadku datového centra nebo během upgradu aplikace. Aktivní geografickou replikaci můžete použít také k zajištění lepšího výkonu dotazů jen pro čtení pro uživatele rozptýlené v různých geografických umístěních.

Pokud primární databáze neočekávaně přejde do režimu offline nebo je třeba ji do režimu offline převést z důvodu údržby, můžete sekundární databázi rychle povýšit na primární databázi (tomu se také říká převzetí služeb při selhání) a nakonfigurovat aplikace, aby se připojovaly k nové primární databázi. U plánovaného převzetí služeb při selhání nedojde ke ztrátě dat. V případě neplánovaného převzetí služeb při selhání může kvůli povaze asynchronní replikace dojít ke ztrátě menšího množství dat u posledních transakcí. Po převzetí služeb při selhání můžete později provést navrácení služeb po obnovení – buď podle plánu, nebo když se datové centrum vrátí do režimu online. Ve všech případech se uživatelé setkají s krátkým výpadkem a budou se muset znovu připojit. 

> [!IMPORTANT]
> Pokud chcete použít aktivní geografickou replikaci, musíte být vlastníkem předplatného nebo musíte mít oprávnění správce na SQL Serveru. Konfigurovat a provádět převzetí služeb při selhání můžete pomocí webu Azure Portal, prostředí PowerShell nebo rozhraní REST API s použitím oprávnění k předplatnému nebo pomocí jazyka Transact-SQL s použitím oprávnění v rámci SQL Serveru.
> 
> 

Aktivní geografickou replikaci použijte v případě, že vaše aplikace splňuje některá z těchto kritérií:

* Je zvlášť důležitá.
* Má smlouvu SLA, která nepovoluje výpadek delší než 24 hodin.
* Výpadek bude mít za následek finanční závazky.
* Pracuje s vysokou mírou změn dat a ztráta dat za jednu hodinu je nepřijatelná.
* Další náklady na aktivní geografickou replikaci jsou nižší než potenciální finanční závazky a související ztráta podnikání.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Obnovení databáze po chybě uživatele nebo aplikace
* Nikdo není dokonalý! Uživatel může omylem odstranit některá data, nedopatřením smazat důležitou tabulku nebo dokonce smazat celou databázi. Nebo může aplikace kvůli vadě náhodou přepsat dobrá data chybnými daty. 

V takovém scénáři máte následující možnosti obnovení.

### <a name="perform-a-point-in-time-restore"></a>Obnovení k určitému bodu v čase
Pomocí automatizovaného zálohování můžete obnovit kopii databáze do známého bodu v čase, kdy byla v pořádku, za předpokladu, že čas spadá do doby uchovávání databáze. Po obnovení databáze můžete původní databázi nahradit obnovenou databází nebo zkopírovat potřebná data z obnovené databáze do původní databáze. Pokud databáze používá aktivní geografickou replikaci, doporučujeme zkopírovat požadovaná data z obnovené kopie do původní databáze. Pokud původní databázi nahradíte obnovenou databází, budete muset překonfigurovat a znovu synchronizovat aktivní geografickou replikaci (to může u velkých databází trvat poměrně dlouho). 

Další informace a podrobné pokyny k obnovení databáze do určitého bodu v čase pomocí webu Azure Portal nebo prostředí PowerShell najdete v části [Obnovení k určitému bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore). Obnovení nelze provést pomocí jazyka Transact-SQL.

### <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze
Pokud se odstraní databáze, ale neodstraní se logický server, můžete odstraněnou databázi obnovit do bodu, kdy byla odstraněna. Tak se záloha databáze obnoví do stejného logického serveru SQL, ze kterého byla odstraněna. Můžete ji obnovit s použitím původního názvu nebo pro obnovenou databázi zadat nový název.

Další informace a podrobné pokyny k obnovení odstraněné databáze pomocí webu Azure Portal nebo prostředí PowerShell najdete v části [Obnovení odstraněné databáze](sql-database-recovery-using-backups.md#deleted-database-restore). Obnovení nelze provést pomocí jazyka Transact-SQL.

> [!IMPORTANT]
> Pokud byl odstraněn logický server, není možné obnovit odstraněnou databázi. 
> 
> 

### <a name="restore-from-azure-backup-vault"></a>Obnovení z úložiště záloh Azure Backup Vault
Pokud došlo ke ztrátě dat mimo aktuální dobu uchovávání pro automatizované zálohování a databáze je nakonfigurovaná pro dlouhodobé uchovávání, můžete ji obnovit z týdenní zálohy v úložišti záloh Azure Backup Vault do nové databáze. V tuto chvíli můžete původní databázi nahradit obnovenou databází nebo zkopírovat potřebná data z obnovené databáze do původní databáze. Pokud potřebujete načíst starší verzi databáze před upgradem hlavní aplikace nebo kvůli vyhovění požadavku auditorů nebo soudnímu příkazu, můžete vytvořit novou databázi pomocí úplné zálohy uložené v úložišti záloh Azure Backup Vault.  Další informace najdete v tématu [Dlouhodobé uchovávání](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Obnovení databáze do jiné oblasti po výpadku regionálního datového centra Azure
<!-- Explain this scenario -->

Přestože je taková situace výjimečná, i u datového centra Azure může dojít k výpadku. Při výpadku dojde k narušení provozu, které může trvat jen několik minut nebo až několik hodin. 

* Jednou z možností je počkat, až výpadek skončí a databáze se vrátí do režimu online. Tento postup funguje pro aplikace, které si mohou dovolit mít databázi v režimu offline. Například vývojový projekt nebo bezplatná zkušební verze, na které nemusíte neustále pracovat. Když dojde k výpadku datového centra, nikdy nevíte, jak dlouho bude trvat, proto tato možnost funguje pouze pokud databázi nějakou dobu nepotřebujete.
* Další možností převzetí služeb při selhání do jiné oblasti dat, pokud používáte aktivní geografickou replikaci, nebo obnovení pomocí geograficky redundantních záloh databáze (geografické obnovení). Převzetí služeb při selhání trvá jenom pár sekund, zatímco obnovení ze záloh trvá hodiny.

Když přijmete opatření, bude doba obnovení a množství ztracených dat v případě výpadku datového centra záviset na tom, jak se rozhodnete využít ve své aplikaci funkce provozní kontinuity popsané výše. Samozřejmě se podle požadavků vaší aplikace můžete rozhodnout pro použití kombinace záloh databáze a aktivní geografické replikace. Diskuzi o aspektech návrhu aplikací pro samostatné databáze a pro elastické fondy pomocí těchto funkcí provozní kontinuity najdete v tématech [Návrh aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [Strategie zotavení po havárii elastických fondů](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Následující části poskytují přehled postupů k obnovení pomocí záloh databáze nebo aktivní geografické replikace. Podrobné pokyny, včetně požadavků na plánování, postupů po obnovení a informací o simulaci výpadku pro vyzkoušení postupu zotavení po havárii, najdete v tématu [Obnovení služby SQL Database po výpadku](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Příprava na výpadek
Bez ohledu na funkce provozní kontinuity, které používáte, musíte:

* Určit a připravit cílový server, včetně pravidel brány firewall na úrovni serveru, přihlášení a oprávnění na úrovni hlavní databáze
* Určit, jak přesměrovat klienty a klientské aplikace na nový server
* Zdokumentovat další závislosti, například nastavení auditování a výstrahy 

Pokud si vše pečlivě nenaplánujete a nepřipravíte, může převod aplikací do režimu online po obnovení nebo převzetí služeb při selhání trvat déle. Navíc pravděpodobně bude vyžadovat řešení potíží ve vypjaté situaci, a to není dobrá kombinace.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Převzetí služeb při selhání do geograficky replikované sekundární databáze
Pokud používáte jako mechanismus obnovení aktivní geografickou replikaci, [vynuťte převzetí služeb při selhání do geograficky replikované sekundární databáze](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). Během několika sekund se sekundární databáze povýší a stane se z ní nová primární databáze připravená zaznamenávat nové transakce a reagovat na všechny dotazy – pouze se ztrátou dat, která se nestihla replikovat v posledních sekundách. Informace o automatizaci procesu převzetí služeb při selhání najdete v tématu [Návrh aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Když se datové centrum vrátí do režimu online, můžete provést navrácení služeb po obnovení do původní databáze (nebo nemusíte).
> 
> 

### <a name="perform-a-geo-restore"></a>Provedení geografického obnovení
Pokud používáte jako mechanismus obnovení automatizované zálohování s replikací geograficky redundantního úložiště, [zahajte obnovení databáze pomocí geografického obnovení](sql-database-disaster-recovery.md#recover-using-geo-restore). K obnovení obvykle dojde během 12 hodin – se ztrátou dat až za jednu hodinu v závislosti na času pořízení a replikace poslední hodinové rozdílové zálohy. Dokud se obnovení nedokončí, databáze není schopná zaznamenávat žádné transakce ani reagovat na dotazy. 

> [!NOTE]
> Pokud datové centrum přejde do režimu online předtím, než svoji aplikaci přepnete na obnovenou databázi, můžete obnovení jednoduše zrušit.  
> 
> 

### <a name="perform-post-failover--recovery-tasks"></a>Provedení úloh po převzetí služeb při selhání nebo obnovení
Po obnovení s použitím libovolného mechanismu musíte provést následující dodatečné úlohy, abyste pro uživatele zprovoznili své aplikace:

* Přesměrujte klienty a klientské aplikace na nový server a obnovenou databázi.
* Ujistěte se, že platí odpovídající pravidla brány firewall na úrovni serveru, aby se uživatelé mohli připojit (nebo použijte [brány firewall na úrovni databáze](sql-database-firewall-configure.md#creating-database-level-firewall-rules)).
* Ujistěte se, že se používají odpovídající přihlášení a oprávnění na úrovni hlavní databáze (nebo použijte [obsažené uživatelé](https://msdn.microsoft.com/library/ff929188.aspx))
* Podle potřeby nakonfigurujte auditování.
* Podle potřeby nakonfigurujte výstrahy.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Upgrade aplikace s minimálními výpadky
Někdy je třeba aplikaci převést do režimu offline kvůli plánované údržbě, jako je upgrade aplikace. Téma [Správa upgradů aplikací](sql-database-manage-application-rolling-upgrade.md) popisuje, jak pomocí aktivní geografické replikace povolit postupné upgrady cloudových aplikací a snížit tak dobu výpadku během upgradů na minimum a zároveň zajistit cestu k obnovení v případě, že dojde k chybě. Tento článek se zabývá dvěma různými způsoby orchestrace procesu upgradu a popisuje výhody i nevýhody obou možností.

## <a name="next-steps"></a>Další kroky
Diskuzi o aspektech návrhu aplikací pro samostatné databáze a pro elastické fondy najdete v tématech [Návrh aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [Strategie zotavení po havárii elastických fondů](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).




<!--HONumber=Dec16_HO2-->


