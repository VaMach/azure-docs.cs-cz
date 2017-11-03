---
title: "Postup konfigurace aplikace pružiny spouštěcí inicializátoru použití Redis Cache"
description: "Naučte se konfigurovat pružiny spuštění aplikace vytvořené pomocí Initializr pružiny použití Azure Redis Cache."
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Mezipaměť Redis pružiny pružiny spouštěcí Starter,"
ms.assetid: 
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 08/31/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: 7a6ec549654d00975494bac8594a6777af5ec415
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-a-spring-boot-initializer-app-to-use-redis-cache"></a>Postup konfigurace aplikace pružiny spouštěcí inicializátoru použití Redis Cache

## <a name="overview"></a>Přehled

**[Pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java. Chcete-li začít pracovat s pružiny spouštěcí vývojáři, jsou k dispozici na několik balíčků spouštěcí pružiny ukázkové <https://github.com/spring-guides/>. Kromě výběr ze seznamu základní pružiny spouštěcí projekty,  **[pružiny Initializr]**  pomáhá vývojářům, jak začít s vytvářením vlastních pružiny spouštění aplikací.

Tento článek vás provede procesem vytvoření mezipaměti Redis pomocí portálu Azure, pak pomocí **Initializr pružiny** vytvořit vlastní aplikaci a pak vytvořit webovou aplikaci Java, která ukládá a načítat data pomocí vaší Redis mezipaměti.

## <a name="prerequisites"></a>Požadavky

Chcete-li postupujte podle kroků v tomto článku jsou požadovány následující součásti:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].

* A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), verze 1.7 nebo novější.

* [Apache Maven](http://maven.apache.org/), verze 3.0 nebo novější.

## <a name="create-a-redis-cache-on-azure"></a>Vytvoření mezipaměti Redis v Azure

1. Přejděte na portál Azure na adrese <https://portal.azure.com/> a klikněte na položku pro **+ nový**.

   ![portál Azure][AZ01]

1. Klikněte na tlačítko **databáze**a potom klikněte na **Redis Cache**.

   ![portál Azure][AZ02]

1. Na **nová mezipaměť Redis** stránky, zadejte následující informace:

   * Zadejte **název DNS** ke svojí mezipaměti.
   * Zadejte vaše **předplatné**, **skupiny prostředků**, **umístění**, a **cenová úroveň**.
   * V tomto kurzu zvolte **odblokovat port 6379**.

   > [!NOTE]
   >
   > Můžete používat protokol SSL s mezipaměti Redis, ale budete muset použít jiné klienta Redis jako Jedis. Další informace najdete v tématu [použití Azure Redis Cache s Javou][Redis Cache with Java].
   >

   Pokud jste zadali tyto možnosti, klikněte na tlačítko **vytvořit** k vytvoření mezipaměti.

   ![portál Azure][AZ03]

1. Po dokončení mezipaměti uvidíte tu najdete na vaše Azure **řídicí panel**, stejně jako v části **všechny prostředky**, a **mezipaměti Redis cache** stránky. Kliknutím na vaší mezipaměti na žádném z těchto umístění, které chcete otevřít stránku vlastnosti ke svojí mezipaměti.

   ![portál Azure][AZ04]

1. Pokud stránky, která obsahuje seznam vlastností, mezipaměť se zobrazí, klikněte na tlačítko **přístupové klíče** a zkopírování přístupových klíčů pro mezipaměť.

   ![portál Azure][AZ05]

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Vytvořit vlastní aplikaci pomocí Initializr pružiny

1. Přejděte do <https://start.spring.io/>.

1. Zadejte, že chcete vygenerovat **Maven** projektu s **Java**, zadejte **skupiny** a **Aritifact** názvy pro vaši aplikaci a pak klikněte na odkaz na **přepnout na plnou verzi** z jara Initializr.

   ![Možnosti Initializr pružiny Basic][SI01]

   > [!NOTE]
   >
   > Initializr pružiny použije **skupiny** a **Aritifact** názvy vytvořit název balíčku; například: *com.contoso.myazuredemo*.
   >

1. Přejděte dolů k položce **webové** části a zaškrtněte políčko pro **webové**, posuňte se dolů k **NoSQL** části a zaškrtněte políčko pro **Redis**, pak Přejděte do dolní části stránky a kliknutím na tlačítko **generovat projektu**.

   ![Úplné pružiny Initializr možnosti][SI02]

1. Po zobrazení výzvy, stáhněte si projekt na cestu v místním počítači.

   ![Stáhněte si vlastní spouštěcí pružiny projekt][SI03]

1. Po extrahování souborů v místním systému bude vaše vlastní spouštěcí pružiny aplikace připravené pro úpravy.

   ![Soubory projektu pružiny vlastní spouštěcí][SI04]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a>Konfigurace vaší vlastní spouštěcí pružiny použití vašeho Redis Cache

1. Vyhledejte *application.properties* v soubor *prostředky* adresář vaší aplikace, nebo pokud ještě neexistuje, vytvořte soubor.

   ![Vyhledejte soubor application.properties][RE01]

1. Otevřete *application.properties* soubor v textovém editoru a přidejte následující řádky do souboru a nahraďte ukázkové hodnoty odpovídající vlastnosti z mezipaměti:

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Úpravy souboru application.properties][RE02]

   > [!NOTE]
   >
   > Pokud používáte jiný klienta Redis jako Jedis, která umožňuje SSL, zadali byste portu 6380 ve vaší *application.properties* souboru. Další informace najdete v tématu [použití Azure Redis Cache s Javou][Redis Cache with Java].
   >

1. Uložte a zavřete *application.properties* souboru.

1. Vytvořte složku s názvem *řadič* pod zdrojové složce balíčku; například:

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   -nebo-

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. Vytvořte nový soubor s názvem *HelloController.java* v *řadič* složky. Otevřete soubor v textovém editoru a přidejte do ní následující kód:

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   Kde je potřeba nahradit `com.contoso.myazuredemo` se název balíčku pro váš projekt.

1. Uložte a zavřete *HelloController.java* souboru.

1. Spouštěcí pružiny aplikace s Maven sestavíte a spustíte. například:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Test webové aplikace tak, že přejde na adrese http://localhost: 8080 pomocí webového prohlížeče, nebo použijte syntaxi jako v následujícím příkladu, pokud máte curl k dispozici:

   ```shell
   curl http://localhost:8080
   ```

   Měli byste vidět "Hello World!" zpráva z ukázkové řadiče zobrazí, se načte se dynamicky z mezipaměti Redis.

## <a name="next-steps"></a>Další kroky

Další informace o používání pružiny spuštění aplikace v Azure najdete v následujících článcích:

* [Nasazení aplikace spouštěcí pružiny do Azure App Service](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Spuštění aplikace spouštěcí Spring v clusteru s podporou Kubernetes v Azure Container Service](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

Další informace o získávání spuštění Redis Cache pomocí Javy v Azure, najdete v tématu [použití Azure Redis Cache s Javou][Redis Cache with Java].

<!-- URL List -->

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny Initializr]: https://start.spring.io/
[Pružiny Framework]: https://spring.io/
[Redis Cache with Java]: cache-java-get-started.md

<!-- IMG List -->

[AZ01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ01.png
[AZ02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ02.png
[AZ03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ03.png
[AZ04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ04.png
[AZ05]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ05.png

[SI01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI01.png
[SI02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI02.png
[SI03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI03.png
[SI04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI04.png

[RE01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE01.png
[RE02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE02.png
