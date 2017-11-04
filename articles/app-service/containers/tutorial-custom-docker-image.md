---
title: "Použít vlastní image Docker pro webovou aplikaci pro kontejnery - Azure | Microsoft Docs"
description: "Jak používat vlastní image Docker pro webovou aplikaci pro kontejnery."
keywords: "služby Azure app service, webové aplikace, linux, docker, kontejneru"
services: app-service
documentationcenter: 
author: SyntaxC4
manager: SyntaxC4
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 4ba53dd1239290c64907ed431d404b2d1be66c36
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="use-a-custom-docker-image-for-web-app-for-containers"></a>Použít vlastní image Docker pro webovou aplikaci pro kontejnery

[Webová aplikace pro kontejnery](app-service-linux-intro.md) poskytuje integrované imagí Dockeru v systému Linux s podporou pro konkrétní verze, jako je například PHP 7.0 a Node.js 4.5. Webové aplikace pro kontejnery používá technologii kontejner Docker k hostování předdefinovaných obrázků a vlastních bitových kopií jako platforma jako služba. V tomto kurzu zjistěte, jak chcete vytvořit vlastní image Docker a nasaďte ho do webové aplikace pro kontejnery. Tento vzor je užitečné, když předdefinovaných obrázků neobsahují vámi zvolený jazyk, nebo pokud vaše aplikace vyžaduje určitou konfiguraci, která není k dispozici v rámci integrované bitové kopie.

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* [Git](https://git-scm.com/downloads)
* Aktivní [předplatného Azure](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* A [účet úložiště Docker Hub](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

V okně terminálu spusťte následující příkaz klonovat úložiště ukázkové aplikace do místního počítače a pak přejděte do adresáře, který obsahuje ukázkový kód.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Sestavení bitové kopie ze souboru Docker

V úložišti Git, podívejte se na _soubor Docker_. Tento soubor popisuje prostředí Python, který je potřeba spustit svoji aplikaci. Kromě toho nastaví bitovou kopii [SSH](https://www.ssh.com/ssh/protocol/) server pro zabezpečenou komunikaci mezi hostiteli a kontejneru.

```docker
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

Chcete-li vytvořit bitovou kopii Docker, spusťte `docker build` příkaz a zadejte název, `mydockerimage`a značky, `v1.0.0`. Nahraďte `<docker-id>` s úložiště Docker Hub účet ID.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Příkaz vytvoří výstup podobný následujícímu:

```bash
# The output from the commands in this article has been shortened for brevity.

Sending build context to Docker daemon  5.558MB
Step 1/13 : FROM python:3.4
 ---> 9ff45ddb54e9
Step 2/13 : RUN mkdir /code
 ---> Using cache
 ---> f3f3ac01db0a
Step 3/13 : WORKDIR /code
 ---> Using cache
 ---> 38b32f15b442
.
.
.
Step 13/13 : ENTRYPOINT init.sh
 ---> Running in 5904e4c70043
 ---> e7cf08275692
Removing intermediate container 5904e4c70043
Successfully built e7cf08275692
Successfully tagged cephalin/mydockerimage:v1.0.0
```

Test, který sestavení funguje tak, že spustíte kontejner Docker. Problém [docker spustit](https://docs.docker.com/engine/reference/commandline/run/) příkazů a jí předat název a značka obrázku. Nezapomeňte zadat pomocí portu `-p` argument.

```bash
docker run -p 2222:8000 <docker-ID>/mydockerimage:v1.0.0
```

Ověření webové aplikace a kontejner fungují správně procházením `http://localhost:2222`.

![Test webové aplikace místně](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

## <a name="push-the-docker-image-to-docker-hub"></a>Odešlete do úložiště Docker Hub bitovou kopii Docker

Registr je aplikace, která hostuje bitové kopie a poskytuje služby bitové kopie a kontejneru. Aby bylo možné sdílet bitové kopie, musí poslat ho přímo registru. 

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry such as Azure Container Registry. Select the appropriate tab for your scenario below (your selection will switch multiple tabs on this page). -->

> [!NOTE]
> Vkládání do registru privátní Docker? Volitelné pokynů v tématu [nabízená bitovou kopii Docker privátní registru](#push-a-docker-image-to-private-registry-optional).

<!--## [Docker Hub](#tab/docker-hub)-->

Úložiště docker Hub je registr pro Docker bitové kopie, který umožňuje hostování vlastní úložiště, veřejné nebo soukromé. K replikaci vlastní image Docker na veřejné úložiště Docker Hub, použijte [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) příkaz a zadejte název úplnou bitovou kopii a značky. Název úplnou bitovou kopii a značky vypadá následující ukázka:

```bash
<docker-id>/image-name:tag
```

Pokud není přihlášený do úložiště Docker Hub, to provést pomocí [docker přihlášení](https://docs.docker.com/engine/reference/commandline/login/) příkazu před pokusem o push bitovou kopii.

```bash
docker login --username <docker-id> --password <docker-hub-password>
```

Zpráva "přihlášení bylo úspěšné" potvrdí, že jste přihlášeni. Po přihlášení, můžete posouvat bitovou kopii pomocí Docker Hub [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) příkaz.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Ověřte, že nabízené úspěšné prověřením příkaz výstupu.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
c33197c3f6d4: Pushed
ccd2c850ee43: Pushed
02dff2853466: Pushed
6ce78153632a: Pushed
efef3f03cc58: Pushed
3439624d77fb: Pushed
3a07adfb35c5: Pushed
2fcec228e1b7: Mounted from library/python
97d2d3bae505: Mounted from library/python
95aadeabf504: Mounted from library/python
b456afdc9996: Mounted from library/python
d752a0310ee4: Mounted from library/python
db64edce4b5b: Mounted from library/python
d5d60fc34309: Mounted from library/python
c01c63c6823d: Mounted from library/python
v1.0.0: digest: sha256:21f2798b20555f4143f2ca0591a43b4f6c8138406041f2d32ec908974feced66 size: 3676
```

<!--
# [Private Registry](#tab/private-registry)

// Move Private Registry instructions here when Tabbed Conceptual bug is fixed

---
-->

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Nasazení aplikace do Azure

Je možné hostovat nativní Linux aplikací v cloudu pomocí Azure Web Apps. K vytvoření webové aplikace pro kontejnery, je nutné spustit příkazy rozhraní příkazového řádku Azure, které vytvořit skupinu, potom plán služeb a nakonec webové aplikace. 

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-linux-app-service-plan"></a>Vytvoření plánu služby App Service pro Linux

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)] 

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ve službě Cloud Shell pomocí příkazu [az webapp create](/cli/azure/webapp#create) vytvořte [webovou aplikaci](app-service-linux-intro.md) v plánu služby App Service `myAppServicePlan`. Nezapomeňte nahradit `<app_name>` s jedinečným názvem aplikace a < docker-ID > s ID Docker.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-container-image-name <docker-ID>/mydockerimage:v1.0.0
```

Po vytvoření webové aplikace Azure CLI ukazuje výstup podobně jako v následujícím příkladu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-environment-variables"></a>Nakonfigurujte proměnné prostředí

Většina imagí Dockeru mít proměnné prostředí, které musí být nakonfigurované. Pokud používáte stávající image Docker vytvořené někdo jiný, může použít bitovou kopii jiný port než 80. Informuje Azure o port, který používá image pomocí `WEBSITES_PORT` nastavení aplikace. Stránku Githubu [ukázka Pythonu v tomto kurzu](https://github.com/Azure-Samples/docker-django-webapp-linux) ukazuje, které je nutné nastavit `WEBSITES_PORT` k _8000_.

Chcete-li nastavení aplikace, použijte [az webapp konfigurace appsettings aktualizovat](/cli/azure/webapp/config/appsettings#update) příkazu v prostředí cloudu. Nastavení aplikací jsou velká a malá písmena a oddělených mezerami.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_PORT=8000
```

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry, such as Azure Container Registry. Select the appropriate tab for your scenario below: -->

> [!NOTE]
> Nasazení z registru privátní Docker? Volitelné pokynů v tématu [nakonfigurovat webovou aplikaci k používání kontejner Docker z privátní registru](#configure-web-app-to-use-docker-container-from-a-private-registry-optional).

<!-- # [Docker Hub](#tab/docker-hub)-->

<!-- # [Private Registry](#tab/private-registry)

// Place Private Registry text back here once Tabbed Conceptual bug is fixed

---
-->

### <a name="test-the-web-app"></a>Test webové aplikace

Ověřte, že webová aplikace funguje tak, že přejde k němu (`http://<app_name>azurewebsites.net`). 

![Konfiguraci portů test webové aplikace](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Změny webové aplikace a znovu ho zaveďte

V místní úložiště Git otevřete app/templates/app/index.html. Najít první prvek HTML a změňte ji na.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">         
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>       
      </div>            
    </div>
  </nav> 
```

Jakmile jste upravit soubor Python a uložíte ho, musíte znovu sestavit a push novou bitovou kopii Docker. Potom restartujte webovou aplikaci se změny projevily. Použijte stejné příkazy, které jste už dřív použili v tomto kurzu. Můžete se podívat do [sestavení bitové kopie ze souboru Docker](#build-the-image-from-the-docker-file) a [Push bitovou kopii Docker do úložiště Docker Hub](#push-the-docker-image-to-docker-hub). Test webové aplikace podle pokynů v [Test webové aplikace](#test-the-web-app).

## <a name="connect-to-web-app-for-containers-using-ssh"></a>Připojení k webové aplikace pro kontejnery pomocí protokolu SSH

SSH umožňuje zabezpečenou komunikaci mezi kontejner a klienta. Aby pro vlastní image Docker pro podporu SSH musí být sestavení do soubor Docker. Povolíte SSH v samotném souboru Docker. Podle pokynů SSH již byly přidány do ukázkový soubor docker, tak můžete postupovat podle těchto pokynů s svoji vlastní image:

* A [spustit](https://docs.docker.com/engine/reference/builder/#run) instrukce, která volá `apt-get`, pak nastaví heslo pro kořenový účet, který chcete `"Docker!"`.

    ```docker
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Tato konfigurace neumožňuje externí připojení ke kontejneru. SSH je k dispozici pouze prostřednictvím webu Kudu/SCM. Kudu/SCM lokality je ověření pomocí přihlašovací údaje pro publikování.

* A [kopie](https://docs.docker.com/engine/reference/builder/#copy) pokyn, dá pokyn modulu Docker zkopírovat [sshd_config](http://man.openbsd.org/sshd_config) do souboru */atd/ssh/* adresáře. Konfigurační soubor by měla být založena na [tento soubor sshd_config](https://github.com/Azure-App-Service/node/blob/master/6.11.1/sshd_config).

    ```docker
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > *Sshd_config* soubor musí zahrnovat následující položky: 
    > * `Ciphers`musí obsahovat alespoň jednu položku v tomto seznamu: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`musí obsahovat alespoň jednu položku v tomto seznamu: `hmac-sha1,hmac-sha1-96`.

* [Vystavit](https://docs.docker.com/engine/reference/builder/#expose) pokyn, aby zpřístupňuje portu 2222 v kontejneru. I když se označuje kořenové heslo, port 2222 není přístupný z Internetu. Je k interní portu přístupné pouze pomocí kontejnery v rámci sítě most privátní virtuální sítě. Potom příkaz Kopírovat podrobnosti o konfiguraci SSH a spustí `ssh` služby.

    ```docker
    EXPOSE 8000 2222

    RUN service ssh start
    ```

### <a name="open-ssh-connection-to-container"></a>Otevřete připojení SSH do kontejneru

Webové aplikace pro kontejnery neumožňuje externí připojení ke kontejneru. SSH je k dispozici pouze prostřednictvím Kudu lokalitě, která je přístupná na `https://<app_name>.scm.azurewebsites.net`.

Pokud chcete připojit, přejděte do `https://<app_name>.scm.azurewebsites.net/webssh/host` a přihlaste se pomocí účtu Azure.

Potom budete přesměrováni na stránku zobrazení interaktivní konzoly. 

Chcete ověřit, že některé aplikace spuštěné v kontejneru. Kontrolovat kontejneru a ověřte spuštěné procesy, vydání `top` příkazu příkazového řádku.

```bash
top
```

`top` Příkaz zpřístupní všechny spuštěné procesy v kontejneru.

```bash
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Blahopřejeme! Vlastní image Docker jste nakonfigurovali pro webovou aplikaci pro kontejnery.

## <a name="use-a-private-image-from-docker-hub-optional"></a>Použít privátní bitovou kopii z úložiště Docker Hub (volitelné)

V [vytvořit webovou aplikaci](#create-a-web-app), jste zadali bitovou kopii na úložiště Docker Hub v `az webapp create` příkaz. Toto je dostatečně vhodné pro veřejnou bitové kopie. Chcete-li použít privátní bitovou kopii, musíte nakonfigurovat Docker ID účtu a heslo v Azure webové aplikace.

V prostředí cloudu, postupujte podle `az webapp create` s [sadu kontejneru konfigurace webapp az](/cli/azure/webapp/config/container#az_webapp_config_container_set). Nahraďte  *\<app_name >*a také _< docker-id >_ a  _<password>_  pomocí Docker ID a hesla.

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-registry-server-user <docker-id> --docker-registry-server-password <password>
```

Příkaz zobrazí se výstup podobný následující řetězce formátu JSON, zobrazující, že změna konfigurace úspěšně:

```bash
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "false"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<docker-id>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  },
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "DOCKER|<image-name-and-tag>"
  }
]
```

## <a name="use-a-docker-image-from-any-private-registry-optional"></a>Použít bitovou kopii Docker z jakékoli privátní registru (volitelné)

V této části Další informace o použití bitovou kopii Docker z privátní registru ve webové aplikaci pro kontejnery a jako příklad používá registru kontejner Azure. Kroky pro používání jiné privátní registrech jsou podobné. 

Azure registru kontejneru je spravovaná služba Docker z Azure pro hostování privátní bitové kopie. Nasazení může být jakéhokoli typu, včetně [Docker Swarm](https://docs.docker.com/engine/swarm/), [Kubernetes](https://kubernetes.io/)a webové aplikace pro kontejnery. 

### <a name="create-an-azure-container-registry"></a>Vytvoření služby Azure Container Registry

V prostředí cloudu, pomocí [az acr vytvořit](https://docs.microsoft.com/cli/azure/acr#az_acr_create) příkazu vytvořte kontejner registru Azure. Předat název skupiny prostředků a `Basic` pro verze SKU. Jsou dostupné edice `Classic`, `Basic`, `Standard`, a `Premium`.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

Vytvoření kontejneru vytvoří následující výstup:

```bash
 - Finished ..
Create a new service principal and assign access:
  az ad sp create-for-rbac --scopes /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --password <password>

Use an existing service principal and assign access:
  az role assignment create --scope /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --assignee <app-id>
{
  "adminUserEnabled": false,
  "creationDate": "2017-08-09T04:21:09.654153+00:00",
  "id": "/subscriptions/<subscriptionId>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name>",
  "location": "westeurope",
  "loginServer": "<azure-container-registry-name>.azurecr.io",
  "name": "<azure-container-registry-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myazurecontainerre042025"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="log-in-to-azure-container-registry"></a>Přihlaste se k Azure kontejneru registru

Pro vkládání obrázku do registru, budete muset zadat přihlašovací údaje, takže registru přijímá nabízeného oznámení. Tyto přihlašovací údaje můžete načíst pomocí [az acr zobrazit](https://docs.microsoft.com/cli/azure/acr/credential#az_acr_credential_show) příkazu v prostředí cloudu. 

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

Příkaz zobrazí dvě hesla, které lze použít s uživatelským jménem.

```json
<
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<registry-username>"
}
```

Z vaší místní okno terminálu, přihlaste se k Azure kontejneru registru pomocí `docker login` příkaz. K přihlášení je vyžadován název serveru. Použijte formát `{azure-container-registry-name>.azurecr.io`.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username> --password <password> 
```

Potvrďte, že přihlášení bylo úspěšné. 

### <a name="push-an-image-to-azure-container-registry"></a>Push bitovou kopii do registru kontejner Azure

Push bitovou kopii pomocí `docker push` příkaz. Značka obrázek s názvem registru, za nímž následuje název bitové kopie a značky.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Ověřte, že nabízeného oznámení úspěšně přidán do kontejneru registru tak, že uvedete ACR úložiště. 

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Výpis bitové kopie v registru potvrdí, že `mydockerimage` je v registru.

```json
[
  "mydockerimage"
]
```

### <a name="configure-web-app-to-use-the-image-from-azure-container-registry-or-any-private-registry"></a>Konfigurovat webovou aplikaci k používání bitovou kopii z registru kontejner Azure (nebo všechny privátní registru)

Webovou aplikaci pro kontejnery můžete nakonfigurovat tak, aby běžel kontejner uložené v registru kontejner Azure. Pomocí klíče registru kontejner Azure je stejně jako pomocí všechny privátní klíče registru, takže pokud budete muset použít vlastní privátní registru, kroky pro dokončení této úlohy jsou podobné.

V prostředí cloudu, spusťte [az acr pověření zobrazit](/cli/azure/acr/credential#az_acr_credential_show) zobrazíte uživatelské jméno a heslo pro registru kontejner Azure. Zkopírujte uživatelské jméno a jeden z hesla, ve kterém můžete konfigurovat webovou aplikaci v dalším kroku.

```bash
az acr credential show --name <azure-container-registry-name>
```

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "password"
    },
    {
      "name": "password2",
      "value": "password2"
    }
  ],
  "username": "<registry-username>"
}
```

V prostředí cloudu, spusťte [sadu kontejneru konfigurace webapp az](/cli/azure/webapp/config/container#az_webapp_config_container_set) příkazu přiřaďte vlastní image Docker do webové aplikace. Nahraďte  *\<app_name >*,  *\<docker registru server-url >*,  _\<registru username >_a  _\<heslo >_. Pro kontejner registru Azure  *\<docker registru server-url >* je ve formátu `https://<azure-container-registry-name>.azurecr.io`. 

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-custom-image-name mydockerimage --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> `https://`je nutné v  *\<docker registru server-url >*.
>

Příkaz zobrazí se výstup podobný následující řetězce formátu JSON, zobrazující, že změna konfigurace úspěšně:

```bash
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "slotSetting": false,
    "value": "mydockerimage"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_URL",
    "slotSetting": false,
    "value": "<azure-container-registry-name>.azurecr.io"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<registry-username>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  }
]
```

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Vytvoření webové aplikace Docker Python a PostgreSQL v Azure](tutorial-docker-python-postgresql-app.md)
