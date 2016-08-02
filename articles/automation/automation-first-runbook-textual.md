<properties
    pageTitle="Můj první runbook pracovního postupu PowerShellu v Azure Automation | Microsoft Azure"
    description="Kurz vás provede vytvořením, otestováním a publikováním jednoduchého textového runbooku, který využívá pracovní postup PowerShellu."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="powershell workflow, powershell workflow examples, workflow powershell"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/02/2016"
    ms.author="magoedte;bwren"/>

# Můj první runbook pracovního postupu PowerShellu

> [AZURE.SELECTOR] - [Grafické](automation-first-runbook-graphical.md) - [prostředí PowerShell](automation-first-runbook-textual-PowerShell.md) - [pracovní postup prostředí PowerShell](automation-first-runbook-textual.md)

V tomto kurzu se seznámíte s vytvořením [runbooku pracovního postupu PowerShellu](automation-runbook-types.md#powerShell-workflow-runbooks) ve službě Azure Automation. Začneme s jednoduchým runbookem, který společně otestujeme a publikujeme a současně vám vysvětlíme, jak sledovat stav úlohy runbooku. Potom runbook upravíme, aby skutečně spravoval prostředky Azure, v tomto případě virtuální počítač Azure. Nakonec runbook zkvalitníme přidáním parametrů.

## Požadavky

K dokončení tohoto kurzu potřebujete:

-   Předplatné Azure. Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).
-   [Účet Automation](automation-security-overview.md), abyste si mohli runbook podržet a mohli ověřovat prostředky Azure.  Tento účet musí mít oprávnění ke spuštění a zastavení virtuálního počítače.
-   Virtuální počítač Azure. Počítač zastavíme a spustíme, proto to nesmí být produkční počítač.

## Krok 1 – vytvoření nového runbooku

Začneme vytvořením jednoduchého runbooku, který zobrazí text *Hello World*.

1.  Na portálu Azure otevřete účet Automation.  
    Stránka účtu Automation nabízí rychlý přehled prostředků v tomto účtu. Už byste tam měli mít nějaké assety. Většina z nich jsou moduly, které jsou automaticky obsažené v novém účtu Automation. Také potřebujete asset přihlašovacích údajů, který je uvedený v [požadavcích](#prerequisites).
2.  Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.<br> ![Řízení runbooků](media/automation-first-runbook-textual/runbooks-control.png)
3.  Kliknutím na tlačítko **Přidat runbook** a potom na **Vytvořit nový runbook** vytvořte nový runbook.
4.  Dejte runbooku název *MyFirstRunbook-Workflow*.
5.  V tomto případě vytvoříme [runbook pracovního postupu PowerShellu](automation-runbook-types.md#powerShell-workflow-runbooks), proto jako **Typ runbooku** vyberte **Pracovní postup PowerShellu**.<br> ![Nový runbook](media/automation-first-runbook-textual/new-runbook.png)
6.  Kliknutím na **Vytvořit** vytvoříte runbook a otevřete textový editor.

## Krok 2 – přidání kódu do runbooku

Kód můžete buď zadat přímo do runbooku, nebo můžete vybrat rutiny, runbooky a assety z ovládacího prvku Knihovna a přidat je do runbooku spolu se všemi souvisejícími parametry. V tomto návodu budeme zapisovat přímo do runbooku.

1.  Náš runbook je teď prázdný a obsahuje jenom požadované klíčové slovo *workflow*, název runbooku a složené závorky, které uzavřou celý pracovní postup. 

    ```
    Workflow MyFirstRunbook-Workflow
    {
    }
    ```

2.  Napište text *Write-Output "Hello World".* do prostoru mezi složené závorky. 
   
    ```
    Workflow MyFirstRunbook-Workflow
    {
      Write-Output "Hello World"
    }
    ```

3.  Kliknutím na **Uložit** runbook uložte.<br> ![Uložení runbooku](media/automation-first-runbook-textual/runbook-edit-toolbar-save.png)

## Krok 3 – otestování runbooku

Před publikováním runbooku, které ho zpřístupní v produkčním prostředí, chceme runbook otestovat a ujistit se, že bude fungovat správně. Když runbook testujete, spustíte jeho  verzi **Koncept** a interaktivně zobrazíte jeho výsledek.

1.  Kliknutím na **Testovací podokno** otevřete testovací podokno.<br> ![Testovací podokno](media/automation-first-runbook-textual/runbook-edit-toolbar-test-pane.png)
2.  Kliknutím na **Spustit** spustíte test. Měla by to být jediná povolená možnost.
3.  Vytvoří se [úloha runbooku](automation-runbook-execution.md) a její stav se zobrazí.  
    Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
4.  Po dokončení úlohy runbooku se zobrazí jeho výstup. V našem případě bychom měli vidět text *Hello World*.<br> ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
5.  Zavřete testovací podokno a vraťte se na plátno.

## Krok 4 – publikování a spuštění runbooku

Právě vytvořený runbook je stále v režimu konceptu. Před jeho spuštěním v produkčním prostředí ho musíme publikovat. Když runbook publikujete, přepíšete vydanou verzi verzí v režimu konceptu. V našem případě zatím ještě publikovanou verzi nemáme, protože jsme runbook teprve vytvořili.

1.  Kliknutím na **Publikovat** runbook publikujte a po zobrazení výzvy klikněte na **Ano**.<br> ![Publikování](media/automation-first-runbook-textual/runbook-edit-toolbar-publish.png)
2.  Pokud se posunete doleva, abyste runbook viděli v podokně **Runbooky**, zobrazí se **Stav vytváření**, který má hodnotu **Publikováno**.
3.  Posuňte se zpět doprava, abyste viděli podokno **MyFirstRunbook-Workflow**.  
    Možnosti v horní části nám umožňují spuštění runbooku, naplánování jeho spuštění někdy v budoucnu nebo vytvoření [webhooku](automation-webhooks.md), který umožní spuštění prostřednictvím volání protokolu HTTP.
4.  My chceme runbook jenom spustit, takže klikněte na **Spustit** a po zobrazení výzvy na **Ano**.<br> ![Spuštění runbooku](media/automation-first-runbook-textual/runbook-toolbar-start.png)
5.  Podokno úlohy runbooku, který jsme právě vytvořili, je otevřené. Toto podokno můžeme zavřít, ale v tomto případě ho necháme otevřené, abychom mohli sledovat průběh úlohy.
6.  Stav úlohy se zobrazuje v **souhrnu úlohy** a odpovídá stavům, které jsme viděli při testování runbooku.<br> ![Souhrn úlohy](media/automation-first-runbook-textual/job-pane-summary.png)
7.  Když se jako stav runbooku zobrazí *Dokončeno*, klikněte na **Výstup**. Otevře se podokno Výstup a my uvidíme text *Hello World*.<br> ![Souhrn úlohy](media/automation-first-runbook-textual/job-pane-output.png)  
8.  Zavřete podokno Výstup.
9.  Klikněte na **Datové proudy** a otevřete podokno Datové proudy, které patří k úloze runbooku. Ve výstupu datového proudu byste měli vidět jenom text *Hello World*, ale můžou se zobrazit i jiné datové proudy z úlohy runbooku, například Podrobný nebo Chyba, pokud do nich runbook zapisuje.<br> ![Souhrn úlohy](media/automation-first-runbook-textual/job-pane-streams.png)
10. Zavřete podokno Datové proudy a podokno Úloha a vraťte se do podokna MyFirstRunbook.
11. Kliknutím na **Úlohy** otevřete podokno Úlohy, které patří k tomuto runbooku. Vypíšou se všechny úlohy, které tento runbook vytvořil. Tady bychom měli vidět jenom jednu úlohu, protože jsme ji spustili jenom jednou.<br> ![Úlohy](media/automation-first-runbook-textual/runbook-control-jobs.png)
12. Na tuto úlohu můžete kliknout a otevřít podokno Úloha, které jsme zobrazili při spuštění runbooku. Pomocí této možnosti se můžete vrátit v čase a zobrazit si podrobnosti libovolné úlohy, která byla pro konkrétní runbook vytvořena.

## Krok 5 – přidání ověřování ke správě prostředků Azure

Runbook jsme otestovali a publikovali, ale zatím nedělá nic užitečného. Chceme po něm, aby spravoval prostředky Azure. To nebude umět, pokud mu neumožníme ověřování pomocí přihlašovacích údajů, které jsou uvedené v [požadavcích](#prerequisites). Provedeme to pomocí rutiny **Add-AzureRMAccount**.

1.  Kliknutím na **Upravit** v podokně MyFirstRunbook-Workflow otevřete textový editor.<br> ![Úprava runbooku](media/automation-first-runbook-textual/runbook-toolbar-edit.png)
2.  Řádek **Write-Output** už nepotřebujeme, takže ho teď můžete odstranit.
3.  Umístěte kurzor na prázdný řádek mezi složenými závorkami.
4.  Zadejte nebo zkopírujte a vložte následující kód, který bude zpracovávat ověřování pomocí vašeho účtu Automation Spustit jako:

    ```
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```

5.  Klikněte na **testovací podokno**, abychom mohli runbook otestovat.
6.  Kliknutím na **Spustit** spustíte test. Po jeho dokončení byste měli obdržet výstup, který zobrazuje základní informace z vašeho účtu. Tím se potvrdí platnost přihlašovacích údajů. <br> ![Ověření](media/automation-first-runbook-textual/runbook-auth-results.png)

## Krok 6 – přidání kódu pro spuštění virtuálního počítače

Teď, když runbook umí ověřit naše předplatné Azure, můžeme začít spravovat prostředky. Přidáme příkaz ke spuštění virtuálního počítače. V rámci vašeho předplatného Azure můžete vybrat jakýkoli virtuální počítač, ale prozatím jeho název pevně zakódujeme do rutiny.

1.  Po *Add-AzureRmAccount* zadejte *Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'NameofResourceGroup'* a nezapomeňte zadat název a název skupiny prostředků virtuálního počítače, který chcete spustit.  

    ```
    workflow MyFirstRunbook-Workflow
    {
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
     Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
    }
    ```

2.  Uložte runbook a klikněte na **testovací podokno**, abychom mohli runbook otestovat.
3.  Kliknutím na **Spustit** spustíte test. Po jeho dokončení zkontrolujte, jestli se virtuální počítač spustil.

## Krok 7 – přidání vstupního parametru do runbooku

Náš runbook aktuálně spouští virtuální počítač, který jsme do něj pevně zakódovali, ale bylo by užitečnější, pokud bychom mohli virtuální počítač zadat při spuštění runbooku. Teď do runbooku přidáme vstupní parametry, které tuto funkci zajistí.

1.  Přidejte do runbooku parametry pro *VMName* a *ResourceGroupName* a použijte tyto proměnné s rutinou **Start-AzureRmVM**, jak vidíte na níže uvedeném příkladu. 

    ```
    workflow MyFirstRunbook-Workflow
    {
       Param(
        [string]$VMName,
        [string]$ResourceGroupName
       )  
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
     Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
    }
    ```

2.  Uložte runbook a otevřete testovací podokno. Všimněte si, že teď můžete zadat hodnoty dvou vstupních proměnných, které budete v testu používat.
3.  Zavřete testovací podokno.
4.  Kliknutím na **Publikovat** publikujte novou verzi runbooku.
5.  Zastavte virtuální počítač, který jste v předchozím kroku spustili.
6.  Kliknutím na **Spustit** spustíte runbook. Zadejte **VMName** a **ResourceGroupName** pro virtuální počítač, který chcete spustit.<br> ![Spuštění runbooku](media/automation-first-runbook-textual/automation-pass-params.png)

7.  Po dokončení runbooku zkontrolujte, jestli se virtuální počítač spustil.

## Další kroky

-  První kroky s grafickými runbooky najdete v článku [Můj první grafický runbook](automation-first-runbook-graphical.md).
-   První kroky s powershellovými runbooky najdete v článku [Můj první powershellový runbook](automation-first-runbook-textual-powershell.md).
-  Další informace o typech runbooků, jejich výhodách a omezeních najdete v článku [Typy runbooků ve službě Azure Automation](automation-runbook-types.md).
-   Další informace o funkci podpory powershellových skriptů najdete v článku [Nativní podpora powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)



<!--HONumber=Jun16_HO2-->


