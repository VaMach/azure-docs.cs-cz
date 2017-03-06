---
title: "Aktualizace řešení pro správu v OMS | Dokumentace Microsoftu"
description: "Tento článek vám objasní, jak toto řešení používat ke správě aktualizací pro počítače s Windows a Linuxem."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: fa9b427afff2c12babde30aa354e59d31c8f5b2c
ms.openlocfilehash: 219fe64481df2c5c5cbfe622afdab11dcc1b7100
ms.lasthandoff: 03/01/2017


---
# <a name="update-management-solution-in-oms"></a>Aktualizace řešení pro správu v OMS
Řešení pro správu aktualizací v OMS umožňuje spravovat aktualizace pro počítače s Windows a Linuxem.  Stav dostupných aktualizací na všech počítačích agenta můžete rychle vyhodnotit a zahájit proces instalace požadovaných aktualizací pro servery. 

## <a name="prerequisites"></a>Požadavky
* Řešení podporuje pouze provádění vyhodnocení aktualizací pro systém Windows Server 2008 nebo novější a nasazení aktualizací pro systém Windows Server 2012 nebo novější.  Možnosti instalace Server Core a Nano Server nejsou podporované.
* Klientské operační systémy Windows nejsou podporované.  
* Agenti Windows musí být buď nakonfigurovaní na komunikaci se službou Windows Server Update Services (WSUS), nebo musí mít přístup ke službě Microsoft Update.  
  
  > [!NOTE]
  > Agenta Windows není možné spravovat současně s nástrojem System Center Configuration Manager.  
  > 
  > 
* Agenty Linux musí mít přístup k úložišti aktualizací.  Agenta OMS pro Linux je možné stáhnout z webu [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Konfigurace
Pomocí následujících kroků přidejte řešení pro správu aktualizací do pracovního prostoru OMS a přidejte agenty Linux. Agenti Windows se přidají automaticky bez dodatečné konfigurace.

> [!NOTE]
> Pokud povolíte toto řešení, každý počítač s Windows připojený k pracovnímu prostoru OMS se automaticky nakonfiguruje jako Hybrid Runbook Worker, aby podporoval runbooky, které jsou zahrnuté v tomto řešení.  Není ale zaregistrovaný v žádné skupině Hybrid Worker, které už jste možná pro svůj účet Automation vytvořili.  Můžete ho přidat do skupiny Hybrid Runbook Worker ve vašem účtu Automation pro podporu runbooků Automation, pokud používáte stejný účet pro toto řešení a i pro členství ve skupině Hybrid Runbook Worker.  Tuto funkci jsme do funkce Hybrid Runbook Worker přidali ve verzi 7.2.12024.0.   

1. Postupem popsaným v části [Přidání řešení OMS](../log-analytics/log-analytics-add-solutions.md) přidejte řešení pro správu aktualizací z galerie řešení do pracovního prostoru.  
2. Na portálu OMS vyberte **Nastavení** a potom **Připojené zdroje**.  Poznamenejte si **ID pracovního prostoru** a **primární klíč** nebo **sekundární klíč**.
3. Pro každý počítač s Linuxem proveďte následující postup.
   
   a.    Nainstalujte nejnovější verzi agenta OMS pro Linux spuštěním následujících příkazů.  Hodnotu <Workspace ID> nahraďte ID pracovního prostoru a hodnotu <Key> primárním nebo sekundárním klíčem.
   
        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh  
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

   b. Pokud chcete agenta odebrat, spusťte následující příkaz.
   
        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Sady Management Pack
Pokud je vaše skupina pro správu Center Operations Manager připojená k pracovnímu prostoru OMS, do Operations Manageru se po přidání tohoto řešení nainstalují následující sady Management Pack. Není potřeba žádná konfigurace ani údržba těchto sad Management Pack. 

* Aktualizace Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Aktualizace sady pro správu nasazení

Další informace o způsobu, jakým se aktualizují sady pro správu řešení, najdete v tématu [Připojení Operations Manageru ke službě Log Analytics](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Shromažďování dat
### <a name="supported-agents"></a>Podporovaní agenti
Následující tabulka popisuje připojené zdroje, které toto řešení podporuje.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| Agenti systému Windows |Ano |Řešení shromažďuje informace o aktualizacích systému pro agenty Windows a inicializuje instalaci požadovaných aktualizací. |
| Agenti systému Linux |Ano |Řešení shromažďuje informace o aktualizacích systému z agentů Linuxu. |
| Skupina pro správu Operations Manageru |Ano |Řešení shromažďuje informace o aktualizacích systému z agentů v připojené skupině pro správu.<br>Přímé připojení z agenta Operations Manageru ke službě Log Analytics není potřeba. Data se přesměrovávají ze skupiny pro správu do úložiště OMS. |
| Účet služby Azure Storage |Ne |Úložiště Azure neobsahuje informace o aktualizacích systému. |

### <a name="collection-frequency"></a>Četnost shromažďování dat
Pro každý spravovaný počítač s Windows se kontrola provádí dvakrát denně.  Při instalaci aktualizace se informace aktualizují do 15 minut.  

Pro každý spravovaný počítač s Linuxem se kontrola provádí každé tři hodiny.  

## <a name="using-the-solution"></a>Použití řešení
Když přidáte řešení pro správu aktualizací do pracovního prostoru OMS, na řídicí panel OMS se přidá dlaždice **Správa aktualizací**. Na této dlaždici se zobrazuje počet a grafické vyjádření počtu počítačů ve vašem prostředí, které v současné době vyžadují aktualizace systému.<br><br>
![Dlaždice Souhrn Správy aktualizací](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Zobrazení posouzení aktualizací
Klikněte na dlaždici **Správa aktualizací**. Otevře se řídicí panel **Správa aktualizací**. Řídicí panel obsahuje sloupce v následující tabulce. Každý sloupec uvádí až deset položek odpovídajících kritériím tohoto sloupce pro zadaný obor a časový rozsah. Kliknutím na **Zobrazit vše** v dolní části sloupce nebo na záhlaví sloupce můžete spustit hledání v protokolu, které vrátí všechny záznamy.

| Sloupec | Popis |
| --- | --- |
| **Počítače s chybějícími aktualizacemi** | |
| Důležité aktualizace nebo aktualizace zabezpečení |Obsahuje seznam prvních deset počítačů, ve kterých chybí aktualizace, seřazený podle počtu aktualizací, které v nich chybí. Kliknutím na název počítače spustíte hledání v protokolu, které vrátí všechny záznamy o aktualizaci pro tento počítač. |
| Důležité aktualizace nebo aktualizace zabezpečení starší než 30 dní |Určuje počet počítačů, ve kterých chybí důležité aktualizace nebo aktualizace zabezpečení seskupené podle doby, která uplynula od publikování aktualizace. Klikněte na jednotlivé položky ke spuštění hledání v protokolu se všemi chybějícími a důležitými aktualizacemi. |
| **Požadované chybějící aktualizace** | |
| Důležité aktualizace nebo aktualizace zabezpečení |Uvádí seznam klasifikací aktualizací, které v počítači chybí, seřazený podle počtu počítačů postrádajících aktualizace v dané kategorii. Kliknutím na klasifikaci spustíte hledání v protokolu, které vrátí všechny záznamy o aktualizacích pro danou klasifikaci. |
| **Nasazení aktualizace** | |
| Nasazení aktualizace |Počet aktuálně plánovaných nasazení aktualizace a doba do dalšího plánovaného spuštění.  Kliknutím na dlaždici zobrazíte aktuálně spuštěné plány a dokončené aktualizace nebo naplánujete nové nasazení. |

<br>  
![Řídicí panel Souhrn Správy aktualizací](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Zobrazení počítače s řídicím panelem Správa aktualizací](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Zobrazení balíčku s řídicím panelem Správa aktualizací](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Instalace aktualizací
Po posouzení aktualizací pro všechny počítače s Windows ve vašem prostředí můžete nechat nainstalovat požadované aktualizace vytvořením *nasazení aktualizace*.  Nasazení aktualizace je plánovaná instalace požadovaných aktualizací pro jeden nebo více počítačů s Windows.  Kromě počítače nebo skupiny počítačů, které mají být součástí nasazení, určíte datum a čas nasazení.  

Aktualizace se instalují podle runbooků ve službě Azure Automation.  Tyto runbooky není možné zobrazit a nevyžadují žádnou konfiguraci.  Při vytvoření nasazení aktualizace se vytvoří plán, ve kterém se spouští hlavní runbook aktualizace v zadanou dobu pro zahrnuté počítače.  Tento hlavní runbook spouští podřízený runbook na každém agentovi Windows, který instaluje požadované aktualizace.  

Pro virtuální počítače vytvořené z imagí Red Hat Enterprise Linux (RHEL) na vyžádání dostupných v Azure Marketplace jsou registrované pro přístup k infrastruktuře [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) nasazené v Azure.  Všechny ostatní distribuce Azure musí být aktualizované z online úložiště souborů distribuce podle podporované metody.  

### <a name="viewing-update-deployments"></a>Zobrazení nasazení aktualizace
Klikněte na dlaždici **Aktualizovat nasazení** zobrazíte seznam existujících nasazení aktualizace.  Jsou seskupené podle stavu – **Naplánované**, **Spuštěné** a **Dokončeno**.<br><br> ![Stránka Plán nasazení aktualizace](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Následující tabulka popisuje vlastnosti zobrazené u každého nasazení aktualizace.

| Vlastnost | Popis |
| --- | --- |
| Název |Název nasazení aktualizace |
| Plán |Typ plánu  Momentálně je jediná možná hodnota *Jednorázově*. |
| Čas spuštění |Datum a čas, na kdy je naplánované spuštění nasazení aktualizace. |
| Doba trvání |Počet minut, po které je povoleno spuštění nasazení aktualizace.  Pokud se všechny aktualizace nenainstalují v rámci této doby trvání, musí zbývající aktualizace počkat až do dalšího nasazení aktualizace. |
| Servery |Počet počítačů ovlivněných nasazením aktualizace |
| Status |Aktuální stav nasazení aktualizace<br><br>Možné hodnoty:<br>-    Není spuštěné<br>- Spuštěné<br>- Dokončeno |

Klikněte na nasazení aktualizace a podívejte se na jeho podrobnou obrazovku, která zahrnuje sloupce v následující tabulce.  Tyto sloupce nebudou naplněné, pokud nasazení aktualizace ještě nezačalo.<br>

| Sloupec | Popis |
| --- | --- |
| **Výsledky v počítači** | |
| Úspěšně dokončeno |Uvádí počet počítačů v nasazení aktualizací podle stavu.  Klikněte na stav, aby se spustilo prohledávání protokolu, které vrátí všechny záznamy aktualizace s tímto stavem pro nasazení aktualizace. |
| Stav instalace počítače |Zobrazí seznam počítačů zahrnutých v nasazení aktualizace a procento aktualizací, které se úspěšně nainstalovaly. Klikněte na jednotlivé položky ke spuštění hledání v protokolu se všemi chybějícími a důležitými aktualizacemi. |
| **Aktualizace výsledků instance** | |
| Stav instalace instance |Uvádí seznam klasifikací aktualizací, které v počítači chybí, seřazený podle počtu počítačů postrádajících aktualizace v dané kategorii. Kliknutím na počítač spustíte hledání v protokolu, které vrátí všechny záznamy o aktualizaci pro tento počítač. |

<br><br> ![Přehled výsledků nasazení aktualizace](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Vytvoření nasazení aktualizace
Vytvořte nové nasazení aktualizace kliknutím na tlačítko **Přidat** v horní části obrazovky. Otevře se stránka **Nasazení nové aktualizace**.  Je nutné zadat hodnoty pro vlastnosti v následující tabulce.

| Vlastnost | Popis |
| --- | --- |
| Name (Název) |Jedinečný název pro identifikaci nasazení aktualizace. |
| Časové pásmo |Časové pásmo, které se má použít pro čas spuštění |
| Čas spuštění |Datum a čas spuštění aktualizace nasazení |
| Doba trvání |Počet minut, po které je povoleno spuštění nasazení aktualizace.  Pokud se všechny aktualizace nenainstalují v rámci této doby trvání, musí zbývající aktualizace počkat až do dalšího nasazení aktualizace. |
| Počítače |Názvy počítačů nebo skupin počítačů, které chcete zahrnout do nasazení aktualizace.  V rozevíracím seznamu vyberte jednu nebo více položek. |

<br><br> ![Stránka Nasazení nové aktualizace](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Časové rozmezí
Ve výchozím nastavení je rozsah dat analyzovaný v řešení Správa aktualizací ze všech připojených skupin pro správu generovaných během posledního dne. 

Pokud chcete rozsah dat změnit, vyberte v horní části řídicího panelu **Podle data**. Můžete vybírat záznamy vytvořené nebo aktualizované během posledních 7 dní, 1 dne nebo 6 hodin. Nebo můžete vybrat **Vlastní** a zadat vlastní rozsah dat.<br><br> ![Možnost vlastního časového rozsahu](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Řešená Správa aktualizací vytváří v úložišti OMS dva typy záznamů.

### <a name="update-records"></a>Záznamy typu Aktualizace
Záznam typu **Aktualizace** se vytvoří pro každou aktualizaci, která je nainstalovaná nebo která je potřeba v každém počítači. Vlastnosti záznamů tohoto typu uvádí následující tabulka.

| Vlastnost | Popis |
| --- | --- |
| Typ |*Aktualizace* |
| SourceSystem |Zdroj, který schválil instalaci aktualizace<br>Možné hodnoty:<br>- Microsoft Update<br>-    Windows Update<br>-    SCCM<br>- Servery Linux (získané ze správců balíčků) |
| Schválené |Určuje, jestli byla instalace aktualizace schválená.<br> Pro servery Linux je tento postup momentálně volitelné, protože OMS nespravuje opravy. |
| Klasifikace pro Windows |Klasifikace aktualizace<br>Možné hodnoty:<br>-    Aplikace<br>- Důležité aktualizace<br>- Aktualizace definic<br>- Balíčky funkcí<br>– Aktualizace zabezpečení<br>- Aktualizace Service Pack<br>- Kumulativní aktualizace<br>- Aktualizace |
| Klasifikace pro Linux |Klasifikace aktualizace<br>Možné hodnoty:<br>- Důležité aktualizace<br>– Aktualizace zabezpečení<br>- Další aktualizace |
| Počítač |Název počítače |
| InstallTimeAvailable |Určuje, jestli je k dispozici čas instalace z jiných agentů, kteří instalovali stejnou aktualizaci. |
| InstallTimePredictionSeconds |Odhadovaná doba instalace v sekundách založená na jiných agentech, kteří instalovali stejnou aktualizaci. |
| KBID |ID článku znalostní báze, který popisuje aktualizaci. |
| ManagementGroupName |Název skupiny pro správu agentů SCOM.  Pro ostatní agenty to je AOI-<workspace ID>. |
| MSRCBulletinID |ID bulletinu zabezpečení Microsoftu popisující aktualizaci. |
| MSRCSeverity |Závažnost bulletinu zabezpečení Microsoftu.<br>Možné hodnoty:<br>- Kritická<br>- Důležitá<br>- Střední |
| Nepovinné |Určuje, jestli je aktualizace volitelná |
| Produkt |Název produktu, ke kterému se aktualizace vztahuje  Klikněte na **Zobrazit** pro otevření článku v prohlížeči. |
| PackageSeverity |Závažnost chyby zabezpečení opravené v této aktualizaci uváděná dodavateli distribuce pro Linux |
| PublishDate |Datum a čas instalace aktualizace |
| RebootBehavior |Určuje, jestli aktualizace vyžaduje restartování.<br>Možné hodnoty:<br>- canrequestreboot<br>- neverreboots |
| RevisionNumber |Číslo revize aktualizace |
| SourceComputerId |Identifikátor GUID k jednoznačné identifikaci počítače |
| TimeGenerated |Datum a čas poslední aktualizace záznamu |
| Název |Název aktualizace |
| UpdateID |Identifikátor GUID k jednoznačné identifikaci aktualizace |
| UpdateState |Určuje, jestli je v tomto počítači nainstalovaná aktualizace.<br>Možné hodnoty:<br>- Nainstalováno – aktualizace je v tomto počítači nainstalovaná.<br>- Vyžadováno – Aktualizace není nainstalovaná a tento počítač ji vyžaduje. |

<br>
Při provádění jakékoli hledání v protokolu, které vrací záznamy typu **Aktualizace**, můžete vybrat zobrazení **Aktualizace** obsahující sadu dlaždic se souhrnem aktualizací vrácených hledáním. Můžete kliknout na položky v dlaždicích **Chybějící a použité aktualizace** a **Požadované a volitelné aktualizace** k určení rozsahu zobrazení této sady aktualizací. Vyberte zobrazení **Seznam** nebo **Tabulka** k vracení jednotlivých záznamů.<br> 

![Zobrazení aktualizace hledání v protokolu s typem záznamu Aktualizace](./media/oms-solution-update-management/update-la-view-updates.png)  

V zobrazení **Tabulka** můžete kliknout na **KBID** u jakéhokoli záznamu, aby se otevřel prohlížeč s článkem znalostní báze. Díky tomu si můžete rychle přečíst o podrobnostech konkrétní aktualizace.<br> 

![Zobrazení tabulky hledání v protokolu s aktualizacemi typu záznamů Dlaždice](./media/oms-solution-update-management/update-la-view-table.png)

V zobrazení **Seznam** klikněte na odkaz **Zobrazení** vedle KBID. Otevře se článek znalostní báze.<br>

![Zobrazení seznamu hledání v protokolu s aktualizacemi typu záznamů Dlaždice](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>Záznamy UpdateSummary
Pro každý počítač s agentem Windows se vytvoří záznam typu **UpdateSummary** . Tento záznam se aktualizuje pokaždé, se provádí vyhledávání aktualizací v počítači. Vlastnosti záznamů typu **UpdateSummary** uvádí následující tabulka.

| Vlastnost | Popis |
| --- | --- |
| Typ |UpdateSummary |
| SourceSystem |OpsManager |
| Počítač |Název počítače |
| CriticalUpdatesMissing |Počet důležitých aktualizací, které v počítači chybí |
| ManagementGroupName |Název skupiny pro správu agentů SCOM. Pro ostatní agenty to je AOI-<workspace ID>. |
| NETRuntimeVersion |Verze běhového prostředí .NET nainstalovaného v počítači |
| OldestMissingSecurityUpdateBucket |Sektor ke kategorizaci času od posledního publikování nejstarší chybějící aktualizace zabezpečení v tomto počítači<br>Možné hodnoty:<br>- Starší<br>-    před 180 dny<br>- před 150 dny<br>-    před 120 dny<br>- před 90 dny<br>- před 60 dny<br>-    před 30 dny<br>-    Poslední |
| OldestMissingSecurityUpdateInDays |Počet dní od publikování nejstarší chybějící aktualizace zabezpečení v tomto počítači |
| OsVersion |Verze operačního systému nainstalovaného v počítači |
| OtherUpdatesMissing |Počet ostatních aktualizací, které v počítači chybí |
| SecurityUpdatesMissing |Počet aktualizací zabezpečení, které v počítači chybí |
| SourceComputerId |Identifikátor GUID k jednoznačné identifikaci počítače |
| TimeGenerated |Datum a čas poslední aktualizace záznamu |
| TotalUpdatesMissing |Celkový počet aktualizací, které v počítači chybí |
| WindowsUpdateAgentVersion |Číslo verze agenta Windows Update v počítači |
| WindowsUpdateSetting |Nastavení způsobu, jakým bude počítač instalovat důležité aktualizace<br>Možné hodnoty:<br>- Zakázáno<br>- Upozornění před instalací<br>- Plánovaná instalace |
| WSUSServer |Adresa URL serveru WSUS, pokud je počítač nakonfigurovaný na jeho používání |

## <a name="sample-log-searches"></a>Ukázky hledání v protokolech
V následující tabulce jsou uvedeny ukázky hledání v protokolech pro záznamy aktualizace shromážděné tímto řešením. 

| Dotaz | Popis |
| --- | --- |
| Všechny počítače s chybějícími aktualizacemi |Type=Update UpdateState=Needed Optional=false &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |
| Chybějící aktualizace v počítači COMPUTER01.contoso.com (nahraďte názvem svého počítače) |Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" &#124; select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate |
| Všechny počítače s chybějícími důležitými aktualizacemi nebo aktualizacemi zabezpečení |Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") |
| Důležité aktualizace nebo aktualizace zabezpečení vyžadované počítači, kde se aktualizace používají ručně |Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual &#124; Distinct Computer} &#124; Distinct KBID |
| Chybové události pro počítače s chybějícími požadovanými důležitými aktualizacemi nebo aktualizacemi zabezpečení |Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false &#124; Distinct Computer} |
| Všechny počítače s chybějícími kumulativními aktualizacemi |Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |
| Konkrétní chybějící aktualizace ve všech počítačích |Type=Update UpdateState=Needed Optional=false &#124; Distinct Title |
| Členství počítačů WSUS |Type=UpdateSummary &#124; measure count() by WSUSServer |
| Automatická konfigurace aktualizace |Type=UpdateSummary &#124; measure count() by WindowsUpdateSetting |
| Počítače se zakázanými automatickými aktualizacemi |Type=UpdateSummary WindowsUpdateSetting=Manual |
| Seznam všech počítačů Linux, které mají k dispozici aktualizaci balíčku |Type=Update and OSType=Linux and UpdateState!="Not needed" &#124; measure count() by Computer |
| Seznam všech počítačů Linux, které mají k dispozici aktualizace balíčku pro řešení kritické chyby nebo chyby zabezpečení |Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") &#124; measure count() by Computer |
| Seznam všech balíčků, které mají k dispozici aktualizaci |Type=Update and OSType=Linux and UpdateState!="Not needed" |
| Seznam všech balíčků pro řešení kritické chyby nebo chyby zabezpečení |Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") |
| Seznam všech počítačů s Ubuntu s jakoukoli dostupnou aktualizací |Type=Update and OSType=Linux and OSName = Ubuntu &#124; measure count() by Computer |

## <a name="next-steps"></a>Další kroky
* K zobrazení podrobných údajů o aktualizaci použijte Hledání v protokolu služby [Log Analytics](../log-analytics/log-analytics-log-searches.md).
* [Vytvářejte vlastní řídicí panely](../log-analytics/log-analytics-dashboards.md) zobrazující shodu aktualizace pro vaše spravované počítače.
* [Vytvářejte výstrahy](../log-analytics/log-analytics-alerts.md) při zjištění, že v počítačích chybí důležité aktualizace nebo že má počítač zakázané automatické aktualizace.  


