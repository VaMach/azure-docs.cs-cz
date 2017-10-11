---
title: "Připojit virtuální počítače Azure k analýze protokolů | Microsoft Docs"
description: "Pro systém Windows a Linux virtuální počítače běžící v Azure je doporučeným způsobem shromažďovat protokoly a metriky instalace rozšíření virtuálního počítače Azure Log Analytics. Instalace rozšíření virtuálního počítače analýzy protokolů na virtuálních počítačích Azure můžete portál Azure nebo PowerShell."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: ca39e586-a6af-42fe-862e-80978a58d9b1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: richrund
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cdae291b546fef4d7fdb8b067c8e4f4c9708d43f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-azure-virtual-machines-to-log-analytics-with-a-log-analytics-agent"></a>Připojit virtuální počítače Azure k analýze protokolů s agentem analýzy protokolů

Doporučené metody pro shromažďování protokolů a metriky pro počítače s Windows a Linux, je instalace agenta analýzy protokolů.

Nejjednodušší způsob, jak nainstalovat agenta analýzy protokolů na virtuálních počítačích Azure je prostřednictvím rozšíření protokolu analýzy virtuálního počítače.  Pomocí rozšíření zjednodušuje proces instalace a automaticky nakonfiguruje agenta k odesílání dat do pracovního prostoru analýzy protokolů, který určíte. Agent je také automaticky aktualizovány, zajistíte, že máte nejnovější funkce a opravy.

Pro virtuální počítače s Windows, povolíte *agenta Microsoft Monitoring Agent* rozšíření virtuálního počítače.
Pro virtuální počítače s Linuxem, povolíte *OMS agenta pro Linux* rozšíření virtuálního počítače.

Další informace o [rozšíření virtuálního počítače Azure](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Pokud používáte kolekce založené na agentovi pro data protokolu, je nutné nakonfigurovat [zdroje dat v analýzy protokolů](log-analytics-data-sources.md) k určení protokoly a metriky, které chcete shromažďovat.

> [!IMPORTANT]
> Pokud nakonfigurujete analýzy protokolů pro data protokolu index pomocí [Azure diagnostics](log-analytics-azure-storage.md)a konfigurace agenta shromažďování stejné protokoly a protokoly se shromažďují dvakrát. Budou se vám účtovat pro obě datové zdroje. Pokud máte nainstalovaného agenta, shromažďování dat protokolu pomocí jenom agenta – není konfigurace analýzy protokolů pro shromažďování dat protokolu z Azure diagnostics.
>
>

Existují tři způsoby snadno povolit rozšíření virtuálního počítače analýzy protokolů:

* Pomocí portálu Azure
* Pomocí prostředí Azure PowerShell
* Pomocí šablony Azure Resource Manager

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Povolit rozšíření virtuálního počítače na portálu Azure
Můžete nainstalovat agenta pro analýzy protokolů a připojit virtuální počítač Azure, který běží na pomocí [portál Azure](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Instalace agenta analýzy protokolů a připojte virtuální počítač do pracovního prostoru analýzy protokolů
1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
2. Vyberte **Procházet** na levé straně portálu, a potom přejděte na **analýzy protokolů (OMS)** a vyberte ho.
3. V seznamu analýzy protokolů pracovních prostorů vyberte ten, který chcete používat s virtuálním Počítačem Azure.  
   ![Pracovní prostory OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. V části **protokolu správy analytics**, vyberte **virtuální počítače**.  
   ![Virtual Machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5. V seznamu **virtuální počítače**, vyberte virtuální počítač, na kterém chcete nainstalovat agenta. **Stav připojení OMS** pro virtuální počítač naznačuje, že je **Nepřipojeno**.  
   ![Virtuální počítač není připojen.](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6. V podrobnostech pro virtuální počítač, vyberte **Connect**. Agent je automaticky nainstalovat a nakonfigurovat pro pracovní prostor analýzy protokolů. Tento proces trvá několik minut, během které doby je stav připojení OMS *připojení...*  
   ![Připojení virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7. Po dokončení instalace a připojit agenta, **OMS připojení** stav bude aktualizován zobrazíte **tento pracovní prostor**.  
   ![Připojení](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)

## <a name="enable-the-vm-extension-using-powershell"></a>Povolit rozšíření virtuálního počítače pomocí prostředí PowerShell
Když nakonfigurujete virtuální počítač pomocí prostředí PowerShell, budete muset zadat **workspaceId** a **workspaceKey**. Názvy vlastností, které ve vaší konfiguraci json jsou **malá a velká písmena**.

Můžete najít Id a klíč na **nastavení** stránky portálu OMS nebo pomocí prostředí PowerShell, jak je znázorněno v předchozím příkladu.

![ID pracovního prostoru a primární klíč](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

Existují jiné příkazy pro virtuální počítače Azure classic a Resource Manager virtuální počítače. Následují příklady pro classic i Resource Manager virtuálních počítačů.

Klasické virtuální počítače použijte následující příklad PowerShell:

```PowerShell
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Pro virtuální počítače s Linuxem Resource Manager pomocí rozhraní příkazového řádku následující
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

Pro virtuální počítače správce prostředků použijte následující příklad PowerShell:

```PowerShell
Login-AzureRMAccount
Select-AzureRMSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```


## <a name="deploy-the-vm-extension-using-a-template"></a>Nasazení rozšíření virtuálního počítače pomocí šablony
Pomocí Azure Resource Manager můžete vytvořit šablonu (ve formátu JSON), která definuje nasazení a konfiguraci vaší aplikace. Tato šablona se označuje jako šablona Resource Manageru a nabízí deklarativní způsob, jak definovat nasazení. Pomocí šablony můžete opakovaně nasazení aplikace v průběhu životního cyklu aplikace a mít jistotu, že se prostředky nasadí v konzistentním stavu.

Zahrnutím agenta analýzy protokolů v rámci šablony Resource Manageru můžete zajistit, že každý virtuální počítač je předem nakonfigurovaná tak, aby odesílaly pracovní prostor analýzy protokolů.

Další informace o šablonách Resource Manager najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Tady je příklad šablony Resource Manageru, který se používá pro nasazení virtuálního počítače se systémem Windows s příponou agenta Microsoft Monitoring Agent nainstalována. Tato šablona je šablonu typické virtuálního počítače s těmito přídavky:

* ID pracovního prostoru a workspaceName parametry
* Microsoft.EnterpriseCloud.Monitoring oddílu rozšíření prostředků
* Výstupy k vyhledání ID pracovního prostoru a workspaceSharedKey

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMS workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Šablonu můžete nasadit pomocí následujícího příkazu Powershellu:

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-the-log-analytics-vm-extension"></a>Řešení potíží s rozšíření virtuálního počítače analýzy protokolů
Obvykle zobrazí zprávu po věcí nefungují z portálu Azure nebo Azure powershell.

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
2. Najít virtuálního počítače a otevřete tak podrobnosti virtuálního počítače.
3. Klikněte na tlačítko **rozšíření** ke kontrole, pokud je povolené rozšíření OMS, nebo ne.

   ![Zobrazení rozšíření virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. Klikněte *MicrosoftMonitoringAgent*(Windows) nebo *OmsAgentForLinux*rozšíření a zobrazení podrobností (Linux). 

   ![Podrobnosti o rozšíření virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a>Řešení potíží virtuální počítače s Windows
Pokud *agenta Microsoft Monitoring Agent* není instalaci rozšíření agenta virtuálního počítače nebo vytváření sestav, můžete provést následující kroky k odstranění problému.

1. Zkontrolujte, zda je nainstalován agent virtuálního počítače Azure a že fungují správně pomocí kroků v [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Můžete také zkontrolovat soubor protokolu agenta virtuálního počítače`C:\WindowsAzure\logs\WaAppAgent.log`
   * Pokud v protokolu neexistuje, není nainstalován agent virtuálního počítače.
     * [Nainstalujte agenta virtuálního počítače Azure na klasické virtuální počítače](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. Potvrďte, že je spuštěn úkol prezenčního signálu agenta Microsoft Monitoring Agent rozšíření, pomocí následujících kroků:
   * Přihlaste se k virtuálnímu počítači
   * Otevřete Plánovač úloh a najít `update_azureoperationalinsight_agent_heartbeat` úloh
   * Potvrďte úloha je povolená a běží každou minutu
   * Zkontrolujte v souboru protokolu prezenčního signálu`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Zkontrolujte soubory protokolů rozšíření Microsoft Monitoring Agent virtuálního počítače v`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Zajistěte, aby byl že virtuální počítač můžete spouštět skripty prostředí PowerShell
5. Ujistěte se, že oprávnění C:\Windows\temp nezměnily
6. Zobrazit stav služby Microsoft Monitoring Agent zadáním následujícího příkazu v okně prostředí PowerShell se zvýšenými oprávněními na virtuálním počítači`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Zkontrolujte soubory protokolu instalace agenta Microsoft Monitoring Agent v`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Další informace najdete v tématu [řešení potíží s rozšířeními Windows](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="troubleshooting-linux-virtual-machines"></a>Řešení potíží virtuální počítače s Linuxem
Pokud *OMS agenta pro Linux* není instalaci rozšíření agenta virtuálního počítače nebo vytváření sestav, můžete provést následující kroky k odstranění problému.

1. Pokud je stav rozšíření *neznámé* zkontrolujte, zda je nainstalován agent virtuálního počítače Azure a že fungují správně, a to prohlédnutím souboru protokolu agenta virtuálního počítače`/var/log/waagent.log`
   * Pokud v protokolu neexistuje, není nainstalován agent virtuálního počítače.
   * [Nainstalujte agenta virtuálního počítače Azure na virtuální počítače s Linuxem](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. Pro ostatní stavy není v pořádku, zkontrolujte agenta OMS pro rozšíření virtuálního počítače s Linuxem soubory protokolů `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` a`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Pokud je v pořádku stav rozšíření, ale není odesílání dat zkontrolujte OMS agenta pro Linux soubory protokolu v`/var/opt/microsoft/omsagent/log/omsagent.log`

## <a name="next-steps"></a>Další kroky
* Konfigurace [zdroje dat v analýzy protokolů](log-analytics-data-sources.md) k určení ke shromažďování metrik a protokolování.
* Chcete-li shromažďovat data z virtuálních počítačů [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).
* [Shromažďování dat pomocí Azure Diagnostics](log-analytics-azure-storage.md) další zdroje, které jsou spuštěné v Azure.

Pro počítače, které nejsou v Azure můžete nainstalovat agenta analýzy protokolů pomocí metody, které jsou popsané v následujících článcích:

* [Připojení počítače se systémem Windows k analýze protokolů](log-analytics-windows-agents.md)
* [Připojení počítačů se systémem Linux k analýze protokolů](log-analytics-linux-agents.md)
