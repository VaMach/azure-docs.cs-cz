## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

Pokud si nejste jisti umístění, které chcete použít, můžete seznam dostupných umístění. Jakmile se zobrazí v seznamu, najít ten, který chcete použít. Tento příklad použije **eastus**. Uložit jako proměnnou a používat proměnné, můžete ho změnit na jednom místě.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Vytvořit účet standardního úložiště pro obecné účely pomocí replikace LRS [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), následně načíst kontext účtu úložiště, který definuje účet úložiště, který se má použít. Když funguje na účet úložiště, můžete odkazovat na kontext místo opakovaně přihlašovací údaje. Tento příklad vytvoří účet úložiště s názvem *můj_účet_úložiště* s místně redundantní úložiště a objektů blob, zapnuté šifrování.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx = $storageAccount.Context
```
