---
title: "Nasazení aplikace spouštěcí pružiny na Kubernetes v Azure Container Service | Microsoft Docs"
description: "Tento kurz vás provede když kroky k nasazení aplikace spouštěcí Spring v Kubernetes clusteru v Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 7f726436b2d459b8c16abb02e07de099abfd8974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a>Nasazení aplikace Spring Boot Application v clusteru Kubernetes ve službě Azure Container Service

 **[Pružiny Framework]**  oblíbených rozhraní open source, které pomáhá vytvářet webové, mobilní a aplikacích API vývojáře v jazyce Java. Tento kurz používá ukázkovou aplikaci vytvořený [pružiny spouštěcí], konvence přístupu při použití pružiny rychle začít.

**[Kubernetes]**  a  **[Docker]**  jsou open-source řešení, která pomáhají vývojáři automatizovat nasazení, škálování a správu jejich aplikace běžící v kontejnery.

Tento kurz vás provede, když kombinaci těchto dvou oblíbených, open-source technologií pro vývoj a nasazení spouštěcí pružiny aplikace do služby Microsoft Azure. Přesněji řečeno, použijete  *[pružiny spouštěcí]*  pro vývoj aplikací  *[Kubernetes]*  pro kontejner nasazení a [ Azure Container Service (ACS)] kvůli hostování vaší aplikace.

### <a name="prerequisites"></a>Požadavky

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
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --nebo--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klon [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře.
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Změňte adresář na dokončený projekt.
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Používání Maven k sestavení a spuštění ukázkové aplikace.
   ```
   mvn package spring-boot:run
   ```

1. Test webové aplikace tak, že přejde na adrese http://localhost: 8080 nebo s následující `curl` příkaz:
   ```
   curl http://localhost:8080
   ```

1. Měli byste vidět zobrazenou následující zprávu: **Hello Docker World**

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Vytvoření registru kontejneru služby Azure pomocí rozhraní příkazového řádku Azure

1. Otevřete příkazový řádek.

1. Přihlaste se k účtu Azure:
   ```azurecli
   az login
   ```

1. Vytvořte skupinu prostředků pro použité v tomto kurzu prostředky Azure.
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Vytvořte kontejner privátní Azure registru ve skupině prostředků. Kurz doručí ukázková aplikace jako obrázek na Docker tento registru v dalších krocích. Nahraďte `wingtiptoysregistry` s jedinečným názvem pro vaše registru.
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a>Push vaší aplikace do registru kontejneru

1. Přejděte do adresáře konfigurace pro instalaci Maven (výchozí ~/.m2/ nebo C:\Users\username\.m2) a otevřete *souborech settings.xml* soubor v textovém editoru.

1. Načtení hesla pro vaše kontejneru registru z příkazového řádku Azure.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Přidat kontejner registru Azure id a heslo na nový `<server>` kolekce *souborech settings.xml* souboru.
`id` a `username` jsou název registru. Použití `password` hodnotu z předchozí příkaz (bez uvozovek).

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí (například "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* ") a otevřete *pom.xml* soubor v textovém editoru.

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
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí a spusťte následující příkaz a vytvořit kontejner Docker push bitovou kopii do registru:

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  Zobrazí chybovou zprávu, která je jednu z následujících při Maven sami bitovou kopii do Azure:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Pokud se tato chyba přihlášení k Azure z příkazového řádku Dockeru.
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Potom push vaší kontejneru:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-the-azure-cli"></a>Vytvoření clusteru s podporou Kubernetes na ACS pomocí rozhraní příkazového řádku Azure

1. Vytvoření clusteru Kubernetes v Azure Container Service. Následující příkaz vytvoří *kubernetes* v clusteru *Northwind kubernetes* prostředků skupiny s *Northwind containerservice* jako název clusteru, a *Northwind kubernetes* jako DNS předpony:
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   Tento příkaz může trvat nějakou dobu pro dokončení.

1. Nainstalujte `kubectl` pomocí rozhraní příkazového řádku Azure. Linux uživatelé mohou mít k předpony tento příkaz s `sudo` vzhledem k tomu, že ji nasadí Kubernetes rozhraní příkazového řádku pro `/usr/local/bin`.
   ```azurecli
   az acs kubernetes install-cli
   ```

1. Stáhněte si informace o konfiguraci clusteru, můžete spravovat cluster z webové rozhraní Kubernetes a `kubectl`. 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>Nasazení bitové kopie do clusteru Kubernetes

V tomto kurzu nasadí aplikace pomocí `kubectl`, pak umožňují prozkoumat nasazení pomocí rozhraní web Kubernetes.

### <a name="deploy-with-the-kubernetes-web-interface"></a>Nasazení s Kubernetes webové rozhraní

1. Otevřete příkazový řádek.

1. Otevřete stránku konfigurace pro váš cluster Kubernetes ve výchozím prohlížeči:
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. Když web konfigurace Kubernetes otevře v prohlížeči, klikněte na odkaz **nasadit kontejnerizované aplikaci**:

   ![Kubernetes konfigurace webu][KB01]

1. Když **nasadit kontejnerizované aplikaci** se zobrazí stránka, určete následující možnosti:

   a. Vyberte **zadejte níže uvedené podrobnosti o aplikaci**.

   b. Zadejte název aplikace pružiny spouštěcí pro **název aplikace**; například: "*gs pružiny spouštěcí docker*".

   c. Zadejte přihlašovací serveru a kontejneru bitové kopie z dříve pro **kontejneru image**; například: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".

   d. Zvolte **externí** pro **služby**.

   e. Zadejte vaše externí i interní porty **Port** a **cíle port** textová pole.

   ![Kubernetes konfigurace webu][KB02]


1. Klikněte na tlačítko **nasadit** nasaďte kontejner.

   ![Nasazení kontejneru][KB05]

1. Po nasazení vaší aplikace, zobrazí se aplikace pružiny spouštěcí uvedené v části **služby**.

   ![Kubernetes služby][KB06]

1. Pokud kliknete na odkaz **externí koncové body**, uvidíte pružiny spouštěcí aplikace spuštěné v Azure.

   ![Kubernetes služby][KB07]

   ![Procházet ukázkovou aplikaci v Azure][SB02]


### <a name="deploy-with-kubectl"></a>Nasazení s kubectl

1. Otevřete příkazový řádek.

1. Spuštění vaší kontejneru v clusteru Kubernetes pomocí `kubectl run` příkaz. Zadejte název služby pro aplikaci v Kubernetes a názvu úplnou bitovou kopii. Například:
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   V tomto příkazu:

   * Název kontejneru `gs-spring-boot-docker` je zadán ihned po `run` příkaz

   * `--image` Parametr určuje název serveru a bitové kopie kombinované přihlášení jako`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. Vystavení clusteru Kubernetes externě pomocí `kubectl expose` příkaz. Zadejte název vaší služby, veřejné port TCP používá pro přístup k aplikaci a interní cílový port, který aplikace naslouchá na. Například:
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   V tomto příkazu:

   * Název kontejneru `gs-spring-boot-docker` je zadán ihned po `expose deployment` příkaz

   * `--type` Parametr určuje, zda cluster používá nástroj pro vyrovnávání zatížení

   * `--port` Parametr určuje veřejné port TCP 80. Máte přístup k aplikaci na tomto portu.

   * `--target-port` Parametr určuje vnitřní TCP port 8080. Nástroje pro vyrovnávání zatížení předá požadavky na aplikace na tomto portu.

1. Po nasazení aplikace do clusteru, dotazování na externí IP adresu a otevřete ve webovém prohlížeči:

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Procházet ukázkovou aplikaci v Azure][SB02]


## <a name="next-steps"></a>Další kroky

Další informace o používání spouštěcí Spring v Azure najdete v následujících článcích:

* [Nasazení aplikace spouštěcí pružiny do Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Nasazení aplikace pružiny spouštění v systému Linux v Azure Container Service](container-service-deploy-spring-boot-app-on-linux.md)

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

Další informace o spouštění pružiny na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].

Následující odkazy obsahují další informace o vytváření aplikací pro spouštěcí pružiny:

* Další informace o vytvoření jednoduché aplikace pružiny spouštěcí najdete v části Initializr Spring v https://start.spring.io/.

Následující odkazy obsahují další informace o používání Kubernetes s Azure:

* [Začínáme s Kubernetes cluster Container Service](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [Pomocí Azure Container Service Kubernetes webového uživatelského rozhraní](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

Další informace o používání rozhraní příkazového řádku Kubernetes je k dispozici v **kubectl** v uživatelské příručce <https://kubernetes.io/docs/user-guide/kubectl/>.

Kubernetes web má několik články, které popisují pomocí bitové kopie v privátní registrech:

* [Konfigurace služby pro pracovními stanicemi soustředěnými kolem účtů]
* [Obory názvů]
* [Stahování bitovou kopii z privátní registru]

Další příklady použití vlastních imagí Dockeru s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].

<!-- URL List -->

[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[ Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[pomocí vlastní image Docker pro webové aplikace Azure v systému Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[Pružiny Framework]: https://spring.io/
[Konfigurace služby pro pracovními stanicemi soustředěnými kolem účtů]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Obory názvů]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Stahování bitovou kopii z privátní registru]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
