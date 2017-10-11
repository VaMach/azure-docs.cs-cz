---
title: "Jak používat Starter spouštěcí pružiny rozhraním Azure Cosmos DB DocumentDB API"
description: "Informace o konfiguraci aplikace vytvořené pomocí inicializátoru spouštěcí pružiny s rozhraním API pro Azure Cosmos databáze DocumentDB."
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Spring pružiny spouštěcí Starter, Cosmos DB"
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/08/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: 273cc750857c5e466882060a38ac0f3475811e98
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a>Jak používat Starter spouštěcí pružiny s rozhraním API pro Azure Cosmos databáze DocumentDB

## <a name="overview"></a>Přehled

 **[Pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java. Chcete-li začít pracovat s pružiny spouštěcí vývojáři, jsou k dispozici na několik balíčků spouštěcí pružiny ukázkové <https://github.com/spring-guides/>. Kromě výběr ze seznamu základní pružiny spouštěcí projekty,  **[pružiny Initializr]**  pomáhá vývojářům, jak začít s vytvářením vlastních pružiny spouštění aplikací.

Azure Cosmos DB je globálně distribuované databáze služba, která umožňuje vývojářům pracovat s daty pomocí různých standardní rozhraní API, například DocumentDB, MongoDB, graf a tabulka rozhraní API. Společnosti Microsoft pružiny spouštěcí Starter umožňuje vývojářům používat spouštěcí pružiny aplikace, které se snadnou integraci s Azure Cosmos DB pomocí rozhraní API DocumentDB.

Tento článek ukazuje, vytváření Azure DB Cosmos pomocí portálu Azure a pak pomocí **Initializr pružiny** vytvořit aplikaci java vlastní, a pak přidat funkci pružiny spouštěcí Starter do vlastní aplikace ukládání dat v a načtení dat z vaší databáze Cosmos Azure pomocí rozhraní API DocumentDB.

## <a name="prerequisites"></a>Požadavky

Chcete-li postupujte podle kroků v tomto článku jsou požadovány následující součásti:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].

* A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), verze 1.7 nebo novější.

* [Apache Maven](http://maven.apache.org/), verze 3.0 nebo novější.

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Vytvořit databázi Cosmos Azure pomocí portálu Azure

1. Přejděte na portál Azure na adrese <https://portal.azure.com/> a klikněte na tlačítko **+ nový**.

   ![portál Azure][AZ01]

1. Klikněte na tlačítko **databáze**a potom klikněte na **Azure Cosmos DB**.

   ![portál Azure][AZ02]

1. Na **Azure Cosmos DB** stránky, zadejte následující informace:

   * Zadejte jedinečný **ID**, které budete používat jako identifikátor URI pro vaši databázi. Příklad: *wingtiptoysdata.documents.azure.com*.
   * Zvolte **SQL (dokument databáze)** pro rozhraní API.
   * Vyberte **předplatné** chcete použít pro vaši databázi.
   * Určete, zda chcete vytvořit nový **skupiny prostředků** pro vaši databázi nebo vyberte existující skupinu prostředků.
   * Zadejte **umístění** pro vaši databázi.
   
   Pokud jste zadali tyto možnosti, klikněte na tlačítko **vytvořit** k vytvoření databáze.

   ![portál Azure][AZ03]

1. Po vytvoření databáze, je uvedena ve vaší službě Azure **řídicí panel**, stejně jako v části **všechny prostředky** a **Azure Cosmos DB** stránky. Kliknutím na vaši databázi na žádném z těchto umístění, které chcete otevřít stránku vlastnosti ke svojí mezipaměti.

   ![portál Azure][AZ04]

1. Když stránku vlastností pro vaši databázi se zobrazí, klikněte na tlačítko **přístupové klíče** a kopírování klíče URI a přístup pro vaši databázi, budete používat tyto hodnoty v aplikaci pružiny spouštěcí.

   ![portál Azure][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Vytvořit jednoduchou aplikaci pružiny spouštěcí s Initializr pružiny

1. Přejděte do <https://start.spring.io/>.

1. Zadejte, že chcete vygenerovat **Maven** projektu s **Java**, zadejte **skupiny** a **artefaktů** názvy pro vaši aplikaci a Klikněte na tlačítko **generovat projektu**.

   ![Možnosti Initializr pružiny Basic][SI01]

   > [!NOTE]
   >
   > Používá Initializr pružiny **skupiny** a **artefaktů** názvy vytvořit název balíčku; například: *com.example.wintiptoys*.
   >

1. Po zobrazení výzvy, stáhněte si projekt na cestu v místním počítači.

   ![Stáhněte si vlastní spouštěcí pružiny projekt][SI02]

1. Po extrahování souborů v místním systému bude jednoduché pružiny spouštěcí aplikace připravené pro úpravy.

   ![Soubory projektu pružiny vlastní spouštěcí][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a>Konfigurace aplikace pružiny spouštěcí používat Starter spouštěcí pružiny Azure

1. Vyhledejte *pom.xml* soubor v adresáři vaší aplikace; například:

   `C:\SpringBoot\wingtiptoys\pom.xml`

   -nebo-

   `/users/example/home/wingtiptoys/pom.xml`

   ![Vyhledejte soubor pom.xml][PM01]

1. Otevřete *pom.xml* soubor v textovém editoru a přidejte následující řádky do seznamu `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Úpravy soubor pom.xml][PM02]

1. Uložte a zavřete *pom.xml* souboru.

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>Konfigurace aplikace pružiny spouštěcí používat vaše Azure DB Cosmos

1. Vyhledejte *application.properties* v soubor *prostředky* adresář vaší aplikace; například:

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   -nebo-

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Vyhledejte soubor application.properties][RE01]

1. Otevřete *application.properties* soubor v textovém editoru a přidejte následující řádky do souboru a nahraďte příslušné vlastnosti pro vaši databázi ukázkové hodnoty:

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Úpravy souboru application.properties][RE02]

1. Uložte a zavřete *application.properties* souboru.

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>Přidejte ukázkový kód pro implementaci funkce základní databáze

V této části vytvoříte dvě třídy Java pro ukládání uživatelských dat a potom upravte třídě hlavní aplikace k vytvoření instance třídy uživatelů a uložte je do vaší databáze.

### <a name="define-a-basic-class-for-storing-user-data"></a>Zadejte základní třídu pro uložení uživatelských dat.

1. Vytvořte nový soubor s názvem *User.java* ve stejném adresáři jako souboru hlavní aplikace Java.

1. Otevřete *User.java* soubor v textovém editoru a přidejte následující řádky do souboru definujte obecné uživatelské třídu, která ukládá a k získávání hodnot v databázi:

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. Uložte a zavřete *User.java* souboru.

### <a name="define-a-data-repository-interface"></a>Definujte rozhraní úložiště dat

1. Vytvořte nový soubor s názvem *UserRepository.java* ve stejném adresáři jako souboru hlavní aplikace Java.

1. Otevřete *UserRepository.java* soubor v textovém editoru a přidejte následující řádky do souboru Definujte uživatelské rozhraní úložiště, který rozšiřuje výchozí rozhraní úložiště DocumentDB:

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. Uložte a zavřete *UserRepository.java* souboru.

### <a name="modify-the-main-application-class"></a>Upravit třída hlavní aplikace

1. Vyhledejte soubor Java hlavní aplikace v adresáři balíčku aplikace; například:

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   -nebo-

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Vyhledejte soubor aplikace Java][JV01]

1. V textovém editoru otevřete soubor hlavní aplikace Java a přidejte následující řádky do souboru:

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. Uložte a zavřete soubor hlavní aplikace Java.

## <a name="build-and-test-your-app"></a>Vytvoření a testování vaší aplikace

1. Otevřete příkazový řádek a změnit adresář, do složky, kde vaše *pom.xml* se nachází soubor; například:

   `cd C:\SpringBoot\wingtiptoys`

   -nebo-

   `cd /users/example/home/wingtiptoys`

1. Spouštěcí pružiny aplikace s Maven sestavíte a spustíte. například:

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. Aplikace se zobrazí několik zpráv runtime a měli byste vidět zprávu `User: testFirstName testLastName` které oznamuje, že hodnoty byly úspěšně uložené a načíst z databáze.

   ![Úspěšný výstup z aplikace][JV02]

1. Volitelné: Můžete portál Azure pro vaši databázi kliknutím zobrazit obsah vaší Azure DB Cosmos na stránce vlastnosti **Průzkumníka dokumentů**a potom vyberete a položku ze seznamu zobrazených zobrazit obsah.

   ![Chcete-li zobrazit data pomocí Průzkumníka dokumentů][JV03]

## <a name="next-steps"></a>Další kroky

Další informace o používání Azure Cosmos DB a Java najdete v následujících článcích:

* [Dokumentace Azure Cosmos DB].

* [Azure Cosmos DB: Vytvoření aplikace DocumentDB API Java a portálu Azure][Build a DocumentDB API app with Java]

Další informace o používání pružiny spuštění aplikace v Azure najdete v následujících článcích:

* [Spring spouštěcí DocumenDB Starter pro Azure.](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [Nasazení aplikace spouštěcí pružiny do Azure App Service](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Spuštění aplikace spouštěcí Spring v clusteru s podporou Kubernetes v Azure Container Service](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

<!-- URL List -->

[Dokumentace Azure Cosmos DB]: /azure/cosmos-db/
[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny Initializr]: https://start.spring.io/
[Pružiny Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ01.png
[AZ02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ02.png
[AZ03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ03.png
[AZ04]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ04.png
[AZ05]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ05.png

[SI01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI01.png
[SI02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI02.png
[SI03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI03.png

[RE01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE01.png
[RE02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE02.png

[PM01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM01.png
[PM02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM02.png

[JV01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV01.png
[JV02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV02.png
[JV03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV03.png
