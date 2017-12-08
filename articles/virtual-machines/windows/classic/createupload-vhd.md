---
title: "Vytvoření a nahrání image virtuálního počítače pomocí prostředí Powershell | Microsoft Docs"
description: "Naučte se vytvořit a odeslat zobecněný Windows serverovou bitovou kopii (VHD) pomocí modelu nasazení classic a Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: c2540120bcb1eca9f4ba62c7dbc0675343bf4f99
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/08/2017
---
# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Vytvoření virtuálního pevného disku s Windows Serverem a jeho nahrání do Azure
Tento článek ukazuje, jak nahrát vlastní zobecněný image virtuálního počítače jako virtuální pevný disk (VHD), takže ho můžete použít k vytvoření virtuálních počítačů. Další podrobnosti o disky a virtuální pevné disky ve službě Microsoft Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Můžete také [nahrát](../upload-generalized-managed.md) virtuálního počítače pomocí modelu Resource Manager.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte:

* **Předplatné Azure** – pokud ji nemáte, můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* **[Microsoft Azure PowerShell](/powershell/azure/overview)**  -máte modulu Microsoft Azure PowerShell nainstalovaný a nakonfigurovaný na používání vašeho předplatného.
* **A. Soubor VHD** – podporované Windows operačního systému uložené v souboru VHD a připojen k virtuálnímu počítači. Zkontrolujte, jestli se role serveru, který je spuštěn na virtuální pevný disk podporovaný pomocí nástroje Sysprep. Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

    > [!IMPORTANT]
    > Formát VHDX není podporované v Microsoft Azure. Disk můžete převést do formátu virtuálního pevného disku pomocí Správce technologie Hyper-V nebo [rutiny Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx). Podrobnosti najdete v tématu to [blogový příspěvek](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Krok 1: Příprava virtuálního pevného disku
Než budete nahrávat VHD do Azure, musí být zobecněn pomocí nástroje Sysprep. To připraví virtuální pevný disk, který se má použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx). Zálohování virtuálního počítače před spuštěním nástroje Sysprep.

Z virtuálního počítače, který byl pro nainstalovaný operační systém proveďte následující postup:

1. Přihlaste se k operačnímu systému.
2. Otevřete okno příkazového řádku jako správce. Změňte adresář na **%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.

    ![Otevřete okno příkazového řádku](./media/createupload-vhd/sysprep_commandprompt.png)
3. **Nástroj pro přípravu systému** zobrazí se dialogové okno.

   ![Spusťte nástroj Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. V **nástroj pro přípravu systému**, vyberte **zadejte systému mimo pole prostředí Spouštěného** a ujistěte se, že **generalizace** je zaškrtnuté.
5. V **možnosti vypnutí**, vyberte **vypnutí**.
6. Klikněte na **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Krok 2: Vytvoření účtu úložiště a kontejner
Účet úložiště v Azure je nutné, abyste získali místo, kde můžete nahrát soubor VHD. Tento krok ukazuje, jak vytvořit účet, nebo získat informace, které je nutné z existující účet. Nahrazení proměnné v &lsaquo; závorky &rsaquo; nahraďte svými vlastními informacemi.

1. Přihlásit

    ```powershell
    Add-AzureAccount
    ```

2. Nastavte vašeho předplatného Azure.

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. Vytvořte nový účet úložiště. Název účtu úložiště musí být jedinečný, 3 až 24 znaků. Název může být libovolnou kombinací písmen a číslic. Budete taky muset zadat umístění jako "Východ USA"

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. Nový účet úložiště nastavte jako výchozí.

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. Vytvořte nový kontejner.

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-the-vhd-file"></a>Krok 3: Odeslání souboru VHD
Použití [přidat AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) odeslání disku VHD.

V okně Azure PowerShell jste použili v předchozím kroku, zadejte následující příkaz a nahraďte proměnné v &lsaquo; závorky &rsaquo; nahraďte svými vlastními informacemi.

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Krok 4: Přidejte bitovou kopii do seznamu vlastních bitových kopií
Použití [přidat AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) rutiny přidat bitovou kopii do seznamu vlastních bitových kopií.

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a>Další kroky
Teď můžete [vytvořit vlastní virtuální](createportal.md) pomocí bitové kopie, který jste nahráli.
