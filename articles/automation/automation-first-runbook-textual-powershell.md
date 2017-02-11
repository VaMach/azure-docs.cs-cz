---
title: "Můj první powershellový runbook v Azure Automation | Dokumentace Microsoftu"
description: "Kurz vás provede vytvořením, otestováním a publikováním jednoduchého powershellového runbooku."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "azure powershell, kurz k powershellovému scriptu, automatizace powershellu"
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/19/2016
ms.author: magoedte;sngun
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 0f8c103b2ce0fcd4596d21af33b0a13e03dad54a


---
# <a name="my-first-powershell-runbook"></a>Můj první powershellový runbook

> [!div class="op_single_selector"]
> * [Grafický](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [Pracovní postup PowerShellu](automation-first-runbook-textual.md)
> 
> 

V tomto kurzu se seznámíte s vytvořením [powershellového runbooku](automation-runbook-types.md#powershell-runbooks) ve službě Azure Automation. Začneme s jednoduchým runbookem, který společně otestujeme a publikujeme a současně vám vysvětlíme, jak sledovat stav úlohy runbooku. Potom runbook upravíme, aby skutečně spravoval prostředky Azure, v tomto případě virtuální počítač Azure. Nakonec runbook zkvalitníme přidáním parametrů.

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto kurzu potřebujete:

* Předplatné Azure. Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).
* [Účet Automation](automation-security-overview.md), abyste si mohli runbook podržet a mohli ověřovat prostředky Azure.  Tento účet musí mít oprávnění ke spuštění a zastavení virtuálního počítače.
* Virtuální počítač Azure. Počítač zastavíme a spustíme, proto to nesmí být produkční počítač.

## <a name="step-1---create-new-runbook"></a>Krok 1 – vytvoření nového runbooku
Začneme vytvořením jednoduchého runbooku, který zobrazí text *Hello World*.

1. Na portálu Azure otevřete účet Automation.  
   Stránka účtu Automation nabízí rychlý přehled prostředků v tomto účtu. Už byste tam měli mít nějaké assety. Většina z nich jsou moduly, které jsou automaticky obsažené v novém účtu Automation. Také potřebujete asset přihlašovacích údajů, který je uvedený v [požadavcích](#prerequisites).
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.  
   ![RunbooksControl](media/automation-first-runbook-textual-powershell/automation-runbooks-control.png)  
3. Kliknutím na tlačítko **Přidat runbook** a potom na **Vytvořit nový runbook** vytvořte nový runbook.
4. Dejte runbooku název *MyFirstRunbook-PowerShell*.
5. V tomto případě vytvoříme [powershellový runbook](automation-runbook-types.md#powershell-runbooks), proto jako **Typ runbooku** vyberte **PowerShell**.  
   ![Typ runbooku](media/automation-first-runbook-textual-powershell/automation-runbook-type.png)  
6. Kliknutím na **Vytvořit** vytvoříte runbook a otevřete textový editor.

## <a name="step-2---add-code-to-the-runbook"></a>Krok 2 – přidání kódu do runbooku
Kód můžete buď zadat přímo do runbooku, nebo můžete vybrat rutiny, runbooky a assety z ovládacího prvku Knihovna a přidat je do runbooku spolu se všemi souvisejícími parametry. V tomto návodu budeme zapisovat přímo do runbooku.

1. Náš runbook je aktuálně prázdný, zadejte text *Write-Output "Hello World".*.  
   ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  
2. Kliknutím na **Uložit** runbook uložte.  
   ![Tlačítko Uložit](media/automation-first-runbook-textual-powershell/automation-save-button.png)  

## <a name="step-3---test-the-runbook"></a>Krok 3 – otestování runbooku
Před publikováním runbooku, které ho zpřístupní v produkčním prostředí, chceme runbook otestovat a ujistit se, že bude fungovat správně. Když runbook testujete, spustíte jeho  verzi **Koncept** a interaktivně zobrazíte jeho výsledek.

1. Kliknutím na **Testovací podokno** otevřete testovací podokno.  
   ![Testovací podokno](media/automation-first-runbook-textual-powershell/automation-testpane.png)  
2. Kliknutím na **Spustit** spustíte test. Měla by to být jediná povolená možnost.
3. Vytvoří se [úloha runbooku](automation-runbook-execution.md) a její stav se zobrazí.  
   Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
4. Po dokončení úlohy runbooku se zobrazí jeho výstup. V našem případě bychom měli vidět text *Hello World*.  
   ![Výstup testovacího podokna](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  
5. Zavřete testovací podokno a vraťte se na plátno.

## <a name="step-4---publish-and-start-the-runbook"></a>Krok 4 – publikování a spuštění runbooku
Právě vytvořený runbook je stále v režimu konceptu. Před jeho spuštěním v produkčním prostředí ho musíme publikovat. Když runbook publikujete, přepíšete vydanou verzi verzí v režimu konceptu. V našem případě zatím ještě publikovanou verzi nemáme, protože jsme runbook teprve vytvořili.

1. Kliknutím na **Publikovat** runbook publikujte a po zobrazení výzvy klikněte na **Ano**.  
   ![Tlačítko Publikovat](media/automation-first-runbook-textual-powershell/automation-publish-button.png)  
2. Pokud se posunete doleva, abyste runbook viděli v podokně **Runbooky**, zobrazí se **Stav vytváření**, který má hodnotu **Publikováno**.
3. Posuňte se zpět doprava, abyste viděli podokno **MyFirstRunbook-PowerShell**.  
   Možnosti v horní části nám umožňují spuštění runbooku, zobrazení runbooku, naplánování jeho spuštění někdy v budoucnu nebo vytvoření [webhooku](automation-webhooks.md), který umožní spuštění prostřednictvím volání protokolu HTTP.
4. My chceme jenom spustit runbook, proto klikněte na **Spustit** a po otevření okna Spuštění runbooku klikněte na tlačítko **OK**.  
   ![Tlačítko Spustit](media/automation-first-runbook-textual-powershell/automation-start-button.png)  
5. Podokno úlohy runbooku, který jsme právě vytvořili, je otevřené. Toto podokno můžeme zavřít, ale v tomto případě ho necháme otevřené, abychom mohli sledovat průběh úlohy.
6. Stav úlohy se zobrazuje v **souhrnu úlohy** a odpovídá stavům, které jsme viděli při testování runbooku.  
   ![Souhrn úlohy](media/automation-first-runbook-textual-powershell/automation-job-summary.png)  
7. Když se jako stav runbooku zobrazí *Dokončeno*, klikněte na **Výstup**. Otevře se podokno Výstup a my uvidíme text *Hello World*.  
   ![Výstup úlohy](media/automation-first-runbook-textual-powershell/automation-job-output.png)
8. Zavřete podokno Výstup.
9. Klikněte na **Všechny protokoly** a otevřete podokno Datové proudy, které patří k úloze runbooku. Ve výstupu datového proudu byste měli vidět jenom text *Hello World*, ale můžou se zobrazit i jiné datové proudy z úlohy runbooku, například Podrobný nebo Chyba, pokud do nich runbook zapisuje.  
   ![Všechny protokoly](media/automation-first-runbook-textual-powershell/automation-alllogs.png)  
10. Zavřete podokno Datové proudy a podokno Úloha a vraťte se do podokna MyFirstRunbook-PowerShell.
11. Kliknutím na **Úlohy** otevřete podokno Úlohy, které patří k tomuto runbooku. Vypíšou se všechny úlohy, které tento runbook vytvořil. Tady bychom měli vidět jenom jednu úlohu, protože jsme ji spustili jenom jednou.  
    ![Seznam úloh](media/automation-first-runbook-textual-powershell/automation-job-list.png)  
12. Na tuto úlohu můžete kliknout a otevřít podokno Úloha, které jsme zobrazili při spuštění runbooku. Pomocí této možnosti se můžete vrátit v čase a zobrazit si podrobnosti libovolné úlohy, která byla pro konkrétní runbook vytvořena.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Krok 5 – přidání ověřování ke správě prostředků Azure
Runbook jsme otestovali a publikovali, ale zatím nedělá nic užitečného. Chceme po něm, aby spravoval prostředky Azure. To nebude umět, pokud mu neumožníme ověřování pomocí přihlašovacích údajů, které jsou uvedené v [požadavcích](#prerequisites). Provedeme to pomocí rutiny **Add-AzureRmAccount**.

1. Kliknutím na **Upravit** v podokně MyFirstRunbook-PowerShell otevřete textový editor.  
   ![Úprava runbooku](media/automation-first-runbook-textual-powershell/automation-edit-runbook.png)  
2. Řádek **Write-Output** už nepotřebujeme, takže ho teď můžete odstranit.
3. Zadejte nebo zkopírujte a vložte následující kód, který bude zpracovávat ověřování pomocí vašeho účtu Automation Spustit jako:
   
   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```
   <br>
4. Klikněte na **testovací podokno**, abychom mohli runbook otestovat.
5. Kliknutím na **Spustit** spustíte test. Po dokončení byste měli obdržet výstup podobný následujícímu se základními informacemi z vašeho účtu. Tím se potvrdí platnost přihlašovacích údajů. <br> ![Ověření](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Krok 6 – přidání kódu pro spuštění virtuálního počítače
Teď, když runbook umí ověřit naše předplatné Azure, můžeme začít spravovat prostředky. Přidáme příkaz ke spuštění virtuálního počítače. V rámci vašeho předplatného Azure můžete vybrat jakýkoli virtuální počítač, ale prozatím jeho název pevně zakódujeme do rutiny.

1. Po *Add-AzureRmAccount* zadejte *Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'NameofResourceGroup'* a nezapomeňte zadat název a název skupiny prostředků virtuálního počítače, který chcete spustit.  
   
   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   ```
   <br>
2. Uložte runbook a klikněte na **testovací podokno**, abychom mohli runbook otestovat.
3. Kliknutím na **Spustit** spustíte test. Po jeho dokončení zkontrolujte, jestli se virtuální počítač spustil.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Krok 7 – přidání vstupního parametru do runbooku
Náš runbook aktuálně spouští virtuální počítač, který jsme do něj pevně zakódovali, ale bylo by užitečnější, pokud bychom mohli virtuální počítač zadat při spuštění runbooku. Teď do runbooku přidáme vstupní parametry, které tuto funkci zajistí.

1. Přidejte do runbooku parametry pro *VMName* a *ResourceGroupName* a použijte tyto proměnné s rutinou **Start-AzureRmVM**, jak vidíte na níže uvedeném příkladu.  
   
   ```
   Param(
    [string]$VMName,
    [string]$ResourceGroupName
   )
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   ```
   <br>
2. Uložte runbook a otevřete testovací podokno. Všimněte si, že teď můžete zadat hodnoty dvou vstupních proměnných, které budete v testu používat.
3. Zavřete testovací podokno.
4. Kliknutím na **Publikovat** publikujte novou verzi runbooku.
5. Zastavte virtuální počítač, který jste v předchozím kroku spustili.
6. Kliknutím na **Spustit** spustíte runbook. Zadejte **VMName** a **ResourceGroupName** pro virtuální počítač, který chcete spustit.  
   ![Předání parametru](media/automation-first-runbook-textual-powershell/automation-pass-params.png)  
7. Po dokončení runbooku zkontrolujte, jestli se virtuální počítač spustil.

## <a name="differences-from-powershell-workflow"></a>Odlišnosti od runbooků pracovních postupů PowerShellu
Powershellové runbooky mají stejný životní cyklus, možnosti a správu jako runbooky pracovních postupů PowerShellu, ale existují tady určité rozdíly a omezení:

1. Powershellové runbooky běží rychleji než runbooky pracovních postupů PowerShellu, protože neobsahují kompilační krok.
2. Runbooky pracovních postupů PowerShellu podporují kontrolní body. Pomocí kontrolních bodů můžou runbooky pracovních postupů PowerShellu pokračovat z libovolného bodu v runbooku, zatímco powershellové runbooky můžou pokračovat jenom od začátku.
3. Runbooky pracovních postupů PowerShellu podporují paralelní a sériové provádění, zatímco powershellové runbooky můžou příkazy provádět jenom sériově.
4. V runboocích pracovních postupů PowerShellu může mít aktivita, příkaz nebo blok skriptu svoje vlastní prostředí runspace, zatímco v powershellovém runbooku běží všechno ve skriptu v jednom prostředí runspace. Existují také určité [syntaktické rozdíly](https://technet.microsoft.com/magazine/dn151046.aspx) mezi nativním powershellovým runbookem a runbookem pracovního postupu PowerShellu.

## <a name="next-steps"></a>Další kroky
* První kroky s grafickými runbooky najdete v článku [Můj první grafický runbook](automation-first-runbook-graphical.md).
* První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).
* Další informace o typech runbooků, jejich výhodách a omezeních najdete v článku [Typy runbooků ve službě Azure Automation](automation-runbook-types.md).
* Další informace o funkci podpory powershellových skriptů najdete v článku [Nativní podpora powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)




<!--HONumber=Feb17_HO2-->


