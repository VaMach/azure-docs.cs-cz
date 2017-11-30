---
title: "Zabezpečení webového serveru pomocí certifikátů SSL v Azure | Microsoft Docs"
description: "Zjistěte, jak zabezpečit NGINX webového serveru pomocí certifikátů SSL na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d2d6a0b00704e1d97be9a4c5bd00ba37374419e5
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a>Zabezpečení webového serveru pomocí certifikátů SSL na virtuální počítač s Linuxem v Azure
Pro zabezpečení webové servery, certifikát později SSL (Secure Sockets) slouží k šifrování webový provoz. Tyto certifikáty SSL můžou být uložená v Azure Key Vault a povolit zabezpečená nasazení certifikátů na virtuálních počítačích (VM) s Linuxem v Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření Azure Key Vault
> * Generovat nebo nahrajte certifikát do služby Key Vault
> * Vytvoření virtuálního počítače a nainstalovat webový server NGINX
> * Vložit certifikát do virtuálního počítače a konfigurace NGINX s vazbou SSL

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).  


## <a name="overview"></a>Přehled
Azure Key Vault chrání kryptografické klíče a tajné klíče, tyto certifikáty a hesla. Key Vault pomáhá zjednodušit proces správy certifikátů a zajišťuje vám kontrolu nad klíči, které přístup těchto certifikátů. Můžete vytvořit certifikát podepsaný svým držitelem v Key Vault nebo nahrát certifikát existující, důvěryhodné, který už vlastníte.

Místo použití vlastní image virtuálního počítače, který zahrnuje certifikáty zaručená v, vložit certifikáty do spuštěného virtuálního počítače. Tento proces zajišťuje, že nejaktuálnější certifikáty jsou nainstalovány na webovém serveru během nasazení. Je-li obnovit nebo nahradit certifikát, nemáte také vytvořit novou vlastní imagi virtuálního počítače. Nejnovější certifikáty jsou automaticky vložit, jako je vytváření dalších virtuálních počítačů. Během celého procesu certifikáty nikdy nechte platformy Azure nebo jsou viditelné ve skriptu, historie příkazového řádku nebo šablony.


## <a name="create-an-azure-key-vault"></a>Vytvoření Azure Key Vault
Před vytvořením Key Vault a certifikáty, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupSecureWeb* v *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Dále vytvořte Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolit pro použití při nasazení virtuálního počítače. Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena. Nahraďte  *<mykeyvault>*  v následujícím příkladu se svůj vlastní jedinečný název pro Key Vault:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Vygenerování certifikátu a uložit v Key Vault
Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [import certifikátu keyvault az](/cli/azure/keyvault/certificate#az_keyvault_certificate_import). V tomto kurzu následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [vytvoření certifikátu keyvault az](/cli/azure/keyvault/certificate#az_keyvault_certificate_create) používající výchozí zásady certifikátu:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Příprava certifikát pro použití s virtuálního počítače
K použití certifikátu během virtuálního počítače vytvořit proces, získat číslo ID vašeho certifikátu s [az keyvault verze sdíleného tajného klíče seznam-](/cli/azure/keyvault/secret#list-versions). Převést certifikát s [az virtuálních počítačů formát secret](/cli/azure/vm#format-secret). Následující příklad přiřadí proměnné pro snadné použití v dalších krocích výstup z těchto příkazů:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-to-secure-nginx"></a>Vytvoření konfigurace cloudu init zabezpečit NGINX
[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup k přizpůsobení virtuálního počítače s Linuxem, jako při prvním spuštění. Init cloudu můžete použít k instalaci balíčků a zapisovat soubory nebo konfigurace zabezpečení a uživatelů. Init cloudu běží během úvodního spouštění, nejsou žádné další kroky nebo požadované agenty použít konfiguraci.

Když vytvoříte virtuální počítač, certifikáty a klíče jsou uložené v chráněného */var/lib/příkazwaagent/* adresáře. Pokud chcete automatizovat certifikát se přidává do virtuálního počítače a konfigurace webového serveru, použijte init cloudu. V tomto příkladu jsme nainstalujte a nakonfigurujte NGINX webový server. Stejný postup můžete použít k instalaci a konfiguraci Apache. 

Vytvořte soubor s názvem *cloudu init webové server.txt* a vložte následující konfiguraci:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a>Vytvoření zabezpečeného virtuálního počítače
Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). Data certifikátu je vložili z trezoru klíčů `--secrets` parametr. Předáte v konfiguraci cloudu init s `--custom-data` parametr:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

Trvá několik minut pro virtuální počítač, který se má vytvořit, balíčky určené k instalaci a aplikaci spusťte. Po vytvoření virtuálního počítače, poznamenejte si `publicIpAddress` zobrazí pomocí rozhraní příkazového řádku Azure. Tato adresa se používá k přístupu k webu ve webovém prohlížeči.

Povolit zabezpečený webový provoz připojit virtuální počítač, otevřete port 443 z Internetu s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-the-secure-web-app"></a>Testování zabezpečení webové aplikace
Nyní můžete otevřít webový prohlížeč a zadejte *https://<publicIpAddress>*  na panelu Adresa. Zadejte vlastní veřejná IP adresa z virtuálního počítače vytvořit proces. Pokud použijete certifikát podepsaný svým držitelem, přijměte upozornění zabezpečení:

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-secure-web-server/browser-warning.png)

Zabezpečené NGINX webu se následně zobrazí jako v následujícím příkladu:

![Zobrazit spuštěná zabezpečené NGINX Web](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu webového serveru se službou NGINX zabezpečená certifikátem SSL uložené v Azure Key Vault. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření Azure Key Vault
> * Generovat nebo nahrajte certifikát do služby Key Vault
> * Vytvoření virtuálního počítače a nainstalovat webový server NGINX
> * Vložit certifikát do virtuálního počítače a konfigurace NGINX s vazbou SSL

Tento odkaz zobrazíte ukázky skriptu předdefinovaných virtuálního počítače.

> [!div class="nextstepaction"]
> [Ukázky skriptu Windows virtuálního počítače](./cli-samples.md)