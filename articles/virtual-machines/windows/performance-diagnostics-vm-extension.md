---
title: "Rozšíření virtuálního počítače Azure výkon diagnostiky pro Windows | Microsoft Docs"
description: "Představuje rozšíření virtuálního počítače Azure výkon diagnostiky pro systém Windows."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/29/2017
ms.author: genli
ms.openlocfilehash: d9384af2cf1d8b3f55f9ec2316046536634c124e
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/02/2017
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Rozšíření virtuálního počítače Azure výkon diagnostiky pro Windows

## <a name="summary"></a>Souhrn
Rozšíření virtuálního počítače Azure výkon diagnostiky pomáhá shromažďování diagnostických dat výkonu z virtuálních počítačů Windows, provede analýzu a poskytuje sestavu výsledků zjištění & doporučení, jak identifikovat a řešit problémy s výkonem na virtuálním počítači. Toto rozšíření nainstaluje nástroj řešení potíží s názvem [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Požadavky
### <a name="operating-systems"></a>Operační systémy
Toto rozšíření je možné nainstalovat na Windows Server 2008 R2, 2012, 2012 R2, 2016; Operační systémy Windows 8.1 a Windows 10.

## <a name="extension-schema"></a>Rozšíření schématu
Následujícím kódu JSON znázorňuje schéma pro rozšíření diagnostiky výkonu Azure. Toto rozšíření vyžaduje název a klíč pro účet úložiště do úložiště výstup diagnostiky a sestavy. Tyto hodnoty jsou citlivé a by měly být uložené v chráněném nastavení konfigurace. Data Azure nastavení rozšíření chráněný virtuální počítač je zašifrovaná a dešifrovat jenom na cílový virtuální počítač. Všimněte si, že jsou storageAccountName a storageAccountKey malá a velká písmena. V následující části jsou uvedeny další požadované parametry.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
```

### <a name="property-values"></a>Hodnoty vlastností

|   **Název**   |**Hodnota nebo příklad**|       **Popis**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|Verze rozhraní API
|Vydavatele|Microsoft.Azure.Performance.Diagnostics|Obor názvů vydavatele pro rozšíření
|type|AzurePerformanceDiagnostics|Typ rozšíření virtuálního počítače
|typeHandlerVersion|1.0|Verze rozšíření obslužné rutiny
|performanceScenario|Základní|Scénář výkonu k zaznamenání dat pro. Platné hodnoty jsou: **základní**, **vmslow**, **azurefiles**, a **vlastní**.
|traceDurationInSeconds|300|Doba trvání trasování, pokud nejsou vybrány žádné možnosti trasování.
|perfCounterTrace|P|Možnost povolit trasování čítače výkonu. Platné hodnoty jsou **p** nebo prázdnou hodnotu. Pokud nechcete zaznamenat trasování, nechte hodnotu jako prázdný.
|networkTrace|n|Možnost povolit trasování sledování sítě. Platné hodnoty jsou  **n**  nebo prázdnou hodnotu. Pokud nechcete zaznamenat trasování, nechte hodnotu jako prázdný.
|xperfTrace|x|Možnost povolit XPerf trasování. Platné hodnoty jsou **x** nebo prázdnou hodnotu. Pokud nechcete zaznamenat trasování, nechte hodnotu jako prázdný.
|storPortTrace|s|Možnost povolit StorPort trasování. Platné hodnoty jsou s nebo prázdnou hodnotu. Pokud nechcete zaznamenat trasování, nechte hodnotu jako prázdný.
|srNumber|123452016365929|Číslo lístku podpory, pokud je k dispozici. Ponechte prázdné, pokud ji nemáte.
|storageAccountName|můj_účet_úložiště|Název účtu úložiště k ukládání protokolů diagnostiky a výsledky.
|storageAccountKey|lDuVvxuZB28NNP... hAiRF3voADxLBTcc ==|Klíč pro účet úložiště.

## <a name="install-the-extension"></a>Instalace rozšíření

Postupujte podle těchto kroků nainstalujte rozšíření virtuálního počítače na virtuální počítače s Windows:

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
2. Vyberte virtuální počítač, ve které chcete nainstalovat tato rozšíření.

    ![Vyberte virtuální počítač](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Vyberte **rozšíření** a klikněte na **přidat** tlačítko.

    ![Výběr rozšíření](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Vyberte rozšíření diagnostiky výkonu Azure, zkontrolujte podmínky a ujednání a klikněte na **vytvořit** tlačítko.

    ![Vytvoření rozšíření diagnostiky Azure výkonu](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Zadejte hodnoty parametrů pro instalaci a klikněte na tlačítko **OK** k instalaci rozšíření. Můžete najít další informace o podporované scénáře řešení potíží [zde](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Instalace rozšíření](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Po úspěšné instalaci zobrazí zprávu s upozorněním zřizování bylo úspěšné.

    ![Zřizování úspěšné zpráv](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Spuštění rozšíření spustí po ní se zřizování je úspěšná a trvá několik minut nebo méně k dokončení spuštění pro základní scénář. Další scénáře spustí se prostřednictvím dobu zadanou během instalace.

## <a name="remove-the-extension"></a>Odeberte rozšíření
Chcete-li odebrat rozšíření z virtuálního počítače, postupujte takto:

1. Přihlaste se na [portál Azure](http://portal.azure.com), vyberte virtuální počítač, ve které chcete odebrat toto rozšíření a potom vyberte rozšíření okno. 
2. Klikněte na (**...** ) pro rozšíření diagnostiky výkonu položky ze seznamu a vyberte možnost odinstalovat.

    ![Odinstalovat rozšíření](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Také můžete vybrat položku rozšíření a vyberte možnost odinstalovat.

## <a name="template-deployment"></a>Nasazení šablony
Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření diagnostiky Azure výkonu při nasazení šablony Azure Resource Manager. Zde je ukázka šablony, která lze použít s nasazení šablony:

````
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell
`Set-AzureRmVMExtension` Příkaz lze použít k nasazení rozšíření virtuálního počítače Azure Diagnostics výkonu do existujícího virtuálního počítače. Před spuštěním příkazu, veřejné a privátní konfigurace muset být uložena v tabulce hash prostředí PowerShell.

PowerShell

````
$PublicSettings = @{ "performanceScenario":"basic","traceDurationInSeconds":300,"perfCounterTrace":"p","networkTrace":"","xperfTrace":"","storPortTrace":"","srNumber":"","requestTimeUtc":"2017-09-28T22:08:53.736Z" }
$ProtectedSettings = @{"storageAccountName":"mystorageaccount","storageAccountKey":"mystoragekey"}

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
````

## <a name="information-on-the-data-captured"></a>Informace o data zaznamenaná
Nástroj PerfInsights shromažďuje různé protokoly, konfiguraci, diagnostických dat atd. v závislosti na vybrané scénáři. Další informace o data shromažďují pro konkrétní scénář, navštivte [PerfInsights dokumentaci](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Zobrazovat a sdílet výsledky

Výstup tohoto rozšíření se ukládají do složky s názvem log_collection v dočasné jednotce (obvykle D:\log_collection) ve výchozím nastavení. V této složce uvidíte soubory zip obsahující diagnostické protokoly a sestavy s výsledky a doporučení.

Soubor zip vytvořený účet úložiště zadaný během instalace se také nahraje a sdílený po dobu 30 dnů pomocí [podpisy sdíleného přístupu (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Textový soubor s názvem *zipfilename*_saslink.txt se také vytvoří ve složce log_collection. Tento soubor obsahuje odkaz SAS vytvořili ke stažení souboru zip. Každý, kdo má tento odkaz je možné stáhnout soubor zip.

Společnost Microsoft může použít tento odkaz SAS ke stahování dat diagnostiky pro další šetření pracovníkem podpory práce v lístku podpory.

Chcete-li zobrazit sestavu, právě extrahování souboru zip a otevřete **PerfInsights Report.html** souboru.

Může být také moct stáhnout soubor zip přímo z portálu výběrem rozšíření.

![Zobrazit podrobné informace o stavu](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Odkaz SAS, které se zobrazí na portálu nemusí fungovat někdy z důvodu poškozená adresa Url (způsobené speciální znaky) během kódování a dekódování operaci. Alternativní řešení je získejte odkaz přímo ze souboru *_saslink.txt z virtuálního počítače.

## <a name="troubleshoot-and-support"></a>Řešení potíží a podpora
### <a name="troubleshoot"></a>Řešení potíží

- Stav nasazení rozšíření (v oznamovací oblasti) může zobrazovat "Nasazení v průběhu", i když je rozšíření úspěšném zřízení.

    Tento problém můžete ignorovat, dokud rozšíření stav označuje, že je rozšíření úspěšně zřízený.
- Některé problémy při instalaci můžete řešit pomocí protokolů rozšíření. Výstupu spuštění rozšíření se zaznamenává soubory, které jsou v následujícím adresáři:

        C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/support/faq/).
