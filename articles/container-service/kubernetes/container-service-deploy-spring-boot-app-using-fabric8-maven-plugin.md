---
title: "Nasazení aplikace pružiny spouštěcí pomocí modulu plug-in Maven Fabric8"
description: "Tento kurz vás provede, když kroky nasazení spouštěcí pružiny aplikací v Microsoft Azure pomocí modulu plug-in Fabric8 pro Apache Maven."
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/15/2017
ms.author: yuwzho;robmcm
ms.custom: 
ms.openlocfilehash: 6ec48caef0eebeacf260b0dbc91b175368faea69
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a>Nasazení aplikace pružiny spouštěcí pomocí modulu plug-in Maven Fabric8

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

**[Fabric8]**  open-source řešení, které je založená na  **[Kubernetes]**, která pomáhá vývojářům vytvářet aplikace v kontejnerech Linux.

Tento kurz vás provede pomocí modulu plug-in Fabric8 pro Maven k vývoji pro nasazení aplikace na hostitele platformy Linux v [Azure Container Service (ACS)].

## <a name="prerequisites"></a>Požadavky

Aby bylo možné provést kroky v tomto kurzu, musíte mít následující požadavky:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].
* [Rozhraní příkazového řádku Azure (CLI)].
* Aktuální [Java Developer Kit (JDK)].
* Apache na [Maven] sestavení nástroj (verze 3).
* A [Git] klienta.
* A [Docker] klienta.

> [!NOTE]
>
> Z důvodu požadavků na virtualizace tohoto kurzu nelze na virtuálním počítači; podle kroků v tomto článku fyzický počítač musí používat s funkcemi virtualizace.
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Vytvoření spouštěcích pružiny ve webové aplikaci Docker Začínáme

Následující postup vás provede procesem vytváření webové aplikace pružiny spouštěcí a místní testování.

1. Otevřete příkazový řádek a vytvořte místní adresář pro uložení aplikace, změnit do tohoto adresáře; například:
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   --nebo--
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. Klon [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře.
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Změňte adresář na dokončené projektu. například:
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   --nebo--
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. Používání Maven k sestavení a spuštění ukázkové aplikace.
   ```shell
   mvn clean package spring-boot:run
   ```

1. Test webové aplikace tak, že přejde na adrese http://localhost: 8080 nebo s následující `curl` příkaz:
   ```shell
   curl http://localhost:8080
   ```

   Měli byste vidět **Hello Docker World** zpráva.

   ![Procházet ukázkovou aplikaci místně][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a>Nainstalujte rozhraní příkazového řádku Kubernetes a vytvořte skupinu prostředků Azure pomocí rozhraní příkazového řádku Azure

1. Otevřete příkazový řádek.

1. Zadejte následující příkaz k přihlášení k účtu Azure:
   ```azurecli
   az login
   ```
   Postupujte podle pokynů dokončete proces přihlášení
   
   Rozhraní příkazového řádku Azure se zobrazí seznam svých účtů; například:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. Pokud již jste rozhraní příkazového řádku Kubernetes (`kubectl`) nainstalován, můžete nainstalovat pomocí příkazového řádku Azure CLI; například:
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > Linux uživatelé mohou mít k předpony tento příkaz s `sudo` vzhledem k tomu, že ji nasadí Kubernetes rozhraní příkazového řádku pro `/usr/local/bin`.
   >
   > Pokud již máte `kubectl`) nainstalována a vaší verzi `kubectl` je příliš starý, může zobrazí chybová zpráva v následujícím příkladu při pokusu provést kroky uvedené dále v tomto článku:
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > V takovém případě budete muset přeinstalovat `kubectl` aktualizovat vaši verzi.
   >

1. Vytvořte skupinu prostředků pro prostředky Azure, které budete používat v tomto kurzu; například:
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   Kde:  
      * *Northwind kubernetes* je jedinečný název skupiny prostředků  
      * *westeurope* odpovídající geografické umístění pro vaši aplikaci  

   Rozhraní příkazového řádku Azure se zobrazí výsledky vaší vytvoření skupiny prostředků; například:  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a>Vytvoření clusteru s podporou Kubernetes pomocí rozhraní příkazového řádku Azure

1. Vytvoření clusteru s podporou Kubernetes v nové skupiny prostředků; například:  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   Kde:  
      * *Northwind kubernetes* je název vaší skupiny prostředků z dříve v tomto článku  
      * *Northwind clusteru* je jedinečný název pro váš cluster Kubernetes
      * *Northwind* je název jedinečný název DNS pro vaši aplikaci

   Rozhraní příkazového řádku Azure se zobrazí výsledky vytváření vašeho clusteru; například:  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
       "parameters": {
         "clientSecret": {
           "type": "SecureString"
         }
       },
       "parametersLink": null,
       "providers": [
         {
           "id": null,
           "namespace": "Microsoft.ContainerService",
           "registrationState": null,
           "resourceTypes": [
             {
               "aliases": null,
               "apiVersions": null,
               "locations": [
                 "westeurope"
               ],
               "properties": null,
               "resourceType": "containerServices"
             }
           ]
         }
       ],
       "provisioningState": "Succeeded",
       "template": null,
       "templateLink": null,
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. Stáhněte si vaše přihlašovací údaje pro nový cluster Kubernetes; například:  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. Ověření připojení pomocí následujícího příkazu:
   ```shell 
   kubectl get nodes
   ```

   Zobrazí seznam uzlů a stavy, které jsou jako v následujícím příkladu:

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a>Vytvořte soubor registru privátní kontejner Azure pomocí rozhraní příkazového řádku Azure

1. Vytvořit kontejner privátní Azure registru ve vaší skupině prostředků pro hostování vaší image Docker; například:
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   Kde:  
      * *Northwind kubernetes* je název vaší skupiny prostředků z dříve v tomto článku  
      * *wingtiptoysregistry* je jedinečný název pro vaši privátní registru
      * *westeurope* odpovídající geografické umístění pro vaši aplikaci  

   Rozhraní příkazového řádku Azure se zobrazí výsledky vytvoření vaší registru; například:  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

1. Načtení hesla pro vaše kontejneru registru z příkazového řádku Azure.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   Rozhraní příkazového řádku Azure se zobrazí heslo pro vaše registru; například:  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Přejděte do adresáře konfigurace pro instalaci Maven (výchozí ~/.m2/ nebo C:\Users\username\.m2) a otevřete *souborech settings.xml* soubor v textovém editoru.

1. Přidat adresu URL registru kontejner Azure, uživatelské jméno a heslo na nový `<server>` kolekce *souborech settings.xml* souboru.
`id` a `username` jsou název registru. Použití `password` hodnotu z předchozí příkaz (bez uvozovek).

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí (například "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete* ") a otevřete *pom.xml* soubor v textovém editoru.

1. Aktualizace `<properties>` kolekce *pom.xml* soubor s hodnotou server přihlášení pro vaše registru kontejner Azure.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aktualizace `<plugins>` kolekce *pom.xml* souboru tak, aby `<plugin>` obsahuje přihlašovací adresu a registru název serveru pro váš registru kontejner Azure.

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

1. Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí a spusťte následující příkaz Maven k sestavení kontejner Docker a push bitovou kopii do registru:

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   Maven zobrazí výsledky buildu; například:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a>Konfigurace aplikace pružiny spouštěcí používat modul plug-in Fabric8 Maven

1. Přejděte do adresáře dokončený projekt pro vaši aplikaci pružiny spouštěcí (například: "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker / dokončení*") a otevřete *pom.xml* soubor v textovém editoru.

1. Aktualizace `<plugins>` kolekce *pom.xml* soubor pro přidání modulu plug-in Fabric8 Maven:

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. Přejděte do adresáře hlavní zdroje pro vaši aplikaci pružiny spouštěcí (například: "*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*"nebo"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker / dokončení, src/main*") a vytvořte novou složku s názvem"*fabric8*".

1. Vytvořte tři soubory fragment YAML v novém *fabric8* složky:

   a. Vytvořte soubor s názvem **deployment.yml** s tímto obsahem:
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   b. Vytvořte soubor s názvem **secrets.yml** s tímto obsahem:
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   c. Vytvořte soubor s názvem **service.yml** s tímto obsahem:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. Spusťte následující příkaz Maven k sestavení souboru seznamu Kubernetes prostředků:

   ```shell
   mvn fabric8:resource
   ```

   Tento příkaz sloučí všechny soubory yaml pod Kubernetes prostředků *src/main/fabric8* složky YAML soubor, který obsahuje seznam zdrojů Kubernetes, které můžete použít pro Kubernetes clusteru přímo nebo export helm grafu.

   Maven zobrazí výsledky buildu; například:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. Spusťte následující příkaz Maven pro použití se soubor seznamu prostředků do clusteru Kubernetes:

   ```shell
   mvn fabric8:apply
   ```

   Maven zobrazí výsledky buildu; například:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. Po nasazení aplikace do clusteru, zadat dotaz na externí IP adresu pomocí `kubectl` aplikace; například:
   ```shell
   kubectl get svc -w
   ```

   `kubectl`Zobrazí IP adresy interních a externích; například:
   
   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```
   
   Otevřete aplikaci ve webovém prohlížeči, můžete použít na externí IP adresu.

   ![Procházet externě ukázkové aplikace][SB02]

## <a name="delete-your-kubernetes-cluster"></a>Odstranění clusteru Kubernetes

Pokud váš cluster Kubernetes již nepotřebujete, můžete použít `az group delete` příkazu odeberte skupinu zdrojů, které budou odebrány všechny jeho souvisejících prostředků; například:

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a>Další kroky

Další informace o používání pružiny spuštění aplikace v Azure najdete v následujících článcích:

* [Nasazení aplikace spouštěcí pružiny do Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Nasazení aplikace pružiny spouštění v systému Linux v Azure Container Service](container-service-deploy-spring-boot-app-on-linux.md)
* [Nasazení aplikace spouštěcí pružiny na Cluster Kubernetes v Azure Container Service](container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

Další informace o spouštění pružiny na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].

Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu **pružiny Initializr** v <https://start.spring.io/>.

Další informace o začátcích se vytvoření jednoduché aplikace pružiny spouštěcí, najdete v části Initializr Spring v <https://start.spring.io/>.

Další příklady použití vlastních imagí Dockeru s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].

<!-- URL List -->

[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[pomocí vlastní image Docker pro webové aplikace Azure v systému Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-using-fabric8-maven-plugin/SB02.png
