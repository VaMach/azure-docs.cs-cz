---
title: "Jak nasadit kontejnerizované pružiny spouštěcí aplikaci do Azure pomocí modulu plug-in Maven pro webovou aplikaci pro kontejnery"
description: "Další informace o použití modulu plug-in Maven pro webovou aplikaci pro kontejnery nasazení spouštěcí pružiny aplikace do Azure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 0329aa9b88c7542ab3235a104a0652cd217ff872
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-use-the-maven-plugin-for-web-app-for-containers-to-deploy-a-containerized-spring-boot-app-to-azure"></a>Jak nasadit kontejnerizované pružiny spouštěcí aplikaci do Azure pomocí modulu plug-in Maven pro webovou aplikaci pro kontejnery

[Maven modulu plug-in pro webovou aplikaci pro kontejnery](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) pro [Apache Maven](http://maven.apache.org/) zajišťuje bezproblémovou integraci služby Azure App Service do projekty Maven a zjednodušuje proces pro vývojáře, jak nasadit webové aplikace do Aplikační služba Azure.

Tento článek ukazuje, pomocí modulu plug-in Maven pro webovou aplikaci pro kontejnery nasazení ukázkové aplikace spouštěcí Spring v kontejner Docker do webové aplikace pro kontejnery.

> [!NOTE]
>
> Modul plug-in Maven pro webovou aplikaci pro kontejnery je aktuálně k dispozici jako náhled. Teď je podporována pouze publikování FTP, i když další funkce, které jsou naplánované pro budoucnost.
>

## <a name="prerequisites"></a>Požadavky

Aby bylo možné provést kroky v tomto kurzu, musíte mít následující požadavky:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].
* [Rozhraní příkazového řádku Azure (CLI)].
* Aktuální [Java Development Kit (JDK)], verze 1.7 nebo novější.
* Apache na [Maven] sestavení nástroj (verze 3).
* A [Git] klienta.
* A [Docker] klienta.

> [!NOTE]
>
> Z důvodu požadavků na virtualizace tohoto kurzu nelze na virtuálním počítači; podle kroků v tomto článku fyzický počítač musí používat s funkcemi virtualizace.
>

## <a name="clone-the-sample-spring-boot-application"></a>Klonování pružiny spuštění ukázkové aplikace

V této části klonovat kontejnerizované pružiny spouštěcí aplikace a otestovat ji místně.

1. Otevřete příkazový řádek nebo okno terminálu a vytvořte místní adresář pro uložení aplikace pružiny spouštěcí a změňte do tohoto adresáře; například:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --nebo--
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klonování ukázkového projektu [pružiny spouštění s Docker] do adresáře, který jste vytvořili; například:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. Změňte adresář na dokončené projektu. například:
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. Sestavení na soubor JAR pomocí nástroje Maven; například:
   ```shell
   mvn clean package
   ```

1. Po vytvoření webové aplikace, spusťte webovou aplikaci pomocí nástroje Maven; například:
   ```shell
   mvn spring-boot:run
   ```

1. Test webové aplikace tak, že přejde k němu místně pomocí webového prohlížeče. Můžete například použít následující příkaz curl k dispozici máte-li:
   ```shell
   curl http://localhost:8080
   ```

1. Měli byste vidět zobrazenou následující zprávu: **Hello Docker World**

## <a name="create-an-azure-service-principal"></a>Vytvořit objekt služby Azure

V této části vytvoříte Azure instanční objekt, který modul plug-in Maven používá při nasazení vaší kontejneru v Azure.

1. Otevřete příkazový řádek.

1. Přihlaste se ke svému účtu Azure pomocí rozhraní příkazového řádku Azure:
   ```shell
   az login
   ```
   Postupujte podle pokynů dokončete proces přihlášení.

1. Vytvořte objekt služby Azure:
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   Kde `uuuuuuuu` je uživatelské jméno a `pppppppp` je heslo pro objekt služby.

1. Azure odpoví JSON, která se podobá následujícímu příkladu:
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > Pokud nakonfigurujete modul plug-in Maven k nasazení vašeho kontejneru na Azure budete používat hodnoty z této odpovědi JSON. `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, A `tttttttt` jsou zástupné hodnoty, které se používají v tomto příkladu, aby bylo snazší mapovat tyto hodnoty na jejich odpovídající prvky při konfiguraci vašeho Maven `settings.xml` souboru v další části.
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a>Konfigurace Maven používat Azure instančního objektu

V této části je použít hodnoty ze služby Azure hlavní postup konfigurace ověřování, který Maven bude používat při nasazování vašeho kontejneru do Azure.

1. Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě, jako jsou následující příklady:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Přidat nastavení hlavní služby Azure z předchozí části tohoto kurzu k `<servers>` kolekce v *souborech settings.xml* souboru; například:

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   Kde:
   Element | Popis
   ---|---|---
   `<id>` | Určuje jedinečný název, který používá Maven k vyhledání nastavení zabezpečení při nasazení webové aplikace do Azure.
   `<client>` | Obsahuje `appId` hodnotu z instanční objekt.
   `<tenant>` | Obsahuje `tenant` hodnotu z instanční objekt.
   `<key>` | Obsahuje `password` hodnotu z instanční objekt.
   `<environment>` | Definuje cílovém prostředí cloudu Azure, což je `AZURE` v tomto příkladu. (Je k dispozici v úplný seznam prostředí [Maven modulu plug-in pro webovou aplikaci pro kontejnery] dokumentace)

1. Uložte a zavřete *souborech settings.xml* souboru.

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a>Volitelné: Nasazení vaší místní soubor Docker do úložiště Docker Hub

Pokud máte účet Docker, můžete sestavit vaše Docker kontejneru image místně a poslat ho do úložiště Docker Hub. Uděláte to tak, použijte následující postup.

1. Otevřete `pom.xml` souboru pružiny spuštění aplikace v textovém editoru.

1. Vyhledejte `<imageName>` podřízený element `<containerSettings>` elementu.

1. Aktualizace `${docker.image.prefix}` hodnotu s názvem svého účtu Docker:
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. Vyberte jednu z následujících metod nasazení:

   * Vytvoření bitové kopie kontejneru místně s Maven a pak pomocí Docker push vaší kontejner Docker hub:
      ```shell
      mvn clean package docker:build
      docker push
      ```

   * Pokud máte [Docker modul plug-in pro Maven] nainstalován, můžete automaticky vytvořit a vaše kontejneru bitové kopie do úložiště Docker Hub pomocí `-DpushImage` parametr:
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a>Volitelné: Přizpůsobení vaší pom.xml před nasazením vaší kontejneru do Azure

Otevřete `pom.xml` souboru pro vaši aplikaci spouštěcí Spring v textovém editoru a najděte `<plugin>` element pro `azure-webapp-maven-plugin`. Tento element by měl podobat následujícímu příkladu:

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

Existuje několik hodnot, které lze upravit pro modul plug-in Maven a podrobný popis pro každou z těchto elementů je k dispozici v [Maven modulu plug-in pro webovou aplikaci pro kontejnery] dokumentaci. Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:

Element | Popis
---|---|---
`<version>` | Určuje verzi [Maven modulu plug-in pro webovou aplikaci pro kontejnery]. Verze uvedené v byste měli zkontrolovat [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) zajistit, že používáte nejnovější verzi.
`<authentication>` | Určuje informace o ověřování pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu k vyhledání objektu služby Azure hodnoty v vaše Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.
`<resourceGroup>` | Určuje, cílová skupina prostředků, který je `maven-plugin` v tomto příkladu. Skupina prostředků se vytvoří během nasazení, pokud ještě neexistuje.
`<appName>` | Určuje název cílového pro webové aplikace. V tomto příkladu je název cílové `maven-linux-app-${maven.build.timestamp}`, kde `${maven.build.timestamp}` v tomto příkladu, aby se zabránilo konfliktu se připojí přípona. (Časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace.)
`<region>` | Určuje cílová oblast, která v tomto příkladu je `westus`. (Úplný seznam je v [Maven modulu plug-in pro webovou aplikaci pro kontejnery] dokumentaci.)
`<appSettings>` | Určuje nastavení jedinečné pro Maven pro použití při nasazení webové aplikace do Azure. V tomto příkladu `<property>` element obsahuje dvojici název – hodnota podřízených elementů, které zadejte port pro vaši aplikaci.

> [!NOTE]
>
> Nastavení můžete změnit číslo portu v tomto příkladu jsou nezbytné, pouze pokud měníte z výchozího portu.
>

## <a name="build-and-deploy-your-container-to-azure"></a>Sestavení a nasazení vaší kontejneru do Azure

Jakmile nakonfigurujete všechna nastavení v předchozí části tohoto článku, jste připraveni k nasazení vašeho kontejneru na Azure. Chcete-li tak učinit, proveďte následující kroky:

1. Z příkazového řádku nebo okno terminálu, který jste dříve používali, znovu vytvořit na soubor JAR pomocí nástroje Maven Pokud jste provedli všechny změny *pom.xml* souboru; například:
   ```shell
   mvn clean package
   ```

1. Nasazení webové aplikace do Azure pomocí Maven; například:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven bude nasazení webové aplikace do Azure; Pokud webová aplikace už neexistuje, bude vytvořen.

> [!NOTE]
>
> Pokud oblast, kterou zadáte v `<region>` element vaše *pom.xml* soubor nemá dostatek serverů, které jsou k dispozici po spuštění nasazení, může dojít k chybě, podobně jako v následujícím příkladu:
>
> ```bash
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> V takovém případě můžete zadat jiné oblasti a znovu spusťte příkaz Maven pro nasazení aplikace.
>
>

Pokud váš web nasazen, bude možné spravovat pomocí [portál Azure].

* Webové aplikace se objeví v **App Services**:

   ![Webové aplikace, které jsou uvedené na portálu Azure App Services][AP01]

* A zobrazí se adresa URL pro webovou aplikaci v **přehled** pro webové aplikace:

   ![Určení adresy URL pro webovou aplikaci][AP02]

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

Další informace o různých technologií, které jsou popsané v tomto článku najdete v následujících článcích:

* [Maven modulu plug-in pro webovou aplikaci pro kontejnery]

* [Přihlaste se k Azure z rozhraní příkazového řádku Azure](/azure/xplat-cli-connect)

* [Jak používat modul plug-in Maven pro webovou aplikaci pro kontejnery k nasazení aplikace pružiny spouštěcí do služby Azure App Service v systému Linux](../app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [Vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referenční příručka k nastavení maven](https://maven.apache.org/settings.html)

* [Docker modul plug-in pro Maven]

<!-- URL List -->

[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Docker modul plug-in pro Maven]: https://github.com/spotify/docker-maven-plugin
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven modulu plug-in pro webovou aplikaci pro kontejnery]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
