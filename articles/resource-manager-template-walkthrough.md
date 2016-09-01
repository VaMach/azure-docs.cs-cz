<properties
   pageTitle="Názorný průvodce šablonou Resource Manageru | Microsoft Azure"
   description="Názorný a podrobný průvodce šablonou Resource Manageru, která zřizuje základní architekturu Azure IaaS."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager=""
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# Názorný průvodce šablonou Resource Manageru

Jednou ze základních otázek při vytváření šablon je, jak vůbec začít. Dá se začít prázdnou šablonou, využít základní strukturu popsanou v článku věnovaném [vytváření šablon](resource-group-authoring-templates.md#template-format) a přidat prostředky a příslušné parametry a proměnné. Dobrou alternativou by také bylo projít si [galerii pro rychlý start](https://github.com/Azure/azure-quickstart-templates) a najít scénáře podobné tomu, který se pokoušíte vytvořit. Můžete sloučit několik šablon nebo upravit stávající šablonu tak, aby vyhovovala vašemu konkrétnímu scénáři. 

Podívejme se na běžnou infrastrukturu:

* Dva virtuální počítače, které využívají stejný účet úložiště, jsou ve stejné skupině dostupnosti a ve stejné podsíti virtuální sítě.
* Jedna síťová karta NIC a IP adresa pro každý virtuální počítač.
* Nástroj pro vyrovnávání zatížení s pravidlem vyrovnávání zatížení na portu 80.

![Architektura](./media/resource-group-overview/arm_arch.png)

Toto téma vás provede jednotlivými kroky při vytváření šablony Resource Manageru pro tuto infrastrukturu. Finální šablona, kterou vytvoříte, je založená na šabloně pro rychlý start s názvem [2 VMs in a Load Balancer and load balancing rules](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/) (2 virtuální počítače v Load Balanceru a pravidla pro vyrovnávání zatížení).

Ale tohle všechno se nedá vytvořit najednou, proto nejdřív vytvoříme účet úložiště a nasadíme ho. Až zvládnete vytvořit účet úložiště, přidáte další prostředky a pak šablonu nasadíte znovu a dokončíte příslušnou infrastrukturu.

>[AZURE.NOTE] K vytvoření šablony můžete použít libovolný typ editoru. Sada Visual Studio sice poskytuje nástroje, které zjednodušují vývoj šablon, ale k dokončení tohoto kurzu ji nepotřebujete. Kurz použití sady Visual Studio k vytvoření nasazení SQL Database a webové aplikace najdete v článku o [vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## Vytvoření šablony Resource Manageru

Šablona je soubor JSON a definuje všechny prostředky, které nasadíte. Umožňuje také definovat parametry, které se zadávají během nasazení, proměnné, které se vytvářejí na základě jiných hodnot a výrazů, a výstupy z nasazení. 

Začněme nejjednodušší šablonou:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Uložte tento soubor jako **azuredeploy.json** (šablona může mít libovolný název, ale vždycky to musí být soubor json).

## vytvořit účet úložiště
V sekci **resources** přidejte objekt definující účet úložiště, jak ukazuje následující obrázek. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Asi vás zajímá, odkud tyto vlastnosti a hodnoty pocházejí. Vlastnosti **type**, **name**, **apiVersion** a **location** jsou standardní elementy, které jsou dostupné pro všechny typy prostředků. Informace o běžných elementech najdete v části [Prostředky](resource-group-authoring-templates.md#resources). Jako **name** je nastavená hodnota parametru, kterou předáte během nasazení, a **location** je umístění, které využívá příslušná skupina prostředků. Určení hodnot **type** a **apiVersion** je popsané v dalších oddílech.

Sekce **properties** obsahuje všechny vlastnosti, které jsou pro konkrétní typ prostředku jedinečné. Hodnoty zadané v této části přesně odpovídají operaci PUT v rozhraní REST API pro vytvoření tohoto typu prostředku. Při vytváření účtu úložiště musíte zadat **accountType**. V článku věnovaném [rozhraní REST API pro vytvoření účtu úložiště](https://msdn.microsoft.com/library/azure/mt163564.aspx) si všimněte, že sekce properties operace REST také obsahuje vlastnost **accountType**. Povolené hodnoty jsou zdokumentovány. V tomto příkladu je typ účtu nastavený na **Standard_LRS**, ale můžete zadat jinou hodnotu nebo uživatelům povolit, aby typ účtu předali jako parametr.

Nyní se vraťme zpátky k sekci **parameters** a podívejme se na to, jak se definuje název účtu úložiště. Další informace o použití parametrů najdete v tématu [Parametry](resource-group-authoring-templates.md#parameters). 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Tady jste definovali parametr typu string, do kterého se uloží název účtu úložiště. Hodnota tohoto parametru bude poskytnuta během nasazení šablony.

## Nasazení šablony
Máme kompletní šablonu pro vytvoření nového účtu úložiště. Jak si vzpomínáte, tato šablona byla uložena v souboru **azuredeploy.json**:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Pro nasazení šablony existuje několik postupů, jak je uvedeno v [článku věnovaném nasazení prostředků](resource-group-template-deploy.md). Pokud chcete šablonu nasadit pomocí Azure PowerShellu, použijte:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Pokud chcete šablonu nasadit pomocí rozhraní příkazového řádku Azure, použijte:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Právě jste se stali hrdými majiteli účtu úložiště!

V dalších krocích se přidají všechny prostředky potřebné k nasazení architektury popsané na začátku tohoto kurzu. Tyto prostředky přidáte do stejné šablony, se kterou právě pracujete.

## Skupina dostupnosti
Za definici účtu úložiště přidejte skupinu dostupnosti pro virtuální počítače. V tomto případě se nevyžadují žádné další vlastnosti, takže příslušná definice je docela jednoduchá. Pokud chcete definovat hodnoty pro počet aktualizačních domén a domén selhání, najdete kompletní sekci vlastností v tématu [REST API pro vytvoření skupiny dostupnosti](https://msdn.microsoft.com/library/azure/mt163607.aspx).

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Všimněte si, že jako parametr **name** je nastavená hodnota proměnné. U této šablony je název skupiny dostupnosti potřeba na několika různých místech. Je proto jednodušší definovat tuto hodnotu jednou a potom ji na různých místech využít.

Hodnota zadaná jako **type** obsahuje poskytovatele prostředku i jeho typ. Poskytovatel prostředků pro skupiny dostupnosti je **Microsoft.Compute** a typ prostředku je **availabilitySets**. Seznam dostupných poskytovatelů prostředků můžete získat spuštěním následujícího příkazu PowerShellu:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Pokud používáte rozhraní příkazového řádku Azure, využijte k tomuto účelu následující příkaz:
```
    azure provider list
```
Vzhledem k tomu, že v tomto tématu pracujete s účty úložiště, virtuálními počítači a virtuální sítí, budete využívat:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Pokud chcete zobrazit typy prostředků pro konkrétního poskytovatele, spusťte následující příkaz PowerShellu:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Následující příkaz v rozhraní příkazového řádku Azure CLI vrátí dostupné typy ve formátu JSON a uloží je do souboru.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Položka **availabilitySets** by se měla zobrazit jako jeden z typů v rámci **Microsoft.Compute**. Úplný název tohoto typu je **Microsoft.Compute/availabilitySets**. Můžete určit název typu prostředku pro libovolný z prostředků ve vaší šabloně.

## Veřejná IP adresa
Definujte veřejnou IP adresu. Vlastnosti, které je potřeba nastavit, najdete v tématu věnovaném [REST API pro veřejné IP adresy](https://msdn.microsoft.com/library/azure/mt163590.aspx).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Je nastavena metoda přidělení **Dynamic**, ale můžete ji nastavit na požadovanou hodnotu nebo ji nastavit tak, aby přijímala hodnotu parametru. Uživatelům vaší šablony jste povolili předávat hodnotu pro popisek názvu domény.

Teď se podíváme, jak určíte **apiVersion**. Hodnota, kterou zadáte, odpovídá verzi rozhraní REST API, kterou chcete při vytváření prostředku použít. Typ tohoto prostředku tedy můžete vyhledat v dokumentaci k REST API. Nebo můžete pro konkrétní typ spustit následující příkaz rozhraní PowerShell.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Vrátí následující hodnoty:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Pokud chcete zjistit verzi API pomocí rozhraní příkazového řádku Azure CLI, spusťte stejný příkaz **azure provider show**, jako je uvedený výše.

Při vytváření nové šablony vyberte nejnovější verzi API.

## Virtuální síť a podsíť
Vytvořte virtuální síť s jednou podsítí. Vlastnosti, které je potřeba nastavit, najdete v tématu [REST API pro virtuální sítě](https://msdn.microsoft.com/library/azure/mt163661.aspx).

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## Nástroj pro vyrovnávání zatížení
Nyní vytvoříte externě zaměřený nástroj pro vyrovnávání zatížení. Vzhledem k tomu, že tento nástroj pro vyrovnávání zatížení využívá veřejnou IP adresu, musíte závislost na veřejné IP adrese deklarovat v sekci **dependsOn**. To znamená, že nástroj pro vyrovnávání zatížení se nasadí až poté, co se dokončí nasazení veřejné IP adresy. Bez definování této závislosti by došlo k chybě, protože Resource Manager by se pokusil nasadit prostředky paralelně a pokusil by se nastavit nástroji pro vyrovnávání zatížení veřejnou IP adresu, která ještě neexistuje. 

Dál v této definici prostředku vytvoříte back-endový fond adres, několik příchozích pravidel NAT pro připojení RDP k virtuálním počítačům a pravidlo vyrovnávání zatížení se sondou tcp na portu 80. Všechny vlastnosti najdete v článku věnovaném [REST API pro nástroj pro vyrovnávání zatížení](https://msdn.microsoft.com/library/azure/mt163574.aspx).

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## Síťové rozhraní
Vytvoříte dvě síťová rozhraní, pro každý virtuální počítač jedno. V síťových rozhraních není nutné zahrnovat duplicitní položky. Místo toho můžete využít [funkci copyIndex()](resource-group-create-multiple.md) k iteraci nad kopírovací smyčkou (je označena jako nicLoop) a vytvořit počet síťových rozhraní určených proměnnými `numberOfInstances`. Síťové rozhraní závisí na vytvoření virtuální sítě a nástroje pro vyrovnávání zatížení. Využívá podsíť definovanou při vytvoření virtuální sítě a ID nástroje pro vyrovnávání zatížení ke konfiguraci fondu adres nástroje pro vyrovnávání zatížení a příchozích pravidel NAT.
Všechny vlastnosti najdete v tématu [REST API pro síťová rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx).

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## Virtuální počítač
Vytvoříte dva virtuální počítače pomocí funkce copyIndex(), a to stejným způsobem jakým jste postupovali při vytváření [síťových rozhraní](#network-interface).
Vytvoření virtuálního počítače závisí na účtu úložiště, síťovém rozhraní a skupině dostupnosti. Tento virtuální počítač se vytvoří na základě image pořízené na Marketplace, jak definuje vlastnost `storageProfile`. K definování nabídky, SKU, verze a vydavatele této image se použije `imageReference`. Nakonec se nakonfiguruje diagnostický profil a povolí se diagnostika virtuálního počítače. 

Relevantní vlastnosti pro image pořízenou prostřednictvím Marketplace najdete v článcích věnovaných [vybraným imagím pro virtuální počítače s Linuxem](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) nebo [vybraným imagím pro virtuální počítače s Windows](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md).

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Pokud je image vydaná **jiným dodavatelem**, budete muset zadat jinou vlastnost s názvem `plan`. Příklad najdete v [této šabloně](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) z galerie pro rychlý start. 

Tím jste dokončili vytváření prostředků pro vaši šablonu.

## Parametry

V sekci parametrů definujte hodnoty, které je možné zadat při nasazení šablony. Definujte parametry jenom pro ty hodnoty, o kterých si myslíte, že by se měly během nasazení měnit. Můžete také zadat výchozí hodnotu parametru, která se použije, pokud hodnota parametru není během nasazení zadána. Můžete také definovat povolené hodnoty, jak je uvedeno pro parametr **imageSKU**.

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## Proměnné

V sekci proměnných můžete definovat hodnoty, které se ve vaší šabloně používají na několika místech, nebo hodnoty, které se vytvářejí na základě jiných hodnot nebo výrazů. Proměnné se často využívají ke zjednodušení syntaxe šablony.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Právě jste šablonu dokončili! Nyní ji můžete porovnat s kompletní šablonou [2 VMs with load balancer and load balancer rules](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules) (2 virtuální počítače v Load Balanceru a pravidla pro vyrovnávání zatížení) v [galerii pro rychlý start](https://github.com/Azure/azure-quickstart-templates). Vzhledem k použití různých čísel verzí může být vaše šablona poněkud odlišná. 

Tuto šablonu můžete znovu nasadit pomocí stejných příkazů, které jste použili k nasazení účtu úložiště. Účet úložiště nemusíte před novým nasazením odstraňovat, protože Resource Manager přeskočí znovuvytváření prostředků, které už existují a nezměnily se.

## Další kroky

- [Azure Resource Manager Template Visualizer (ARMViz)](http://armviz.io/#/) je skvělý nástroj pro vizualizaci šablon ARM v případě, že jsou příliš velké a nedají se pochopit pouhým čtením souboru json.
- Další informace o struktuře šablon najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).
- Informace o nasazení šablony najdete v tématu věnovaném [nasazení skupiny prostředků pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md)



<!--HONumber=Aug16_HO4-->


