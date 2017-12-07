## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#az_group_create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#az_vm_create). 

Následující příklad vytvoří virtuální počítač *myVM*, a pokud ve výchozím umístění klíčů ještě neexistují klíče SSH, vytvoří je. Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`. Příkaz také nastaví *azureuser* jako uživatelské jméno správce. Tento název použijete později pro připojení k virtuálnímu počítači. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Po vytvoření virtuálního počítače se v Azure CLI zobrazí podobné informace jako v následujícím příkladu. Poznamenejte si `publicIpAddress`. Tato adresa se používá pro přístup k virtuálnímu počítači v dalších krocích.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```



## <a name="open-port-80-for-web-traffic"></a>Otevření portu 80 pro webový provoz 

Ve výchozím nastavení jsou povoleny pouze připojení SSH do virtuálních počítačů Linux nasazené v Azure. Protože tento virtuální počítač bude webový server, budete muset otevřít port 80 z Internetu. Požadovaný port otevřete pomocí příkazu [az vm open-port](/cli/azure/vm#az_vm_open_port).  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a>Připojení SSH k virtuálnímu počítači


Pokud si nejste jisti již veřejnou IP adresu vašeho virtuálního počítače, spusťte [seznam veřejné ip sítě az](/cli/azure/network/public-ip#list) příkaz. Je třeba tuto IP adresu pro několik dalších krocích.


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH. Nahraďte správné veřejnou IP adresu virtuálního počítače. V tomto příkladu IP adresa je *40.68.254.142*. *azureuser* je uživatelské jméno správce nastavit při vytváření virtuálního počítače.

```bash
ssh azureuser@40.68.254.142
```

