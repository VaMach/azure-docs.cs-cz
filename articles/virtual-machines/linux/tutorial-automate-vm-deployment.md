---
title: "Přizpůsobení virtuálního počítače s Linuxem na při prvním spuštění v Azure | Microsoft Docs"
description: "Další informace o použití cloudu init a Key Vault pro virtuální počítače s Linuxem customze při prvním spuštění v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 83773e513ee2c92da733df05cd17dda2940a28cd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-customize-a-linux-virtual-machine-on-first-boot"></a>Postup přizpůsobení virtuální počítač s Linuxem na při prvním spuštění
V předchozích kurzu jste se dozvěděli, jak chcete SSH pro virtuální počítač (VM) a ručně nainstalujte NGINX. Pokud chcete vytvořit virtuální počítače rychlý a konzistentním způsobem, je obvykle potřeby nějaký způsob automatizace. Běžný postup přizpůsobení virtuálního počítače při prvním spuštění počítače se má používat [cloudu init](https://cloudinit.readthedocs.io). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořte soubor konfigurace cloudu init
> * Vytvořit virtuální počítač, který používá soubor init cloudu
> * Zobrazit spuštěné aplikaci Node.js, po vytvoření virtuálního počítače
> * Pomocí Key Vault bezpečně uložit certifikáty
> * Automatizovat zabezpečená nasazení NGINX s inicializací cloudu


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).  



## <a name="cloud-init-overview"></a>Přehled Cloud-init
[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup k přizpůsobení virtuálního počítače s Linuxem, jako při prvním spuštění. Init cloudu můžete použít k instalaci balíčků a zapisovat soubory nebo konfigurace zabezpečení a uživatelů. Init cloudu běží během úvodního spouštění, nejsou žádné další kroky nebo požadované agenty použít konfiguraci.

Init cloudu také funguje v různých distribucí. Například nepoužívejte **výstižný get instalace** nebo **yum nainstalovat** nainstalovat balíček. Místo toho můžete definovat seznam balíčků pro instalaci. Init cloudu automaticky používá nástroj pro správu nativní balíčku pro distro, kterou vyberete.

Pracujeme s našimi partnery získat cloudu init zahrnuté a práci v bitové kopie, které poskytují do Azure. Následující tabulka popisuje aktuální dostupnosti cloudu init Image platformy Azure:

| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 LTS |nejnovější |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |nejnovější |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |
| | OpenLogic | CentOS | 7 CI | nejnovější |
| | RedHat | RHEL | 7 NEZPRACOVANÁ POLOŽEK KONFIGURACE | nejnovější


## <a name="create-cloud-init-config-file"></a>Vytvoření cloudové init konfiguračního souboru
Informace o cloudu init v akci, vytvořte virtuální počítač, který nainstaluje NGINX a spustí jednoduchý "zdravím svět" aplikace Node.js. Následující konfigurace cloudu init nainstaluje požadované balíčky, vytvoří aplikace Node.js, potom inicializaci a spuštění aplikace.

V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a vložte následující konfigurace. Například vytvoření souboru v prostředí cloudu není na místním počítači. Můžete použít libovolný editor, které chcete. Zadejte `sensible-editor cloud-init.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory. Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:

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

Další informace o možnostech konfigurace cloudu init najdete v tématu [příklady konfigurace cloudu init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). Použití `--custom-data` parametr předávat do cloudu init konfiguračního souboru. Zadejte úplnou cestu k *cloudu init.txt* konfigurace, pokud jste uložili soubor mimo pracovní adresář existuje. Následující příklad vytvoří virtuální počítač s názvem *myAutomatedVM*:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Trvá několik minut pro virtuální počítač, který se má vytvořit, balíčky určené k instalaci a aplikaci spusťte. Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku. To může být jiná několik minut, než může aplikaci používat. Po vytvoření virtuálního počítače, poznamenejte si `publicIpAddress` zobrazí pomocí rozhraní příkazového řádku Azure. Tato adresa se používá pro přístup k aplikaci Node.js pomocí webového prohlížeče.

Povolit webový provoz připojit virtuální počítač, otevřete port 80 z Internetu s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a>Test webové aplikace
Nyní můžete otevřít webový prohlížeč a zadejte *http://<publicIpAddress>*  na panelu Adresa. Zadejte vlastní veřejná IP adresa z virtuálního počítače vytvořit proces. Aplikace Node.js se zobrazí jako v následujícím příkladu:

![Zobrazení spuštěných NGINX lokality](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a>Vložit certifikáty z Key Vault
Této volitelné části ukazuje, jak můžete bezpečně uložit certifikáty v Azure Key Vault a vložit je při nasazení virtuálního počítače. Místo použití vlastní image, která obsahuje certifikáty zaručená v, tento proces zajišťuje, že nejaktuálnější certifikáty jsou aplikován na virtuální počítač při prvním spuštění počítače. Během procesu certifikát nikdy opustí platformy Azure nebo je vystaven ve skriptu, historie příkazového řádku nebo šablony.

Azure Key Vault chrání kryptografické klíče a tajné klíče, jako je například certifikáty nebo hesla. Key Vault pomáhá zjednodušit proces správy klíčů a zajišťuje vám kontrolu nad klíči, které k přístupu a šifrování vaše data. Tento scénář jsou uvedené některé pojmy Key Vault, jak vytvořit a používat certifikát, ale není vyčerpávající přehled o tom, jak používat Key Vault.

Následující kroky ukazují, jak můžete:

- Vytvoření Azure Key Vault
- Generovat nebo nahrajte certifikát do služby Key Vault
- Vytvoření tajného klíče z certifikátu se vložit v pro virtuální počítač
- Vytvoření virtuálního počítače a vložit certifikátu

### <a name="create-an-azure-key-vault"></a>Vytvoření Azure Key Vault
Nejprve vytvořte Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolit pro použití při nasazení virtuálního počítače. Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena. Nahraďte *mykeyvault* v následujícím příkladu se svůj vlastní jedinečný název pro Key Vault:

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a>Vygenerování certifikátu a uložit v Key Vault
Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [import certifikátu keyvault az](/cli/azure/keyvault/certificate#import). V tomto kurzu následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [vytvoření certifikátu keyvault az](/cli/azure/keyvault/certificate#create) používající výchozí zásady certifikátu:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a>Příprava certifikátu pro použití s virtuálním Počítačem.
K použití certifikátu během virtuálního počítače vytvořit proces, získat číslo ID vašeho certifikátu s [az keyvault verze sdíleného tajného klíče seznam-](/cli/azure/keyvault/secret#list-versions). Virtuální počítač, musí certifikát v určitém formátu vložit na spuštění, takže převést certifikát s [az virtuálních počítačů formát secret](/cli/azure/vm#format-secret). Následující příklad přiřadí proměnné pro snadné použití v dalších krocích výstup z těchto příkazů:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-to-secure-nginx"></a>Vytvoření konfigurace cloudu init zabezpečit NGINX
Když vytvoříte virtuální počítač, certifikáty a klíče jsou uložené v chráněného */var/lib/příkazwaagent/* adresáře. K automatizaci certifikát se přidává do virtuálního počítače a konfigurace NGINX, můžete použít aktualizované cloudu init konfigurace z předchozího příkladu.

Vytvořte soubor s názvem *cloudu. init secured.txt* a vložte následující konfigurace. Znovu Pokud používáte cloudové prostředí, vytvořte konfigurační soubor cloudu init tam a není na místním počítači. Použití `sensible-editor cloud-init-secured.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory. Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:

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
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
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
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-secure-vm"></a>Vytvoření zabezpečeného virtuálního počítače
Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). Data certifikátu je vložili z trezoru klíčů `--secrets` parametr. Jako v předchozím příkladu můžete předat také v konfiguraci cloudu init s `--custom-data` parametr:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-secured.txt \
    --secrets "$vm_secret"
```

Trvá několik minut pro virtuální počítač, který se má vytvořit, balíčky určené k instalaci a aplikaci spusťte. Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku. To může být jiná několik minut, než může aplikaci používat. Po vytvoření virtuálního počítače, poznamenejte si `publicIpAddress` zobrazí pomocí rozhraní příkazového řádku Azure. Tato adresa se používá pro přístup k aplikaci Node.js pomocí webového prohlížeče.

Povolit zabezpečený webový provoz připojit virtuální počítač, otevřete port 443 z Internetu s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a>Testování zabezpečení webové aplikace
Nyní můžete otevřít webový prohlížeč a zadejte *https://<publicIpAddress>*  na panelu Adresa. Zadejte vlastní veřejná IP adresa z virtuálního počítače vytvořit proces. Pokud použijete certifikát podepsaný svým držitelem, přijměte upozornění zabezpečení:

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-automate-vm-deployment/browser-warning.png)

Zabezpečené NGINX lokality a Node.js aplikace se potom zobrazí jako v následujícím příkladu:

![Zobrazit spuštěná zabezpečené NGINX Web](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali virtuální počítače při prvním spuštění počítače s inicializací cloudu. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvořte soubor konfigurace cloudu init
> * Vytvořit virtuální počítač, který používá soubor init cloudu
> * Zobrazit spuštěné aplikaci Node.js, po vytvoření virtuálního počítače
> * Pomocí Key Vault bezpečně uložit certifikáty
> * Automatizovat zabezpečená nasazení NGINX s inicializací cloudu

Přechodu na v dalším kurzu se dozvíte, jak vytvořit vlastní Image virtuálních počítačů.

> [!div class="nextstepaction"]
> [Vytváření vlastních imagí virtuálních počítačů](./tutorial-custom-images.md)
