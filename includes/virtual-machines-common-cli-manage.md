Azure CLI 2.0 umožňuje vytvořit a spravovat prostředky Azure v systému macOS, Linux a Windows. Tento článek podrobně popisuje některé z nejběžnějších příkazů vytvářet a spravovat virtuální počítače (VM).

Tento článek vyžaduje Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli). Můžete také použít [cloudové prostředí](/azure/cloud-shell/quickstart) z prohlížeče.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Základní příkazy Azure Resource Manageru v Azure CLI
Další pomoc s přepínače konkrétní příkazového řádku a možnosti, můžete použít možnosti online příkaz nápovědy a zadáním `az <command> <subcommand> --help`.

### <a name="create-vms"></a>Vytvoření virtuálních počítačů
| Úkol | Příkazy Azure CLI |
| --- | --- |
| Vytvoření skupiny prostředků | `az group create --name myResourceGroup --location eastus` |
| Vytvoření virtuálního počítače s Linuxem | `az vm create --resource-group myResourceGroup --name myVM --image ubuntults` |
| Vytvoření virtuálního počítače s Windows | `az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter` |

### <a name="manage-vm-state"></a>Spravovat stav virtuálního počítače
| Úkol | Příkazy Azure CLI |
| --- | --- |
| Spuštění virtuálního počítače | `az vm start --resource-group myResourceGroup --name myVM` |
| Zastavení virtuálního počítače | `az vm stop --resource-group myResourceGroup --name myVM` |
| Uvolnění virtuálního počítače | `az vm deallocate --resource-group myResourceGroup --name myVM` |
| Restartování virtuálního počítače | `az vm restart --resource-group myResourceGroup --name myVM` |
| Znovunasazení virtuálního počítače | `az vm redeploy --resource-group myResourceGroup --name myVM` |
| Odstranění virtuálního počítače | `az vm delete --resource-group myResourceGroup --name myVM` |

### <a name="get-vm-info"></a>Získání informací o virtuálních počítačů
| Úkol | Příkazy Azure CLI |
| --- | --- |
| Výpis virtuálních počítačů | `az vm list` |
| Získání informací o virtuálním počítači | `az vm show --resource-group myResourceGroup --name myVM` |
| Získání využití prostředků virtuálního počítače | `az vm list-usage --location eastus` |
| Získání všech dostupných velikostí virtuálních počítačů | `az vm list-sizes --location eastus` |

## <a name="disks-and-images"></a>Disky a obrázků
| Úkol | Příkazy Azure CLI |
| --- | --- |
| Přidání datového disku k virtuálnímu počítači | `az vm disk attach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk --size-gb 128 --new ` |
| Odebrání datového disku z virtuálního počítače | `az vm disk detach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk` |
| Změna velikosti disku | `az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 256` |
| Pořízení snímku disku | `az snapshot create --resource-group myResourceGroup --name mySnapshot --source myDataDisk` |
| Vytvoření bitové kopie virtuálního počítače | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| Vytvořit virtuální počítač z bitové kopie | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a>Další kroky
Další příklady příkazů rozhraní příkazového řádku najdete v tématu [vytvořit a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/tutorial-manage-vm.md) kurzu.

