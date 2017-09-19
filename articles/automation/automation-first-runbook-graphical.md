---
title: "Můj první grafický runbook v Azure Automation | Dokumentace Microsoftu"
description: "Kurz vás provede vytvořením, otestováním a publikováním jednoduchého grafického runbooku."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "runbook, šablona sady runbook, automatizace sady runbook, runbook azure"
ms.assetid: dcb88f19-ed2b-4372-9724-6625cd287c8a
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/17/2017
ms.author: magoedte;bwren
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: d06052099acbb5544581ef3e8e2007dd751a7544
ms.contentlocale: cs-cz
ms.lasthandoff: 09/13/2017

---
# <a name="my-first-graphical-runbook"></a>Můj první grafický runbook

> [!div class="op_single_selector"]
> * [Grafický](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [Pracovní postup PowerShellu](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 

V tomto kurzu se seznámíte s vytvořením [grafického runbooku](automation-runbook-types.md#graphical-runbooks) ve službě Azure Automation.  Začneme s jednoduchým runbookem pro testování a publikování, zatímco budeme vysvětlovat, jak sledovat stav úlohy runbooku.  Potom runbook upravíme, aby skutečně spravoval prostředky Azure, v tomto případě virtuální počítač Azure.  Na závěr kurzu runbook rozšíříme přidáním parametrů runbooku a podmíněných propojení.

## <a name="prerequisites"></a>Požadavky
Pro absolvování tohoto kurzu potřebujete:

* Předplatné Azure. Pokud ještě žádné nemáte, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Účet Automation](automation-offering-get-started.md), abyste si mohli runbook podržet a mohli ověřovat prostředky Azure.  Tento účet musí mít oprávnění ke spuštění a zastavení virtuálního počítače.
* Virtuální počítač Azure. Počítač zastavíme a spustíme, proto to nesmí být produkční virtuální počítač.

## <a name="step-1---create-runbook"></a>Krok 1 – vytvoření runbooku
Začneme vytvořením jednoduchého runbooku, který zobrazí text *Hello World*.

1. Na webu Azure Portal otevřete účet Automation.  
   Stránka účtu Automation nabízí rychlý přehled prostředků v tomto účtu.  Už byste tam měli mít nějaké assety.  Většina z nich jsou moduly, které jsou automaticky obsažené v novém účtu Automation.  Také potřebujete asset přihlašovacích údajů, který je uvedený v [požadavcích](#prerequisites).
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.<br> ![Řízení runbooků](media/automation-first-runbook-graphical/runbooks-resources-tile.png)
3. Kliknutím na tlačítko **Přidat runbook** a potom na **Vytvořit nový runbook** vytvořte nový runbook.
4. Dejte runbooku název *MyFirstRunbook-Graphical*.
5. V tomto případě vytvoříme [grafický runbook](automation-graphical-authoring-intro.md), proto jako **Typ runbooku** vyberte **Grafický**.<br> ![Nový runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Kliknutím na **Vytvořit** vytvoříte runbook a otevřete grafický editor.

## <a name="step-2---add-activities-to-the-runbook"></a>Krok 2 – přidání aktivit do runbooku
Ovládací prvek Knihovna na levé straně editoru vám umožní vybrat aktivity, které chcete do runbooku přidat.  Přidáme rutinu **Write-Output**, která zajistí textový výstup z runbooku.

1. V ovládacím prvku Knihovna klikněte do textového pole hledání a zadejte text **Write-Output**.  Výsledky hledání se zobrazí níže. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
2. Přejděte do dolní části seznamu.  Můžete buď kliknout pravým tlačítkem na **Write-Output** a vybrat **Přidat na plátno**, nebo kliknout na tři tečky vedle rutiny a vybrat **Přidat na plátno**.
3. Klikněte na aktivitu **Write-Output** na plátně.  Tím otevřete okno pro kontrolu konfigurace, které umožňuje aktivitu konfigurovat.
4. **Popisek** obsahuje ve výchozím nastavení název rutiny, ale můžeme ho změnit na něco popisnějšího. Změňte ho na *Textový výstup Hello World*.
5. Klikněte na tlačítko **Parametry** a zadejte hodnoty parametru rutiny.  
   Některé rutiny obsahují více sad parametrů, proto musíte vybrat, kterou chcete používat. V tomto případě **Write-Output** obsahuje jenom jednu sadu parametrů, takže není nutné žádnou vybírat. <br> ![Vlastnosti Write-Output](media/automation-first-runbook-graphical/write-output-properties-b.png)
6. Vyberte parametr **InputObject**.  V tomto parametru budeme zadávat text, který se odešle do výstupního datového proudu.
7. V rozevíracím seznamu **Zdroj dat** vyberte **Powershellový výraz**.  Rozevírací seznam **Zdroj dat** obsahuje různé zdroje, které můžete použít k naplnění hodnoty parametru.  
   Můžete použít výstup z těchto zdrojů, například další aktivitu, asset Automation nebo powershellový výraz.  V tomto případě chceme jako výstup jenom text *Hello World*. Můžeme použít powershellový výraz a zadat řetězec.
8. Do pole **Výraz** zadejte *"Hello World"* a potom dvakrát klikněte na tlačítko **OK**, a vraťte se tak na plátno.<br> ![Powershellový výraz](media/automation-first-runbook-graphical/expression-hello-world.png)
9. Kliknutím na **Uložit** runbook uložte.<br> ![Uložení runbooku](media/automation-first-runbook-graphical/runbook-toolbar-save-revised20165.png)

## <a name="step-3---test-the-runbook"></a>Krok 3 – otestování runbooku
Před publikováním runbooku, které ho zpřístupní v produkčním prostředí, chceme runbook otestovat a ujistit se, že bude fungovat správně.  Když runbook testujete, spustíte jeho  verzi **Koncept** a interaktivně zobrazíte jeho výsledek.

1. Kliknutím na **Testovací podokno** otevřete okno Test.<br> ![Testovací podokno](media/automation-first-runbook-graphical/runbook-toolbar-test-revised20165.png)
2. Kliknutím na **Spustit** spustíte test.  Měla by to být jediná povolená možnost.
3. Vytvoří se [úloha runbooku](automation-runbook-execution.md) a její stav se zobrazí v podokně.  
   Počáteční stav úlohy bude *Ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu.  Když pracovní proces úlohu převezme, změní se stav na *Spouštění*, a když je runbook skutečně spuštěn, změní se stav na *Spuštěno*.  
4. Po dokončení úlohy runbooku se zobrazí jeho výstup. V našem případě bychom měli vidět text *Hello World*.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
5. Zavřením okna Test se vraťte na plátno.

## <a name="step-4---publish-and-start-the-runbook"></a>Krok 4 – publikování a spuštění runbooku
Vytvořený runbook je stále v režimu konceptu. Před jeho spuštěním v produkčním prostředí ho musíme publikovat.  Když runbook publikujete, přepíšete vydanou verzi verzí v režimu konceptu.  V našem případě zatím ještě publikovanou verzi nemáme, protože jsme runbook teprve vytvořili.

1. Kliknutím na **Publikovat** runbook publikujte a po zobrazení výzvy klikněte na **Ano**.<br> ![Publikování](media/automation-first-runbook-graphical/runbook-toolbar-publish-revised20166.png)
2. Pokud se posunete doleva, abyste runbook viděli v okně **Runbooky**, zobrazí se **Stav vytváření**, který má hodnotu **Publikováno**.
3. Posuňte se zpět doprava, abyste viděli okno **MyFirstRunbook**.  
   Možnosti v horní části nám umožňují spuštění runbooku, naplánování jeho spuštění někdy v budoucnu nebo vytvoření [webhooku](automation-webhooks.md), který umožní spuštění prostřednictvím volání protokolu HTTP.
4. My chceme runbook jenom spustit, takže klikněte na **Spustit** a po zobrazení výzvy na **Ano**.<br> ![Spuštění runbooku](media/automation-first-runbook-graphical/runbook-controls-start-revised20165.png)
5. Otevře se okno úlohy runbooku, který jsme vytvořili.  Toto okno můžeme zavřít, ale v tomto případě ho necháme otevřené, abychom mohli sledovat průběh úlohy.
6. Stav úlohy se zobrazuje v **souhrnu úlohy** a odpovídá stavům, které jsme viděli při testování runbooku.<br> ![Souhrn úlohy](media/automation-first-runbook-graphical/runbook-job-summary.png)
7. Když se jako stav runbooku zobrazí *Dokončeno*, klikněte na **Výstup**. Otevře se okno **Výstup** a v podokně uvidíme náš text *Hello, World*.<br> ![Souhrn úlohy](media/automation-first-runbook-graphical/runbook-job-output.png)  
8. Zavřete okno Výstup.
9. Klikněte na **Všechny protokoly** a otevřete okno Datové proudy, které patří k úloze runbooku.  Ve výstupu datového proudu byste měli vidět jenom text *Hello World*, ale můžou se zobrazit i jiné datové proudy z úlohy runbooku, například Podrobný nebo Chyba, pokud do nich runbook zapisuje.<br> ![Souhrn úlohy](media/automation-first-runbook-graphical/runbook-job-AllLogs.png)
10. Zavřete okno Všechny protokoly a okno Úloha a vraťte se do okna MyFirstRunbook.
11. Kliknutím na **Úlohy** otevřete okno Úloha, které patří k tomuto runbooku.  Vypíšou se všechny úlohy, které tento runbook vytvořil. Tady bychom měli vidět jenom jednu úlohu, protože jsme ji spustili jenom jednou.<br> ![Úlohy](media/automation-first-runbook-graphical/runbook-control-jobs.png)
12. Na tuto úlohu můžete kliknout a otevřít podokno Úloha, které jsme zobrazili při spuštění runbooku.  Pomocí této možnosti se můžete vrátit v čase a zobrazit si podrobnosti libovolné úlohy, která byla pro konkrétní runbook vytvořena.

## <a name="step-5---create-variable-assets"></a>Krok 5 – vytvoření proměnných assetů
Runbook jsme otestovali a publikovali, ale zatím nedělá nic užitečného. Chceme po něm, aby spravoval prostředky Azure.  Než runbook nakonfigurujeme pro ověřování, vytvoříme proměnnou, která bude uchovávat ID předplatného a bude na něj odkazovat poté, co v kroku 6 níže nastavíme aktivitu k ověření.  Zahrnutí odkazu na kontext předplatného usnadňuje práci s několika předplatnými.  Než budete pokračovat, zkopírujte si ID předplatného z možnosti Předplatná, která se nachází v navigačním podokně.  

1. V okně Účty služby Automation klikněte na dlaždici **Assety** a otevřete okno **Assety**.
2. V okně Assety klikněte na dlaždici **Proměnné**.
3. V okně Proměnné klikněte na **Přidat proměnnou**.<br>![Proměnná služby Automation](media/automation-first-runbook-graphical/create-new-subscriptionid-variable.png)
4. V okně Nová proměnná zadejte do pole **Název** text **AzureSubscriptionId** a do pole **Hodnota** zadejte ID předplatného.  Ponechte *řetězec* možnosti **Typ** a výchozí hodnotu možnosti **Šifrování**.  
5. Kliknutím na **Vytvořit** vytvořte proměnnou.  

## <a name="step-6---add-authentication-to-manage-azure-resources"></a>Krok 6 – přidání ověřování ke správě prostředků Azure
Teď, když máme proměnnou, do které se uloží naše ID předplatného, můžeme runbook nakonfigurovat k ověřování pomocí přihlašovacích údajů Spustit jako, které jsou uvedené v [požadavcích](#prerequisites).  Provedeme to přidáním **assetu** připojení Spustit v Azure jako a přidáním rutiny **Add-AzureRMAccount** na plátno.  

1. Kliknutím na **Upravit** v okně MyFirstRunbook otevřete grafický editor.<br> ![Úprava runbooku](media/automation-first-runbook-graphical/runbook-controls-edit-revised20165.png)
2. **Textový výstup Hello World** už nepotřebujeme, takže na něj klikněte pravým tlačítkem a vyberte **Odstranit**.
3. V ovládacím prvku Knihovna rozbalte **Připojení** a výběrem možnosti **Přidat na plátno** přidejte **AzureRunAsConnection** na plátno.
4. Na plátně vyberte **AzureRunAsConnection** a v podokně ovládacího prvku Konfigurace zadejte do textového pole **Popisek** text **Get Run As Connection**.  Toto je připojení
5. V ovládacím prvku Knihovna zadejte do textového pole hledání text **Add-AzureRmAccount**.
6. Přidejte **Add-AzureRmAccount** na plátno.<br> ![Add-AzureRMAccount](media/automation-first-runbook-graphical/search-powershell-cmdlet-addazurermaccount.png)
7. Najeďte ukazatelem myši nad položku **Get Run As Connection**, dokud se v dolní části obrazce nezobrazí kruh. Klikněte na kruh a přetáhněte šipku na **Add-AzureRmAccount**.  Šipka, kterou jste vytvořili, je *odkazem*.  Runbook se spustí pomocí aktivity **Get Run As Connection**. Potom spusťte rutinu **Add-AzureRmAccount**.<br> ![Vytvoření propojení mezi aktivitami](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
8. Na plátně vyberte **Add-AzureRmAccount** a v podokně ovládacího prvku Konfigurace zadejte do textového pole **Popisek** text **Přihlášení k Azure**.
9. Po kliknutí na **Parametry** se zobrazí okno Konfigurace parametru aktivity.
10. **Add-AzureRmAccount** obsahuje několik sad parametrů, takže před zadáním hodnot parametru musíme nejdřív jednu sadu vybrat.  Klikněte na položku **Sada parametrů** a potom vyberte sadu parametrů **ServicePrincipalCertificatewithSubscriptionId**.
11. Po výběru sady parametrů se parametry zobrazí v okně Konfigurace parametru aktivity.  Klikněte na **APPLICATIONID**.<br> ![Přidání parametrů účtu Azure RM](media/automation-first-runbook-graphical/add-azurermaccount-params.png)
12. V okně Hodnota parametru v části **Zdroj dat** vyberte **Výstup aktivity**, v seznamu vyberte **Get Run As Connection**, do textového pole **Cesta pole** zadejte **ApplicationId** a potom klikněte na tlačítko **OK**.  Název vlastnosti pro cestu pole zadáváme kvůli tomu, že výstupem aktivity je objekt s více vlastnostmi.
13. Klikněte na **CERTIFICATETHUMBPRINT**, v okně Hodnota parametru v části **Zdroj dat** vyberte **Výstup aktivity**.  V seznamu vyberte **Get Run As Connection**, do textového pole **Cesta pole** zadejte **CertificateThumbrprint**. Potom klikněte na **OK**.
14. Klikněte na **SERVICEPRINCIPAL**, v okně Hodnota parametru v části **Zdroj dat** vyberte **ConstantValue**, klikněte na možnost **Pravda** a potom klikněte na tlačítko **OK**.
15. Klikněte na **TENANTID**, v okně Hodnota parametru v části **Zdroj dat** vyberte **Výstup aktivity**.  V seznamu vyberte **Get Run As Connection**, do textového pole **Cesta pole** zadejte **TenantId** a potom dvakrát klikněte na tlačítko **OK**.  
16. V ovládacím prvku Knihovna zadejte do textového pole hledání text **Set-AzureRmContext**.
17. Přidejte **Set-AzureRmContext** na plátno.
18. Na plátně vyberte **Set-AzureRmContext** a v podokně ovládacího prvku Konfigurace zadejte do textového pole **Popisek** text **Zadat ID předplatného**.
19. Po kliknutí na **Parametry** se zobrazí okno Konfigurace parametru aktivity.
20. **Set-AzureRmContext** obsahuje několik sad parametrů, takže před zadáním hodnot parametru musíme nejdřív jednu sadu vybrat.  Klikněte na **Sada parametrů** a potom vyberte sadu **SubscriptionId**.  
21. Po výběru sady parametrů se parametry zobrazí v okně Konfigurace parametru aktivity.  Klikněte na **ID předplatného**.
22. V okně Hodnota parametru v části **Zdroj dat** vyberte **Proměnný asset**, v seznamu vyberte **AzureSubscriptionId** a potom dvakrát klikněte na tlačítko **OK**.   
23. Pozastavte ukazatel myši nad možností **Přihlášení k Azure**, dokud se v dolní části obrazce nezobrazí kruh. Klikněte na kruh a přetáhněte šipku na **Zadat ID předplatného**.

Váš runbook by měl v tuto chvíli vypadat následovně: <br>![Konfigurace ověření runbooku](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="step-7---add-activity-to-start-a-virtual-machine"></a>Krok 7 – přidání aktivity pro spuštění virtuálního počítače
Tady přidáme aktivitu **Start-AzureRmVM**, která spustí virtuální počítač.  V rámci vašeho předplatného Azure můžete vybrat jakýkoli virtuální počítač, ale prozatím jeho název pevně zakódujete do rutiny.

1. V ovládacím prvku Knihovna zadejte do textového pole hledání text **Start-AzureRm**.
2. Přidejte aktivitu **Start-AzureRmVM** na plátno a potom na ni klikněte a přetáhněte ji pod **Zadat ID předplatného**.
3. Pozastavte ukazatel myši nad možností **Zadat ID předplatného**, dokud se v dolní části obrazce nezobrazí kruh.  Klikněte na kruh a přetáhněte šipku na **Start-AzureRmVM**.
4. Vyberte **Start-AzureRmVM**.  Pokud chcete zobrazit sady, které patří k **Start-AzureRmVM**, klikněte na **Parametry** a potom na **Sada parametrů**.  Vyberte sadu parametrů **ResourceGroupNameParameterSetName**. Všimněte si, že vedle **ResourceGroupName** a **Název** se zobrazuje vykřičník.  To znamená, že tyto parametry jsou povinné.  Všimněte si také, že oba očekávají řetězcové hodnoty.
5. Vyberte **Název**.  V části **Zdroj dat** vyberte **Powershellový výraz** a zadejte název virtuálního počítače (v uvozovkách), který pomocí tohoto runbooku spustíme.  Klikněte na **OK**.<br>![Hodnota parametru názvu Start-AzureRmVM](media/automation-first-runbook-graphical/runbook-startvm-nameparameter.png)
6. Vyberte **ResourceGroupName**. V části **Zdroj dat** použijte **Powershellový výraz** a zadejte název skupiny prostředků (v uvozovkách).  Klikněte na tlačítko **OK**.<br> ![Parametry Start-AzureRmVM](media/automation-first-runbook-graphical/startazurermvm-params.png)
7. Klikněte na testovací podokno, abychom mohli runbook otestovat.
8. Kliknutím na **Spustit** spustíte test.  Po jeho dokončení zkontrolujte, jestli se virtuální počítač spustil.

Váš runbook by měl v tuto chvíli vypadat následovně: <br>![Konfigurace ověření runbooku](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="step-8---add-additional-input-parameters-to-the-runbook"></a>Krok 8 – přidání dalších vstupních parametrů do runbooku
Náš runbook nyní spouští virtuální počítač ve skupině prostředků, kterou jsme určili v rutině **Start-AzureRmVM**.  Náš runbook by mohl být užitečnější, pokud bychom mohli obojí zadat při spuštění runbooku.  Teď do runbooku přidáme vstupní parametry, které tuto funkci zajistí.

1. Kliknutím na **Upravit** v podokně **MyFirstRunbook** otevřete grafický editor.
2. Kliknutím na **Vstup a výstup** a potom na **Přidat vstup** otevřete podokno Vstupní parametr runbooku.<br> ![Vstup a výstup z runbooku](media/automation-first-runbook-graphical/runbook-toolbar-InputandOutput-revised20165.png)
3. V části **Název** zadejte *VMName*.  U možnosti **Typ** nechte *řetězec*, ale možnost **Povinné** změňte na *Ano*.  Klikněte na tlačítko **OK**.
4. Vytvořte druhý povinný vstupní parametr s názvem *ResourceGroupName* a potom kliknutím na tlačítko **OK** zavřete podokno **Vstup a výstup**.<br> ![Vstupní parametry runbooku](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
5. Vyberte aktivitu **Start-AzureRmVM** a potom klikněte na **Parametry**.
6. Změňte **Zdroj dat** možnosti **Název** na **Vstup z runbooku** a potom vyberte **VMName**.<br>
7. Změňte **Zdroj dat** možnosti **ResourceGroupName** na **Vstup z runbooku** a potom vyberte **ResourceGroupName**.<br> ![Parametry Start-AzureVM](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
8. Uložte runbook a otevřete testovací podokno.  Všimněte si, že teď můžete zadat hodnoty dvou vstupních proměnných, které v testu používáte.
9. Zavřete testovací podokno.
10. Kliknutím na **Publikovat** publikujte novou verzi runbooku.
11. Zastavte virtuální počítač, který jste v předchozím kroku spustili.
12. Kliknutím na **Spustit** spustíte runbook.  Zadejte **VMName** a **ResourceGroupName** pro virtuální počítač, který chcete spustit.<br> ![Spuštění runbooku](media/automation-first-runbook-graphical/runbook-start-inputparams.png)
13. Po dokončení runbooku zkontrolujte, jestli se virtuální počítač spustil.

## <a name="step-9---create-a-conditional-link"></a>Krok 9 – vytvoření podmíněného propojení
Teď runbook upravíme, aby se pokusil virtuální počítač spustit jenom v případě, že ještě není spuštěný.  To provedete tak, že do runbooku přidáte rutinu **Get-AzureRmVM**, která získá stav úrovně instance virtuálního počítače. Potom přidáte modul s kódem pracovního postupu PowerShellu s názvem **Get Status** s fragmentem kódu PowerShellu, který bude zjišťovat, jestli je virtuální počítač ve spuštěném nebo zastaveném stavu.  Podmíněné propojení z modulu **Get Status** spustí rutinu **Start-AzureRmVM** jenom v případě, že je počítač v zastaveném stavu.  Nakonec pomocí rutiny PowerShellu Write-Output odešleme výstup v podobě zprávy, která vás bude informovat o tom, jestli byl virtuální počítač úspěšně spuštěn, anebo ne.

1. Otevřete **MyFirstRunbook** v grafickém editoru.
2. Kliknutím na propojení a stisknutím klávesy *Delete* odebere propojení mezi **Zadat ID předplatného** a **Start-AzureRmVM**.
3. V ovládacím prvku Knihovna zadejte do textového pole hledání text **Get-AzureRm**.
4. Přidejte **Get-AzureRmVM** na plátno.
5. Pokud chcete zobrazit sady, které patří ke **Get-AzureRmVM**, vyberte **Get-AzureRmVM** a potom **Sada parametrů**.  Vyberte sadu parametrů **GetVirtualMachineInResourceGroupNameParamSet**.  Všimněte si, že vedle **ResourceGroupName** a **Název** se zobrazuje vykřičník.  To znamená, že tyto parametry jsou povinné.  Všimněte si také, že oba očekávají řetězcové hodnoty.
6. V části **Zdroj dat** u možnosti **Název** vyberte **Vstup z runbooku** a potom vyberte **VMName**.  Klikněte na tlačítko **OK**.
7. V části **Zdroj dat** u možnosti **ResourceGroupName** vyberte **Vstup z runbooku** a potom vyberte **ResourceGroupName**.  Klikněte na **OK**.
8. V části **Zdroj dat** u možnosti **Stav** vyberte **Konstantní hodnota** a potom klikněte na **Pravda**.  Klikněte na **OK**.  
9. Propojte **Zadat ID předplatného** s **Get-AzureRmVM**.
10. V ovládacím prvku Knihovna rozbalte **Ovládací prvek runbooku** a přidejte **Kód** na plátno.  
11. Propojte **Get-AzureRmVM** s možností **Kód**.  
12. Klikněte na **Kód** a v podokně Konfigurace změňte popisek na **Získání stavu**.
13. Vyberte parametr **Kód**, zobrazí se okno **Editor kódu**.  
14. Do editoru kódu vložte následující fragment kódu:
    
     ```
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```
15. Propojte modul **Získání stavu** se **Start-AzureRmVM**.<br> ![Runbook s modulem kódu](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
16. Vyberte propojení a v podokně Konfigurace změňte možnost **Použít podmínku** na **Ano**.   Všimněte si, že propojení se změní na přerušovanou čáru, která označuje, že cílová aktivita se spustí jenom v případě, že se podmínka přeloží na hodnotu pravda.  
17. V případě **výrazu podmínky** zadejte text *$ActivityOutput['Get Status'] -eq "Stopped"*.  Rutina **Start-AzureRmVM** se spustí jenom v případě, že je virtuální počítač zastavený.
18. V ovládacím prvku Knihovna rozbalte položku **Rutiny** a potom **Microsoft.PowerShell.Utility**.
19. Přidejte **Write-Output** dvakrát na plátno.<br> ![Runbook s aktivitou Write-Output](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
20. V prvním ovládacím prvku **Write-Output** klikněte na **Parametry** a změňte hodnotu **Popisek** na *Oznámit spuštění virtuálního počítače*.
21. U položky **InputObject** změňte**Zdroj dat** na **Powershellový výraz** a zadejte výraz *"$VMName successfully started."*.
22. V druhém ovládacím prvku **Write-Output** klikněte na **Parametry** a změňte hodnotu **Popisek** na *Oznámit neúspěšné spuštění virtuálního počítače*.
23. U položky **InputObject** změňte **Zdroj dat** na **Powershellový výraz** a zadejte výraz *"$VMName could not start."*.
24. Propojte **Start-AzureRmVM** s **Oznámit spuštění virtuálního počítače** a **Oznámit neúspěšné spuštění virtuálního počítače**.
25. Vyberte propojení s **Oznámit spuštění virtuálního počítače** a změňte možnost **Použít podmínku** na **Pravda**.
26. V případě **výrazu podmínky** zadejte *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -eq $true*.  Tato rutina Write-Output se teď spustí jenom v případě, že je virtuální počítač úspěšně spuštěn.
27. Vyberte propojení na **Oznámit neúspěšné spuštění virtuálního počítače** a změňte možnost **Použít podmínku** na **Pravda**.
28. V případě **výrazu podmínky** zadejte *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -ne $true*.  Tato rutina Write-Output se teď spustí jenom v případě, že virtuální počítač není úspěšně spuštěn.
29. Uložte runbook a otevřete testovací podokno.
30. Spusťte runbook se zastaveným virtuálním počítačem  a virtuální počítač by se měl spustit.

## <a name="next-steps"></a>Další kroky
* Další informace o vytváření grafického obsahu najdete v článku [Vytváření grafického obsahu v Azure Automation](automation-graphical-authoring-intro.md).
* První kroky s powershellovými runbooky najdete v článku [Můj první powershellový runbook](automation-first-runbook-textual-powershell.md).
* První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).


