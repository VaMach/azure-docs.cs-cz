---
title: "Jak používat modul plug-in Maven pro webové aplikace Azure k nasazení aplikace spouštěcí Spring v registru kontejner Azure do Azure App Service"
description: "Tento kurz vás provede, když postup nasazení aplikace spouštěcí Spring v registru kontejner Azure do Azure do Azure App Service pomocí modulu plug-in Maven."
services: 
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: b087003b3a1e236e4a306678904107b8bf99395e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a>Jak používat modul plug-in Maven pro webové aplikace Azure k nasazení aplikace spouštěcí Spring v registru kontejner Azure do Azure App Service

**[Pružiny Framework]**  oblíbených rozhraní open source, které pomáhá vytvářet webové, mobilní a aplikacích API vývojáře v jazyce Java. Tento kurz používá ukázkovou aplikaci vytvořený [pružiny spouštěcí], konvence přístupu při použití pružiny rychle začít.

Tento článek ukazuje, jak nasadit ukázkovou aplikaci pružiny spouštěcí do registru kontejner Azure a pak použijte modul plug-in Maven pro Azure Web Apps pro nasazení aplikace do služby Azure App Service.

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
* A [Docker] klienta.

> [!NOTE]
>
> Z důvodu požadavků na virtualizace tohoto kurzu nelze na virtuálním počítači; podle kroků v tomto článku fyzický počítač musí používat s funkcemi virtualizace.
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a>Clone – ukázka pružiny spouštěcí ve webové aplikaci Docker

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

1. Klon [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře, který jste vytvořili; například:
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
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

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-service-principal"></a>Vytvořit objekt služby Azure

V této části vytvoříte Azure instanční objekt, který modul plug-in Maven používá při nasazení vaší kontejneru v Azure.

1. Otevřete příkazový řádek.

1. Přihlaste se ke svému účtu Azure pomocí rozhraní příkazového řádku Azure:
   ```azurecli
   az login
   ```
   Postupujte podle pokynů dokončete proces přihlášení.

1. Vytvořte objekt služby Azure:
   ```azurecli
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

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Vytvoření registru kontejneru služby Azure pomocí rozhraní příkazového řádku Azure

1. Otevřete příkazový řádek.

1. Přihlaste se k účtu Azure:
   ```azurecli
   az login
   ```

1. Vytvořte skupinu prostředků pro prostředky Azure, které budete používat v tomto článku:
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   Nahraďte `wingtiptoysresources` v tomto příkladu s jedinečným názvem skupiny prostředků.

1. Vytvořte kontejner privátní Azure registru ve skupině prostředků pro vaši aplikaci pružiny spouštěcí: 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   Nahraďte `wingtiptoysregistry` v tomto příkladu se jedinečný název vašeho kontejneru registru.

1. Načtení hesla pro vaše registru kontejneru:
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   Azure bude odpovídat pomocí hesla; například:
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a>Přidat do Maven nastavení registru kontejner Azure a Azure instančního objektu

1. Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě, jako jsou následující příklady:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Přidejte nastavení registru kontejner Azure přístup z předchozí části tohoto článku a `<servers>` kolekce v *souborech settings.xml* souboru; například:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   Kde:
   Element | Popis
   ---|---|---
   `<id>` | Obsahuje název vaší privátní kontejner Azure registru.
   `<username>` | Obsahuje název vaší privátní kontejner Azure registru.
   `<password>` | Obsahuje heslo, které jste získali v předchozí části tohoto článku.

1. Přidat nastavení hlavní služby Azure z předcházející části tohoto článku `<servers>` kolekce v *souborech settings.xml* souboru; například:

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

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a>Sestavení vaší Docker kontejneru bitové kopie a poslat ho přímo v registru kontejner Azure

1. Přejděte do adresáře dokončený projekt pro vaši aplikaci pružiny spouštěcí (např.) "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") a otevřete *pom.xml* soubor v textovém editoru.

1. Aktualizace `<properties>` kolekce *pom.xml* soubor s hodnotou server přihlášení pro vaši Azure kontejneru registru z předchozí části tohoto kurzu; například:

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   Kde:
   Element | Popis
   ---|---|---
   `<azure.containerRegistry>` | Určuje název vaší privátní kontejner Azure registru.
   `<docker.image.prefix>` | Určuje adresu URL registru privátní kontejner Azure, které je odvozeno připojením ". azurecr.io" na název vaší privátní kontejneru registru.

1. Ověřte, že `<plugin>` pro modul plug-in Docker ve vaší *pom.xml* soubor obsahuje správné vlastnosti pro přihlášení adresu a registru název serveru z předchozího kroku v tomto kurzu. Například:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   Kde:
   Element | Popis
   ---|---|---
   `<serverId>` | Určuje vlastnost, která obsahuje název vaší privátní kontejner Azure registru.
   `<registryUrl>` | Určuje vlastnost, která obsahuje adresu URL vaší privátní kontejner Azure registru.

1. Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí a spusťte příkaz znovu sestavte aplikaci a nabízená kontejneru registr kontejner Azure:

   ```
   mvn package docker:build -DpushImage 
   ```

1. Volitelné: Přejděte do [portál Azure] a ověřte, zda je image kontejner Docker s názvem **gs pružiny spouštěcí docker** v registru kontejneru.

   ![Ověřte kontejneru na portálu Azure][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a>Přizpůsobení pom.xml, pak sestavení a nasazení vaší kontejneru do Azure

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
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
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

Existuje několik hodnot, které lze upravit pro modul plug-in Maven a podrobný popis pro každou z těchto elementů je k dispozici v [Maven modul plug-in pro Azure Web Apps] dokumentaci. Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:

Element | Popis
---|---|---
`<version>` | Určuje verzi [Maven modul plug-in pro Azure Web Apps]. Verze uvedené v byste měli zkontrolovat [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) zajistit, že používáte nejnovější verzi.
`<authentication>` | Určuje informace o ověřování pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu k vyhledání objektu služby Azure hodnoty v vaše Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.
`<resourceGroup>` | Určuje, cílová skupina prostředků, který je `wingtiptoysresources` v tomto příkladu. Skupina prostředků se vytvoří během nasazení, pokud ještě neexistuje.
`<appName>` | Určuje název cílového pro webové aplikace. V tomto příkladu je název cílové `maven-linux-app-${maven.build.timestamp}`, kde `${maven.build.timestamp}` v tomto příkladu, aby se zabránilo konfliktu se připojí přípona. (Časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace.)
`<region>` | Určuje cílová oblast, která v tomto příkladu je `westus`. (Úplný seznam je v [Maven modul plug-in pro Azure Web Apps] dokumentaci.)
`<containerSettings>` | Určuje vlastnosti, které obsahují název a adresu URL vašeho kontejneru.
`<appSettings>` | Určuje nastavení jedinečné pro Maven pro použití při nasazení webové aplikace do Azure. V tomto příkladu `<property>` element obsahuje dvojici název – hodnota podřízených elementů, které zadejte port pro vaši aplikaci.

> [!NOTE]
>
> Nastavení můžete změnit číslo portu v tomto příkladu jsou nezbytné, pouze pokud měníte z výchozího portu.
>

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
> ```
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

## <a name="next-steps"></a>Další kroky

Další informace o různých technologií, které jsou popsané v tomto článku najdete v následujících článcích:

* [Maven modul plug-in pro Azure Web Apps]

* [Přihlaste se k Azure z rozhraní příkazového řádku Azure](/azure/xplat-cli-connect)

* [Vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referenční příručka k nastavení maven](https://maven.apache.org/settings.html)

* [Modul plug-in docker pro Maven]

<!-- URL List -->

[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[Maven modul plug-in pro Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: tutorial-custom-docker-image.md
[Docker]: https://www.docker.com/
[Modul plug-in docker pro Maven]: https://github.com/spotify/docker-maven-plugin
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

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
