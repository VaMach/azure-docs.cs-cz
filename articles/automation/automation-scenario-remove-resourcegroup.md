<properties
    pageTitle="Automatizace odebrání skupin prostředků | Microsoft Azure"
    description="Verze scénáře Azure Automation s pracovními postupy PowerShellu, včetně runbooků pro odebrání všech skupin prostředků v rámci vašeho předplatného."
    services="automation"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="magoedte"/>


# Scénář Azure Automation – automatizace odebrání skupin prostředků

Mnoho zákazníků vytvoří více než jednu skupinu prostředků. Některé z nich jsou vyhrazené pro správu aplikací v produkčním prostředí, ostatní mohou být určené pro vývojová, testovací a přípravná prostředí. Automatizace nasazení těchto prostředků je jedna věc, ale možnost vyřadit skupinu prostředků z provozu jediným kliknutím něco úplně jiného.  Použití služby Automation k tomuto účelu je ideálním řešením, které nabízí možnost tuto běžnou úlohu správy zjednodušit. Je také užitečné v případě, že pracujete s předplatným Azure, které má nastavený limit útraty prostřednictvím členské nabídky, jako je například MSDN nebo Microsoft Partner Network Cloud Essentials. 

Tento scénář je založený na runbooku PowerShellu a je určený k odebrání jedné nebo několika skupin prostředků, které zadáte ze svého předplatného.  Runbook podporuje, že se nejdřív provede testování a teprve pak se pokračuje. Je to jeho výchozí hodnota.  Tímto způsobem neprovedete odstranění omylem dřív, než si budete naprosto jistí, že jste k dokončení tohoto postupu připravení.   

## Získání scénáře

Tento scénář se skládá z runbooku PowerShellu, který můžete stáhnout z [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript) nebo naimportovat přímo z [galerie runbooků](automation-runbook-gallery.md) na webu Azure Portal.<br><br> 

Runbook | Popis|
----------|------------|
Remove-ResourceGroup | Odebere z předplatného jednu nebo více skupin prostředků Azure a jejich prostředků.  
<br>
Pro tento runbook jsou definované tyto vstupní parametry:

Parametr | Popis|
----------|------------|
NameFilter (povinný) | Umožňuje zadat filtr názvů pro omezení skupin prostředků, které máte v úmyslu odstranit. Můžete předat několik hodnot ve formě seznamu odděleného čárkami.<br>Ve filtru se nerozlišují velká a malá písmena. Filtru odpovídá libovolná skupina prostředků, která obsahuje zadaný řetězec.|
PreviewMode (volitelný) | Spustí runbook, aby bylo vidět, které skupiny prostředků se odstraní, ale neprovede žádnou akci.<br>Výchozí hodnota je **true**, která pomáhá zabránit nechtěnému odstranění jedné nebo několika skupin prostředků předaných do runbooku.  

## Instalace a konfigurace tohoto scénáře

### Požadavky

Tento runbook se ověřuje pomocí [účtu Spustit v Azure jako](automation-sec-configure-azure-runas-account.md).    

### Instalace a zveřejnění runbooků

Po stažení můžete tento runbook naimportovat pomocí postupu uvedeného v tématu věnovaném [importu runbooků](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-Azure-Automation).  Po úspěšném dokončení importu do účtu Automation můžete runbook publikovat.


## Použití runbooku

Následující kroky vás provedou spuštěním této sady runbook a pomohou vám seznámit se s tím, jak funguje.  V tomto příkladu budeme tento runbook jenom testovat a nebudeme odstraňovat žádné skupiny prostředků.  

1. Na webu Azure Portal otevřete svůj účet Automation a klikněte na dlaždici **Runbooky**.
2. Vyberte runbook **Remove-ResourceGroup** a klikněte na **Spustit**.
3. Po spuštění runbooku se otevře okno **Spustit runbook** a pro parametry je možné nakonfigurovat následující hodnoty.  Zadejte název jedné nebo několika více skupin prostředků ve vašem předplatném, které chcete použít pro testování a které by při náhodném odstranění nezpůsobily žádné potíže.<br> ![Parametry Remove-ResouceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)
    
    >[AZURE.NOTE] Zkontrolujte, že možnost **Previewmode** je nastavená na **true**, abyste zabránili odstranění vybraných skupin prostředků.  **Důležitá poznámka:** Tento runbook neodebere skupinu prostředků obsahující účet Automation, který spouští tento runbook.  

4. Po dokončení konfigurace hodnot všech parametrů klikněte na **OK**. Runbook se zařadí do fronty pro spuštění.  

Pokud chcete zobrazit podrobné informace o úloze runbooku **Remove-ResourceGroup** na webu Azure Portal, vyberte v runbooku dlaždici **Úlohy**. V souhrnu úlohy se zobrazí vstupní parametry a výstupní datový proud a také obecné informace o příslušné úloze a případné výjimky, které nastaly.<br> ![Stav úlohy runbooku Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png)

**Souhrn úlohy** zahrnuje zprávy z datových proudů výstupu, upozornění a chyb. Pokud chcete zobrazit podrobné výsledky spuštění runbooku, vyberte dlaždici **Výstup**.<br> ![Výsledky výstupu runbooku Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png) 

## Další kroky

- Pokud se chcete pustit do vytváření vlastního runbooku, přečtěte si téma [Vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md).
- První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).


<!--HONumber=Sep16_HO4-->


