## <a name="overview"></a>Přehled
Při vytváření nového virtuálního počítače (VM) ve skupině prostředků nasazením bitové kopie z [Azure Marketplace](https://azure.microsoft.com/marketplace/), výchozí jednotce operačního systému je často 127 GB (některé obrázky, mívají menší velikost disku operačního systému ve výchozím nastavení). I když je možné k virtuálnímu počítači přidat datové disky (počet závisí na zvolené skladové položce) a navíc se na tyto dodatečné disky doporučuje instalovat aplikace a úlohy náročné na procesor, zákazníci často potřebují rozšířit jednotku operačního systému pro zajištění podpory určitých scénářů, jako jsou například tyto:

1. Podpora starších verzí aplikací, které instalují komponenty na jednotku operačního systému.
2. Migrace fyzického nebo virtuálního počítače s větší jednotkou operačního systému z místního prostředí.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: Resource Manager a Classic. Tento článek se týká použití modelu Resource Manager. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.
> 
> 

## <a name="resize-the-os-drive"></a>Změna velikosti jednotky operačního systému
V tomto článku provedeme změnu velikosti jednotky operačního systému pomocí modulů Resource Manageru v [Azure PowerShellu](/powershell/azureps-cmdlets-docs). Otevřete integrované skriptovací prostředí (ISE) v prostředí PowerShell nebo okno PowerShellu v režimu správy a postupujte podle následujících kroků:

1. Přihlaste se ke svému účtu Microsoft Azure v režimu správy prostředků a následujícím způsobem vyberte své předplatné:
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Nastavte název skupiny prostředků a název virtuálního počítače následujícím způsobem:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Získejte odkaz na virtuální počítač následujícím způsobem:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Před změnou velikosti disku zastavte virtuální počítač následujícím způsobem:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. A tady přichází moment, na který jsme čekali! Nastavte velikost disku operačního systému na požadovanou hodnotu a aktualizujte virtuální počítač následujícím způsobem:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > Nová velikost musí být větší než stávající velikost disku. Maximální povolený počet je 2048 GB. (Je možné rozšířit nad rámec této velikosti objektu blob virtuálního pevného disku, ale pouze operačního systému bude moct pracovat s první 2048 GB místa.)
   > 
   > 
6. Aktualizace virtuálního počítače může trvat několik sekund. Jakmile se dokončí provádění příkazu, restartujte virtuální počítač následujícím způsobem:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

A to je vše! Teď se pomocí RDP připojte k virtuálnímu počítači, otevřete Správu počítače (nebo Správu disků) a rozšiřte jednotku s použitím nově přiděleného místa.

## <a name="summary"></a>Souhrn
V tomto článku jsme pomocí modulů Azure Resource Manageru v PowerShellu rozšířili jednotku operačního systému virtuálního počítače IaaS. Níže je uvedený celý skript, který můžete použít jako referenci:

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a>Další kroky
I když jsme se v tomto článku zaměřili hlavně na rozšíření disku operačního systému virtuálního počítače, vytvořený skript můžete použít také k rozšíření datových disků připojených k virtuálnímu počítači. Stačí k tomu změna jediného řádku kódu. Pokud například chcete rozšířit datový disk připojený k virtuálnímu počítači, nahraďte objekt ```OSDisk``` v části ```StorageProfile``` polem ```DataDisks``` a použijte číselný index k získání odkazu na první připojený datový disk, jak je znázorněno níže:

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
Podobným způsobem můžete odkazovat na další datové disky připojené k virtuálnímu počítači, buď pomocí výše uvedeného indexu, nebo pomocí vlastnosti ```Name``` disku, jak je znázorněno níže:

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

Informace o připojování disků k virtuálnímu počítači Azure Resource Manageru najdete v tomto [článku](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

