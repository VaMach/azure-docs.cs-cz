---
title: "Do vaší služby VSTS průběžnou integraci a doručení kanálu integrovat Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak integrovat Azure DevTest Labs do vaší služby VSTS průběžnou integraci a doručení kanálu"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: tarcher
ms.openlocfilehash: 3cd6939e890f59e7c5b188edd8620da5c9fcc935
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="integrate-azure-devtest-labs-into-your-vsts-continuous-integration-and-delivery-pipeline"></a>Integrovat Azure DevTest Labs do vaší služby VSTS průběžnou integraci a doručení kanálu
Můžete použít **rozšíření Azure DevTest Labs úloh** nainstalován ve Visual Studio Team Services (služby VSTS) snadno integrovat buildu CI/CD a kanál pomocí Azure DevTest Labs verzi. Instalaci rozšíření je tři úlohy k vytvoření virtuálního počítače, vytvořte vlastní image z virtuálního počítače a odstranění virtuálního počítače. Tento proces usnadňuje, například rychle nasadit "zlaté image" pro konkrétní testovací úlohu, a potom ho odstranit až po dokončení testu.

Tento článek ukazuje, jak vytvořit a nasadit virtuální počítač, vytvořit vlastní image a pak odstraňte virtuální počítač, všechny jako jeden kanál dokončení. Obvykle ale byste použili úlohy jednotlivě v vlastní vlastní sestavení testovací nasazení kanálu.

## <a name="before-you-begin"></a>Než začnete
Před svůj kanál CI/CD může integrovat Azure DevTest Labs, musíte nejprve nainstalovat rozšíření Visual Studio Marketplace.
1. Přejděte na [Azure DevTest Labs úlohy](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)
1. Zvolte instalace
1. Dokončit průvodce

## <a name="create-a-resource-manager-template"></a>Vytvoření šablony Resource Manageru
Tato část popisuje postup vytvoření šablony Azure Resource Manager, který použijete k vytvoření virtuálního počítače Azure na vyžádání.

1. Postupujte podle kroků v [pomocí šablony Resource Manageru](devtest-lab-use-resource-manager-template.md) k vytvoření šablony správce prostředků ve vašem předplatném.
1. Přidat [WinRM artefaktů](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) při vytváření virtuálního počítače (před vygenerováním šablony Resource Manageru).

   WinRM přístupu je potřeba použít nasazení úlohy, jako **Azure File Copy** a **prostředí PowerShell na cílových počítačích**.

   > [!NOTE]
   > Pokud používáte WinRM se sdílet IP adresou, je nutné přidat pravidlo NAT mapovat externí port na port vzdálené správy systému Windows. Nejedná vyžaduje, pokud je virtuální počítač vytvořený s veřejnou IP adresu.
   >
   >

1. Uložte šablonu jako soubor ve vašem počítači. Název souboru **CreateVMTemplate.json**.
1. Zkontrolujte šablonu, kterou do systému správy zdrojů.
1. Otevřete v textovém editoru a zkopírujte následující skript do ní.

   ```powershell
   Param( [string] $labVmId)

   $labVmComputeId = (Get-AzureRmResource -Id $labVmId).Properties.ComputeId

   # Get lab VM resource group name
   $labVmRgName = (Get-AzureRmResource -Id $labVmComputeId).ResourceGroupName

   # Get the lab VM Name
   $labVmName = (Get-AzureRmResource -Id $labVmId).Name

   # Get lab VM public IP address
   $labVMIpAddress = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
                   -Name $labVmName).IpAddress

   # Get lab VM FQDN
   $labVMFqdn = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
              -Name $labVmName).DnsSettings.Fqdn

   # Set a variable labVmRgName to store the lab VM resource group name
   Write-Host "##vso[task.setvariable variable=labVmRgName;]$labVmRgName"

   # Set a variable labVMIpAddress to store the lab VM Ip address
   Write-Host "##vso[task.setvariable variable=labVMIpAddress;]$labVMIpAddress"

   # Set a variable labVMFqdn to store the lab VM FQDN name
   Write-Host "##vso[task.setvariable variable=labVMFqdn;]$labVMFqdn"
   ```

1. Zkontrolujte skript do systému správy zdrojů. Název na něco podobného jako **GetLabVMParams.ps1**.

   Tento skript, když v agentovi spouštěna jako součást definice vydání, shromažďuje hodnoty, které potřebujete k nasazení aplikace do virtuálního počítače, pokud použijete kroky úloh, jako **Azure File Copy** nebo **prostředí PowerShell na cílových počítačích**. Obvykle používají tyto úlohy pro nasazování aplikací na virtuálním počítači Azure a vyžadují hodnoty, jako je název skupiny prostředků virtuálního počítače, IP adresu a plně kvalifikovaný název domény (FDQN).

## <a name="create-the-release-definition-in-release-management"></a>Vytvořit definici verze v Release Management
Proveďte tyto kroky k vytvoření definice verzí.

1. Otevřete **verze** kartě **sestavení a verze** rozbočovače a zvolte "**+**" ikonu.
1. V **vytvořit verze definice** dialogovém okně, vyberte **prázdný** šablony a zvolte **Další**.
1. Na další stránce vyberte **zvolte později** a potom zvolte **vytvořit** k vytvoření nové definice vydání s jedno prostředí a žádné propojené artefaktů.
1. V nové verzi definice, vyberte symbol tří teček (...) vedle názvu prostředí a otevřete v místní nabídce vyberte **nakonfigurovat proměnné**. 
1. V **konfigurovat - prostředí** dialogové okno, zadejte následující hodnoty pro proměnné použít v definici úlohy verze:
   - **vmName**: Zadejte název, který jste přiřadili k virtuálnímu počítači při vytváření šablony Resource Manageru na portálu Azure.
   - **uživatelské jméno**: Zadejte uživatelské jméno, které jste přiřadili k virtuálnímu počítači při vytváření šablony Resource Manageru na portálu Azure.
   - **heslo**: Zadejte heslo přiřazené k virtuálnímu počítači při vytváření šablony Resource Manageru na portálu Azure. Použijte na ikonu "zámku" Skrýt a zabezpečené heslo.

   <a name="create-a-vm"></a>Vytvoření virtuálního počítače
   ---

   První fáze v tomto nasazení je vytvoření virtuálního počítače, který chcete použít jako "zlatá image" pro následné nasazení. Vytvoření virtuálního počítače v rámci vaší instanci Azure DevTest Lab pomocí úloh vytvořených speciálně pro tento účel. V definici verze vyberte **+ přidat úlohy** a přidejte **Azure DevTest Labs vytvoření virtuálního počítače** úloh na kartě nasazení. Nakonfigurujte následujícím způsobem:

   V tématu [Azure DevTest Labs úlohy](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) pro vytvoření virtuálního počítače použít pro další nasazení.
   - **Předplatné Azure RM**: vybrat ze seznamu v části připojení **dostupné připojení služby Azure** nebo vytvořit více omezená oprávnění připojení k předplatnému Azure. Další informace najdete v tématu [koncový bod služby Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Název testovacího prostředí**: vyberte název instance, které jste vytvořili dříve.
   - **Název šablony**: Zadejte úplnou cestu a název souboru šablony jste uložili do vašeho úložiště zdrojového kódu. Můžete použít předdefinované vlastnosti správy verzí pro zjednodušení cestu, například:
      ```
      $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
      ```
   - **Parametry šablony**: Zadejte parametry pro proměnné definované v šabloně. Pomocí názvů proměnných definovaných v prostředí, například:
      ```
      -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
      ```
   - **Výstupní proměnné – ID virtuálního počítače testovacího prostředí**: je třeba ID nově vytvořený virtuální počítač pro následné kroky. Výchozí název proměnné prostředí, která je automaticky vyplněno s tímto ID je nastavena v **výstup proměnné** části. Můžete upravit proměnnou v případě potřeby, ale musíte použít správný název v dalších úlohách. ID virtuálního počítače testovacího prostředí je ve formátu:
      ```
      /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
      ```
1. Spusťte skript, který jste vytvořili dříve shromažďovat podrobnosti virtuálního počítače DevTest Labs. V definici verze vyberte **+ přidat úlohy** a přidejte **prostředí Azure PowerShell** úlohy z **nasadit** kartě. Úloha nakonfigurujte následujícím způsobem:

   V tématu [nasazení: Azure PowerShell](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/AzurePowerShell) a spusťte tento skript ke shromáždění podrobnosti virtuálního počítače DevTest Labs.

   - **Typ připojení Azure**: Azure Resource Manager.
   - **Předplatné Azure RM**: vybrat ze seznamu v části připojení **dostupné připojení služby Azure** nebo vytvořit více omezená oprávnění připojení k předplatnému Azure. Další informace najdete v tématu [koncový bod služby Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Typ skriptu**: soubor skriptu.
   - **Skript cesta**: Zadejte úplnou cestu a název skriptu, který jste uložili do vašeho úložiště zdrojového kódu. Můžete použít předdefinované vlastnosti správy verzí pro zjednodušení cestu, například:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   - **Argumenty skriptu**: Zadejte název proměnné prostředí, která byla automaticky vyplněna ID v prostředí virtuálního počítače předchozí úlohou, například jako argument skriptu: 
      ```
      -labVmId '$(labVMId)'
      ```
   Skript shromažďuje požadované hodnoty a ukládá je do proměnné prostředí v rámci definice vydání, tak, aby snadno najdete je v následných kroků úkolů.

1. Teď můžete nasadit aplikace do nového virtuálního počítače DevTest Labs. Úlohy, obvykle použijete k nasazení jsou **Azure File Copy** a **prostředí PowerShell na cílových počítačích**.
   - Informace o virtuálním počítači, musíte pro parametry tyto úlohy jsou uloženy v tři konfigurační proměnné s názvem **labVmRgName**, **labVMIpAddress**, a **labVMFqdn**v rámci definice verzí.
   - Pokud chcete experimentovat s vytváření virtuálního počítače DevTest Labs a vlastní image, bez nasazení aplikace do něj, můžete tento krok přeskočit.

   <a name="create-an-image"></a>Vytvoření image
   ---

   Další fáze je vytvoření bitové kopie nově nasazené virtuální počítač v Azure DevTest Labs instanci. Potom můžete tuto bitovou kopii k vytvoření kopie virtuálního počítače na vyžádání, vždy, když chcete spustit úlohu dev nebo některé testy. V definici verze vyberte **+ přidat úlohy** a přidejte **Azure DevTest Labs vytvořit vlastní Image** úlohy z **nasadit** kartě. Nakonfigurujte následujícím způsobem:

   V tématu [Azure DevTest Labs úlohy](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) na vytvoření bitové kopie.
   - **Předplatné Azure RM**: vybrat ze seznamu v části připojení **dostupné připojení služby Azure** nebo vytvořit více omezená oprávnění připojení k předplatnému Azure. Další informace najdete v tématu [koncový bod služby Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Název testovacího prostředí**: vyberte název instance, které jste vytvořili dříve.
   - **Vlastní název bitové kopie**: Zadejte název pro vlastní image.
   - **Popis**: Volitelně zadejte popis, který usnadnit výběr správné bitové kopie později.
   - **Zdrojové prostředí virtuálního počítače – ID virtuálního počítače zdrojového prostředí**: Pokud jste změnili výchozí název proměnné prostředí, která se vyplní automaticky s ID v prostředí virtuálního počítače podle předchozích úloh, upravte ho sem. Výchozí hodnota je *$(labVMId)*.
   - **Výstupní proměnné - ID bitové kopie vlastní**: je třeba ID nově vytvořené bitové kopie, pokud chcete spravovat nebo odstranit. Výchozí název proměnné prostředí, která je automaticky vyplněno s tímto ID je nastavena v **výstup proměnné** části. V případě potřeby můžete upravit proměnnou.
   
   <a name="delete-the-vm"></a>Odstranění virtuálního počítače
   ---
   
   Závěrečná fáze v tomto příkladu je odstranit virtuální počítač nasadit v Azure DevTest Labs instanci. Po spuštění úlohy dev nebo spustit testy, které potřebujete pro nasazený virtuální počítač se obvykle odstranit virtuální počítač. V definici verze vyberte **+ přidat úlohy** a přidejte **Azure DevTest Labs odstranit počítač** úlohy z **nasadit** kartě. Nakonfigurujte následujícím způsobem:

   V tématu [Azure DevTest Labs úlohy](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) odstranit virtuální počítač.
   - **Předplatné Azure RM**: vybrat ze seznamu v části připojení **dostupné připojení služby Azure** nebo vytvořit více omezená oprávnění připojení k předplatnému Azure. Další informace najdete v tématu [koncový bod služby Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **ID virtuálního počítače testovacího prostředí**: Pokud jste změnili výchozí název proměnné prostředí, která se vyplní automaticky s ID v prostředí virtuálního počítače podle předchozích úloh, upravte ho sem. Výchozí hodnota je *$(labVMId)*.
1. Zadejte název pro definici verze a uložte ho.
1. Vytvořit novou verzi, vyberte na nejnovější verzi a nasadíte ho do jednoho prostředí v definici.

V každé fázi aktualizujte zobrazení vaší instance DevTest Labs na portálu Azure virtuálních počítačů a vytváření bitové kopie a virtuální počítač se znovu odstranit.

Vlastní image teď můžete vytvořit virtuální počítače v případě potřeby.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [vytvoření prostředí více virtuálních počítačů pomocí šablony Resource Manageru](devtest-lab-create-environment-from-arm.md).
* Prozkoumejte další šablony Resource Manageru rychlý start pro automatizaci DevTest Labs z [veřejného úložiště DevTest Labs GitHub](https://github.com/Azure/azure-quickstart-templates).
* V případě potřeby, najdete v článku [řešení potíží s služby VSTS](https://docs.microsoft.com/vsts/build-release/actions/troubleshooting) stránky.
