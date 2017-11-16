---
title: "Jak načíst vyvážit virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o použití nástroje pro vyrovnávání zatížení Azure k vytvoření vysoce dostupné a zabezpečené aplikace napříč tři virtuální počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/13/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: dc25d6106ad67710660b1a5c48270a7082688d51
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Jak načíst vyvážit virtuální počítače s Linuxem v Azure k vytvoření vysoce dostupné aplikace
Vyrovnávání zatížení poskytuje vyšší úroveň dostupnosti rozloží příchozí žádosti napříč více virtuálních počítačů. V tomto kurzu informace o různé součásti nástroje pro vyrovnávání zatížení Azure, které distribuci přenosů a zajištění vysoké dostupnosti. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření pro vyrovnávání zatížení Azure
> * Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
> * Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení
> * Pomocí cloud init můžete vytvořit základní aplikaci Node.js
> * Vytváření virtuálních počítačů a připojit ke službě Vyrovnávání zatížení
> * Zobrazit nástroj pro vyrovnávání zatížení v akci
> * Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="azure-load-balancer-overview"></a>Přehled nástroje pro vyrovnávání zatížení Azure
K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku. Sondu stavu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom přenosy na provozní virtuální počítač.

Můžete definovat na front-endové konfiguraci protokolu IP, která obsahuje jeden nebo více veřejné IP adresy. Tuto konfiguraci front-end IP adresy umožňuje Vyrovnávání zatížení a aplikace přístupné přes Internet. 

Virtuální počítače připojit k nástroji pro vyrovnávání zatížení pomocí jejich virtuální síťová karta (NIC). K distribuci provoz na virtuální počítače, fond back-end adres obsahuje IP adresy virtuální (NIC) připojené ke službě Vyrovnávání zatížení.

Pokud chcete řídit tok přenosů dat, definujete pravidla nástroje pro vyrovnávání zatížení pro určité porty a protokoly, které jsou mapovány na virtuální počítače.

Pokud jste postupovali podle předchozích kurzu [vytvořit škálovací sadu virtuálních počítačů](tutorial-create-vmss.md), nástroj pro vyrovnávání zatížení pro vás vytvořil. Všechny tyto součásti byly nakonfigurovány pro vás v rámci sady škálování.


## <a name="create-azure-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení Azure
Tato část podrobně popisuje, jak můžete vytvořit a nakonfigurovat jednotlivé komponenty služby Vyrovnávání zatížení. Než bude možné vytvořit nástroj pro vyrovnávání zatížení, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupLoadBalancer* v *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy
Pro přístup k vaší aplikace v síti Internet, musíte nástroj pro vyrovnávání zatížení veřejnou IP adresu. Vytvoření veřejné IP adresy s [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create). Následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* v *myResourceGroupLoadBalancer* skupiny prostředků:

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Vytvoření nástroje pro vyrovnávání zatížení
Vytvořit nástroj pro vyrovnávání zatížení s [az sítě lb vytvořit](/cli/azure/network/lb#create). Následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem *myLoadBalancer* a přiřadí *myPublicIP* ke konfiguraci front-end IP adresy:

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Vytvoření test stavu
Povolit službu Vyrovnávání zatížení k monitorování stavu aplikace, použijte Test stavu. Test stavu dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení, podle jejich reakce na kontroly stavu. Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení po dvě po sobě jdoucích selhání v intervalech 15 sekund. Můžete vytvořit test stavu na základě protokolu nebo na stránce Kontrola specifickém stavu pro vaši aplikaci. 

Následující příklad vytvoří sondou TCP. Můžete také vytvořit vlastní sondy HTTP pro další kontroly podrobné stavu. Pokud používáte vlastní sondu HTTP, musíte vytvořit stránka pro kontrolu stavu, jako například *healthcheck.js*. Sondy musí vrátit **HTTP 200 OK** odpovědi pro vyrovnávání zatížení na hostiteli mějte otočení.

K vytvoření stavu sondou TCP, použijete [vytvoření testu vyrovnáváním zatížení sítě az](/cli/azure/network/lb/probe#create). Následující příklad vytvoří kontrolu stavu s názvem *myHealthProbe*:

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Vytvořit pravidlo Vyrovnávání zatížení.
Pravidlo Vyrovnávání zatížení se používá k definování, jak se provoz rozděluje k virtuálním počítačům. Můžete definovat front-endové konfiguraci protokolu IP pro příchozí provoz a fond back-end IP příjem provozu, společně s požadovaný zdrojový a cílový port. Pokud chcete mít jistotu, že virtuální počítače pouze v pořádku přijímat přenosy, také definovat test stavu použít.

Vytvořit pravidlo Vyrovnávání zatížení s [vytvořit pravidlo vyrovnáváním zatížení sítě az](/cli/azure/network/lb/rule#create). Následující příklad vytvoří pravidlo s názvem *myLoadBalancerRule*, používá *myHealthProbe* test stavu a zůstatky přenosy na portu *80*:

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Konfigurace virtuální sítě
Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte doprovodné materiály virtuální sítě. Další informace o virtuálních sítích najdete v tématu [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.

### <a name="create-network-resources"></a>Vytvoření síťové prostředky
Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Následující příklad vytvoří virtuální síť s názvem *myVnet* s podsítí s názvem *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

Chcete-li přidat skupinu zabezpečení sítě, je použít [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

Vytvoření pravidla skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create). Následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem *myNetworkSecurityGroupRule*:

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Virtuální síťové adaptéry jsou vytvořeny pomocí [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Následující příklad vytvoří tři virtuálních síťových karet. (Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro vaši aplikaci v následujících krocích). Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a jejich přidání do Vyrovnávání zatížení:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

Když se vytváří všechny tři virtuální síťové karty, pokračujte k dalšímu kroku


## <a name="create-virtual-machines"></a>Vytváření virtuálních počítačů

### <a name="create-cloud-init-config"></a>Vytvoření konfigurace cloudu init
V předchozích kurz [postup přizpůsobení virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md), jste se dozvěděli, jak automatizovat přizpůsobení virtuálního počítače s inicializací cloudu. Konfiguračního souboru stejné cloudové init můžete použít k instalaci NGINX a spuštění jednoduchý "zdravím svět" aplikace Node.js v dalším kroku. Chcete-li zobrazit nástroje pro vyrovnávání zatížení v akci na konci tohoto kurzu máte přístup k této jednoduchou aplikaci ve webovém prohlížeči.

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

### <a name="create-virtual-machines"></a>Vytváření virtuálních počítačů
Pokud chcete zvýšit vysokou dostupnost vaší aplikace, umístíte virtuální počítače v nastavení dostupnosti. Další informace o nastavení dostupnosti, najdete v předchozí [vytváření vysoce dostupných virtuálních počítačů](tutorial-availability-sets.md) kurzu.

Vytvořit sadu s dostupnosti [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create). Následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

Nyní můžete vytvořit virtuálních počítačů s [vytvořit virtuální počítač az](/cli/azure/vm#create). Následující příklad vytvoří tři virtuální počítače a generuje klíče SSH, pokud už neexistují:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku. `--no-wait` Parametr nečeká všechny na dokončení úlohy. To může být jiná několik minut, než může aplikaci používat. Test stavu nástroje pro vyrovnávání zatížení automaticky rozpozná, když aplikace běží na každém virtuálním počítači. Jakmile aplikace běží, spustí se pravidlo Vyrovnávání zatížení k distribuci přenosů.


## <a name="test-load-balancer"></a>Nástroj pro vyrovnávání zatížení testu
Získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show). Následující příklad, získá IP adresu pro *myPublicIP* vytvořili dříve:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Potom můžete zadat veřejnou IP adresu v do webového prohlížeče. Mějte na paměti, – trvá několik minut pro virtuální počítače bude připravená, před spuštěním nástroje pro vyrovnávání zatížení softwaru k distribuci přenosů na ně. Aplikace se zobrazí, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz jako v následujícím příkladu:

![Spuštěné aplikace Node.js](./media/tutorial-load-balancer/running-nodejs-app.png)

Nástroje pro vyrovnávání zatížení provoz distribuovat mezi všechny tři virtuální počítače spuštěné aplikace najdete můžete můžete vynutit obnovení webového prohlížeče.


## <a name="add-and-remove-vms"></a>Přidání a odebrání virtuálních počítačů
Potřebujete provést údržbu na virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému. Jak nakládat s zvýšení provozu do vaší aplikace, musíte pro přidání dalších virtuálních počítačů. V této části se dozvíte, jak odebrat nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení.

### <a name="remove-a-vm-from-the-load-balancer"></a>Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení
Virtuální počítač můžete odebrat z fondu adres back-end s [odebrat az sítě síťový adaptér ip-config fond adres](/cli/azure/network/nic/ip-config/address-pool#remove). Následující příklad odebere virtuální síťový adaptér pro **Můjvp2** z *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

Zobrazit nástroje pro vyrovnávání zatížení provoz distribuovat mezi zbývající dva virtuální počítače používající vaši aplikaci je můžete vynutit obnovení webového prohlížeče. Nyní můžete provést údržbu na virtuální počítač, jako je instalace aktualizací operačního systému nebo provádění restartování virtuálního počítače.

Chcete-li zobrazit seznam virtuálních počítačů s virtuální síťové adaptéry připojené ke službě Vyrovnávání zatížení, použijte [az sítě lb fond adres zobrazit](/cli/azure/network/lb/address-pool#show). Dotazování a filtrovat podle ID virtuálního síťového adaptéru následujícím způsobem:

```azurecli-interactive
az network lb address-pool show \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myBackEndPool \
    --query backendIpConfigurations \
    --output tsv | cut -f4
```

Výstup bude vypadat v následujícím příkladu, který ukazuje, že virtuální síťový adaptér pro virtuální počítač 2 je už součástí fondu adres back-end:

```bash
/subscriptions/<guid>/resourceGroups/myResourceGroupLoadBalancer/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/ipconfig1
/subscriptions/<guid>/resourceGroups/myResourceGroupLoadBalancer/providers/Microsoft.Network/networkInterfaces/myNic3/ipConfigurations/ipconfig1
```

### <a name="add-a-vm-to-the-load-balancer"></a>Přidat virtuální počítač ke službě Vyrovnávání zatížení
Po provedení údržby virtuálních počítačů, nebo pokud potřebujete rozšířit kapacitu, můžete přidat virtuální počítač do fondu adres back-end s [az sítě síťový adaptér ip-config fond adres přidat](/cli/azure/network/nic/ip-config/address-pool#add). Následující příklad přidá virtuální síťovou kartu pro **Můjvp2** k *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```

Chcete-li ověřit, že virtuální síťový adaptér je připojený k fondu adres back-end, použijte [az sítě lb fond adres zobrazit](/cli/azure/network/lb/address-pool#show) znovu z předchozího kroku.


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste vytvořili pro vyrovnávání zatížení a je připojený virtuální počítače. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření pro vyrovnávání zatížení Azure
> * Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
> * Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení
> * Pomocí cloud init můžete vytvořit základní aplikaci Node.js
> * Vytváření virtuálních počítačů a připojit ke službě Vyrovnávání zatížení
> * Zobrazit nástroj pro vyrovnávání zatížení v akci
> * Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení

Přechodu na v dalším kurzu se dozvíte informace o komponentách virtuální síť Azure.

> [!div class="nextstepaction"]
> [Správa virtuálních počítačů a virtuálních sítí](tutorial-virtual-network.md)
