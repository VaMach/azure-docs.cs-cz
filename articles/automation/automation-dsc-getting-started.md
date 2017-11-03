---
title: "Začínáme s Azure Automation DSC. | Microsoft Docs"
description: "Vysvětlení a příklady nejběžnější úlohy v Azure Automation požadovaného stavu konfigurace (DSC)"
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;eslesar
ms.openlocfilehash: 8a10d961ad7c107c68b57c64ee6c88544ff8832b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a>Začínáme s Azure Automation DSC.
Toto téma vysvětluje, jak provádět běžné úkoly s Azure Automation požadovaného stavu konfigurace (DSC), jako je například vytváření, importování a kompilování konfigurace, registrace počítače, které chcete spravovat a zobrazování sestav. Přehled co Azure Automation DSC je, najdete v části [přehled Azure Automation DSC](automation-dsc-overview.md). DSC dokumentaci najdete v tématu [Přehled konfigurace prostředí Windows PowerShell požadovaného stavu](https://msdn.microsoft.com/PowerShell/dsc/overview).

Toto téma obsahuje podrobný návod, jak pomocí Azure Automation DSC. Pokud chcete, aby ukázkové prostředí, které jsou již nastaveny bez kroků popsaných v tomto tématu, můžete použít [následující šablony ARM](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup). Tato šablona nastavuje dokončené prostředí Azure Automation DSC, včetně virtuálního počítače Azure, který je spravovaný nástrojem Azure Automation DSC.

## <a name="prerequisites"></a>Požadavky
Abyste mohli dokončit příklady v tomto tématu, je potřeba splnit následující nástroje:

* Účet Azure Automation. Pokyny k vytvoření účtu Azure Automation Spustit jako najdete v tématu [Účet Spustit jako pro Azure](automation-sec-configure-azure-runas-account.md).
* Virtuální počítač Azure Resource Manager (ne Classic) systémem Windows Server 2008 R2 nebo novější. Pokyny k vytvoření virtuálního počítače najdete v tématu [Vytvoření vašeho prvního virtuálního počítače s Windows na webu Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

## <a name="creating-a-dsc-configuration"></a>Vytvoření konfigurace DSC
Vytvoříme jednoduchou [konfigurace DSC](https://msdn.microsoft.com/powershell/dsc/configurations) zajistí se tak přítomnosti nebo absenci **Webový Server** Windows funkce (IIS), v závislosti na tom, jak přiřadit uzly.

1. Spusťte Windows PowerShell ISE (nebo libovolného textového editoru).
2. Zadejte následující text:
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
   
            }
        }
   
        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
   
            }
        }
        }
    ```
3. Uložte soubor jako `TestConfig.ps1`.

Tato konfigurace volá jeden prostředek v každém uzlu bloku [WindowsFeature prostředků](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), zajistí se tak přítomnosti nebo absenci **Webový Server** funkce.

## <a name="importing-a-configuration-into-azure-automation"></a>Konfigurace importu do Azure Automation
V dalším kroku jsme budete importovat konfiguraci do účtu Automation.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **konfigurace DSC**.
4. Na **konfigurace DSC** okně klikněte na tlačítko **přidání konfigurace**.
5. Na **importovat konfiguraci** okně Procházet a `TestConfig.ps1` souboru ve svém počítači.
   
    ![Snímek obrazovky ** okno importu konfigurace **](./media/automation-dsc-getting-started/AddConfig.png)
6. Klikněte na **OK**.

## <a name="viewing-a-configuration-in-azure-automation"></a>Zobrazení konfigurace ve službě Azure Automation
Po dokončení importu konfigurace, můžete ji zobrazit na portálu Azure.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **konfigurace DSC**
4. Na **konfigurace DSC** okno, klikněte na **TestConfig** (Toto je název konfigurace, které jste importovali v předchozím postupu).
5. Na **TestConfig konfigurace** okno, klikněte na **zdroj konfigurace zobrazení**.
   
    ![Snímek obrazovky okna TestConfig konfigurace](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    A **zdroj konfigurace TestConfig** otevře okno se zobrazí kód prostředí PowerShell pro konfiguraci.

## <a name="compiling-a-configuration-in-azure-automation"></a>Kompilování konfigurace ve službě Azure Automation
Než použijete požadovaný stav do uzlu Konfigurace DSC, definování tohoto stavu musí být zkompilovány do minimálně jedna konfigurace uzlu (dokument MOF) a na Server pro vyžádání obsahu Automation DSC. Podrobnější popis kompilování konfigurace v Azure Automation DSC, najdete v části [kompilování konfigurace v Azure Automation DSC](automation-dsc-compile.md). Další informace o kompilování konfigurace najdete v tématu [konfigurace DSC](https://msdn.microsoft.com/PowerShell/DSC/configurations).

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **konfigurace DSC**
4. Na **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (název dřív naimportovaný konfigurace).
5. Na **TestConfig konfigurace** okně klikněte na tlačítko **zkompilovat**a potom klikněte na **Ano**. Spustí úlohu kompilace.
   
    ![Snímek obrazovky okna konfigurace TestConfig zvýraznění tlačítko kompilace](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Při kompilaci konfigurace ve službě Azure Automation, automaticky se nasadí žádnou konfiguraci vytvořený uzel soubory MOF na server vyžádané replikace.
> 
> 

## <a name="viewing-a-compilation-job"></a>Zobrazení úlohu kompilace
Po spuštění kompilace, můžete ji v zobrazit **úlohy kompilace** dlaždice v nástroji **konfigurace** okno. **Úlohy kompilace** dlaždice ukazuje aktuálně spuštěna, byla dokončena a neúspěšné úlohy. Když otevřete okno úlohy kompilace, zobrazuje informace o této úloze, včetně žádné chyby nebo upozornění došlo, vstupní parametry použít v konfiguraci a kompilace protokoly.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **konfigurace DSC**.
4. Na **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (název dřív naimportovaný konfigurace).
5. Na **úlohy kompilace** dlaždici **TestConfig konfigurace** okně klikněte na některé z uvedených úloh. A **úloha kompilace** otevře se okno s názvem bez přípony s datem, která byla spuštěna úloha kompilace.
   
    ![Snímek obrazovky okna úlohy kompilace](./media/automation-dsc-getting-started/CompilationJob.png)
6. Kliknutím na libovolnou dlaždici v **úloha kompilace** okno zobrazíte další podrobnosti o úloze.

## <a name="viewing-node-configurations"></a>Zobrazení konfigurace uzlu
Úspěšné dokončení úlohy kompilace vytvoří jeden nebo více nové konfigurace uzlu. Konfigurace uzlu je dokument MOF, který je nasazen na server vyžádané replikace a připravené k vyžádat a použít jeden nebo více uzly. Konfigurace uzlu si můžete prohlédnout v účtu Automation v **konfigurace uzlu DSC** okno. Konfigurace uzlu, má název s formulářem *ConfigurationName*. *NodeName*.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **konfigurace uzlu DSC**.
   
    ![Snímek obrazovky okna konfigurace uzlu DSC](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Registrace virtuální počítač Azure pro správu s Azure Automation DSC.
Azure Automation DSC můžete použít ke správě virtuálních počítačích Azure (Classic i Resource Manager), místní virtuální počítače, Linux počítače, virtuální počítače AWS a místní fyzických počítačů. V tomto tématu se nabídneme postupy zařadit pouze virtuální počítače Azure Resource Manager. Informace o registraci najdete v části Další typy počítačů, [registrace počítačů pro správu Azure Automation DSC](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>Se budou registrovat virtuální počítač Azure Resource Manager pro správu Azure Automation DSC.
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.
4. V **uzly DSC** okně klikněte na tlačítko **přidat virtuální počítač Azure**.
   
    ![Snímek obrazovky okna uzly DSC zvýraznění na tlačítko Přidat virtuální počítač Azure](./media/automation-dsc-getting-started/OnboardVM.png)
5. V **přidat virtuální počítače Azure** okně klikněte na tlačítko **vybrat virtuální počítače chcete zařadit**.
6. V **vyberte virtuální počítače** okno Vyberte virtuální počítač, který chcete připojit a klikněte na tlačítko **OK**.
   
   > [!IMPORTANT]
   > Tato hodnota musí být virtuální počítač Azure Resource Manager systémem Windows Server 2008 R2 nebo novější.
   > 
   > 
7. V **přidat virtuální počítače Azure** okně klikněte na tlačítko **konfigurace registrační data**.
8. V **registrace** okno, zadejte název konfigurace uzlu, kterou chcete použít pro virtuální počítač v **název konfigurace uzlu** pole. Toto musí přesně odpovídat názvu konfigurace uzlu v účtu Automation. Poskytnutí názvu v tomto okamžiku je volitelné. Můžete změnit konfiguraci přiřazené uzlu po registraci uzlu.
   Zkontrolujte **restartovat uzel v případě potřeby**a potom klikněte na **OK**.
   
    ![Snímek obrazovky okna registrace](./media/automation-dsc-getting-started/RegisterVM.png)
   
    Konfigurace uzlu, který jste zadali se použijí pro virtuální počítač v intervalech určeného **frekvence režimu konfigurace**, a virtuální počítač bude vyhledávat aktualizace konfigurace uzlu v intervalech určeného **obnovovací frekvence**. Další informace o tom, jak se používají tyto hodnoty, najdete v části [konfigurace správce místní konfigurace](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
9. V **přidat virtuální počítače Azure** okně klikněte na tlačítko **vytvořit**.

Azure se spustí proces registrace virtuálních počítačů. Po dokončení virtuálního počítače se zobrazí v **uzly DSC** okno v účtu Automation.

## <a name="viewing-the-list-of-dsc-nodes"></a>Zobrazení seznamu uzlů DSC
Můžete zobrazit seznam všech počítačů, které byly zařazený, nemá pro správu ve vašem účtu Automation v **uzly DSC** okno.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.

## <a name="viewing-reports-for-dsc-nodes"></a>Prohlížení sestav pro uzly DSC
Pokaždé, když Azure Automation DSC provede kontrolu konzistence na spravovaný uzel, uzel odešle zprávu o stavu zpět na server vyžádané replikace. Tyto sestavy můžete zobrazit v okně pro tento uzel.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.
4. Na **sestavy** dlaždici, klikněte na žádnou z těchto sestav v seznamu.
   
    ![Snímek obrazovky okna sestavy](./media/automation-dsc-getting-started/NodeReport.png)

V okně pro jednotlivé sestavy zobrazí se následující informace o stavu odpovídající kontrolu konzistence:

* Sestava stavu – jestli uzlu je "Kompatibilní", "Failed" konfigurace nebo uzel "Neodpovídá" (Pokud je uzel v **applyandmonitor** režimu a počítač není v požadovaném stavu).
* Čas zahájení pro kontrolu konzistence.
* Celkové doby běhu pro kontrolu konzistence.
* Typ kontroly konzistence.
* Všechny chyby, včetně kód chyby a chybová zpráva. 
* Veškeré prostředky DSC v konfiguraci a stavu každého prostředku (jestli uzlu je v požadovaném stavu pro tento prostředek), můžete kliknutím na každého prostředku, chcete-li získat podrobnější informace pro tento prostředek.
* Název, IP adresu a režim konfigurace uzlu.

Můžete také kliknout na **zobrazit nezpracované sestavy** zobrazíte skutečná data, která uzlu odešle na server. Další informace o používání dat najdete v tématu [pomocí serveru sestav DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).

Může trvat delší dobu, po uzel, který je zařazený nemá před první sestava je k dispozici. Možná budete muset po čekat, až 30 minut pro první zprávu můžete zaváděním uzlu.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Opětovné přiřazování uzlu do jiného uzlu Konfigurace
Můžete přiřadit uzel, který použijete jiný uzel konfigurace než ten, který původně přiřazený.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.
4. Na **uzly DSC** okně klikněte na název uzlu, který chcete přiřadit.
5. V okně pro tento uzel, klikněte na **přiřazení uzlu**.
   
    ![Snímek obrazovky okna uzlu zvýraznění tlačítko přiřadit uzlu](./media/automation-dsc-getting-started/AssignNode.png)
6. Na **přiřadit konfigurace uzlu** okně vyberte konfigurace uzlu, ke kterému chcete přiřadit uzlu a pak klikněte na tlačítko **OK**.
   
    ![Snímek obrazovky okna přiřadit konfigurace uzlu](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Zrušení registrace uzlu
Pokud již nechcete uzlu lze spravovat pomocí Azure Automation DSC, můžete zrušení její registrace.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.
3. Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.
4. Na **uzly DSC** okně klikněte na název uzlu, kterou chcete zrušit registraci.
5. V okně pro tento uzel, klikněte na **Unregister**.
   
    ![Snímek obrazovky okna uzlu zvýraznění tlačítko Unregister](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Související články
* [Přehled služby Azure Automation DSC](automation-dsc-overview.md)
* [Registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md)
* [Přehled stavu konfigurace požadovaného prostředí Windows PowerShell](https://msdn.microsoft.com/powershell/dsc/overview)
* [Rutiny Azure Automation DSC](/powershell/module/azurerm.automation/#automation)
* [Ceny služby Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)

