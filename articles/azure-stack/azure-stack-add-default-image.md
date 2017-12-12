---
title: "Přidat výchozí image virtuálních počítačů do Azure Marketplace zásobníku | Microsoft Docs"
description: "Přidejte výchozí image virtuálního počítače Windows serveru 2016 do Azure Marketplace zásobníku."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 2849E53F-3D58-48A5-8007-3238FC39F630
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: mabrigg
ms.openlocfilehash: ed62f2f8441220eb37aea7f4c848702e9821698b
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="add-the-windows-server-2016-vm-image-to-the-azure-stack-marketplace"></a>Přidat bitovou kopii virtuálního počítače Windows serveru 2016 do Azure Marketplace zásobníku

Ve výchozím nastavení se žádné Image virtuálního počítače jsou k dispozici v Azure Marketplace zásobníku. Operátor zásobník Azure musíte přidat bitovou kopii na Marketplace s cílem pro přístup k uživatelům. Můžete přidat bitovou kopii systému Windows Server 2016 pro Azure Marketplace zásobníku pomocí jedné z následujících metod:

* [Stáhnout bitovou kopii z Azure Marketplace](#add-the-image-by-downloading-it-from-the-azure-marketplace). Tuto možnost použijte, pokud pracujete v připojených scénář a zaregistrovali vaší instanci Azure zásobník Azure.

* [Přidejte bitovou kopii pomocí prostředí PowerShell](#add-the-image-by-using-powershell). Tuto možnost použijte, pokud jste nasadili zásobník Azure ve scénáři odpojené, nebo ve scénářích s omezené připojení.

## <a name="add-the-image-by-downloading-it-from-the-azure-marketplace"></a>Přidání bitové kopie stažením z Azure Marketplace

1. Nasaďte zásobník Azure a pak se přihlaste k vaší Azure zásobníku Development Kit.

2. Vyberte **další služby** > **Marketplace správu** > **přidat z Azure**. 

3. Hledání nebo nalezení **Windows Server 2016 Datacenter – Eval** bitovou kopii a potom vyberte **Stáhnout**.

   ![Stáhnout bitovou kopii z Azure](media/azure-stack-add-default-image/download-image.png)

Po dokončení stahování bitová kopie je k dispozici v části **Marketplace správu**. Obrázek je také k dispozici v části **virtuální počítače**.

## <a name="add-the-image-by-using-powershell"></a>Přidejte bitovou kopii pomocí prostředí PowerShell

### <a name="prerequisites"></a>Požadavky 

Spusťte následující předpoklady, buď z [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) nebo externí klienta se systémem Windows, pokud jste [připojení prostřednictvím VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

1. Nainstalujte [modulů prostředí Azure PowerShell kompatibilní s Azure zásobníku](azure-stack-powershell-install.md).  

2. Stažení [nástroje potřebné pro práci s Azure zásobníku](azure-stack-powershell-download.md).  

3. Na stránce hodnocení serveru Windows [stažení zkušební verze systému Windows Server 2016](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016). Po zobrazení výzvy vyberte verzi ISO stahování. Poznamenejte si cestu k umístění stahování, který je používán později kroků popsaných v tomto článku. Tento krok vyžaduje připojení k Internetu.  

### <a name="add-the-image-to-the-azure-stack-marketplace"></a>Přidání bitové kopie do Azure Marketplace zásobníku
   
1. Naimportujte moduly Azure připojit zásobníku a ComputeAdmin pomocí následujících příkazů:

   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.   
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   ```

2. Přihlaste se do prostředí Azure zásobníku. Spusťte jeden z následujících skriptů, v závislosti na tom, jestli jste nasadili prostředí zásobníku Azure pomocí Azure Active Directory (Azure AD) nebo Active Directory Federation Services (AD FS). (Nahraďte Azure AD `tenantName`, `GraphAudience` koncovému bodu, a `ArmEndpoint` hodnoty tak, aby odrážela konfiguraci prostředí.)  

   * **Azure Active Directory**. Použijte následující rutinu:

    ```PowerShell
    # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Resource Manager endpoint for your environment>"

    # For Azure Stack Development Kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
    $GraphAudience = "<GraphAuidence endpoint for your environment>"
    
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Login-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID 
    ```

   * **Služba Active Directory Federation Services**. Použijte následující rutinu:
    
    ```PowerShell
    # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Resource Manager endpoint for your environment>"

    # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
    $GraphAudience = "<GraphAuidence endpoint for your environment>"

    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience `
      -EnableAdfsAuthentication:$true

   $TenantID = Get-AzsDirectoryTenantId `
     -ADFS `
     -EnvironmentName "AzureStackAdmin" 

    Login-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID 
    ```
   
3. Přidejte bitovou kopii systému Windows Server 2016 do Azure Marketplace zásobníku. (Nahraďte *fully_qualified_path_to_ISO* cestou k Windows serveru 2016 ISO, který jste si stáhli.)

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

Zajistěte, aby bitovou kopii virtuálního počítače Windows serveru 2016 nejnovější kumulativní aktualizace, zahrňte `IncludeLatestCU` parametr při spuštění `New-AzsServer2016VMImage` rutiny. Informace o povolených parametry `New-AzsServer2016VMImage` rutiny, najdete v části [parametry](#parameters). Jak dlouho trvá přibližně jednu hodinu publikovat bitovou kopii do Azure Marketplace zásobníku. 

## <a name="parameters"></a>Parametry

|Nové AzsServer2016VMImage parametry|Povinné?|Popis|
|-----|-----|------|
|ISOPath|Ano|Úplná cesta k stažené ISO systému Windows Server 2016.|
|Net35|Ne|Modul runtime rozhraní .NET 3.5 nainstaluje bitovou kopii systému Windows Server 2016. Ve výchozím nastavení je tato hodnota nastavena **true**.|
|Verze|Ne|Určuje **základní**, **úplné**, nebo **i** bitové kopie systému Windows Server 2016. Ve výchozím nastavení je tato hodnota nastavena **úplné**.|
|VHDSizeInMB|Ne|Nastaví velikost image virtuálního pevného disku, které mají být přidány do prostředí Azure zásobníku (v MB). Ve výchozím nastavení je tato hodnota nastavena na 40 960 MB.|
|CreateGalleryItem|Ne|Určuje, jestli musíte vytvořit položku Marketplace pro bitovou kopii systému Windows Server 2016. Ve výchozím nastavení je tato hodnota nastavena **true**.|
|location |Ne |Určuje umístění, ke které je nutné ji publikovat bitovou kopii systému Windows Server 2016.|
|IncludeLatestCU|Ne|Nejnovější kumulativní aktualizaci Windows Server 2016 se vztahuje na nový virtuální pevný disk.|
|CUUri |Ne |Sady Windows Server 2016 kumulativní aktualizace pro spouštění z konkrétní identifikátoru URI. |
|CUPath |Ne |Sady Windows Server 2016 kumulativní aktualizace pro spouštění z místní cestu. Tato možnost je užitečná, pokud jste nasadili instanci Azure zásobníku v odpojeném prostředí.|

## <a name="next-steps"></a>Další kroky

[Zřízení virtuálního počítače](azure-stack-provision-vm.md)
