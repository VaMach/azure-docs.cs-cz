---
title: "Jak používat modul plug-in Maven pro webové aplikace Azure k nasazení spouštěcí pružiny aplikace do Azure"
description: "Další informace o použití modulu plug-in Maven pro webové aplikace Azure k nasazení pružiny spouštění aplikace na Azure."
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
ms.openlocfilehash: f22089349e149d8e71df88d5c173869d2a5abad7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-to-azure"></a>Jak používat modul plug-in Maven pro webové aplikace Azure k nasazení spouštěcí pružiny aplikace do Azure

[Maven modul plug-in pro Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) pro [Apache Maven](http://maven.apache.org/) zajišťuje bezproblémovou integraci služby Azure App Service do projekty Maven a zjednodušuje proces pro vývojáře, jak nasadit webové aplikace do služby Azure App Service.

Tento článek ukazuje, pomocí modulu plug-in Maven pro webové aplikace Azure k nasazení ukázkové aplikace pružiny spouštěcí do Azure App Services.

> [!NOTE]
>
> Modul plug-in Maven pro Azure Web Apps je aktuálně k dispozici jako náhled. Teď je podporována pouze publikování FTP, i když další funkce, které jsou naplánované pro budoucnost.
>

## <a name="prerequisites"></a>Požadavky

Aby bylo možné provést kroky v tomto kurzu, musíte mít následující požadavky:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].
* [Rozhraní příkazového řádku Azure (CLI)].
* Aktuální [Java Development Kit (JDK)], verze 1.7 nebo novější.
* Apache na [Maven] sestavení nástroj (verze 3).
* A [Git] klienta.

## <a name="clone-the-sample-spring-boot-web-app"></a>Naklonujte ukázkovou webovou aplikaci pružiny spouštěcí

V této části klonovat hotová aplikace pružiny spouštěcí a otestovat ji místně.

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

1. Klon [pružiny spouštěcí Začínáme] ukázkový projekt do adresáře, který jste vytvořili; například:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. Změňte adresář na dokončené projektu. například:
   ```shell
   cd gs-spring-boot/complete
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

1. Měli byste vidět zobrazenou následující zprávu: **pozdrav z jara spouštěcí!**

## <a name="create-an-azure-service-principal"></a>Vytvořit objekt služby Azure

V této části vytvoříte Azure instanční objekt, který modul plug-in Maven používá při nasazení webové aplikace v Azure.

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
   > Pokud nakonfigurujete modul plug-in Maven k nasazení vaší webové aplikace do Azure budete používat hodnoty z této odpovědi JSON. `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, A `tttttttt` jsou zástupné hodnoty, které se používají v tomto příkladu, aby bylo snazší mapovat tyto hodnoty na jejich odpovídající prvky při konfiguraci vašeho Maven `settings.xml` souboru v další části.
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a>Konfigurace Maven používat Azure instančního objektu

V této části je použít hodnoty ze služby Azure hlavní postup konfigurace ověřování, který Maven používá při nasazení webové aplikace v Azure.

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
   `<environment>` | Definuje cílovém prostředí cloudu Azure, což je `AZURE` v tomto příkladu. (Úplný seznam prostředí je k dispozici v [Maven modul plug-in pro Azure Web Apps] dokumentace)

1. Uložte a zavřete *souborech settings.xml* souboru.

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a>Volitelné: Přizpůsobení vaší pom.xml před nasazením webové aplikace do Azure

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

Existuje několik hodnot, které lze upravit pro modul plug-in Maven a podrobný popis pro každou z těchto elementů je k dispozici v [Maven modul plug-in pro Azure Web Apps] dokumentaci. Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:

Element | Popis
---|---|---
`<version>` | Určuje verzi [Maven modul plug-in pro Azure Web Apps]. Verze uvedené v byste měli zkontrolovat [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) zajistit, že používáte nejnovější verzi.
`<authentication>` | Určuje informace o ověřování pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu k vyhledání objektu služby Azure hodnoty v vaše Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.
`<resourceGroup>` | Určuje, cílová skupina prostředků, který je `maven-plugin` v tomto příkladu. Skupina prostředků se vytvoří během nasazení, pokud ještě neexistuje.
`<appName>` | Určuje název cílového pro webové aplikace. V tomto příkladu je název cílové `maven-web-app-${maven.build.timestamp}`, kde `${maven.build.timestamp}` v tomto příkladu, aby se zabránilo konfliktu se připojí přípona. (Časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace.)
`<region>` | Určuje cílová oblast, která v tomto příkladu je `westus`. (Úplný seznam je v [Maven modul plug-in pro Azure Web Apps] dokumentaci.)
`<javaVersion>` | Určuje verzi jazyka Java runtime pro vaši webovou aplikaci. (Úplný seznam je v [Maven modul plug-in pro Azure Web Apps] dokumentaci.)
`<deploymentType>` | Určuje typ nasazení pro vaši webovou aplikaci. Prozatím se pouze `ftp` je podporováno, i když podporu pro jiné typy nasazení je ve vývoji.
`<resources>` | Určuje prostředky a cílové umístění, které Maven používá při nasazení webové aplikace do Azure. V tomto příkladu dvě `<resource>` elementy určit, že budou Maven nasazovat na soubor JAR pro vaši webovou aplikaci a *web.config* souboru z jara spouštěcí projektu.

## <a name="build-and-deploy-your-web-app-to-azure"></a>Sestavení a nasazení webové aplikace do Azure

Jakmile nakonfigurujete všechna nastavení v předchozí části tohoto článku, budete chtít nasadit webovou aplikaci do Azure. Chcete-li tak učinit, proveďte následující kroky:

1. Z příkazového řádku nebo okno terminálu, který jste dříve používali, znovu vytvořit na soubor JAR pomocí nástroje Maven Pokud jste provedli všechny změny *pom.xml* souboru; například:
   ```shell
   mvn clean package
   ```

1. Nasazení webové aplikace do Azure pomocí Maven; například:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven bude nasazení webové aplikace do Azure; Pokud webová aplikace už neexistuje, bude vytvořen.

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

* [Maven modul plug-in pro Azure Web Apps]

* [Přihlaste se k Azure z rozhraní příkazového řádku Azure](/azure/xplat-cli-connect)

* [Jak nasadit kontejnerizované pružiny spouštěcí aplikaci do Azure pomocí modulu plug-in Maven pro webové aplikace Azure](containers/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [Vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referenční příručka k nastavení maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí Začínáme]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven modul plug-in pro Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
