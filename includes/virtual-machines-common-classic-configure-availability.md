


Sadu dostupnosti pomáhá udržovat vaše virtuální počítače, které jsou k dispozici při výpadku, například při údržbě. Umístění podobně nakonfigurované dva nebo více virtuálních počítačů v nastavení dostupnosti vytvoří redundance potřebné pro zachování dostupnost aplikace nebo služby, které běží ve virtuálním počítači. Podrobnosti o tomto postupu najdete v tématu [Správa dostupnosti virtuálních počítačů][Manage the availability of virtual machines].

Je vhodné použít skupiny dostupnosti a vyrovnávání zatížení koncové body k zajištění, že aplikace je vždy k dispozici a spuštěné efektivně. Podrobnosti o koncové body s vyrovnáváním zatížení najdete v tématu [pro infrastrukturu Azure služby Vyrovnávání zatížení][Load balancing for Azure infrastructure services].

Klasické virtuální počítače můžete přidat do dostupnosti nastavit pomocí jedné ze dvou možností:

* [Možnost 1: Vytvoření virtuálního počítače a sadu ve stejnou dobu dostupnosti][Option 1: Create a virtual machine and an availability set at the same time]. Potom přidáte nové virtuální počítače do sady, při vytváření těchto virtuálních počítačů.
* [Možnost 2: Přidání existujícího virtuálního počítače do skupiny dostupnosti][Option 2: Add an existing virtual machine to an availability set].

> [!NOTE]
> V klasickém modelu virtuální počítače, které chcete umístit do stejné skupiny dostupnosti musí patřit do stejné cloudové služby.
> 
> 

## <a id="createset"></a>Možnost 1: vytvoření virtuálního počítače a současně sadu dostupnosti
K tomu můžete portálu Azure nebo Azure PowerShell příkazy.

Použití portálu Azure:

1. Pokud jste to ještě neudělali, přihlaste se k [Portálu Azure](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **+ nový**a potom klikněte na **virtuální počítač**.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. Vyberte Marketplace image virtuálního počítače, který chcete použít. Můžete vytvořit virtuální počítač se systémem Linux nebo Windows.
4. Pro vybraný virtuální počítač, ověřte, že model nasazení je nastavena na **Classic** a pak klikněte na tlačítko **vytvořit**
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Zadejte název virtuálního počítače, uživatelské jméno a heslo (pro počítače s Windows) nebo veřejný klíč SSH (pro počítače se systémem Linux). 
6. Zvolte velikost virtuálního počítače a pak klikněte na tlačítko **vyberte** pokračujte.
7. Zvolte **volitelné konfigurace > sadu dostupnosti**, a vyberte sadu dostupnosti, které chcete přidat virtuální počítač.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Zkontrolujte nastavení konfigurace. Když jste hotovi, klikněte na tlačítko **vytvořit**.
9. Zatímco Azure vytváří virtuální počítač, můžete sledovat průběh v části **virtuální počítače** v nabídce centra.

Pomocí příkazů prostředí Azure PowerShell k vytvoření virtuálního počítače Azure a přidejte ji do nové nebo existující dostupnost sady naleznete v části [pomocí prostředí Azure PowerShell k vytvoření a nastavení virtuálních počítačích se systémem Windows](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"></a>Možnost 2: Přidání existujícího virtuálního počítače do skupiny dostupnosti
Na portálu Azure můžete přidat existující klasické virtuální počítače na existující dostupnost nastavit nebo vytvořte novou pro ně. (Mějte na paměti, že virtuální počítače ve stejné skupině dostupnosti musí patřit do stejné cloudové služby.) Kroky jsou téměř stejný. V prostředí Azure PowerShell můžete přidat virtuální počítač do stávající sady dostupnosti.

1. Pokud jste tak již neučinili, přihlaste se k [portál Azure](https://portal.azure.com).
2. V nabídce centra klikněte na tlačítko **virtuálních počítačů (klasické)**.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Seznam virtuálních počítačů vyberte název virtuálního počítače, který chcete přidat do sady.
4. Zvolte **sadu dostupnosti** z virtuálního počítače **nastavení**.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Vyberte skupinu dostupnosti, které chcete přidat virtuální počítač. Virtuální počítač musí patřit do stejné cloudové služby jako skupiny dostupnosti.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Klikněte na **Uložit**.

Chcete-li použít příkazy prostředí Azure PowerShell, otevřete relaci prostředí Azure PowerShell úrovni správce a spusťte následující příkaz. Zástupné symboly (například &lt;VmCloudServiceName&gt;), nahraďte všechna data v uvozovkách, včetně < a > znaky s správné názvy.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> Může mít virtuální počítač restartovat, aby dokončit ho přidáte do skupiny dostupnosti.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

