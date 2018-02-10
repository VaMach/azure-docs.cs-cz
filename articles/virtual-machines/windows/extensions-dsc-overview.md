---
title: "Konfigurace požadovaného stavu Azure přehled | Microsoft Docs"
description: "Přehled pro používání obslužná rutina rozšíření Microsoft Azure pro konfigurace požadovaného stavu prostředí PowerShell. Včetně požadavky, architektura, rutiny..."
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: ed8b5bc54baa3a5abfb596b202f0af58e1b6c74f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Agent virtuálního počítače Azure a související rozšíření jsou součástí infrastrukturní služby Microsoft Azure.
Rozšíření virtuálního počítače jsou softwarové komponenty, které rozšiřují funkce virtuálních počítačů a zjednodušit různé operace správy virtuálních počítačů.

Primární případ použití pro rozšíření konfigurace požadovaného stavu je k navázání připojení virtuální počítač, abyste [služby Azure Automation DSC](../../automation/automation-dsc-overview.md) , který poskytuje [výhody](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig.md#pull-service) včetně průběžnou správu konfigurace virtuálního počítače a integrace s Další provozní nástroje, jako jsou monitorování Azure.

Je také možné použít rozšíření DSC nezávisle ze služby Azure Automation DSC, ale toto je singulární akci, která nastane během nasazení a není žádná probíhající reporting nebo správu konfigurace jiný než místně v rámci virtuálního počítače.

Tento článek obsahuje informace pro scénáře, rozšíření DSC pro registraci Azure Automation a jak používat rozšíření DSC jako nástroj pro přiřazení konfigurací k virtuálním počítačům pomocí sady Azure SDK.

## <a name="prerequisites"></a>Požadavky

- **Místní počítač** – k interakci se rozšíření virtuálního počítače Azure, budete muset použít portál Azure nebo Azure PowerShell SDK.
- **Agent hosta** -virtuálního počítače Azure, který je nakonfigurovaný pomocí konfigurace DSC musí být operační systém, který podporuje Windows Management Framework (WMF) 4.0 nebo novější. Úplný seznam podporovaných verzí operačního systému najdete na stránce [historie verzí rozšíření DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Podmínky a koncepty

Tato příručka předpokládá znalost následující koncepty:

- **Konfigurace** -dokumentu konfigurace A DSC.
- **Uzel** -cíle pro konfigurace DSC. V tomto dokumentu vždy "uzel" odkazuje na virtuální počítač Azure.
- **Konfigurační Data** – .psd1 soubor obsahující data prostředí pro konfiguraci.

## <a name="architectural-overview"></a>Přehled architektury

Rozšíření Azure DSC používá rozhraní agenta virtuálního počítače Azure, aby doručily, uplatní a sestav o konfiguracích DSC běžící na virtuálních počítačích Azure.
Rozšíření DSC přijme dokumentu konfigurace a sadu parametrů.
Pokud je zadaný žádný soubor, [výchozí konfigurační skript](###default-configuration-script) vložené s příponou, která se používá pouze pro nastavení metadata v [správce místní konfigurace](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).

Při prvním volání rozšíření, nainstaluje verzi Windows Management Framework (WMF), ke pomocí následujícího postupu:

1. Je-li operačním systémem virtuálního počítače Azure Windows Server 2016, nebyla provedena žádná akce. Windows Server 2016 již nejnovější verzi prostředí PowerShell nainstalovaný.
2. Pokud `wmfVersion` je zadána vlastnost, je nainstalovaná této verze WMF, pokud není kompatibilní s operačním systémem Virtuálního počítače.
3. Pokud žádné `wmfVersion` je zadána vlastnost, je nainstalovaná nejnovější verze WMF, která je použít.

Instalace WMF vyžaduje restart.
Po restartování rozšíření stáhne zadaný v souboru ZIP `modulesUrl` vlastnost, pokud zadaná.
Pokud toto umístění je v Azure blob storage, SAS token lze zadat v `sasToken` vlastnost, která má přístup k souboru.
Po stažení a vybaleno ZIP funkce konfigurace definované v `configurationFunction` běží ke generování souboru MOF.
Rozšíření se pak spustí `Start-DscConfiguration -Force` pomocí vygenerovaný soubor MOF.
Přípona zachytí výstup a zapíše ho do stav kanál Azure.

### <a name="default-configuration-script"></a>Výchozí konfigurační skript

Rozšíření DSC Azure obsahuje výchozí konfiguraci skript určený k použití při připojování virtuální počítač do služby Azure Automation DSC.
Parametry skriptu jsou v souladu s konfigurovatelné vlastnosti [správce místní konfigurace](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).
Parametry skriptu jsou [zdokumentovaný](extensions-dsc-template.md##default-configuration-script) a je k dispozici v úplné skript [Githubu](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-arm-templates"></a>Rozšíření DSC v šablony ARM

Nasazení šablony Azure Resource Manager (ARM) jsou očekávané způsob, jak pracovat s příponou DSC pro většinu scénářů.
Informace a příklady včetně rozšíření DSC v nasazení šablony ARM, naleznete na stránce vyhrazené dokumentace [požadovaného stavu konfigurace rozšíření s šablon Azure Resource Manageru](extensions-dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>Rutiny prostředí PowerShell rozšíření DSC

Rutiny prostředí PowerShell pro správu rozšíření DSC jsou nejvhodnější pro interaktivní řešení problémů a informace o shromažďování scénáře.
Rutiny slouží k balíčku, publikovat a monitorování nasazení rozšíření DSC.
Upozorňujeme, že rutiny pro rozšíření DSC nebyly aktualizovány ještě pro práci s [výchozí konfigurační skript](###default-configuration-script).

`Publish-AzureRMVMDscConfiguration`trvá v konfiguračním souboru, hledá závislé prostředky DSC a vytvoří soubor ZIP obsahující konfiguraci a potřebné k uplatní konfiguraci prostředků DSC.
Můžete také vytvořit balíček místně pomocí `-ConfigurationArchivePath` parametr.
Jinak publikuje soubor .zip do Azure blob storage a zabezpečuje s tokenem SAS.

Soubor .zip vytvořených touto rutinou má .ps1 konfigurační skript v kořenové složce archivu.
Prostředky mít složce modulu, který je umístěn ve složce archivu.

`Set-AzureRMVMDscExtension`Vloží nastavení vyžadovaná rozšíření DSC Powershellu do objekt konfigurace virtuálního počítače.

`Get-AzureRMVMDscExtension`načte stav rozšíření DSC konkrétní virtuálního počítače.

`Get-AzureRMVMDscExtensionStatus`načte stav konfigurace DSC vydaných obslužná rutina rozšíření DSC.
Tuto akci lze provést na jeden virtuální počítač nebo skupinu virtuálních počítačů.

`Remove-AzureRMVMDscExtension`Obslužná rutina rozšíření odebere z daného virtuálního počítače.
Tato rutina nemá **není** odebrat konfiguraci, WMF odinstalovat nebo změnit nastavení použitá na virtuálním počítači.
Odebere pouze obslužná rutina rozšíření. 

Důležité informace týkající se rozšíření DSC AzureRM rutiny:

- Rutiny Azure Resource Manager jsou synchronní.
- Název skupiny prostředků, VMName, ArchiveStorageAccountName, verzi a umístění jsou všechny požadované parametry.
- ArchiveResourceGroupName je volitelný parametr. Tento parametr můžete zadat, když váš účet úložiště patří do jiné skupině prostředků než ten, kde se má vytvořit virtuální počítač.
- Přepínač automatických aktualizací umožňuje automatické aktualizace rozšíření obslužné rutiny na nejnovější verzi jako a není k dispozici. Všimněte si, že tento parametr má by mohly mít restartování virtuálního počítače po vydání nové verze WMF.

### <a name="getting-started-with-cmdlets"></a>Začínáme s rutinami

Rozšíření Azure DSC je schopen použití dokumentů konfigurace DSC přímo ke konfiguraci virtuálních počítačích Azure během nasazení, i když to nebude zaregistrujte uzlu pro Azure Automation, aby se uzel **není*-centrálně spravovat.

Následuje jednoduchý příklad konfigurace.
Uložte místně jako "IisInstall.ps1":

```powershell
configuration IISInstall
{
    node "localhost"
    {
        WindowsFeature IIS
        {
            Ensure = "Present"
            Name = "Web-Server"
        }
    }
}
```

Následující kroky umístěte skript IisInstall.ps1 na zadaný virtuální počítač, provedení konfigurace a zpětně hlásit stav.

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.72 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"
```

## <a name="azure-portal-functionality"></a>Funkce Azure portálu

Přejděte do virtuálního počítače. V části Nastavení -> Obecné klikněte na tlačítko "Rozšíření".
Vytvoří se nové podokno.
Klikněte na tlačítko "Přidat" a vyberte DSC prostředí PowerShell.

Na portálu musí vstup.
**Konfigurace moduly nebo skriptu**: Toto pole je povinné (nebyla aktualizována formuláře [výchozí konfigurační skript](###default-configuration-script).
Vyžaduje souboru s příponou .ps1 obsahující konfigurační skript, nebo soubor ZIP s .ps1 konfigurační skript v kořenu a všechny závislé zdroje ve složkách modul v rámci ZIP.
Může být vytvořen pomocí `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` rutiny, které jsou součástí sady SDK Azure PowerShell.
Soubor .zip nahraje do úložiště objektů blob uživatele zabezpečeny SAS token.

**Soubor konfiguračních dat PSD1**: Toto pole je nepovinné.
Pokud vaše konfigurace vyžaduje datový soubor konfigurace v .psd1, použijte toto pole vyberte ho a odešlete ji do úložiště objektů blob uživatele, kde je zabezpečená službou SAS token.

**Modul kvalifikovaný název konfigurace**: soubory .ps1 může mít několik konfiguračních funkcí.
Zadejte název skriptu konfigurace .ps1, za nímž následuje '\' a název funkce konfigurace.
Například pokud váš skript .ps1 má název "configuration.ps1" a "IisInstall" je konfigurace, můžete zadáním:`configuration.ps1\IisInstall`

**Konfigurace argumentů**: Pokud konfigurace funkce přijímá argumenty, zadejte je sem ve formátu `argumentName1=value1,argumentName2=value2`.
Všimněte si, že tento formát je do jiného formátu než jak konfigurace argumenty jsou přijímány prostřednictvím rutin prostředí PowerShell nebo šablony Resource Manageru.

## <a name="logging"></a>Protokolování
Protokoly jsou umístěny ve:

```powerShell
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]
```

## <a name="next-steps"></a>Další postup
Další informace o DSC Powershellu [přejděte do centra dokumentace k prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Zkontrolujte [šablony Azure Resource Manageru pro rozšíření DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Najít další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.

Podrobnosti o předávání citlivých parametry do konfigurace najdete v tématu [spravovat pověření bezpečně s obslužná rutina rozšíření DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
