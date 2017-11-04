---
title: "Vytvořte virtuální počítač Azure s Accelerated sítě | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač pomocí Accelerated sítě."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e887230a102f5c6289ca2eec0e4700a0e1fdfde
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a>Vytvoření virtuálního počítače pomocí Accelerated sítě

V tomto kurzu zjistěte, jak vytvořit virtuální počítač Azure (VM) pomocí Accelerated sítě. Zrychlený sítě je GA pro systém Windows a ve veřejné verzi Preview pro konkrétní distribuce systému Linux. Zrychlený sítě umožňuje jeden kořenový vstupně-výstupních operací virtualizace (SR-IOV) na virtuální počítače, výrazně zlepšit sítě. Tato cesta vysoce výkonné obchází hostitel datapath snižuje latence, zpoždění a využití procesoru pro použití s nejnáročnější zatížení sítě v podporované typy virtuálních počítačů. Následující obrázek znázorňuje komunikaci mezi dva virtuální počítače (VM) s i bez Zrychlený sítě:

![Porovnání](./media/virtual-network-create-vm-accelerated-networking/image1.png)

Bez Zrychlený sítě, musí procházet všechny síťové přenosy do/z virtuálního počítače hostitele a virtuálního přepínače. Virtuální přepínač poskytuje všechny vynucení zásad, například skupin zabezpečení sítě, seznamy řízení přístupu, izolace a dalším službám síťové virtualizované síťový provoz. Další informace o virtuální přepínače, přečtěte si [virtualizace sítě Hyper-V a virtuálního přepínače](https://technet.microsoft.com/library/jj945275.aspx) článku.

S Zrychlený sítě síťový provoz dorazí na rozhraní sítě Virtuálního počítače (NIC) a předá ji virtuálního počítače. Všechny zásady sítě, které se vztahuje na virtuální přepínač bez Zrychlený sítě se sníženou zátěží a použijí v hardwaru. Použití zásad v hardwaru umožňuje síťový adaptér přesměrovat provoz sítě přímo k virtuálnímu počítači, obcházení hostitele a virtuálního přepínače, při zachování všech zásad, které se použijí v hostiteli.

Výhody Zrychlený sítě se vztahují pouze na virtuálním počítači, který je zapnutá. Nejlepších výsledků dosáhnete je ideální pro povolení této funkce na alespoň dva virtuální počítače připojené ke stejné virtuální síti Azure (VNet). Při komunikaci mezi virtuálními sítěmi nebo připojování místní, tato funkce má minimální dopad na celkové latence.

> [!WARNING]
> Tuto verzi Public Preview Linux nemusí mít stejnou úroveň dostupnost a spolehlivost, podle verze funkce, které jsou obecné dostupnosti. Funkce není podporována, může mít omezené možnosti a nemusí být k dispozici ve všech Azure umístění. Nejaktuálnější upozornění na stav této funkce a dostupnost zkontrolujte stránku Aktualizace virtuální sítě Azure.

## <a name="benefits"></a>Výhody
* **Nižší latenci vyšší pakety za sekundu (pps):** odebráním virtuálního přepínače datapath odebere čas, který potřebují paketů v hostiteli pro zpracování zásad a zvyšuje počet paketů, které lze zpracovat ve virtuálním počítači.
* **Snižuje zpoždění:** virtuální přepínač zpracování závisí na množství zásady, které je nutné použít a zatížení procesoru, který provádí zpracování. Snižování zátěže vynucení zásad do hardwaru, odstraní tento variabilita tím, že doručování paketů přímo k virtuálnímu počítači, odebrání hostitele do komunikace virtuálních počítačů a všechny softwaru přerušení a kontext přepínače.
* **Snížení využití procesoru:** obcházení virtuální přepínač na hostiteli vede k menší využití procesoru při zpracování síťového provozu.

## <a name="Limitations"></a>Omezení
Při použití této funkce, existují tato omezení:

* **Vytvoření rozhraní sítě:** Accelerated sítě lze povolit pouze pro nový síťový adaptér. Nelze nastavit pro existující síťovou.
* **Vytvoření virtuálního počítače:** A síťovým Adaptérem s Zrychlený sítě povolené lze připojit pouze k virtuálnímu počítači, když je vytvořen virtuální počítač. Síťový adaptér nelze připojit k existující virtuální počítač.
* **Oblasti:** virtuálních počítačů Windows s Zrychlený sítě jsou nabízena v oblastech nejvíce Azure. Virtuální počítače Linux s Zrychlený sítě jsou nabízena v několika oblastech. Oblasti, které tato funkce je dostupná v zvětšuje. Naleznete v blogu aktualizace virtuální sítě na Azure pod nejnovější informace.   
* **Podporované operační systémy:** Windows: Microsoft Windows Server 2012 R2 Datacenter a Windows Server 2016. Linux: Ubuntu Server 16.04 LTS s 4.4.0-77 jádra nebo vyšší, SLES 12 SP2, RHEL 7.3 a CentOS 7.3 (publikováno "Podvodný Wave software").
* **Velikost virtuálního počítače:** velikost optimalizované výpočetní instance s osmi nebo více jader a obecné účely. Další informace najdete v tématu [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů. Sadu podporované velikosti instance virtuálních počítačů bude v budoucnu rozšířit.
* **Nasazení pomocí Azure Resource Manager (ARM) pouze:** Accelerated síťové služby není k dispozici pro nasazení prostřednictvím ASM/RDFE.

Změny těchto omezení jsou oznámeny prostřednictvím [virtuální sítí Azure aktualizace](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview/) stránky.

## <a name="create-a-windows-vm"></a>Vytvoření virtuálního počítače s Windows
Můžete použít portál Azure nebo Azure [prostředí PowerShell](#windows-powershell) vytvořte virtuální počítač.

### <a name="windows-portal"></a>Portál

1. Z internetového prohlížeče, otevřete Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Na portálu, klikněte na tlačítko **+ nový** > **výpočetní** > **Windows Server 2016 Datacenter**. 
3. V **Windows Server 2016 Datacenter** okno, které se zobrazí, ponechejte *Resource Manager* vybrané pod **vybrat model nasazení**a klikněte na tlačítko **vytvořit** .
4. V **Základy** okno, které se zobrazí, zadejte následující hodnoty, ponechejte zbývající výchozí možnosti nebo vyberte nebo zadejte vlastní hodnoty a klikněte **OK** tlačítko:

    |Nastavení|Hodnota|
    |---|---|
    |Name (Název)|Můjvp|
    |Skupina prostředků|Nechte **vytvořit nový** vybrané a zadejte *MyResourceGroup*|
    |Umístění|Západní USA 2|

    Pokud jste Azure ještě nepoužívali, další informace o [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), a [umístění](https://azure.microsoft.com/regions) (které jsou také označovány jako oblasti).
5. V **zvolte velikost** okno, které se zobrazí, zadejte *8* v **minimální počet jader** pole a pak klikněte na **zobrazit všechny**.
6. Klikněte na tlačítko **DS4_V2 standardní**, nebo podporují se všechny virtuální počítač, klikněte **vyberte** tlačítko.
7. V **nastavení** okno, které se zobrazí, ponechejte všechna nastavení jako-je kromě klikněte na tlačítko **povoleno** pod **Accelerated sítě**, klikněte **OK** tlačítko. **Poznámka:** , pokud v předchozích krocích jste vybrali hodnoty pro velikost virtuálního počítače, operační systém nebo umístění, které nejsou uvedené v [omezení](#Limitations) tohoto článku **Accelerated sítě** není viditelné.
8. V **Souhrn** okno, které se zobrazí, klikněte **OK** tlačítko. Azure začne vytvářet virtuální počítač. Vytvoření virtuálního počítače trvá několik minut.
9. Pokud chcete nainstalovat Zrychlený síťové ovladače pro Windows, proveďte kroky v [konfigurace systému Windows](#configure-windows) tohoto článku.

### <a name="windows-powershell"></a>Prostředí PowerShell
1. Nainstalujte nejnovější verzi prostředí Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste prostředí Azure PowerShell nové, přečtěte si [Přehled prostředí Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
2. Kliknutím na tlačítko Start systému Windows spusťte relaci prostředí PowerShell zadáním **prostředí powershell**, pak levým na **prostředí PowerShell** ve výsledcích hledání.
3. V okně prostředí PowerShell, zadejte `login-azurermaccount` příkaz se přihlásit pomocí vašeho Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
4. V prohlížeči zkopírujte následující skript:
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. V okně prostředí PowerShell klikněte pravým tlačítkem na vložte skript a spusťte ho spustíte. Zobrazí se výzva k zadání uživatelského jména a hesla. Použijte tyto přihlašovací údaje pro přihlášení do virtuálního počítače při připojování k němu v dalším kroku. Pokud skript selže a změnit hodnoty ve skriptu před jeho provedením, potvrďte hodnoty používané pro velikost virtuálního počítače, operační systém, a umístění jsou uvedeny v [omezení](#Limitations) tohoto článku.
6. Pokud chcete nainstalovat Zrychlený síťové ovladače pro Windows, proveďte kroky v [konfigurace systému Windows](#configure-windows) tohoto článku.

### <a name="configure-windows"></a>Konfigurace systému Windows
Po vytvoření virtuálního počítače v Azure, je nutné nainstalovat Zrychlený síťové ovladače pro Windows. Před dokončením následujících kroků, kterou jste vytvořili virtuální počítač s Windows v Zrychlený sítí pomocí [portál](#windows-portal) nebo [prostředí PowerShell](#windows-powershell) kroky v tomto článku. 

1. Z internetového prohlížeče, otevřete Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *Můjvp*. Když **Můjvp** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **Můjvp** okno, které se zobrazí, klikněte **Connect** tlačítko v levém horním rohu okna. **Poznámka:** Pokud **vytváření** se zobrazí v části **Connect** tlačítko Azure ještě nebyla dokončena vytváření virtuálního počítače. Klikněte na tlačítko **připojit** až poté, co se již nezobrazují **vytváření** pod **Connect** tlačítko.
4. Povolit prohlížeč ke stažení **MyVm.rdp** souboru.  Po stažení, klikněte na soubor otevřete. 
5. Klikněte na tlačítko **připojit** v tlačítko **připojení ke vzdálené ploše** pole, který se zobrazí upozornění, že nelze identifikovat vydavatele vzdáleného připojení.
6. V **zabezpečení systému Windows** pole, které se zobrazí, klikněte na tlačítko **další možnosti**, pak klikněte na tlačítko **použít jiný účet**. Zadejte uživatelské jméno a heslo, které jste zadali v kroku 4 a pak klikněte na **OK** tlačítko.
7. Klikněte **Ano** tlačítko v poli připojení ke vzdálené ploše, který se zobrazí upozornění, že nelze ověřit identitu vzdáleného počítače.
8. Klikněte pravým tlačítkem na tlačítko Start systému Windows a klikněte na tlačítko **Správce zařízení**. Rozbalte **síťové adaptéry** uzlu. Potvrďte, že **Mellanox ConnectX 3 virtuální adaptér Ethernet funkce** se zobrazí, jak je znázorněno na následujícím obrázku:
   
    ![Správce zařízení](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. Pro virtuální počítač je nyní k dispozici Zrychlený sítě.

## <a name="create-a-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem
Můžete použít portál Azure nebo Azure [prostředí PowerShell](#linux-powershell) pro vytvoření aplikace Ubuntu nebo SLES virtuálních počítačů. RHEL a CentOS virtuální počítače se jiný pracovní postup.  Podrobnosti najdete níže uvedených pokynů.

### <a name="linux-portal"></a>Portál
1. Zaregistrovat pro Zrychlený sítě pro Linux preview provedením kroků 1 až 5 z [vytvoření virtuálního počítače s Linuxem - PowerShell](#linux-powershell) tohoto článku.  Nelze zaregistrovat na portálu ve verzi Preview.
2. Dokončete kroky 1-8 v [vytvoření virtuálního počítače s Windows – portál](#windows-portal) tohoto článku. V kroku 2, klikněte na tlačítko **Ubuntu Server 16.04 LTS** místo **Windows Server 2016 Datacenter**. V tomto kurzu rozhodnete použít heslo, nikoli klíč SSH, i když pro nasazení v produkčním prostředí, můžete použít buď. Pokud **Accelerated sítě** po dokončení kroku 7 není k dispozici [vytvoření virtuálního počítače s Windows – portál](#windows-portal) části tohoto článku je pravděpodobně pro jednu z následujících důvodů:
    - Nejsou ještě registrované ve verzi Preview. Potvrďte, že je váš stav registrace **registrovaná**, jak je popsáno v kroku 4 [vytvoření virtuálního počítače s Linuxem - Powershell](#linux-powershell) tohoto článku. **Poznámka:** Pokud jste se účastnili Accelerated sítě pro virtuální počítače Windows preview (je už nezbytné k registraci a k použití Accelerated sítě virtuálních počítačů pro Windows), které nejsou automaticky registrované pro Accelerated sítě pro virtuální počítače s Linuxem Zobrazte náhled. Je třeba zaregistrovat pro Accelerated sítě pro virtuální počítače s Linuxem preview k účasti v něm.
    - Nevybrali jste velikost virtuálního počítače, operační systém, nebo umístění uvedené v [omezení](#limitations) tohoto článku.
3. Instalace Zrychlený síťové ovladače pro Linux, proveďte kroky v [konfigurace Linux](#configure-linux) tohoto článku.

### <a name="linux-powershell"></a>Prostředí PowerShell

>[!WARNING]
>Pokud vytvoříte virtuální počítače s Linuxem se Zrychlený sítěmi v odběru a poté se pokusí vytvořit virtuální počítač s Windows s Zrychlený sítě ve stejném předplatném, vytvoření virtuálního počítače Windows se pravděpodobně nezdaří. Během této verzi preview se doporučuje otestovat Linux a virtuálních počítačů Windows s Zrychlený sítě v samostatných předplatných.
>

1. Nainstalujte nejnovější verzi prostředí Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste prostředí Azure PowerShell nové, přečtěte si [Přehled prostředí Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
2. Kliknutím na tlačítko Start systému Windows spusťte relaci prostředí PowerShell zadáním **prostředí powershell**, pak levým na **prostředí PowerShell** ve výsledcích hledání.
3. V okně prostředí PowerShell, zadejte `login-azurermaccount` příkaz se přihlásit pomocí vašeho Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Registrace pro Zrychlený sítě pro Azure preview podle následujících kroků:
    - E-mailovou zprávu na [ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) s ID předplatného Azure a zamýšlené použití. Počkejte na potvrzení e-mailu společnosti Microsoft o vaše předplatné povolený.
    - Zadejte následující příkaz a ověřte, zda že jste zaregistrováni pro verzi preview:
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        Pokračujte krokem 5 až **registrovaná** po zadání příkazu předchozí se zobrazí ve výstupu. Výstupu musí před pokračováním vypadat podobně jako následující výstup:
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      >Pokud jste se účastnili Accelerated sítě pro virtuální počítače Windows preview (je už nezbytné k registraci a k použití Accelerated sítě pro virtuální počítače Windows), můžete nejsou registrované automaticky Accelerated sítě pro virtuální počítače s Linuxem náhled. Je třeba zaregistrovat pro Accelerated sítě pro virtuální počítače s Linuxem preview k účasti v něm.
      >
5. V prohlížeči zkopírujte následující skript podle potřeby nahraďte Ubuntu nebo SLES.  Znovu Redhat a CentOS mít jiný pracovní postup uvedené níže:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define the new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. V okně prostředí PowerShell klikněte pravým tlačítkem na vložte skript a spusťte ho spustíte. Zobrazí se výzva k zadání uživatelského jména a hesla. Použijte tyto přihlašovací údaje pro přihlášení do virtuálního počítače při připojování k němu v dalším kroku. Pokud skript selže, potvrďte, že:
    - Jste zaregistrováni pro verzi preview, jak je popsáno v kroku 4
    - Pokud jste změnili velikost, typ operačního systému nebo hodnoty umístění ve skriptu virtuálního počítače před jeho provedením, že hodnoty jsou uvedeny v [omezení](#Limitations) tohoto článku.
7. Instalace Zrychlený síťové ovladače pro Linux, proveďte kroky v [konfigurace Linux](#configure-linux) tohoto článku.

### <a name="configure-linux"></a>Konfigurace systému Linux

Po vytvoření virtuálního počítače v Azure, je nutné nainstalovat Zrychlený síťové ovladače pro Linux. Před dokončením následujících kroků, kterou jste vytvořili virtuální počítač s Linuxem v Zrychlený sítí pomocí [portál](#linux-portal) nebo [prostředí PowerShell](#linux-powershell) kroky v tomto článku. 

1. Z internetového prohlížeče, otevřete Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. V horní části portálu napravo od *vyhledávání prostředků* panelu, klikněte na tlačítko **> _** ikonu můžete spustit prostředí Bash cloudu (Preview). V podokně cloudové prostředí Bash se zobrazuje v dolní části portálu a za několik sekund, uvede  **username@Azure:~ $** řádku. I když můžete SSH pro virtuální počítač z počítače, nikoli cloudové prostředí, pokyny v tomto kurzu se předpokládá, že používáte prostředí cloudu.
3. V horní části portálu, do pole obsahující text *vyhledávání prostředků*, typ *Můjvp*. Když **Můjvp** se zobrazí ve výsledcích hledání klikněte na něj.
4. V **Můjvp** okno, které se zobrazí, klikněte **Connect** tlačítko v levém horním rohu okna. **Poznámka:** Pokud **vytváření** se zobrazí v části **Connect** tlačítko Azure ještě nebyla dokončena vytváření virtuálního počítače. Klikněte na tlačítko **připojit** až poté, co se již nezobrazují **vytváření** pod **Connect** tlačítko.
5. Pole s výzvou k zadání otevře Azure `ssh adminuser@<ipaddress>`. Zadejte tento příkaz cloudového prostředí (nebo kopírování z pole, která objevil v kroku 4 a vložte jej do prostředí cloudu), stiskněte klávesu Enter.
6. Zadejte **Ano** na otázku s dotazem, pokud chcete pokračovat, připojení, potom stisknete Enter.
7. Zadejte heslo, které jste zadali při vytváření virtuálního počítače. Po úspěšném přihlášení k virtuálnímu počítači, uvidíte adminuser@MyVm:~ $ řádku. Nyní jste přihlášeni k virtuálnímu počítači prostřednictvím relace prostředí cloudu. **Poznámka:** cloudové prostředí relace vyprší po 10 minutách nečinnosti.

V tomto okamžiku lišit podle pokynů v závislosti na distribuce, kterou používáte. 

#### <a name="ubuntusles"></a>Ubuntu/SLES

1. Do příkazového řádku zadejte `uname -r` a ověřit verzi pro:

    * Ubuntu je "4.4.0-77-generic", nebo vyšší
    * SLES je "4.4.59-92.20-default" nebo vyšší

2. Vytvoření vazby mezi standardní síťového adaptéru vNIC a Zrychlený síťového adaptéru vNIC spuštěním příkazů, které následují. Síťový provoz používá vyšší provádění Zrychlený sítě vNIC při dokumentových zajistí, že síťové přenosy proces se nepřerušil napříč změny konfigurace.
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. Po spuštění skriptu, virtuální počítač se restartuje po pozastavení 60 sekund.
4. Po restartování virtuálního počítače, znovu ho podle kroků 5 až 7 znovu.
5. Spustit `ifconfig` příkaz a zkontrolujte, jestli má bond0 spolu a rozhraní se zobrazuje jako nahoru. 
 
 >[!NOTE]
      >Aplikace, které používají Zrychlený sítě musí komunikovat přes *bond0* rozhraní není *eth0*.  Název rozhraní může změnit než Zrychlený sítě dosáhne obecné dostupnosti.

#### <a name="rhelcentos"></a>RHEL nebo CentOS

Vytváření Red Hat Enterprise Linux nebo CentOS 7.3 virtuálního počítače vyžaduje několik kroků navíc načíst nejnovější ovladače potřebné pro rozhraní SR-IOV a ovladač virtuální funkce (VF) pro síťové karty. První fáze pokynů připraví image, které je možné, aby jeden nebo více virtuálních počítačů, které jsou předem načtená ovladače.

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a>Fáze 1: Příprava Red Hat Enterprise Linux nebo CentOS 7.3 základní image. 

1.  Zřídit jiný - SR-IOV CentOS 7.3 virtuálního počítače v Azure

2.  Instalovat 4.2.2:
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  Stáhnout konfigurační soubory
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  Zrušení zřízení tohoto virtuálního počítače

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  Z portálu Azure Zastavit tento virtuální počítač; a přejděte do Virtuálního počítače "disky", zaznamenat URI virtuálního pevného disku OSDisk. Tento identifikátor URI obsahuje název základní image virtuálního pevného disku a jeho účet úložiště. 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a>Fáze 2: zřizovat nové virtuální počítače v Azure

1.  Zřídit nové virtuální počítače na základě pomocí New-AzureRMVMConfig pomocí základní image virtuálního pevného disku zaznamenané v první fázi, s AcceleratedNetworking zapnuta vNIC:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify the base image's VHD URI (from phase one step 5). 
    # Note: The storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need to replace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for the location from which the new image binary large object (BLOB) is copied to start the virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create the virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  Po spustit virtuální počítače, ověřte VF zařízení podle "lspci" a zkontrolujte položku Mellanox. Například bychom měli vidět této položky ve výstupu lspci:
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  Spusťte skript záruční:

    ```bash
    sudo bondvf.sh
    ```

4.  Restartujte nové virtuální počítače:

    ```bash
    sudo reboot
    ```

Virtuální počítač by měl začínat bond0 nakonfigurované a cestu Accelerated sítě povolené.  Spustit `ifconfig` k potvrzení.
