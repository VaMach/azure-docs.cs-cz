---
title: "Začínáme s pracovním prostorem služby Azure Log Analytics | Microsoft Docs"
description: "Pracovní prostor ve službě Log Analytics můžete zprovoznit během několika minut."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/12/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 634b4c99b746ad0b8b3238e11b3c305d2cf7082f
ms.contentlocale: cs-cz
ms.lasthandoff: 03/28/2017


---
# <a name="get-started-with-a-log-analytics-workspace"></a>Začínáme s pracovním prostorem služby Log Analytics
Službu Azure Log Analytics, která vám usnadní vyhodnocování informací o provozu získaných z infrastruktury IT, můžete zprovoznit během několika minut. S použitím informací z tohoto článku můžete snadno začít zkoumat a analyzovat data, která *zdarma* shromáždíte, a pracovat s nimi.

Tento článek slouží jako úvod ke službě Log Analytics. Stručný kurz vás provede procesem minimálního nasazení v Azure, abyste službu mohli začít používat. Logický kontejner, ve kterém se v Azure ukládají vaše data správy, se nazývá pracovní prostor. Po přezkoumání těchto informací a dokončení vlastního hodnocení můžete zkušební pracovní prostor odebrat. Tento článek je kurz, a proto se nezabývá obchodními požadavky, plánováním ani doporučeními ohledně architektury.

>[!NOTE]
>Pokud používáte cloud Microsoft Azure Government, najdete informace spíše v [dokumentaci k monitorování a správě Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#log-analytics).

Tady je rychlý přehled počátečního postupu:

![diagram procesu](./media/log-analytics-get-started/onboard-oms.png)

## <a name="1-create-an-azure-account-and-sign-in"></a>1. Vytvoření účtu Azure a přihlášení

Pokud účet Azure ještě nemáte, budete ho muset vytvořit, abyste mohli používat službu Log Analytics. Můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup k veškerým službám Azure po dobu 30 dní.

### <a name="to-create-a-free-account-and-sign-in"></a>Vytvoření bezplatného účtu a přihlášení
1. Postupujte podle pokynů v části [Bezplatné vytvoření účtu Azure](https://azure.microsoft.com/free/).
2. Přejděte na web [Azure Portal](https://portal.azure.com) a přihlaste se.

## <a name="2-create-a-workspace"></a>2. Vytvoření pracovního prostoru

Dalším krokem je vytvoření pracovního prostoru.

1. Na webu Azure Portal vyhledejte v seznamu služeb v Marketplace položku *Log Analytics* a potom vyberte **Log Analytics**.  
    ![Azure Portal](./media/log-analytics-get-started/log-analytics-portal.png)
2. Klikněte na **Vytvořit** a podle potřeby změňte hodnoty následujících položek:
   * **Pracovní prostor OMS** – zadejte název pracovního prostoru.
   * **Předplatné** – pokud pracujete s více předplatnými, vyberte to, které chcete přidružit k novému pracovnímu prostoru.
   * **Skupina prostředků**
   * **Umístění**
   * **Cenová úroveň**  
       ![Rychlé vytvoření](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Po kliknutí na **OK** se zobrazí seznam vašich pracovních prostorů.
4. Po výběru pracovního prostoru se na webu Azure Portal zobrazí jeho podrobnosti.       
    ![Podrobnosti o pracovním prostoru](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         

## <a name="3-add-solutions-and-solution-offerings"></a>3. Přidávání řešení a nabídek řešení

V další fázi přidejte řešení pro správu a nabídky řešení. Řešení pro správu jsou kolekce pravidel pro logiku, vizualizace a získávání dat, které poskytují metriky zaměřené na oblast konkrétního problému oblast. Nabídka řešení je sada řešení pro správu.

Přidání řešení do pracovního prostoru umožňuje službě Log Analytics shromažďovat různé druhy dat z počítačů připojených k vašemu pracovnímu prostoru s použitím agentů. Připojováním agentů se budeme zabývat později.

### <a name="to-add-solutions-and-solution-offerings"></a>Přidávání řešení a nabídek řešení

1. Na webu Azure Portal klikněte na **Nový** a pak do pole **Hledat na Marketplace** zadejte **Activity Log Analytics** a stiskněte klávesu ENTER.
2. V okně Vše vyberte **Activity Log Analytics** a pak klikněte na **Vytvořit**.  
    ![Activity Log Analytics](./media/log-analytics-get-started/activity-log-analytics.png)  
3. V okně *název řešení pro správu* vyberte pracovní prostor, který chcete přidružit k příslušnému řešení pro správu.
4. Klikněte na možnost **Vytvořit**.  
    ![pracovní prostor řešení](./media/log-analytics-get-started/solution-workspace.png)  
5. Opakováním kroků 1 až 4 přidejte následující položky:
    - Nabídka služby**Zabezpečení a dodržování předpisů** s řešeními pro posouzení antimalwaru a pro zabezpečení a audit.
    - Nabídka služby **Automatizace a řízení** služby s řešeními pro Automation Hybrid Worker, sledování změn a posouzení aktualizace systému (označuje se také jako správa aktualizací). Když přidáte nabídku řešení, musíte vytvořit účet Automation.  
        ![Účet Automation](./media/log-analytics-get-started/automation-account.png)  
6. Řešení pro správu, která jste přidali do pracovního prostoru, můžete zobrazit přechodem k možnosti **Log Analytics** > **Subscriptions** > ***název pracovního prostoru*** > **Přehled**. Zobrazí se dlaždice řešení pro správu, která jste přidali.  
    >[!NOTE]
    >Protože jsme k pracovnímu prostoru ještě nepřipojili žádné agenty, nezobrazí se žádná data pro řešení, která jste přidali.  

    ![dlaždice řešení bez dat](./media/log-analytics-get-started/solutions-no-data.png)

## <a name="4-create-a-vm-and-onboard-an-agent"></a>4. Vytvoření virtuálního počítače a připojení agenta

V dalším kroku vytvoříte v Azure jednoduchý virtuální počítač. Po vytvoření virtuálního počítače připojíte agenta OMS, který ho povolí. Povolením agenta se zahájí shromažďování dat z virtuálního počítače a odesílání data do služby Log Analytics.

### <a name="to-create-a-virtual-machine"></a>Vytvoření virtuálního počítače

- Postupujte podle pokynů v části [Vytvoření vašeho prvního virtuálního počítače s Windows na webu Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md) a spusťte nový virtuální počítač.

### <a name="connect-the-virtual-machine-to-log-analytics"></a>Připojení virtuálního počítače ke službě Log Analytics

- Postupujte podle pokynů v části [Připojení virtuálních počítačů Azure ke službě Log Analytics](log-analytics-azure-vm-extension.md) a připojte virtuální počítač ke službě Log Analytics prostřednictvím webu Azure Portal.

## <a name="5-view-and-act-on-data"></a>5. Zobrazování dat a práce s nimi

Dříve jste povolili řešení Activity Log Analytics a nabídky služeb pro zabezpečení a dodržování předpisů a pro automatizaci a řízení. V dalším kroku začneme s prohlížením dat shromažďovaných řešeními a výsledků v rámci prohledávání protokolů.

Začněte tím, že se podíváte na data zobrazená z řešení. Pak se podívejte na některá z dostupných prohledávání protokolů. Prohledávání protokolů umožňuje zjišťovat kombinace a korelace jakýchkoli dat o počítačích z více zdrojů v rámci prostředí. Další informace najdete v tématu [Prohledávání protokolů v Log Analytics](log-analytics-log-searches.md). Nakonec proveďte požadované akce s daty nalezenými prostřednictvím portálu OMS, který se nachází mimo web Azure Portal.

### <a name="to-view-antimalware-data"></a>Zobrazení antimalwarových dat

1. Na webu Azure Portal přejděte do části **Log Analytics** > ***váš pracovní prostor***.
2. V okně pro příslušný pracovní prostor v části **Obecné**, klikněte na **Přehled**.  
    ![Přehled](./media/log-analytics-get-started/overview.png)
3. Klikněte na dlaždici **Posouzení antimalwaru**. V tomto příkladu je vidět, že v jednom počítači je nainstalován program Windows Defender, ale jeho podpis je zastaralý.  
    ![Antimalware](./media/log-analytics-get-started/solution-antimalware.png)
4. V tomto příkladu můžete v části **Stav ochrany**, kliknutím na tlačítko **Zastaralý podpis** otevřít prohledávání protokolů a zobrazit podrobnosti o počítačích se zastaralými podpisy. V tomto příkladu si všimněte, že příslušný počítač má název *getstarted*. Kdyby existovalo více počítačů se zastaralými podpisy, všechny by se zobrazily ve výsledcích prohledávání protokolů.  
    ![Zastaralý antimalware](./media/log-analytics-get-started/antimalware-search.png)

### <a name="to-view-security-and-audit-data"></a>Zobrazení dat zabezpečení a auditu

1. V okně pro příslušný pracovní prostor v části **Obecné**, klikněte na **Přehled**.  
2. Klikněte na dlaždici **Zabezpečení a audit**. V tomto příkladu jsou vidět dva výrazné problémy: v jednom počítači chybí důležité aktualizace a ochrana jednoho počítače je nedostatečná.  
    ![Zabezpečení a audit](./media/log-analytics-get-started/security-audit.png)
3. V tomto příkladu v části **Významné problémy** kliknutím na **Počítače s chybějícími důležitými aktualizacemi** otevřete prohledávání protokolů a zobrazte podrobnosti o počítačích s chybějícími důležitými aktualizacemi. V tomto příkladu chybí jedna důležitá aktualizace a 63 dalších aktualizací.  
    ![Prohledávání protokolů zabezpečení a auditu](./media/log-analytics-get-started/security-audit-log-search.png)

### <a name="to-view-and-act-on-system-update-data"></a>Zobrazování dat o aktualizacích systému a práce s nimi

1. V okně pro příslušný pracovní prostor v části **Obecné**, klikněte na **Přehled**.  
2. Klikněte na dlaždici **Posouzení aktualizace systému**. V tomto příkladu je vidět jeden počítač se systémem Windows s názvem *getstarted*, který vyžaduje důležité aktualizace, a jeden, který vyžaduje aktualizace definic.  
    ![Aktualizace systému](./media/log-analytics-get-started/system-updates.png)
3. V tomto příkladu v části **Chybějící aktualizace** kliknutím na **Důležité aktualizace** otevřete prohledávání protokolů a zobrazte podrobnosti o počítačích s chybějícími důležitými aktualizacemi. V tomto příkladu chybí jedna důležitá aktualizace a jedna požadovaná aktualizace.  
    ![Prohledávání protokolů aktualizací systému](./media/log-analytics-get-started/system-updates-log-search.png)
4. Přejděte na web [Operations Management Suite](http://microsoft.com/oms) a přihlaste se pomocí účtu Azure. Po přihlášení si všimněte, že informace o řešení jsou podobné jako ty, které se zobrazují na webu Azure Portal.  
    ![Portál OMS](./media/log-analytics-get-started/oms-portal.png)
5. Klikněte na dlaždici **Správa aktualizací**.
6. Na řídicím panelu Správa aktualizací si všimněte, že informace o aktualizacích systému jsou podobné informacím o aktualizacích systému, které se zobrazily na webu Azure Portal. Dlaždice **Spravovat nasazení aktualizace** je však nová. Klikněte na dlaždici **Spravovat nasazení aktualizace**.  
    ![Dlaždice Správa aktualizací](./media/log-analytics-get-started/update-management.png)
7. Na stránce **Nasazení aktualizace** klikněte na **Přidat** a vytvořte položku *Hromadná postupná aktualizace*.  
    ![Nasazení aktualizace](./media/log-analytics-get-started/update-management-update-deployments.png)
8.  Na stránce **Nové nasazení aktualizace** zadejte název pro nasazení aktualizace, vyberte počítače, které chcete aktualizovat (v tomto příkladu *getstarted*), vyberte plán a pak klikněte na **Uložit**.  
    ![Nové nasazení](./media/log-analytics-get-started/new-deployment.png)  
    Po uložení nasazení aktualizace se zobrazí plánovaná aktualizace.  
    ![plánovaná aktualizace](./media/log-analytics-get-started/scheduled-update.png)  
    Po dokončení hromadné postupné aktualizace se zobrazí stav **Dokončeno**.
    ![dokončená aktualizace](./media/log-analytics-get-started/completed-update.png)
9. Po dokončení hromadné postupné aktualizace můžete zobrazit informace o tom, jestli byla nebo nebyla úspěšná, a podrobnosti o aktualizacích, které byly použity.

## <a name="after-evaluation"></a>Po vyzkoušení

V tomto kurzu jste nainstalovali agenta na virtuálním počítači a začali s prací. Kroky, které jste provedli, byly rychlé a jednoduché. Místní infrastruktura většiny velkých organizací a podniků je však složitá. Shromažďování dat z takových komplexních prostředí tedy vyžaduje více plánování a úsilí než tento kurz. V rámci informací v části Další kroky najdete odkazy na užitečné články.

Pracovní prostor, který jste vytvořili v tomto kurzu, můžete volitelně odebrat.

## <a name="next-steps"></a>Další kroky
* Informace o připojení [agentů systému Windows](log-analytics-windows-agents.md) ke službě Log Analytics.
* Informace o připojení [agentů nástroje Operations Manager](log-analytics-om-agents.md) ke službě Log Analytics.
* Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.
* Chcete-li zobrazit podrobné informace shromážděné řešeními, seznamte se s [vyhledáváním protokolů](log-analytics-log-searches.md).

