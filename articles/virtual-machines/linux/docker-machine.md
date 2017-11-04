---
title: "Použít počítač Docker v Azure vytvořit hostitele systému Linux | Microsoft Docs"
description: "Popisuje, jak použít počítač Docker k vytvoření hostitelů Docker v Azure."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: iainfou
ms.openlocfilehash: 7772381e9796ddc2e0db215bab4f230473eaa462
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-docker-machine-to-create-hosts-in-azure"></a>Jak vytvořit hostitele v Azure pomocí počítač Docker
Tento článek popisuje, jak používat [počítač Docker](https://docs.docker.com/machine/) v Azure vytvořit hostitele. `docker-machine` Příkaz vytvoří virtuální počítač (VM) s Linuxem v Azure, pak nainstaluje Docker. Pak můžete spravovat hostitelů Docker v Azure pomocí stejných místní nástrojů a pracovních postupů. Použití docker počítače v systému Windows 10, musíte použít Linux bash.

## <a name="create-vms-with-docker-machine"></a>Vytvořit virtuální počítače s počítačem Docker
Nejdřív získat svoje ID předplatného Azure s [az účet zobrazit](/cli/azure/account#show) následujícím způsobem:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

Vytvořte virtuální počítače hostitelů Docker v Azure s `docker-machine create` zadáním *azure* jako ovladač. Další informace najdete v tématu [Docker Azure ovladač dokumentace](https://docs.docker.com/machine/drivers/azure/)

Následující příklad vytvoří virtuální počítač s názvem *Můjvp*, na základě plánu "Standardní D2 v2", vytvoří uživatelský účet s názvem *azureuser*a otevře port *80* na hostiteli virtuálního počítače. Postupujte podle výzev k přihlášení k účtu Azure a udělte počítač Docker oprávnění k vytvoření a správě prostředků.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    --azure-size "Standard_D2_v2 \
    myvm
```

Výstup bude vypadat podobně jako v následujícím příkladu:

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvmdocker) Completed machine pre-create checks.
Creating machine...
(myvmdocker) Querying existing resource group.  name="docker-machine"
(myvmdocker) Creating resource group.  name="docker-machine" location="westus"
(myvmdocker) Configuring availability set.  name="docker-machine"
(myvmdocker) Configuring network security group.  name="myvmdocker-firewall" location="westus"
(myvmdocker) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvmdocker) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvmdocker) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvmdocker) Creating public IP address.  name="myvmdocker-ip" static=false
(myvmdocker) Creating network interface.  name="myvmdocker-nic"
(myvmdocker) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvmdocker) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvmdocker"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env myvmdocker
```

## <a name="configure-your-docker-shell"></a>Konfigurovat své prostředí Docker
Pro připojení k hostiteli Docker v Azure, definujte nastavení odpovídající připojení. Jak je uvedeno na konci výstupu, zobrazení informací o připojení pro váš hostitel Docker následujícím způsobem: 

```bash
docker-machine env myvmdocker
```

Výstup se podobá následujícímu příkladu:

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env myvmdocker)
```

K definování připojení nastavení, můžete buď spustit příkaz navrhovaná konfigurace (`eval $(docker-machine env myvmdocker)`), nebo můžete ručně nastavit proměnné prostředí. 

## <a name="run-a-container"></a>Spusťte kontejner
Chcete-li zobrazit kontejner v akci, umožňuje spustit základní webový server NGINX. Vytvořit kontejner s `docker run` a vystavit port 80 pro webové přenosy následujícím způsobem:

```bash
docker run -d -p 80:80 --restart=always nginx
```

Výstup se podobá následujícímu příkladu:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
ff3d52d8f55f: Pull complete
226f4ec56ba3: Pull complete
53d7dd52b97d: Pull complete
Digest: sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
Status: Downloaded newer image for nginx:latest
675e6056cb81167fe38ab98bf397164b01b998346d24e567f9eb7a7e94fba14a
```

Zobrazení spuštěných kontejnerů s `docker ps`. Následující příklad výstupu zobrazuje kontejner nginx a SVÁŽE s portem 80 zveřejněné:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-the-container"></a>Test kontejneru
Získejte veřejnou IP adresu hostitele Docker následujícím způsobem:


```bash
docker-machine ip myvmdocker
```

Informace o kontejneru v akci, otevřete webový prohlížeč a zadejte veřejnou IP adresu uvedeno ve výstupu předchozí příkaz:

![Spuštěné ngnix kontejneru](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Další kroky
Můžete také vytvořit hostitele s [rozšíření virtuálního počítače Docker](dockerextension.md). Příklady na pomocí Docker Compose najdete v tématu [začít pracovat s Docker a vytvořit v Azure](docker-compose-quickstart.md).
