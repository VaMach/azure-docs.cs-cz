---
title: "Řešení potíží s synchronizaci dat Azure SQL | Microsoft Docs"
description: "Další informace k řešení běžných problémů s synchronizaci dat SQL Azure"
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: bbfcac5a54f04f20dbdeeecef7c06b91128b8c6a
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/08/2017
---
# <a name="troubleshoot-issues-with-azure-sql-data-sync-preview"></a>Řešení problémů s synchronizaci dat SQL Azure (Preview)

Tento článek popisuje postup řešení potíží s aktuální problémy, které jsou známé týmem synchronizaci dat SQL (Preview). Pokud je alternativní řešení problému, je tady uvedené.

Přehled synchronizaci dat SQL najdete v tématu [synchronizaci dat mezi několika databází cloudu a místně s synchronizaci dat SQL Azure (Preview)](sql-database-sync-data.md).
                                                           
## <a name="my-client-agent-doesnt-work"></a>Moje Klientský agent nebude fungovat.

### <a name="description-and-symptoms"></a>Popis a symptomy

Při pokusu o použití agenta klienta získáte následující chybové zprávy.

"Synchronizace se nezdařila s výjimkou, ke které došlo k chybě při pokusu o deserializaci www.microsoft.com/.../05:GetBatchInfoResult parametr. Další podrobnosti najdete v InnerException.

"Zpráva o vnitřní výjimce: typ 'Microsoft.Synchronization.ChangeBatch' je neplatný typ kolekce, protože nemá výchozí konstruktor."


### <a name="cause"></a>Příčina

Tato chyba je problém se synchronizací dat SQL (Preview).

Nejpravděpodobnější příčinou tohoto problému je:

-   Se systémem Windows 8 Developer Preview, nebo

-   Máte nainstalované rozhraní .NET 4.5.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Ujistěte se, že instalujete agenta klienta na počítači se systémem Windows 8 Developer Preview a že není nainstalováno rozhraní .NET Framework 4.5.

## <a name="my-client-agent-doesnt-work-after-i-cancel-the-uninstall"></a>Moje Klientský agent nefunguje po zruším odinstalace

### <a name="description-and-symptoms"></a>Popis a symptomy

Klientský agent nefunguje, i když zrušeno jeho odinstalaci.

### <a name="cause"></a>Příčina

K tomuto problému dochází, protože klientský agent pro synchronizaci dat SQL (Preview) neumožňuje ukládání přihlašovacích údajů.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Existují dvě řešení můžete vyzkoušet:

-   Nejprve pomocí services.msc znovu zadat přihlašovací údaje pro agenta klienta.

-   Druhý odinstalujte tohoto agenta klienta a nainstalujte nový. Stáhněte a nainstalujte nejnovější verzi agenta klienta z [Download Center](http://go.microsoft.com/fwlink/?linkid=221479).

## <a name="my-database-isnt-listed-in-the-agent-dropdown"></a>Databáze není uveden v rozevírací nabídce agenta

### <a name="description-and-symptoms"></a>Popis a symptomy

Při pokusu o přidání existující databázi systému SQL Server do skupiny synchronizace databáze se neuvádějí v rozevírací nabídce.

### <a name="cause"></a>Příčina

Existuje několik možných příčin tohoto problému:

-   Klientský agent a synchronizace skupiny jsou v různých datových centrech.

-   Klientský agent seznam databází není aktuální.

### <a name="solution"></a>Řešení

Řešení závisí na příčinu.

#### <a name="the-client-agent-and-sync-group-are-in-different-data-centers"></a>Klientský agent a synchronizace skupiny jsou v různých datových centrech

Ve stejném datovém centru, musí mít agenta klienta a synchronizace skupiny. Tuto konfiguraci můžete nastavit pomocí jedné z následujících akcí:

-   Vytvořte nový agent ve stejném datovém centru jako synchronizace skupiny. Zaregistrujte si databáze tohoto agenta. V tématu [postupy: Instalace agenta klienta synchronizaci dat SQL (Preview)](#install-a-sql-data-sync-client-agent) Další informace.

-   Odstraňte aktuální synchronizace skupiny. Znovu ji vytvořte ve stejném datovém centru jako agenta.

#### <a name="the-client-agents-list-of-databases-is-not-current"></a>Klientský agent seznam databází není aktuální

Zastavte a potom restartujte službu agenta klienta.
Místní agent stáhne seznam přidružených databázích jenom na první odesílání klíč agenta, nikoli na klíče odesílání dalších agenta. Proto databází během přesunutí agenta zaregistrovány se nezobrazí na původní instanci agenta.

## <a name="client-agent-doesnt-start-error-1069"></a>Agent klienta nelze spustit (chyba. 1069)

### <a name="description-and-symptoms"></a>Popis a symptomy

Zjistíte, že agent není spuštěn v počítači, který je hostitelem SQL serveru. Při pokusu o spuštění agenta ručně, se zobrazí dialogové okno chyby s chybovou zprávou "Chyba. 1069: službu se nepodařilo spustit z důvodu selhání přihlášení."

![Dialogové okno chyby. 1069 synchronizace dat](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

### <a name="cause"></a>Příčina

Pravděpodobnou příčinou této chyby je, že vzhledem k tomu, že jste vytvořili agenta a zadali heslo pro přihlášení došlo ke změně hesla na místním serveru.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Aktualizace agenta hesla pro vaše aktuální heslo serveru.

1. Najděte agenta klienta Preview service synchronizaci dat SQL (Preview).

    a. Klikněte na tlačítko **spustit**.

    b. Do vyhledávacího pole zadejte "services.msc".

    c. Ve výsledcích hledání klikněte na tlačítko "Služby".

    d. V **služby** okno, přejděte k položce pro **synchronizaci dat SQL (Preview) agenta Preview**.

2. Pravým tlačítkem na položku a vyberte **Zastavit**.

3. Pravým tlačítkem na položku a pak klikněte na **vlastnosti**.

4. V **synchronizaci dat SQL (Preview) agenta Náhled Vlastnosti** okně klikněte na tlačítko **přihlásit** kartě.

5. Zadejte heslo do textového pole heslo.

6. Potvrďte heslo do textového pole Potvrdit heslo.

7. Klikněte na tlačítko **Použít** a potom **OK**.

8. V **služby** okna, klikněte pravým tlačítkem myši **Preview agenta synchronizaci dat SQL (Preview)** služby a pak klikněte na **spustit**.

9. Zavřít **služby** okno.

## <a name="i-get-a-disk-out-of-space-message"></a>Zobrazí se zpráva "nedostatek místa na disku"

### <a name="cause"></a>Příčina

Zpráva "nedostatek místa na disku" se může zobrazit, pokud soubory, které mají být odstraněny zůstanou za. Tento stav může nastat v důsledku antivirový software, nebo protože soubory jsou otevřete operace odstranění se pokus o.

### <a name="solution"></a>Řešení

Řešení je ručně odstranit soubory synchronizace v `%temp%` (`del \*sync\* /s`) a pak odeberte podadresáře také.

> [!IMPORTANT]
> Počkejte na dokončení synchronizace před odstraněním žádné soubory.

## <a name="i-cannot-delete-my-sync-group"></a>Nelze odstranit mé skupině synchronizace

### <a name="description-and-symptoms"></a>Popis a symptomy

Můžete služeb při selhání v pokusu o odstranění skupiny synchronizace.

### <a name="causes"></a>Způsobí, že

Některé z následujících akcí může způsobit selhání při odstranění skupiny synchronizace.

-   Agenta klienta je offline.

-   Agenta klienta je odinstalovaný nebo chybí. 

-   Databáze je offline. 

-   Skupiny synchronizace je zřizování nebo synchronizace. 

### <a name="solutions"></a>Řešení

Chcete-li vyřešit chyby, které chcete odstranit skupinu synchronizaci, zkontrolujte následující akce:

-   Ujistěte se, že agenta klienta je online a zkuste to znovu.

-   Pokud agenta klienta je odinstalovaný nebo jinak chybí:

    a. Pokud soubor existuje, odeberte soubor XML agenta z instalační složky synchronizaci dat SQL (Preview).

    b. Instalace agenta na stejný nebo jiný místní počítač, odešlete klíč agenta z portálu vygenerované agenta, který se zobrazuje v režimu offline.

-   **Služba synchronizaci dat SQL (Preview) je zastavena.**

    a. V **spustit** nabídky, vyhledávání služby.

    b. Ve výsledcích hledání klepněte na položku služby.

    c. Najít **synchronizaci dat SQL (Preview) Preview** služby.

    d. Pokud je stav služby **Zastaveno**, klikněte pravým tlačítkem na název služby a vyberte **spustit** z rozevírací nabídky.

-   Zkontrolujte vaše databáze SQL a SQL Server databáze a zajistit tak, že jsou všechny online.

-   Počkejte na dokončení procesu zřizování nebo synchronizace. Zkuste znovu odstraňování synchronizace skupiny.

## <a name="sync-fails-in-the-portal-ui-for-on-premises-databases-associated-with-the-client-agent"></a>Selhání synchronizace na portálu uživatelského rozhraní pro místní databáze přidružené agenta klienta

### <a name="description-and-symptoms"></a>Popis a symptomy

Selhání synchronizace na portálu synchronizaci dat SQL (Preview) uživatelského rozhraní pro místní databáze související s agentem. V místním počítači běží agent zobrazí System.IO.IOException chyby v protokolu událostí, s informacemi o tom, že na disku není dostatek místa.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Vytvořte další místo na disku, na kterém je umístěn adresáři % TEMP %.

## <a name="i-cant-unregister-an-on-premises-sql-server-database"></a>I nelze zrušit registraci místní databázi systému SQL Server

### <a name="cause"></a>Příčina

S největší pravděpodobností pokoušíte se zrušit registraci databázi, která již byla odstraněna.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Chcete-li zrušit registraci místní databázi systému SQL Server, vyberte databázi a klikněte na **Vynutit odstranění**.

Pokud se tato operace se nepodaří odebrat databázi ze skupiny pro synchronizaci, proveďte následující akce:

1. Zastavte a restartujte službu Hostitel agenta klienta.

    a. V nabídce Start.

    b. Zadejte *services.msc* do vyhledávacího pole.

    c. V aplikacích v podokně výsledků dvakrát klikněte na panel **služby**.

    d. Najít a klikněte pravým tlačítkem na službu **synchronizaci dat SQL (Preview)**.

    e. Pokud je služba spuštěná, zastavte ji.

    f. Klikněte pravým tlačítkem a vyberte **spustit**.

    g. Zkontrolujte, zda databáze již není zaregistrována. Pokud je již zaregistrován, jste hotovi. V opačném případě pokračujte dalším krokem.

2. Otevřete aplikaci agent klienta (SqlAzureDataSyncAgent).

3. Klikněte na tlačítko **upravit pověření** a zadat přihlašovací údaje pro databázi tak, aby byl dostupný.

4. Pokračujte zrušení registrace.

## <a name="i-cannot-submit-the-agent-key"></a>Mohu nelze odeslat klíč agenta

### <a name="description-and-symptoms"></a>Popis a symptomy

Po vytvoření nebo znovu vytvořte klíč pro agenta, pokusu o odeslání klíči prostřednictvím SqlAzureDataSyncAgent aplikace, ale odesílání nepodaří dokončit.

![Dialogové okno chyby synchronizace – klíč agenta nelze odeslat.](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

Než budete pokračovat, zkontrolujte, zda že selhání jedné z následujících podmínek není příčinou problém.

-   Je spuštěna služba systému Windows pro synchronizaci dat SQL (Preview).

-   Účet služby pro službu systému Windows verze Preview synchronizaci dat SQL (Preview) má přístup k síti.

-   Agenta klienta je schopen kontaktovat službu lokátoru. Zkontrolujte, že následující klíč registru má hodnotu "https://locator.sync.azure.com/LocatorServiceApi.svc"

    -   Na x86 počítače:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

    -   Na x64 počítače:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

### <a name="cause"></a>Příčina

Klíč agenta jednoznačně identifikuje každého agenta na místní. Klíč musí splňovat pro ho na spolupráci dvě podmínky:

-   Klíč agenta klienta na serveru pro synchronizaci dat SQL (Preview) a místní počítač musí být identický.

-   Klíč agenta klienta lze použít pouze jednou.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Pokud agenta není funkční, je to, protože jedna nebo obě tyto podmínky nejsou splněné. Pokud chcete získat agenta znovu fungovat:

1. Vygenerujte nový klíč.

2. Použít nový klíč k agentovi.

Chcete agenta použít nový klíč, proveďte následující akce:

1. Pomocí Průzkumníku souborů přejděte do adresáře instalace agenta. Výchozí instalační adresář je `c:\\program files (x86)\\microsoft sql data sync`.

2. Dvakrát klikněte `bin` podadresáři.

3. Spusťte `SqlAzureDataSyncAgent` aplikace.

4. Klikněte na tlačítko **odeslání klíč agenta**.

5. Vložte klíč ze schránky v poskytnutém prostoru.

6. Klikněte na **OK**.

7. Ukončete program.

## <a name="i-do-not-have-sufficient-privileges-to-start-system-services"></a>Nemám dostatečná oprávnění pro spouštění systémových služeb

### <a name="cause"></a>Příčina

K této chybě dojde ve dvou situacích:

-   Uživatelské jméno a heslo jsou nesprávné.

-   Zadaný uživatelský účet nemá dostatečná oprávnění přihlásit jako službu.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Udělit protokolu na jako-service pověření pro uživatelský účet.

1. Přejděte na **spustit | Ovládací panely | Nástroje pro správu |  Místní zásady zabezpečení | Místní zásady | Správa přístupových práv uživatele**.

2. Vyhledejte a vyberte **přihlásit jako službu** položku.

3. Přidání uživatelského účtu v **přihlásit jako službu – vlastnosti** dialogové okno.

4. Klikněte na tlačítko **použít** pak **OK**.

5. Okna zavřete.

## <a name="local-sync-agent-app-is-unable-to-connect-to-the-local-sync-service"></a>Místní Agent synchronizace aplikace se nemůže připojit ke službě místní synchronizace

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Vyzkoušejte následující kroky:

1. Ukončete aplikaci.

2. Otevření panelu součást služby.

    a. Do vyhledávacího pole na hlavním panelu zadejte "services.msc."

    b. Ve výsledcích hledání klikněte dvakrát na "Služby".

3. Zastavte a potom restartujte službu "Náhled SQL dat synchronizace (Preview)".

4. Restartujte aplikaci.

## <a name="install-uninstall-or-repair-fails"></a>Instalace, odinstalace nebo opravte selže

### <a name="cause"></a>Příčina

Existuje mnoho možných příčin selhání. Pokud chcete určit konkrétní příčinu této chyby, budete muset v protokolech.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Najít konkrétní příčinu selhání, který se vyskytl, musíte vygenerovat a prohlédněte si protokoly Instalační služby systému Windows. Můžete zapnout protokolování z příkazového řádku. Předpokládejme například, že stažený soubor AgentServiceSetup.msi je LocalAgentHost.msi. Generování a zkontrolujte soubory protokolů pomocí následujících příkazových řádků:

-   Pro instalaci:`msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`

-   Pro odinstaluje:`msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

Můžete také povolit protokolování pro všechny instalace provést instalační služba systému Windows. Článek znalostní báze Microsoft Knowledge Base [povolení protokolování Instalační služby systému Windows](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) poskytuje řešení jedním kliknutím zapnout protokolování pro Instalační služby systému Windows. Nabízí taky umístění těchto protokolů.

## <a name="a-database-has-an-out-of-date-status"></a>Databáze má "Zastaralé" stav

### <a name="cause"></a>Příčina

Synchronizaci dat SQL (Preview) odebere databází, které byly offline 45 dní nebo více (jak je počítáno od okamžiku databázi přešel do režimu offline) ze služby. Pokud databáze je v režimu offline pro 45 dnů nebo déle a pak přejde do režimu online, jeho stav nastaven na "Zastaralé".

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

"Zastaralé" stav se můžete vyhnout tím, že zajistí, že žádná z databází přejděte režimu offline. pro 45 dnů nebo déle.

Pokud je stav do databáze "Zastaralé", musíte udělat následující věci:

1. Odeberte databázi "Zastaralé" ze synchronizace skupiny.

2. Přidejte databázi zpět do skupiny synchronizace v.

> [!WARNING]
> Přijdete o všechny změny provedené tuto databázi v době, kdy byl v režimu offline.

## <a name="a-sync-group-has-an-out-of-date-status"></a>Synchronizace skupiny má "Zastaralé" stav

### <a name="cause"></a>Příčina

Pokud jeden nebo více změn se nepodaří použít pro dobu uchování celou 45 dní, se může stát zastaralé synchronizace skupiny.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Abyste se vyhnuli "Zastaralé" stavu, podívejte se na výsledky úlohy synchronizace v prohlížeči historie v pravidelných intervalech a prozkoumat a vyřešit všechny změny, které se nepodařilo použít.

Pokud je stav skupiny synchronizace "Zastaralé", budete muset odstranit skupinu sync a vytvořte je znovu.

## <a name="i-see-erroneous-data-in-my-tables"></a>V tématu chybná data ve svých tabulek

### <a name="description-and-symptoms"></a>Popis a symptomy

Pokud tabulky se stejným názvem, ale z různých schémata v databázi se podílejí synchronizované, zobrazí chybná data v těchto tabulkách po synchronizaci.

### <a name="cause-and-fix"></a>Příčinu a opravte

Zřizování synchronizaci dat SQL (Preview) používá stejné tabulky sledování pro tabulky se stejným názvem, ale v různých schématech. V důsledku toho z obou tabulek se projeví ve stejné tabulce sledování a toto chování způsobuje chybná data změny během synchronizace.

### <a name="resolution-or-workaround"></a>Řešení nebo alternativní řešení

Ujistěte se, že názvy tabulek zahrnutých v synchronizaci se liší, i v případě, že patří do různých schémata.

## <a name="i-see-inconsistent-primary-key-data-after-a-successful-synchronization"></a>V tématu nekonzistentní data primárního klíče po úspěšné synchronizace

### <a name="description-and-symptoms"></a>Popis a symptomy

Po synchronizaci, která je uvedená jako úspěšné, a v protokolu se zobrazuje žádné řádky se nezdařilo nebo bylo vynecháno, že zjistíte, že primární klíče dat není konzistentní mezi databází ve skupině synchronizace.

### <a name="cause"></a>Příčina

Toto chování je záměrné. Změny v jakékoli sloupec primárního klíče za následek nekonzistentní data v řádcích změnou primární klíč.

### <a name="resolution-or-workaround"></a>Řešení nebo alternativní řešení

Nechcete-li tento problém, ujistěte se, že žádná data v sloupec primárního klíče se změnila.

Po došlo k odstranění tohoto problému, musíte vyřadit z všechny koncové body ve skupině synchronizace příslušného řádku a potom znovu vložit řádek.

## <a name="i-see-a-significant-degradation-in-performance"></a>Najdete v části významnému zhoršení výkonu

### <a name="description-and-symptoms"></a>Popis a symptomy

Výkon výrazně zhorší, pravděpodobně do bodu, kde nelze i spustit uživatelské rozhraní synchronizace Data.

### <a name="cause"></a>Příčina

Nejpravděpodobnější příčinou je smyčku synchronizace. Smyčku synchronizace dojde při synchronizaci aktivačními skupiny A synchronizace a synchronizace skupinou synchronizace B, který zase aktivuje synchronizaci synchronizace skupiny a. Skutečné situace může být složitější, zahrnující více než dvě skupiny pro synchronizaci ve smyčce, ale je důležitým faktorem se cyklické spuštění synchronizace způsobené navzájem překrývající se skupiny pro synchronizaci.

### <a name="resolution-or-workaround"></a>Řešení nebo alternativní řešení

Nejlepší je prevence. Ujistěte se, nemají cyklické odkazy v synchronizaci skupin. Libovolný řádek, který je synchronizován jednu skupinu synchronizace nelze synchronizovat pomocí jiné synchronizace skupiny.

## <a name="client-agent-cannot-be-deleted-from-the-portal-if-its-associated-on-premises-database-is-unreachable"></a>Klientský agent nelze odstranit z portálu, pokud jeho přidružené místní databázi nedostupný

### <a name="description-and-symptoms"></a>Popis a symptomy

Pokud místní koncový bod (tj. databáze), která je zaregistrovaná u Klientský agent synchronizaci dat SQL (Preview) bude nedostupný, nelze odstranit agenta klienta.

### <a name="cause"></a>Příčina

Místní agent nelze odstranit, protože je nedostupný databáze pořád bude registrovaný s agentem. Při pokusu o odstranění agenta proces odstraňování se pokusí připojit databázi, která se nezdaří.

### <a name="resolution-or-workaround"></a>Řešení nebo alternativní řešení

Použití "Vynutit odstranění" odstranit databázi nedostupný.

> [!NOTE]
> Pokud po tabulky metadat synchronizace "odstranit force" zůstanou deprovisioningutil.exe použijte k jejich vyčištění.

## <a name="a-sync-group-cannot-be-deleted-within-three-minutes-of-uninstallingstopping-the-agent"></a>Synchronizace skupiny nelze odstranit v rámci tři minuty odinstalace nebo zastavení agenta

### <a name="description-and-symptoms"></a>Popis a symptomy

Nemůžete se nepodařilo odstranit skupinu synchronizace do tří minut po odinstalaci nebo zastavení přidružené Klientský agent pro synchronizaci dat SQL (Preview).

### <a name="resolution-or-workaround"></a>Řešení nebo alternativní řešení

1. Odebrání skupiny pro synchronizaci agenty přidružené synchronizace jsou online (doporučeno).

2. Pokud agenta je offline, ale nainstalovaná, převeďte ho do online režimu v místním počítači. Poté počkejte stavu agenta zobrazí jako online portálu synchronizaci dat SQL (Preview) a odebrání synchronizace skupiny.

3. Pokud je agent offline, protože byla odinstalována, proveďte následující akce. Pokuste se odstranit synchronizace skupiny.

    a.  Pokud soubor existuje, odstraňte soubor XML agenta z instalační složku synchronizaci dat SQL (Preview).

    b.  Instalace agenta na stejný nebo jiný místní počítač, odešlete klíč agenta z portálu vygenerované agenta, který se zobrazuje v režimu offline.

## <a name="my-sync-group-is-stuck-in-the-processing-state"></a>Moje skupina synchronizace je zablokované ve stavu zpracování

### <a name="description-and-symptoms"></a>Popis a symptomy

Synchronizace skupiny v synchronizaci dat SQL (Preview) byla ve stavu zpracování na dlouhou dobu nereaguje na příkaz k ukončení a protokoly zobrazit žádné nové položky.

### <a name="causes"></a>Způsobí, že

Některé z následujících podmínek může způsobit skupinu synchronizace se zablokuje ve stavu zpracování.

-   **Agenta klienta je offline.** Ujistěte se, že agenta klienta je online a zkuste to znovu.

-   **Agenta klienta je odinstalovaný nebo chybí.** Pokud agenta klienta je odinstalovaný nebo jinak chybí:

    1. Pokud soubor existuje, odeberte soubor XML agenta z instalační složky synchronizaci dat SQL (Preview).

    2. Nainstalujte agenta na stejný nebo jiný místní počítač. Pak odešlete klíč agenta z portálu vygenerované agenta, který se zobrazuje v režimu offline.

-   **Služba synchronizaci dat SQL (Preview) je zastavena.**

    1. V **spustit** nabídky, vyhledávání služby.

    2. Ve výsledcích hledání klepněte na položku služby.

    3. Najít **synchronizaci dat SQL (Preview)** služby.

    4. Pokud je stav služby **Zastaveno**, klikněte pravým tlačítkem na název služby a vyberte **spustit** z rozevírací nabídky.

### <a name="solution-or-workaround"></a>Řešení nebo alternativní řešení

Pokud se nemůžete vyřešit problém, stav skupiny pro synchronizaci můžete obnovit pomocí podporu společnosti Microsoft. Aby bylo možné používat váš stav resetovat, vytvořit příspěvek ve fóru na [fórum Azure SQL Database](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)a zahrnovat svoje ID předplatného a ID skupiny synchronizace pro skupinu, které je třeba obnovit. Pracovníka podpory společnosti Microsoft bude reagovat na vaši post a umožňují vědět, kdy byl obnoven stav.

## <a name="next-steps"></a>Další kroky
Další informace o synchronizaci dat SQL najdete v tématu:

-   [Synchronizaci dat mezi několika databází cloudu a místně s synchronizaci dat SQL Azure](sql-database-sync-data.md)
-   [Začínáme s Azure SQL synchronizací dat](sql-database-get-started-sql-data-sync.md)
-   [Osvědčené postupy pro synchronizaci dat SQL Azure](sql-database-best-practices-data-sync.md)

-   Dokončete příklady prostředí PowerShell, které ukazují, jak nakonfigurovat synchronizaci dat SQL:
    -   [Pomocí prostředí PowerShell k synchronizaci mezi více databází Azure SQL](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Synchronizace mezi databáze SQL Azure a místní databáze SQL serveru pomocí prostředí PowerShell](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Stáhněte si dokumentaci rozhraní API REST synchronizaci dat SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Další informace o databázi SQL najdete v tématu:

-   [Databáze SQL – přehled](sql-database-technical-overview.md)
-   [Správa životního cyklu databáze](https://msdn.microsoft.com/library/jj907294.aspx)
