---
title: "Automatizace odebrání skupin prostředků | Dokumentace Microsoftu"
description: "Verze scénáře Azure Automation s pracovními postupy prostředí PowerShell, včetně runbooků pro odebrání všech skupin prostředků v rámci vašeho předplatného."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: 
ms.assetid: b848e345-fd5d-4b9d-bc57-3fe41d2ddb5c
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/26/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 00b217a4cddac0a893564db27ffb4f460973c246
ms.openlocfilehash: 6aa3897d44b146483204d43e026d3d158c3e13b6


---
# <a name="azure-automation-scenario-automate-removal-of-resource-groups"></a>Scénář Azure Automation – automatizace odebrání skupin prostředků
Mnoho zákazníků vytváří více než jednu skupinu prostředků. Některé mohou sloužit ke správě produkčních aplikací, jiné mohou sloužit jako vývojové, testovací nebo přípravné prostředí. Automatizace nasazení těchto prostředků je jedna věc, ale možnost vyřadit skupinu prostředků z provozu jediným kliknutím něco úplně jiného. Tuto běžnou úlohu správy můžete zjednodušit pomocí Azure Automation. To je užitečné v případě, že pracujete s předplatným Azure, které má nastavený limit útraty prostřednictvím členské nabídky, jako je například MSDN nebo program Microsoft Partner Network Cloud Essentials.

Tento scénář je založený na runbooku PowerShellu a je určený k odebrání jedné nebo několika skupin prostředků, které zadáte ze svého předplatného. Ve výchozím nastavení runbook nejdříve testuje a poté pokračuje. Tím je zajištěno, že nechtěně neodstraníte skupinu prostředků dříve, než budete připraveni tento postup dokončit.   

## <a name="getting-the-scenario"></a>Získání scénáře
Tento scénář se skládá z runbooku prostředí PowerShell, který si můžete stáhnou z [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). Můžete jej také importovat přímo z [Galerie runbooků](automation-runbook-gallery.md) na webu Azure Portal.<br><br>

| Runbook | Popis |
| --- | --- |
| Remove-ResourceGroup |Odebere z předplatného jednu nebo více skupin prostředků Azure a přidružené prostředky. |

<br>
Pro tento runbook jsou definované tyto vstupní parametry:

| Parametr | Popis |
| --- | --- |
| NameFilter (povinný) |Zadává filtr názvů pro omezení skupin prostředků, které máte v úmyslu odstranit. Můžete předat několik hodnot ve formě seznamu odděleného čárkami.<br>Ve filtru se nerozlišují velká a malá písmena. Filtru odpovídá libovolná skupina prostředků, která obsahuje zadaný řetězec. |
| PreviewMode (volitelný) |Spustí runbook, aby bylo vidět, které skupiny prostředků se odstraní, ale neprovede žádnou akci.<br>Výchozí hodnota je **true**, která pomáhá zabránit nechtěnému odstranění jedné nebo několika skupin prostředků předaných do runbooku. |

## <a name="install-and-configure-this-scenario"></a>Instalace a konfigurace tohoto scénáře
### <a name="prerequisites"></a>Požadavky
Tento runbook se ověřuje pomocí [účtu Spustit v Azure jako](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-the-runbooks"></a>Instalace a zveřejnění runbooků
Po stažení můžete tento runbook naimportovat pomocí postupu uvedeného v tématu [Postupy importování runbooků](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation). Po úspěšném dokončení importu do účtu Automation můžete runbook publikovat.

## <a name="using-the-runbook"></a>Použití runbooku
Následující kroky vás provedou spuštěním této sady runbook a pomohou vám seznámit se s tím, jak funguje. V tomto příkladu budete tento runbook pouze testovat a nebudete odstraňovat žádné skupiny prostředků.  

1. Na webu Azure Portal otevřete svůj účet Automation a klikněte na **Runbooky**.
2. Vyberte runbook **Remove-ResourceGroup** a klikněte na **Spustit**.
3. Po spuštění runbooku se otevře okno **Spustit runbook** a můžete nakonfigurovat parametry. Zadejte názvy skupin prostředků ve vašem předplatném, které můžete použít pro testování a které by při náhodném odstranění nezpůsobily žádné potíže.<br> ![Parametry Remove-ResouceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)

   > [!NOTE]
   > Zkontrolujte, že možnost **Previewmode** je nastavená na **true**, abyste zabránili odstranění vybraných skupin prostředků.  **Poznámka**: Tento runbook neodebere skupinu prostředků obsahující účet Automation, který spouští tento runbook.  
   >
   >
4. Po dokončení konfigurace hodnot všech parametrů klikněte na **OK**. Runbook se zařadí do fronty pro spuštění.  

Pokud chcete zobrazit podrobné informace o úloze runbooku **Remove-ResourceGroup** na webu Azure Portal, vyberte v runbooku **Úlohy**. V souhrnu úlohy se zobrazí vstupní parametry a výstupní datový proud a také obecné informace o příslušné úloze a případné výjimky, které nastaly.<br> ![Stav úlohy runbooku Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png)

**Souhrn úlohy** zahrnuje zprávy z datových proudů výstupu, upozornění a chyb. Pokud chcete zobrazit podrobné výsledky spuštění runbooku, vyberte **Výstup**.<br> ![Výsledky výstupu runbooku Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Další kroky
* Pokud se chcete pustit do vytváření vlastního runbooku, přečtěte si téma [Vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md).
* První kroky s runbooky pracovních postupů prostředí PowerShell najdete v článku [Můj první runbook pracovního postupu prostředí PowerShell](automation-first-runbook-textual.md).



<!--HONumber=Nov16_HO2-->


