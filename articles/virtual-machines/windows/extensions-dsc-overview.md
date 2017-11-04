---
title: "Konfigurace požadovaného stavu Azure přehled | Microsoft Docs"
description: "Přehled pro používání obslužná rutina rozšíření Microsoft Azure pro konfigurace požadovaného stavu prostředí PowerShell. Včetně požadavky, architektura, rutiny..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: c05c2d541a5f526f362f9cd72fe6d878374112b6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Agent virtuálního počítače Azure a související rozšíření jsou součástí infrastrukturní služby Microsoft Azure. Rozšíření virtuálního počítače jsou softwarové komponenty, které rozšiřují funkce virtuálních počítačů a zjednodušit různé operace správy virtuálních počítačů. Například slouží rozšíření VMAccess k resetování hesla správce nebo rozšíření vlastních skriptů můžete použít ke spuštění skriptu ve virtuálním počítači.

Tento článek představuje rozšíření prostředí PowerShell požadovaného stavu konfigurace (DSC) pro virtuální počítače Azure v rámci sady Azure SDK prostředí PowerShell. Nové rutiny můžete používat k odesílání a použití konfigurace DSC prostředí PowerShell ve virtuálním počítači Azure s příponou DSC prostředí PowerShell povolené. DSC prostředí PowerShell rozšíření volání do prostředí PowerShell DSC uplatní obdržená konfigurace DSC do virtuálního počítače. Tato funkce je také k dispozici prostřednictvím portálu Azure.

## <a name="prerequisites"></a>Požadavky
**Místní počítač** pro interakci s rozšíření virtuálního počítače Azure, budete muset použít portál Azure nebo Azure PowerShell SDK. 

**Agent hosta** virtuálního počítače Azure, který je nakonfigurovaný pomocí konfigurace DSC musí být operační systém, který podporuje buď Windows Management Framework (WMF) 4.0 nebo 5.0. Úplný seznam podporovaných verzí operačního systému najdete na [historie verzí rozšíření DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Podmínky a koncepty
Tato příručka předpokládá znalost následující koncepty:

Konfigurace – dokumentu konfigurace DSC. 

Uzel - cíle pro konfigurace DSC. V tomto dokumentu vždy "uzel" odkazuje na virtuální počítač Azure.

Konfigurační Data - .psd1 soubor obsahující data prostředí pro konfiguraci

## <a name="architectural-overview"></a>Přehled architektury
Rozšíření Azure DSC používá rozhraní agenta virtuálního počítače Azure, aby doručily, uplatní a sestav o konfiguracích DSC běžící na virtuálních počítačích Azure. Rozšíření DSC očekává, že soubor ZIP obsahující alespoň dokumentu konfigurace a sadu parametrů zadaných prostřednictvím sady SDK Azure PowerShell nebo prostřednictvím portálu Azure.

Při prvním volání rozšíření, spustí instalační proces. Tento proces nainstaluje verzi Windows Management Framework (WMF), ke pomocí následujícího postupu:

1. Je-li operačním systémem virtuálního počítače Azure Windows Server 2016, nebyla provedena žádná akce. Windows Server 2016 již nejnovější verzi prostředí PowerShell nainstalovaný.
2. Pokud `wmfVersion` je zadána vlastnost, je nainstalovaná této verze WMF, pokud není kompatibilní s operačním systémem Virtuálního počítače.
3. Pokud žádné `wmfVersion` je zadána vlastnost, je nainstalovaná nejnovější verze WMF, která je použít.

Instalace WMF vyžaduje restart. Po restartování rozšíření stáhne zadaný v souboru ZIP `modulesUrl` vlastnost. Pokud toto umístění je v Azure blob storage, SAS token lze zadat v `sasToken` vlastnost, která má přístup k souboru. Po stažení a vybaleno ZIP funkce konfigurace definované v `configurationFunction` běží ke generování souboru MOF. Rozšíření se pak spustí `Start-DscConfiguration -Force` na vygenerovaný soubor MOF. Přípona zachytí výstup a zápis zpátky stav kanál Azure. Z tohoto bodu na DSC LCM zpracovává monitorování a oprava jako normální. 

## <a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell
Rutiny prostředí PowerShell použít s Azure Resource Manager nebo modelu nasazení classic do balíčku, publikovat a monitorování nasazení rozšíření DSC. Moduly nasazení classic jsou následující rutiny uvedené, ale lze "Azure" nahradit "AzureRm" pomocí modelu Azure Resource Manager. Například `Publish-AzureVMDscConfiguration` používá model nasazení classic, kde `Publish-AzureRmVMDscConfiguration` používá Azure Resource Manager. 

`Publish-AzureVMDscConfiguration`trvá v konfiguračním souboru, hledá závislé prostředky DSC a vytvoří soubor ZIP obsahující konfiguraci a potřebné k uplatní konfiguraci prostředků DSC. Můžete také vytvořit balíček místně pomocí `-ConfigurationArchivePath` parametr. Jinak publikuje soubor .zip do Azure blob storage a zabezpečuje s tokenem SAS.

Soubor .zip vytvořených touto rutinou má .ps1 konfigurační skript v kořenové složce archivu. Prostředky mít složce modulu, který je umístěn ve složce archivu. 

`Set-AzureVMDscExtension`Vloží nastavení vyžadovaná rozšíření DSC Powershellu do objekt konfigurace virtuálního počítače. V modelu nasazení classic, změny virtuálního počítače je nutné použít na virtuální počítač Azure s `Update-AzureVM`. 

`Get-AzureVMDscExtension`načte stav rozšíření DSC konkrétní virtuálního počítače. 

`Get-AzureVMDscExtensionStatus`načte stav konfigurace DSC vydaných obslužná rutina rozšíření DSC. Tuto akci lze provést na jeden virtuální počítač nebo skupinu virtuálních počítačů.

`Remove-AzureVMDscExtension`Obslužná rutina rozšíření odebere z daného virtuálního počítače. Tato rutina nemá **není** odebrat konfiguraci, WMF odinstalovat nebo změnit nastavení použitá na virtuálním počítači. Odebere pouze obslužná rutina rozšíření. 

**Hlavní rozdíly ve rutiny ASM a Azure Resource Manager**

* Rutiny Azure Resource Manager jsou synchronní. ASM rutiny jsou asynchronní.
* Název skupiny prostředků, VMName, ArchiveStorageAccountName, verzi a umístění jsou všechny požadované parametry ve službě Správce prostředků Azure.
* ArchiveResourceGroupName je nový volitelný parametr pro Azure Resource Manager. Tento parametr můžete zadat, když váš účet úložiště patří do jiné skupině prostředků než ten, kde se má vytvořit virtuální počítač.
* ConfigurationArchive nazývá ArchiveBlobName ve službě Správce prostředků Azure
* ContainerName nazývá ArchiveContainerName ve službě Správce prostředků Azure
* StorageEndpointSuffix nazývá ArchiveStorageEndpointSuffix ve službě Správce prostředků Azure
* Přepínač automatických aktualizací byla přidána do Azure Resource Manager povolit automatické aktualizace rozšíření obslužné rutiny na nejnovější verzi jako a není k dispozici. Všimněte si, že tento parametr má by mohly mít restartování virtuálního počítače po vydání nové verze WMF. 

## <a name="azure-portal-functionality"></a>Funkce Azure portálu
Přejděte do virtuálního počítače. V části Nastavení -> Obecné klikněte na tlačítko "Rozšíření". Vytvoří se nové podokno. Klikněte na tlačítko "Přidat" a vyberte DSC prostředí PowerShell.

Na portálu musí vstup.
**Konfigurace moduly nebo skriptu**: Toto pole je povinné. Vyžaduje souboru s příponou .ps1 obsahující konfigurační skript, nebo soubor ZIP s .ps1 konfigurační skript v kořenu a všechny závislé zdroje ve složkách modul v rámci ZIP. Může být vytvořen pomocí `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` rutiny, které jsou součástí sady SDK Azure PowerShell. Soubor .zip nahraje do úložiště objektů blob uživatele zabezpečeny SAS token. 

**Soubor konfiguračních dat PSD1**: Toto pole je nepovinné. Pokud vaše konfigurace vyžaduje datový soubor konfigurace v .psd1, použijte toto pole vyberte ho a odešlete ji do úložiště objektů blob uživatele, kde je zabezpečená službou SAS token. 

**Modul kvalifikovaný název konfigurace**: soubory .ps1 může mít několik konfiguračních funkcí. Zadejte název skriptu konfigurace .ps1, za nímž následuje '\' a název funkce konfigurace. Například pokud váš skript .ps1 má název "configuration.ps1" a "IisInstall" je konfigurace, můžete zadáním:`configuration.ps1\IisInstall`

**Konfigurace argumentů**: Pokud konfigurace funkce přijímá argumenty, zadejte je sem ve formátu `argumentName1=value1,argumentName2=value2`. Všimněte si, že tento formát je do jiného formátu než jak konfigurace argumenty jsou přijímány prostřednictvím rutin prostředí PowerShell nebo šablony Resource Manageru. 

## <a name="getting-started"></a>Začínáme
Rozšíření Azure DSC trvá v dokumentech konfigurace DSC a představuje je na virtuálních počítačích Azure. Následuje jednoduchý příklad konfigurace. Uložte místně jako "IisInstall.ps1":

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
###<a name="classic-model"></a>Klasického modelu
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a>Azure modelu Resource Manager

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a>Protokolování
Protokoly jsou umístěny ve:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[číslo verze]

## <a name="next-steps"></a>Další kroky
Další informace o DSC Powershellu [přejděte do centra dokumentace k prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Zkontrolujte [šablony Azure Resource Manageru pro rozšíření DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Najít další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.

Podrobnosti o předávání citlivých parametry do konfigurace najdete v tématu [spravovat pověření bezpečně s obslužná rutina rozšíření DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

