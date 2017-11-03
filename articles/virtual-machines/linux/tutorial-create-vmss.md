---
title: "Vytvoření sady škálování virtuálního počítače pro Linux v Azure | Microsoft Docs"
description: "Vytvoření a nasazení vysoce dostupné aplikace na virtuální počítače s Linuxem pomocí škálovací sadu virtuálních počítačů"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 09/08/2017
ms.author: iainfou
ms.openlocfilehash: 1f54bb04023ad61f4eae51389c6a902a029e9399
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Vytvořit sadu škálování virtuálního počítače a nasazení vysoce dostupné aplikace v systému Linux
Škálovací sadu virtuálních počítačů můžete nasadit a spravovat sadu identické, automatické škálování virtuálních počítačů. Můžete škálovat počet virtuálních počítačů v sadě škálování ručně, nebo definovat pravidla pro automatické škálování podle využití prostředků, jako je například CPU, paměť vyžádání či síťový provoz. V tomto kurzu nasadíte škálování virtuálních počítačů, nastavte v Azure. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Použít k vytvoření aplikace škálovat init cloudu
> * Vytvoření sady škálování virtuálního počítače
> * Zvýšení nebo snížení počtu instancí v sadě škálování
> * Vytvoření pravidel škálování
> * Zobrazit informace o připojení pro instance škálovací sady
> * Použití datových disků ve škálovací sadě


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="scale-set-overview"></a>Přehled sady škálování
Škálovací sadu virtuálních počítačů můžete nasadit a spravovat sadu identické, automatické škálování virtuálních počítačů. Virtuální počítače ve škálovací sadě jsou rozmístěny v doménách selhání a aktualizace logiku v jednom nebo více *umístění skupiny*. Toto jsou skupiny podobně nakonfigurovaných virtuálních počítačů, podobně jako [skupiny dostupnosti](tutorial-availability-sets.md).

Podle potřeby ve škálovací sadě virtuálních počítačů vytvořeny. Můžete definovat pravidla škálování řídit, jak a kdy se přidají nebo odeberou ze sady škálování virtuálních počítačů. Tato pravidla můžete spustit na základě metriky například zatížení procesoru, využití paměti nebo síťový provoz.

Pokud používáte image platformy Azure sadách škálování podporu až 1 000 virtuálních počítačů. Pro úlohy s významné instalace nebo požadavky přizpůsobení virtuálního počítače, můžete chtít [vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md). Můžete vytvořit až 300 virtuálních počítačů v škálování nastavit při použití vlastní image.


## <a name="create-an-app-to-scale"></a>Vytvoření aplikace pro škálování
Pro použití v provozním prostředí, můžete chtít [vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md) , který obsahuje aplikace nainstalována a nakonfigurována. V tomto kurzu umožňuje přizpůsobit virtuální počítače na při prvním spuštění rychle zobrazit nastavení v akci škálování.

V předchozích kurzu jste se dozvěděli [postup přizpůsobení virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md) s inicializací cloudu. Konfiguračního souboru stejné cloudové init můžete použít k instalaci NGINX a spuštění jednoduchou aplikaci Node.js "Zdravím svět". 

V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a vložte následující konfigurace. Například vytvoření souboru v prostředí cloudu není na místním počítači. Zadejte `sensible-editor cloud-init.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory. Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```


## <a name="create-a-scale-set"></a>Vytvoření sady škálování
Před vytvořením sady škálování, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupScaleSet* v *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

Nyní vytvoří škálování virtuálního počítače s [vytvořit az vmss](/cli/azure/vmss#create). Následující příklad vytvoří škálování nastavení s názvem *myScaleSet*, použije cloudu init soubor pro přizpůsobení virtuálního počítače a generuje klíče SSH, pokud neexistují:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```

Trvá několik minut vytvořit a nakonfigurovat všechny škálovací sadu prostředků a virtuálních počítačů. Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku. To může být jiná několik minut, než může aplikaci používat.


## <a name="allow-web-traffic"></a>Povolit webový provoz
Nástroj pro vyrovnávání zatížení byl vytvořen automaticky jako součást sady škálování virtuálního počítače. Nástroje pro vyrovnávání zatížení rozděluje zatížení mezi sadu definované virtuálních počítačů pomocí pravidla nástroje pro vyrovnávání zatížení. Další informace o konceptech služby Vyrovnávání zatížení a konfiguraci v dalším kurzu [postup vyrovnávat zatížení virtuálních počítačů v Azure](tutorial-load-balancer.md).

Pokud chcete povolit přenosy k dosažení webové aplikace, vytvořte pravidlo s [vytvořit pravidlo vyrovnáváním zatížení sítě az](/cli/azure/network/lb/rule#create). Následující příklad vytvoří pravidlo s názvem *myLoadBalancerRuleWeb*:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a>Testování aplikace
Chcete-li aplikace Node.js najdete na webu získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show). Následující příklad, získá IP adresu pro *myScaleSetLBPublicIP* vytvořené jako součást sady škálování:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Zadejte veřejnou IP adresu ve webovém prohlížeči. Aplikace se zobrazí, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz:

![Spuštěné aplikace Node.js](./media/tutorial-create-vmss/running-nodejs-app.png)

Sad v akci škálování najdete můžete můžete vynutit aktualizujte webový prohlížeč zobrazit nástroje pro vyrovnávání zatížení provoz distribuovat mezi všechny virtuální počítače používající vaši aplikaci.


## <a name="management-tasks"></a>Úlohy správy
V průběhu cyklu škálovací sady můžete spustit jeden nebo více úloh správy. Kromě toho můžete vytvořit skripty, které automatizují různé úlohy životního cyklu. Azure CLI 2.0 poskytuje rychlý způsob, jak provést tyto úlohy. Tady jsou několik běžných úloh.

### <a name="view-vms-in-a-scale-set"></a>Zobrazení virtuální počítače ve škálovací sadě
Chcete-li zobrazit seznam virtuálních počítačů spuštěných ve škálovací sadě, použijte [az vmss seznamu instance](/cli/azure/vmss#list-instances) následujícím způsobem:

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

Výstup se podobá následujícímu příkladu:

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>Zvýšení nebo snížení instance virtuálních počítačů
Pokud chcete zobrazit počet instancí, které máte aktuálně v sadě škálování, použijte [az vmss zobrazit](/cli/azure/vmss#show) a dotazovat se na *sku.capacity*:

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Potom můžete ručně zvýšení nebo snížení počtu virtuálních počítačů v sad s škálování [az vmss škálování](/cli/azure/vmss#scale). Následující příklad nastaví počet virtuálních počítačů ve vaší škálování nastavena na *5*:

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```


### <a name="configure-autoscale-rules"></a>Konfigurace pravidel automatického škálování
Místo ručně škálování počtu instancí ve vaší škálovací sady, můžete definovat pravidla pro automatické škálování. Tato pravidla monitorovat instance ve škálovací sadě a reagují odpovídajícím způsobem na základě metriky a prahové hodnoty, které definujete. Následující příklad škáluje se počet instancí jedním při průměrné zatížení procesoru je větší než 60 % během období 5 minut. Pokud průměrná zatížení procesoru pak klesne pod 30 % během období 5 minut, jsou škálovat instance v jednou instancí. Vaše ID odběru slouží k vytvoření prostředku identifikátory URI pro různé měřítka součástí sady. K vytvoření pravidla s [vytvořit nastavení automatického škálování monitorování az](/cli/azure/monitor/autoscale-settings#create), zkopírujte a vložte následující příkaz profil škálování:

```azurecli-interactive 
sub=$(az account show --query id -o tsv)

az monitor autoscale-settings create \
    --resource-group myResourceGroupScaleSet \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "East US",
      "notifications": [],
      "profiles": [
        {
          "name": "Auto created scale condition",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
    }'
```

Pokud chcete znovu použít profil škálování, můžete vytvořit soubor JSON (JavaScript Object Notation) a předat to `az monitor autoscale-settings create` s `--parameters @autoscale.json` parametr. Další návrhu informace o použití automatického škálování, najdete v části [škálování osvědčené postupy](/azure/architecture/best-practices/auto-scaling).


### <a name="get-connection-info"></a>Získání informací o připojení
Chcete-li získat informace o připojení o virtuální počítače ve škálovací sady, použijte [az vmss seznamu--připojení-informace o instanci](/cli/azure/vmss#list-instance-connection-info). Tento příkaz vypíše veřejnou IP adresu a port pro každý virtuální počítač, který umožňuje připojit pomocí protokolu SSH:

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a>Použití datových disků s sady škálování
Můžete vytvořit a používat datových disků s sady škálování. V předchozích kurzu jste se dozvěděli, jak [disky spravovat Azure](tutorial-manage-disks.md) , popisuje osvědčené postupy a vylepšení výkonu pro vytváření aplikací na datové disky, nikoli disk operačního systému.

### <a name="create-scale-set-with-data-disks"></a>Vytvořit sadu škálování s datovými disky
Chcete-li vytvořit sadu škálování a připojte datových disků, přidejte `--data-disk-sizes-gb` parametru [az vmss vytvořit](/cli/azure/vmss#create) příkaz. Následující příklad vytvoří škálování s *50*Gb dat disků připojených ke každé instanci:

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

Při odebrání instance ze sady škálování, budou odebrány také všechny připojené datových disků.

### <a name="add-data-disks"></a>Přidat datových disků
Chcete-li přidat datový disk k instancím ve škálovací sadě, použijte [připojit az vmss disk](/cli/azure/vmss/disk#attach). Následující příklad přidá *50*GB místa na disku k jednotlivým instancím:

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a>Odpojit datových disků
Chcete-li odebrat datový disk k instancím ve škálovací sadě, použijte [odpojit az vmss disk](/cli/azure/vmss/disk#detach). Následující příklad odebere datový disk na logické jednotce *2* z každé instance:

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste vytvořili škálovací sadu virtuálních počítačů. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Použít k vytvoření aplikace škálovat init cloudu
> * Vytvoření sady škálování virtuálního počítače
> * Zvýšení nebo snížení počtu instancí v sadě škálování
> * Vytvoření pravidel škálování
> * Zobrazit informace o připojení pro instance škálovací sady
> * Použití datových disků ve škálovací sadě

Přechodu na v dalším kurzu se dozvíte další informace o konceptech pro virtuální počítače Vyrovnávání zatížení.

> [!div class="nextstepaction"]
> [Vyrovnávat zatížení virtuálních počítačů](tutorial-load-balancer.md)
