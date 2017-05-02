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
ms.date: 04/20/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: b0624bde9ab53231768beb0c832a4a49b21a8975
ms.lasthandoff: 04/20/2017


---
# <a name="update-management-solution-in-oms"></a>Aktualizace řešení pro správu v OMS
Řešení pro správu aktualizací v OMS umožňuje spravovat aktualizace pro počítače s Windows a Linuxem.  Stav dostupných aktualizací na všech počítačích agenta můžete rychle vyhodnotit a zahájit proces instalace požadovaných aktualizací pro servery. 

## <a name="solution-components"></a>Součásti řešení

Počítače spravované pomocí OMS používají k provádění vyhodnocení a nasazení aktualizací následující: 

* Agenta OMS pro Windows nebo Linux
* Konfiguraci požadovaného stavu (DSC) PowerShellu pro Linux 
* Funkci Hybrid Runbook Worker služby Automation 
* Microsoft Update nebo Windows Server Update Services pro počítače s Windows

Následující diagramy ukazují konceptuální zobrazení chování a toku dat s tím, jak toto řešení vyhodnocuje a aplikuje aktualizace na všechny připojené počítače s Windows Serverem a Linuxem v pracovním prostoru.    

#### <a name="windows-server"></a>Windows Server
![Proces správy aktualizací pro Windows Server](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![Proces správy aktualizací pro Linux](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

Jakmile počítač provede kontrolu kompatibility aktualizací, agent OMS hromadně předá tyto informace do OMS. Na počítačích s Windows se kontrola kompatibility ve výchozím nastavení provádí každých 12 hodin.  Mimo plán kontrol se kontrola kompatibility aktualizací také spustí během 15 minut v případě restartování agenta Microsoft Monitoring Agent (MMA), před instalací aktualizací a po instalaci aktualizací.  U počítače s Linuxem se kontrola aktualizací ve výchozím nastavení provádí každé 3 hodiny a také během 15 minut v případě restartování agenta MMA.  

Informace o kompatibilitě se pak zpracují a vytvoří se z nich souhrny v řídicích panelech, které jsou součástí řešení, nebo je lze prohledávat pomocí předdefinovaných nebo uživatelem definovaných dotazů.  Řešení podá zprávu o aktuálnosti počítače podle toho, s jakým zdrojem je nakonfigurována synchronizace.  Pokud je počítač s Windows nakonfigurovaný tak, aby ukládal data do služby WSUS, v závislosti na času poslední synchronizace služby WSUS s Microsoft Update se výsledky můžou lišit od toho, co ukazuje Microsoft Update.  To samé platí pro počítače s Linuxem, které jsou nakonfigurované k ukládání dat do místního úložiště, nebo naopak do veřejného úložiště.   

Na počítače, které vyžadují aktualizace softwaru, můžete tyto aktualizace nasadit a nainstalovat tak, že vytvoříte plánované nasazení.  Do oboru nasazení pro počítače s Windows nejsou zahrnuty aktualizace klasifikované jako *Volitelné*, pouze požadované aktualizace.  Plánované nasazení definuje, které cílové počítače obdrží příslušné aktualizace, buď tak, že počítače explicitně zadáte, nebo vyberete [skupinu počítačů](../log-analytics/log-analytics-computer-groups.md) na základě prohledávání protokolů konkrétní sady počítačů.  Zadáte také plán pro schválení a vyhrazení časového období, kdy je možné aktualizace nainstalovat.  Aktualizace se instalují podle runbooků ve službě Azure Automation.  Tyto runbooky není možné zobrazit a nevyžadují žádnou konfiguraci.  Při vytvoření nasazení aktualizací se vytvoří plán, který v zadanou dobu spustí hlavní runbook aktualizace pro zahrnuté počítače.  Tento hlavní runbook spouští podřízený runbook na každém agentovi, který provádí instalaci požadovaných aktualizací.       

V den a čas, který zadáte v nasazení aktualizací, spustí cílové počítače paralelně nasazení.  Nejprve se provede kontrola pro ověření, že se aktualizace stále vyžadují, a pak se aktualizace nainstalují.  U klientských počítačů služby WSUS je důležité si uvědomit, že pokud nejsou aktualizace schválené ve službě WSUS, nasazení aktualizací se nezdaří.  Výsledky použitých aktualizací se předají ke zpracování do OMS a vytvoří se souhrny, které najdete na řídicích panelech nebo pomocí vyhledání událostí.     

## <a name="prerequisites"></a>Požadavky
* Řešení podporuje provádění vyhodnocení aktualizací pro systém Windows Server 2008 nebo novější a nasazení aktualizací pro systém Windows Server 2012 nebo novější.  Možnosti instalace Server Core a Nano Server nejsou podporované.
* Klientské operační systémy Windows nejsou podporované.  
* Agenti Windows musí být buď nakonfigurovaní na komunikaci se službou Windows Server Update Services (WSUS), nebo musí mít přístup ke službě Microsoft Update.  
  
    > [!NOTE]
    > Agenta Windows není možné spravovat současně s nástrojem System Center Configuration Manager.  
    >
* CentOS 6 (x86/x64) a 7 (x64)
* Red Hat Enterprise 6 (x86/x64) a 7 (x64)
* SUSE Linux Enterprise Server 11 (x86/x64) a 12 (x64)
* Ubuntu 12.04 LTS a novější x86/x64  
* Agenty Linux musí mít přístup k úložišti aktualizací.  

    > [!NOTE]
    > Agent OMS pro Linux nakonfigurovaný k ukládání dat do více pracovních prostorů OMS se v tomto řešení nepodporuje.  
    > 

Další informace o tom, jak nainstalovat agenta OMS pro Linux, a odkaz na stažení nejnovější verze najdete na stránce [Operations Management Suite Agent for Linux](https://github.com/microsoft/oms-agent-for-linux) (Agent Operations Management Suite pro Linux).  Informace o tom, jak nainstalovat agenta OMS pro Windows, najdete v tématu popisujícím [agenta Operations Management Suite pro Windows](../log-analytics/log-analytics-windows-agents.md).  

## <a name="solution-components"></a>Součásti řešení
Toto řešení se skládá z následujících prostředků, které se přidají do vašeho účtu Automation, a přímo připojených agentů nebo skupiny pro správu připojené k Operations Manageru. 

### <a name="management-packs"></a>Sady Management Pack
Pokud je vaše skupina pro správu System Center Operations Manageru připojená k pracovnímu prostoru OMS, do Operations Manageru se nainstalují následující sady Management Pack.  Tyto sady Management Pack se po přidání tohoto řešení nainstalují také na přímo připojené počítače s Windows. U těchto sad Management Pack není nutné nic konfigurovat ani spravovat. 

* Aktualizace Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Aktualizace sady pro správu nasazení

Další informace o způsobu, jakým se aktualizují sady pro správu řešení, najdete v tématu [Připojení Operations Manageru ke službě Log Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="hybrid-worker-groups"></a>Skupiny Hybrid Worker
Jakmile povolíte toto řešení, každý počítač s Windows přímo připojený k pracovnímu prostoru OMS se automaticky nakonfiguruje jako Hybrid Runbook Worker, aby podporoval runbooky, které jsou zahrnuté v tomto řešení.  Pro každý počítač s Windows spravovaný tímto řešením bude uveden v účtu Automation v okně Skupiny Hybrid Runbook Worker a bude splňovat zásadu vytváření názvů *Název_hostitele FQDN_GUID*.  Tyto skupiny nemůžete vybrat jako cíl runbooků ve vašem účtu, jinak se runbooky nezdaří. Tyto skupiny jsou určeny pouze pro podporu tohoto řešení pro správu.   

Počítače s Windows ale můžete přidat do skupiny Hybrid Runbook Worker ve vašem účtu Automation pro podporu runbooků Automation, pokud používáte stejný účet pro toto řešení i pro členství ve skupině Hybrid Runbook Worker.  Tuto funkci jsme do funkce Hybrid Runbook Worker přidali ve verzi 7.2.12024.0.  

## <a name="configuration"></a>Konfigurace
Pomocí následujících kroků přidejte řešení pro správu aktualizací do pracovního prostoru OMS a potvrďte, že se agenti hlásí. Agenti Windows se přidají automaticky bez dodatečné konfigurace. 

Toto řešení můžete přidat třemi různými způsoby: z Azure Marketplace na webu Azure Portal výběrem nabídky Automation and Control nebo řešení pro správu aktualizací, nebo z Galerie řešení OMS ve vašem pracovním prostoru OMS.  Pokud již máte propojený účet Automation s pracovním prostorem OMS ve stejné skupině prostředků a oblasti, po výběru nabídky Automation and Control se ověří vaše konfigurace a řešení se nainstaluje a nakonfiguruje pouze v těchto dvou službách.  Výběr řešení pro správu aktualizací z Azure Marketplace se bude chovat stejně.  Pokud některou z těchto služeb nemáte ve svém předplatném nasazenou, postupujte podle kroků v okně **Vytvoření nového řešení** a potvrďte, že chcete nainstalovat další předem vybraná doporučená řešení.  Volitelně můžete přidat řešení pro správu aktualizací do svého pracovního prostoru OMS z galerie řešení pomocí postupu popsaného v části [Přidání řešení OMS](../log-analytics/log-analytics-add-solutions.md).  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-to-oms"></a>Ověření připojení agentů OMS a skupiny pro správu Operations Manageru k OMS

Pokud chcete ověřit, že přímo připojení agenti OMS pro Linux a Windows komunikují s OMS, po několika minutách můžete spustit následující prohledávání protokolů:

* Linux – `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`.  

* Windows – `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

Na počítači s Windows můžete ověřit připojení agenta k OMS následujícím způsobem:

1.  V Ovládacích panelech otevřete agenta Microsoft Monitoring Agent a na kartě **Azure Log Analytics (OMS)** agent zobrazí zprávu: **The Microsoft Monitoring Agent has successfully connected to the Microsoft Operations Management Suite service** (Microsoft Monitoring Agent se úspěšně připojil ke službě Microsoft Operations Management Suite).   
2.  Otevřete protokol událostí systému Windows, přejděte do **Application and Services Logs\Operations Manager** a vyhledejte ID události 3000 a 5002 ze zdrojového konektoru Service Connector.  Tyto události značí, že se počítač zaregistroval do pracovního prostoru OMS a přijímá konfiguraci.  

Pokud agent není schopen komunikovat se službou OMS a má nakonfigurovanou komunikaci s internetem prostřednictvím brány firewall nebo proxy serveru, přečtěte si téma [Konfigurace nastavení proxy serveru a brány firewall v Log Analytics](../log-analytics/log-analytics-proxy-firewall.md) a ověřte správnou konfiguraci brány firewall nebo proxy serveru.
  
Stav nově přidaných agentů systému Linux bude po provedení vyhodnocení **Aktualizovaný**.  Tento proces může trvat až 6 hodin. 

Pokud chcete ověřit, že skupina pro správu Operations Manageru komunikuje s OMS, přečtěte si téma [Ověření integrace Operations Manageru s OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms).

## <a name="data-collection"></a>Shromažďování dat
### <a name="supported-agents"></a>Podporovaní agenti
Následující tabulka popisuje připojené zdroje, které toto řešení podporuje.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| Agenti systému Windows |Ano |Řešení shromažďuje informace o aktualizacích systému pro agenty Windows a inicializuje instalaci požadovaných aktualizací. |
| Agenti systému Linux |Ano |Řešení shromažďuje informace o aktualizacích systému od agentů systému Linux a zahajuje instalaci požadovaných aktualizací v podporovaných distribucích. |
| Skupina pro správu Operations Manageru |Ano |Řešení shromažďuje informace o aktualizacích systému z agentů v připojené skupině pro správu.<br>Přímé připojení z agenta Operations Manageru ke službě Log Analytics není potřeba. Data se přesměrovávají ze skupiny pro správu do úložiště OMS. |
| Účet služby Azure Storage |Ne |Úložiště Azure neobsahuje informace o aktualizacích systému. |

### <a name="collection-frequency"></a>Četnost shromažďování dat
Pro každý spravovaný počítač s Windows se kontrola provádí dvakrát denně. Každých 15 minut se volá rozhraní Windows API pro zadání dotazu na čas poslední aktualizace, podle kterého zjistí, jestli se změnil stav, a pokud ano, zahájí se kontrola kompatibility.  Pro každý spravovaný počítač s Linuxem se kontrola provádí každé tři hodiny. 

Může trvat 30 minut až 6 hodin, než se na řídicím panelu zobrazí aktualizovaná data ze spravovaných počítačů.   

## <a name="using-the-solution"></a>Použití řešení
Když přidáte řešení pro správu aktualizací do pracovního prostoru OMS, na řídicí panel OMS se přidá dlaždice **Správa aktualizací**. Na této dlaždici se zobrazuje počet a grafické vyjádření počtu počítačů ve vašem prostředí a jejich kompatibilita s aktualizacemi.<br><br>
![Dlaždice Souhrn Správy aktualizací](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>Zobrazení posouzení aktualizací
Klikněte na dlaždici **Správa aktualizací**. Otevře se řídicí panel **Správa aktualizací**.<br><br> ![Řídicí panel Souhrn Správy aktualizací](./media/oms-solution-update-management/update-management-dashboard.png)<br> 

Tento řídicí panel poskytuje podrobný přehled stavu aktualizací rozdělený podle typu operačního systému a klasifikace aktualizace – důležitá aktualizace, aktualizace zabezpečení, nebo jiná (například aktualizace definic). Po výběru dlaždice **Nasazení aktualizací** budete přesměrováni na stránku Nasazení aktualizací, kde můžete zobrazit plány, aktuálně spuštěná nasazení a dokončená nasazení nebo můžete naplánovat nové nasazení.  

Kliknutím na konkrétní dlaždici můžete spustit prohledávání protokolů, které vrátí všechny záznamy, nebo pokud chcete spustit dotaz určité kategorie s předdefinovanými kritérii, vyberte některý ze seznamu ve sloupci **Běžné dotazy na aktualizace**.    

## <a name="installing-updates"></a>Instalace aktualizací
Po posouzení aktualizací pro všechny počítače s Linuxem a Windows ve vašem prostředí můžete nechat nainstalovat požadované aktualizace vytvořením *nasazení aktualizací*.  Nasazení aktualizací je plánovaná instalace požadovaných aktualizací pro jeden nebo více počítačů.  Kromě počítače nebo skupiny počítačů, které mají být součástí rozsahu nasazení, zadáte datum a čas nasazení.  Další informace o skupinách počítačů najdete v tématu [Skupiny počítačů v Log Analytics](../log-analytics/log-analytics-computer-groups.md).  Když do svého nasazení aktualizací zahrnete skupiny počítačů, členství ve skupině se vyhodnotí pouze jednou při vytvoření plánu.  Další změny ve skupině se neprojeví.  Můžete to obejít tak, že naplánované nasazení aktualizací odstraníte a znovu vytvoříte. 

> [!NOTE]
> Virtuální počítače s Windows nasazené z Azure Marketplace jsou standardně nastaveny na přijímání automatických aktualizací ze služby Windows Update.  Toto chování se po přidání tohoto řešení nebo virtuálních počítačů s Windows do vašeho pracovního prostoru nezmění.  Pokud aktivně nespravujete aktualizace pomocí tohoto řešení, použije se výchozí chování (automatické aktualizace).  

Pro virtuální počítače vytvořené z imagí Red Hat Enterprise Linux (RHEL) na vyžádání dostupných v Azure Marketplace jsou registrované pro přístup k infrastruktuře [Red Hat Update Infrastructure (RHUI)](../virtual-machines/linux/update-infrastructure-redhat.md) nasazené v Azure.  Všechny ostatní distribuce Azure musí být aktualizované z online úložiště souborů distribuce podle podporované metody.  

### <a name="viewing-update-deployments"></a>Zobrazení nasazení aktualizace
Klikněte na dlaždici **Aktualizovat nasazení** zobrazíte seznam existujících nasazení aktualizace.  Jsou seskupené podle stavu – **Naplánované**, **Spuštěné** a **Dokončeno**.<br><br> ![Stránka Plán nasazení aktualizace](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Následující tabulka popisuje vlastnosti zobrazené u každého nasazení aktualizace.

| Vlastnost | Popis |
| --- | --- |
| Název |Název nasazení aktualizace |
| Plán |Typ plánu  Dostupné možnosti jsou *Jednou*, *Týdenní opakování* nebo *Měsíční opakování*. |
| Čas spuštění |Datum a čas, na kdy je naplánované spuštění nasazení aktualizace. |
| Doba trvání |Počet minut, po které je povoleno spuštění nasazení aktualizace.  Pokud se všechny aktualizace nenainstalují v rámci této doby trvání, musí zbývající aktualizace počkat až do dalšího nasazení aktualizace. |
| Servery |Počet počítačů ovlivněných nasazením aktualizace  |
| Status |Aktuální stav nasazení aktualizace<br><br>Možné hodnoty:<br>- Není spuštěné<br>- Spuštěné<br>- Dokončeno |

Vyberte dokončené nasazení aktualizací a podívejte se na obrazovku s podrobnostmi, která zahrnuje sloupce v následující tabulce.  Tyto sloupce nebudou naplněné, pokud nasazení aktualizace ještě nezačalo.<br><br> ![Přehled výsledků nasazení aktualizace](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| Sloupec | Popis |
| --- | --- |
| **Zobrazení počítačů** | |
| Počítače s Windows |Uvádí počet počítačů s Windows v nasazení aktualizací podle stavu.  Klikněte na stav, aby se spustilo prohledávání protokolu, které vrátí všechny záznamy aktualizace s tímto stavem pro nasazení aktualizace. |
| Počítače s Linuxem |Uvádí počet počítačů s Linuxem v nasazení aktualizací podle stavu.  Klikněte na stav, aby se spustilo prohledávání protokolu, které vrátí všechny záznamy aktualizace s tímto stavem pro nasazení aktualizace. |
| Stav instalace počítače |Zobrazí seznam počítačů zahrnutých v nasazení aktualizace a procento aktualizací, které se úspěšně nainstalovaly. Klikněte na jednotlivé položky ke spuštění hledání v protokolu se všemi chybějícími a důležitými aktualizacemi. |
| **Zobrazení aktualizací** | |
| Aktualizace pro Windows |Uvádí seznam aktualizací pro Windows zahrnutých v nasazení aktualizací a stav instalace jednotlivých aktualizací.  Výběrem aktualizace spustíte prohledávání protokolů, které vrátí všechny záznamy aktualizací pro danou konkrétní aktualizaci, nebo můžete kliknutím na stav spustit prohledávání protokolů, které vrátí všechny záznamy aktualizací pro dané nasazení. | 
| Aktualizace pro Linux |Uvádí seznam aktualizací pro Linux zahrnutých v nasazení aktualizací a stav instalace jednotlivých aktualizací.  Výběrem aktualizace spustíte prohledávání protokolů, které vrátí všechny záznamy aktualizací pro danou konkrétní aktualizaci, nebo můžete kliknutím na stav spustit prohledávání protokolů, které vrátí všechny záznamy aktualizací pro dané nasazení. | 

### <a name="creating-an-update-deployment"></a>Vytvoření nasazení aktualizace
Vytvořte nové nasazení aktualizace kliknutím na tlačítko **Přidat** v horní části obrazovky. Otevře se stránka **Nasazení nové aktualizace**.  Je nutné zadat hodnoty pro vlastnosti v následující tabulce.

| Vlastnost | Popis |
| --- | --- |
| Name (Název) |Jedinečný název pro identifikaci nasazení aktualizace. |
| Časové pásmo |Časové pásmo, které se má použít pro čas spuštění |
| Typ plánu | Typ plánu  Dostupné možnosti jsou *Jednou*, *Týdenní opakování* nebo *Měsíční opakování*.  
| Čas spuštění |Datum a čas spuštění aktualizace nasazení **Poznámka:** Nasazení se může spustit nejdříve 30 minut od aktuálního času, pokud potřebujete nasazení provést okamžitě. |
| Doba trvání |Počet minut, po které je povoleno spuštění nasazení aktualizace.  Pokud se všechny aktualizace nenainstalují v rámci této doby trvání, musí zbývající aktualizace počkat až do dalšího nasazení aktualizace. |
| Počítače |Názvy počítačů nebo skupin počítačů, které chcete zahrnout do nasazení aktualizací.  V rozevíracím seznamu vyberte jednu nebo více položek. |

<br><br> ![Stránka Nasazení nové aktualizace](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Časové rozmezí
Ve výchozím nastavení je rozsah dat analyzovaný v řešení Správa aktualizací ze všech připojených skupin pro správu generovaných během posledního dne. 

Pokud chcete rozsah dat změnit, vyberte v horní části řídicího panelu **Podle data**. Můžete vybírat záznamy vytvořené nebo aktualizované během posledních 7 dní, 1 dne nebo 6 hodin. Nebo můžete vybrat **Vlastní** a zadat vlastní rozsah dat.

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Řešená Správa aktualizací vytváří v úložišti OMS dva typy záznamů.

### <a name="update-records"></a>Záznamy typu Aktualizace
Záznam typu **Aktualizace** se vytvoří pro každou aktualizaci, která je nainstalovaná nebo která je potřeba v každém počítači. Vlastnosti záznamů tohoto typu uvádí následující tabulka.

| Vlastnost | Popis |
| --- | --- |
| Typ |*Aktualizace* |
| SourceSystem |Zdroj, který schválil instalaci aktualizace<br>Možné hodnoty:<br>- Microsoft Update<br>- Windows Update<br>- SCCM<br>- Servery Linux (získané ze správců balíčků) |
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
|Počítače serveru s Windows, které potřebují aktualizace |`Type:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false | measure count() by Computer` |
|Servery s Linuxem, které potřebují aktualizace | `Type:Update OSType=Linux UpdateState!="Not needed" | measure count() by Computer` |
| Všechny počítače s chybějícími aktualizacemi |`Type=Update UpdateState=Needed Optional=false | select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| Chybějící aktualizace v určitém počítači (nahraďte hodnotu názvem svého počítače) |`Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" | select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate` |
| Všechny počítače s chybějícími důležitými aktualizacemi nebo aktualizacemi zabezpečení |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates"`) |
| Důležité aktualizace nebo aktualizace zabezpečení vyžadované počítači, kde se aktualizace používají ručně |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | Distinct Computer} | Distinct KBID` |
| Chybové události pro počítače s chybějícími požadovanými důležitými aktualizacemi nebo aktualizacemi zabezpečení |`Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false | Distinct Computer}` |
| Všechny počítače s chybějícími kumulativními aktualizacemi |`Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed| select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| Konkrétní chybějící aktualizace ve všech počítačích |`Type=Update UpdateState=Needed Optional=false | Distinct Title` |
| Počítač serveru s Windows a aktualizacemi, které se nezdařily při hromadné postupné aktualizaci | `Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Title, UpdateRunName` |
| Server s Linuxem a aktualizacemi, které se nezdařily při hromadné postupné aktualizaci |`Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Product, UpdateRunName` |
| Členství počítačů WSUS |`Type=UpdateSummary | measure count() by WSUSServer` |
| Automatická konfigurace aktualizace |`Type=UpdateSummary | measure count() by WindowsUpdateSetting` |
| Počítače se zakázanými automatickými aktualizacemi |`Type=UpdateSummary WindowsUpdateSetting=Manual` |
| Seznam všech počítačů Linux, které mají k dispozici aktualizaci balíčku |`Type=Update and OSType=Linux and UpdateState!="Not needed" | measure count() by Computer` |
| Seznam všech počítačů Linux, které mají k dispozici aktualizace balíčku pro řešení kritické chyby nebo chyby zabezpečení |`Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") | measure count() by Computer` |
| Seznam všech balíčků, které mají k dispozici aktualizaci |Type=Update and OSType=Linux and UpdateState!="Not needed" |
| Seznam všech balíčků pro řešení kritické chyby nebo chyby zabezpečení |`Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates")` |
| Vypsat nasazení aktualizací, která provedla úpravy počítačů |`Type:UpdateRunProgress | measure Count() by UpdateRunName` |
|Počítače aktualizované při této hromadné postupné aktualizaci (nahraďte hodnotu názvem vašeho nasazení aktualizací) |`Type:UpdateRunProgress UpdateRunName="DeploymentName" | measure Count() by Computer` |
| Seznam všech počítačů s Ubuntu s jakoukoli dostupnou aktualizací |`Type=Update and OSType=Linux and OSName = Ubuntu &| measure count() by Computer` |

## <a name="troubleshooting"></a>Řešení potíží 

Tato část obsahuje informace, které vám pomohou s řešením potíží s řešením pro správu aktualizací.  

### <a name="how-do-i-troubleshoot-update-deployments"></a>Jak mám řešit problémy s nasazeními aktualizací?
V okně Úlohy vašeho účtu Automation, který je propojený s pracovním prostorem OMS podporujícím toto řešení, můžete zobrazit výsledky runbooku odpovědného za nasazení aktualizací zahrnutých v naplánovaném nasazení aktualizací.  Runbook **Patch-MicrosoftOMSComputer** je podřízený runbook, jehož cílem je konkrétní spravovaný počítač, a kontrolou podrobného datového proudu získáte podrobné informace k příslušnému nasazení.  Na výstupu se zobrazí, které požadované aktualizace se dají použít, stav stahování, stav instalace a další podrobnosti.<br><br> ![Stav úlohy nasazení aktualizací](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

Další informace najdete v tématu [Výstup a zprávy runbooku Automation](../automation/automation-runbook-output-and-messages.md).   
  
## <a name="next-steps"></a>Další kroky
* K zobrazení podrobných údajů o aktualizaci použijte Hledání v protokolu služby [Log Analytics](../log-analytics/log-analytics-log-searches.md).
* [Vytvářejte vlastní řídicí panely](../log-analytics/log-analytics-dashboards.md) zobrazující shodu aktualizace pro vaše spravované počítače.
* [Vytvářejte výstrahy](../log-analytics/log-analytics-alerts.md) při zjištění, že v počítačích chybí důležité aktualizace nebo že má počítač zakázané automatické aktualizace.  


