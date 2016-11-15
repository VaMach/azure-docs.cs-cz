---
title: "Začínáme se službou Log Analytics | Dokumentace Microsoftu"
description: "Log Analytics v Microsoft Operations Management Suite (OMS) můžete nastavit a začít používat během několika minut."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/10/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2f8defce183e61825d9df3397ea1082dbdb4b11a


---
# <a name="get-started-with-log-analytics"></a>Začínáme se službou Log Analytics
Log Analytics v Microsoft Operations Management Suite (OMS) můžete nastavit a začít používat během několika minut. Při vytváření pracovního prostoru OMS máte dvě možnosti, které odpovídají dvěma typům účtů:

* Web Microsoft Operations Management Suite
* Předplatné Microsoft Azure

Bezplatný pracovní prostor OMS můžete vytvořit pomocí webu OMS. Anebo můžete k vytvoření pracovního prostoru OMS použít předplatné Microsoft Azure. Oba pracovní prostory jsou funkčně ekvivalentní, jedinou výjimkou je denní limit 500 MB na data odeslaná do služby OMS v případě bezplatného pracovního prostoru OMS. Pokud máte předplatné Azure, můžete ho použít i k přístupu k jiným službám Azure. Bez ohledu na použitou metodu vytváříte pracovní prostor buď s účtem Microsoft, nebo s účtem organizace.

Tady je přehled celého procesu:

![Diagram připojování](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Požadavky služby Log Analytics a aspekty nasazení
* K plnému využívání služby Log Analytics budete potřebovat předplatné Microsoft Azure. Pokud předplatné Azure nemáte, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup ke službám Azure. Nebo můžete vytvořit bezplatný účet OMS na webu [Operations Management Suite](http://microsoft.com/oms) a kliknout na **Vyzkoušet bezplatně**.
* Potřebujete pracovní prostor OMS.
* Každý počítač se systémem Windows, ze kterého chcete shromažďovat data, musí mít Windows Server 2008 SP1 nebo vyšší.
* Je třeba nastavit v [bráně firewall](log-analytics-proxy-firewall.md) přístup k adresám webové služby OMS.
* Je třeba nastavit [server pro předávání Log Analytics v OMS](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (bránu) pro přenos dat ze serverů do služby OMS, počítače nemají přístup k internetu.
* Pokud používáte nástroj Operations Manager, služba Log Analytics podporuje Operations Manager 2012 SP1 UR6 a vyšší a Operations Manager 2012 R2 UR2 a vyšší. V nástrojích Operations Manager 2012 SP1 UR7 a Operations Manager 2012 R2 UR3 je přidaná podpora proxy serverů. Určete, jak se budou integrovat s OMS.
* Určete, jestli mají vaše počítače přímý přístup k internetu. Pokud ne, je pro přístup k serverům webové služby OMS nutná brána. Veškerý přístup probíhá přes protokol HTTPS.
* Určete, které technologie a servery budou odesílat data do OMS. Například řadiče domény, SQL Server atd.
* Udělte oprávnění uživatelům v OMS a Azure.
* Pokud vám dělají starosti objemy dat, nasaďte každé řešení jednotlivě a před přidáním dalších řešení otestujte dopad na výkon.
* Kontrolujte využití dat a výkon při postupném přidávání řešení a funkcí do služby Log Analytics. To zahrnuje shromažďování událostí, protokolů, dat o výkonu atd. Je lepší začít s minimálním sběrem dat, než ověříte dopad na výkon.
* Ověřte, že agenti Windows nejsou současně spravovaní nástrojem Operations Manager, aby nedocházelo k vytváření duplicitních dat. To platí i pro agenty v prostředí Azure s povolenou službou Azure Diagnostics.
* Až nainstalujete agenty, zkontrolujte, že správně fungují. Pokud ne, zkontrolujte, jestli není v zásadách skupiny zakázaná izolace klíčů v API kryptografické služby nové generace (CNG).
* Některá řešení v rámci služby Log Analytics mají další požadavky.

## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>Registrace ve třech krocích pomocí Operations Management Suite
1. Přejděte na web [Operations Management Suite](http://microsoft.com/oms) a klikněte na **Vyzkoušet bezplatně**. Přihlaste se pomocí účtu Microsoft, např. Outlook.com, nebo pomocí účtu organizace poskytnutého vaší společností nebo vzdělávací institucí pro použití s Office 365 nebo jinými službami Microsoftu.
2. Zadejte jedinečný název pracovního prostoru. Pracovní prostor je logický kontejner, ve kterém se ukládají vaše data správy. Poskytne vám způsob, jak oddělit data různých týmů v organizaci, protože data zůstávají výhradně ve svém pracovním prostoru. Určete e-mailovou adresu a oblast, ve které chcete mít data uložená.  
    ![Vytvoření pracovního prostoru a propojení předplatného](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Dále můžete vytvořit nové předplatné Azure nebo pracovní prostor propojit s existujícím předplatným. Pokud chcete pokračovat s bezplatnou zkušební verzi, klikněte na **Nyní ne**.  
   ![vytvoření pracovního prostoru a propojení předplatného](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Teď můžete začít pracovat s portálem Operations Management Suite.

Podrobnější informace o vytváření pracovního prostoru a propojování existujících účtů Azure s pracovními prostory vytvořenými pomocí Operations Management Suite obsahuje článek [Správa přístupu ke službě Log Analytics](log-analytics-manage-access.md).

## <a name="sign-up-quickly-using-microsoft-azure"></a>Rychlá registrace pomocí Microsoft Azure
1. Přejděte na web [Azure Portal](https://portal.azure.com), přihlaste se a v seznamu služeb vyberte **Log Analytics (OMS)**.  
    ![Azure Portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Klikněte na **Přidat** a podle potřeby změňte hodnoty následujících položek:
   * Název **pracovního prostoru OMS**
   * **Předplatné** – pokud pracujete s více předplatnými, vyberte to, které chcete přidružit k novému pracovnímu prostoru.
   * **Skupina prostředků**
   * **Umístění**
   * **Cenová úroveň**  
       ![Rychlé vytvoření](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klikněte na **Vytvořit**. Zobrazí se podrobnosti o pracovním prostoru na webu Azure Portal.       
    ![podrobnosti o pracovním prostoru](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Kliknutím na odkaz **OMS Portal** přejděte na web Operations Management Suite k novému pracovnímu prostoru.

Teď můžete začít pracovat s portálem Operations Management Suite.

Podrobnější informace o vytváření pracovního prostoru a propojování existujících pracovních prostorů vytvořených pomocí Operations Management Suite s předplatnými Azure obsahuje článek [Správa přístupu ke službě Log Analytics](log-analytics-manage-access.md).

## <a name="get-started-with-the-operations-management-suite-portal"></a>Zahájení práce s portálem Operations Management Suite
Při výběru řešení a připojení serverů, které chcete spravovat, postupujte na dlaždici **Settings** (Nastavení) podle kroků v této části.  

![Začínáme](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Přidání řešení** – podívejte se na nainstalovaná řešení.  
    ![řešení](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Klikněte na tlačítko **Navštívit galerii**, abyste mohli přidat více řešení.  
    ![řešení](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Vyberte řešení a potom klikněte na **Přidat**.
2. **Připojte zdroj** – zvolte způsob připojení k serverovému prostředí pro shromažďování dat:
   
   * Připojte libovolný systém Windows Server nebo klienta přímo instalací agenta.
   * Připojte servery se systémem Linux s agentem OMS pro Linux.
   * Použijte účet úložiště Azure nakonfigurovaný v rozšíření VM Azure Diagnostic ve Windows nebo v Linuxu.
   * Pomocí nástroje System Center Operations Manager připojte skupiny správy nebo celé nasazení nástroje Operations Manager.
   * Povolte Telemetrii Windows používat Upgrade Analytics.
       ![připojené zdroje](./media/log-analytics-get-started/oms-onboard-data-sources.png)    
3. **Shromáždění dat** Nakonfigurujte alespoň jeden zdroj dat k naplnění dat do pracovního prostoru. Po dokončení klikněte na **Uložit**.    
   
    ![shromažďování dat](./media/log-analytics-get-started/oms-onboard-logs.png)    

## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>Volitelné připojení serverů přímo k Operations Management Suite instalací agenta
Následující příklad ukazuje, jak nainstalovat agenta Windows.

1. Klikněte na dlaždici **Nastavení**, na kartu **Připojené zdroje** a potom na kartu pro typ zdroje, který chcete přidat. Pak buďto stáhněte agenta nebo zjistěte, jak agenta povolit. Klikněte například na **Stáhnout agenta Windows (64 bitů)**. Agenta Windows můžete nainstalovat jenom v systému Windows Server 2008 SP1 a novějším nebo Windows 7 SP1 a novějším.
2. Nainstalujte agenta na jeden nebo více serverů. Agenty můžete instalovat po jednom, nebo můžete použít automatizovanou metodu s [vlastním skriptem](log-analytics-windows-agents.md), případně použijte vaše nasazené řešení distribuce softwaru.
3. Jakmile vyjádříte svůj souhlas s licenční smlouvou a zvolíte instalační složku, vyberte **Connect the agent to Microsoft Azure Operational Insights** (Připojit agenta k Microsoft Azure Operational Insights).   
    ![instalace agenta](./media/log-analytics-get-started/oms-onboard-agent.png)
4. Na další stránce budete vyzváni k zadání ID a klíče pracovního prostoru. ID a klíč pracovního prostoru najdete na obrazovce, na které jste stáhli soubor agenta.  
    ![klíče agenta](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  
   
    ![připojení serverů](./media/log-analytics-get-started/oms-onboard-key.png)
5. Během instalace můžete kliknutím na **Advanced** (Upřesnit) nastavit proxy server a zadat ověřovací informace. Kliknutím na **Next** (Další) se vraťte na obrazovku informací o pracovním prostoru.
6. Kliknutím na **Next** ověřte ID a klíč pracovního prostoru. Pokud zjistíte nějaké chyby, klikněte na **Back** (Zpět) a opravte je. Po ověření ID a klíče pracovního prostoru klikněte na **Install** (Instalovat) a dokončete instalaci agenta.
7. V ovládacích panelech klikněte na Microsoft Monitoring Agent > karta Azure Log Analytics (OMS). Jakmile agenti začnou komunikovat s Operations Management Suite, zobrazí se ikona zeleného zaškrtnutí. Na počátku tato akce trvá přibližně 5 až 10 minut.

> [!NOTE]
> Řešení pro posouzení správy kapacity a konfigurace nejsou aktuálně podporované pro servery připojené přímo k Operations Management Suite.
> 
> 

Můžete také připojit agenta k nástroji System Center Operations Manager 2012 SP1 a novějšímu. Provedete to výběrem možnosti **Připojit agenta k nástroji System Center Operations Manager**. Když vyberete tuto možnost, budete odesílat do služby data bez dalších hardwarových nebo výkonových nároků na skupiny správy.

Další informace o připojení agentů k sadě Operations Management Suite obsahuje článek [Propojení počítačů s Windows se službou Log Analytics](log-analytics-windows-agents.md).

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Volitelné připojení serverů pomocí nástroje System Center Operations Manager
1. V konzole nástroje Operations Manager vyberte pracovní prostor **Správa**.
2. Rozbalte uzel **Operational Insights** a poté vyberte **Připojení služby Operational Insights**.
   
   > [!NOTE]
   > V závislosti na tom, jaké kumulativní aktualizace SCOM používáte, může se zobrazit uzel pro *System Center Advisor*, *Operational Insights* nebo *Operations Management Suite*.
   > 
   > 
3. Klikněte na odkaz **Registrovat se ve službě Operational Insights Service** směrem doprava nahoře a postupujte podle pokynů.
4. Po dokončení průvodce registrací klikněte na odkaz **Přidat počítač/skupinu** odkaz.
5. V dialogovém okna **Hledání počítače** můžete vyhledat počítače nebo skupiny sledované nástrojem Operations Manager. Vyberte počítače nebo skupiny, které chcete zahrnout do služby Log Analytics, klikněte na **Přidat** a potom na **OK**. Na dlaždici **Využití** na portálu Operations Management Suite můžete zkontrolovat, jestli OMS přijímá nějaká data. Data by se měla objevit během 5–10 minut.

Další informace o připojení nástroje Operations Manager k Operations Management Suite najdete v článku [Připojení nástroje Operations Manager ke službě Log Analytics](log-analytics-om-agents.md).

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Volitelná analýza dat z cloudových služeb Microsoft Azure
S Operations Management Suite můžete rychle prohledávat události a protokoly služby IIS cloudových služeb a virtuálních počítačů tím, že povolíte diagnostiku pro Azure Cloud Services. Můžete také přijímat další statistiky pro virtuální počítače Azure, pokud na ně nainstalujete agenta Microsoft Monitoring Agent. Další informace o konfiguraci prostředí Azure pro použití s Operations Management Suite najdete v článku [Připojení úložiště Azure ke službě Log Analytics](log-analytics-azure-storage.md).

## <a name="next-steps"></a>Další kroky
* Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.
* Chcete-li zobrazit podrobné informace shromážděné řešeními, seznamte se s [vyhledáváním protokolů](log-analytics-log-searches.md).
* Pomocí [řídicích panelů](log-analytics-dashboards.md) můžete ukládat a zobrazovat vlastní vyhledávání.




<!--HONumber=Nov16_HO2-->


