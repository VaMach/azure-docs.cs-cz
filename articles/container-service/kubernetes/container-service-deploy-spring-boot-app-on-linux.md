---
title: "Nasazení webové aplikace pružiny spouštění v systému Linux v Azure Container Service | Microsoft Docs"
description: "Tento kurz vás provede, když kroky nasazení spouštěcí pružiny aplikace jako webové aplikace v Microsoft Azure Linux."
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
ms.openlocfilehash: 5f0b470bd46cfeaf00b3092dbe9db507ed50f622
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a>Nasazení aplikace pružiny spouštění v systému Linux v Azure Container Service

 **[Pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.

**[Docker]**  je open-source řešení, které pomáhá vývojářům automatizovat nasazení, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.

Tento kurz vás provede pomocí Docker pro vývoj a nasazení aplikace na hostitele platformy Linux v pružiny spouštěcí [Azure Container Service (ACS)].

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

Následující kroky vás provedou kroky, které jsou potřebné k vytvoření jednoduché webové aplikace pružiny spouštěcí a otestovat ji místně.

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

1. Klon [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře, který jste vytvořili; například:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Změňte adresář na dokončené projektu. například:
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Sestavení na soubor JAR pomocí nástroje Maven; například:
   ```
   mvn package
   ```

1. Po vytvoření webové aplikace, změňte adresáře `target` adresáře, kde se nachází na soubor JAR a spuštění webové aplikace, například:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. Test webové aplikace tak, že přejde k němu místně pomocí webového prohlížeče. Například pokud máte curl k dispozici a je nakonfigurován server Tomcat ke spuštění na portu 80:
   ```
   curl http://localhost
   ```

1. Měli byste vidět zobrazenou následující zprávu: **Docker Vítáme vás!**

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Vytvoření registru kontejner Azure používat jako privátní registru Docker

Následující postup vás provede procesem vytvoření registru kontejneru služby Azure pomocí portálu Azure.

> [!NOTE]
>
> Pokud chcete používat rozhraní příkazového řádku Azure místo portálu Azure, postupujte podle kroků v [vytvořit privátní registru kontejner Docker pomocí Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).
>

1. Vyhledejte [portál Azure] a přihlaste se.

   Po přihlášení ke svému účtu na portálu Azure, můžete podle kroků v [privátní registru kontejner Docker pomocí portálu Azure vytvořit] článku, které jsou v následujících krocích pro sake z parafrázována vhodnosti.

1. Klikněte na ikonu nabídky **+ nový**, pak klikněte na tlačítko **kontejnery**a potom klikněte na **registru kontejner Azure**.
   
   ![Vytvořit nový kontejner registru Azure][AR01]

1. Pokud stránka informace o šabloně registru kontejner Azure se zobrazí, klikněte na tlačítko **vytvořit**. 

   ![Vytvořit nový kontejner registru Azure][AR02]

1. Při **vytvořit kontejner registru** zobrazí se stránka, zadejte vaše **název registru** a **skupiny prostředků**, zvolte **povolit** pro **Uživatel s oprávněními správce**a potom klikněte na **vytvořit**.

   ![Konfigurace nastavení registru kontejner Azure][AR03]

1. Po vytvoření kontejneru registr, přejděte do kontejneru registr na portálu Azure a pak klikněte na tlačítko **přístupové klíče**. Poznamenejte si uživatelské jméno a heslo pro další kroky.

   ![Azure přístupové klíče registru kontejneru][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>Konfigurace Maven použití klíče pro přístup k registru kontejner Azure

1. Přejděte do adresáře konfigurace pro instalaci Maven a otevřete *souborech settings.xml* soubor v textovém editoru.

1. Přidejte nastavení registru kontejner Azure přístup z předchozí části tohoto kurzu k `<servers>` kolekce *souborech settings.xml* souboru; například:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Přejděte do adresáře dokončený projekt pro vaši aplikaci pružiny spouštěcí (například: "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* ") a otevřete *pom.xml* soubor v textovém editoru.

1. Aktualizace `<properties>` kolekce *pom.xml* soubor s hodnotou server přihlášení pro vaši Azure kontejneru registru z předchozí části tohoto kurzu; například:

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aktualizace `<plugins>` kolekce *pom.xml* souboru tak, aby `<plugin>` obsahuje přihlašovací adresu a registru název serveru pro váš Azure kontejneru registru z předchozí části tohoto kurzu. Například:

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

1. Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí a spusťte příkaz znovu sestavte aplikaci a nabízená kontejneru registru kontejneru systému Azure:

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> Pokud vaše kontejner Docker nabízíte do Azure, můžete obdržet chybovou zprávu, která je podobná jednu z následujících i když vaše kontejner Docker byla úspěšně vytvořena:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Pokud k tomu dojde, budete se muset přihlásit k účtu Azure z příkazového řádku Dockeru; například:
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Potom můžete posouvat vašeho kontejneru z příkazového řádku; například:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Vytvoření webové aplikace v systému Linux v Azure App Service pomocí bitové kopie kontejneru

1. Vyhledejte [portál Azure] a přihlaste se.

1. Klikněte na ikonu nabídky **+ nový**, pak klikněte na tlačítko **Web + mobilní**a potom klikněte na **webové aplikace v systému Linux**.
   
   ![Vytvořit novou webovou aplikaci na portálu Azure][LX01]

1. Když **webové aplikace v systému Linux** se zobrazí stránka, zadejte následující informace:

   a. Zadejte jedinečný název **název aplikace**; například: "*wingtiptoyslinux*."

   b. Zvolte vaše **předplatné** z rozevíracího seznamu.

   c. Vybrat existující **skupiny prostředků**, nebo zadejte název a vytvořit novou skupinu prostředků.

   d. Klikněte na tlačítko **kontejneru konfigurace** a zadejte následující informace:

      * Zvolte **privátní registru**.

      * **Bitové kopie a volitelné značky**: Zadejte název kontejneru z dříve; například: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"

      * **Adresa URL serveru**: Zadejte svoji adresu URL registru z dříve; například: "*https://wingtiptoysregistry.azurecr.io*"

      * **Uživatelské jméno přihlášení** a **heslo**: Zadejte své přihlašovací údaje z vaší **přístupové klíče** který jste použili v předchozích krocích.
   
   e. Až zadáte všechny výše uvedené informace, klikněte na **OK**.

   ![Konfigurovat nastavení webové aplikace][LX02]

1. Klikněte na možnost **Vytvořit**.

> [!NOTE]
>
> Azure bude automaticky mapovat žádosti internetových embedded server Tomcat, která běží na standardní porty 80 nebo 8080. Ale pokud jste nakonfigurovali embedded server Tomcat, abyste mohli spustit na vlastní port, budete muset přidat proměnnou prostředí do webové aplikace, která definuje port pro embedded serveru Tomcat. Chcete-li tak učinit, proveďte následující kroky:
>
> 1. Vyhledejte [portál Azure] a přihlaste se.
> 
> 2. Klikněte na ikonu **App Services**. (Viz položku #1 na obrázku níže.)
>
> 3. Vyberte ze seznamu vaši webovou aplikaci. (Položka #2 na obrázku níže.)
>
> 4. Klikněte na tlačítko **nastavení aplikace**. (Položka #3 na obrázku níže.)
>
> 5. V **nastavení aplikace** přidejte novou proměnnou prostředí s názvem **PORT** a zadejte vlastní port číslo pro hodnotu. (Položka #4 na obrázku níže.)
>
> 6. Klikněte na **Uložit**. (Položka #5 na obrázku níže.)
>
> ![Ukládání vlastní číslo portu na portálu Azure][LX03]
>

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a>Další kroky

Další informace o používání pružiny spuštění aplikace v Azure najdete v následujících článcích:

* [Nasazení aplikace spouštěcí pružiny do Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Nasazení aplikace spouštěcí pružiny na Cluster Kubernetes v Azure Container Service](container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

Další informace o spouštění pružiny na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].

Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu **pružiny Initializr** v https://start.spring.io/.

Další informace o začátcích se vytvoření jednoduché aplikace pružiny spouštěcí najdete v části Initializr Spring v https://start.spring.io/.

Další příklady použití vlastních imagí Dockeru s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].

<!-- URL List -->

[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[privátní registru kontejner Docker pomocí portálu Azure vytvořit]: /azure/container-registry/container-registry-get-started-portal
[pomocí vlastní image Docker pro webové aplikace Azure v systému Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[Pružiny Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
