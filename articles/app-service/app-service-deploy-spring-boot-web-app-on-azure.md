---
title: "Nasazení aplikace spouštěcí pružiny do Azure App Service | Microsoft Docs"
description: "V tomto kurzu Průvodce vývojářům provede kroky nasazení spouštěcí pružiny Začínáme webové aplikace do služby Azure App Service."
services: app-service\web
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
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 8776142d5452bf5057990702c89aa1a541382ffc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a>Nasazení aplikace Spring Boot do služby Azure App Service

**[Pružiny Framework]**  open-source řešení, která pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java a jedním z dalších oblíbených projekty, které je postavená na této platformě je [ Pramenité spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.

Tento kurz vás provede, když vytvoření ukázkové pružiny spouštěcí Začínáme webové aplikace a nasazení jeho [Azure App Service].

### <a name="prerequisites"></a>Požadavky

Aby bylo možné provést kroky v tomto kurzu, musíte mít následující:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].
* Aktuální [Java Developer Kit (JDK)].
* Apache na [Maven] sestavení nástroj (verze 3).
* A [Git] klienta.

## <a name="create-the-spring-boot-getting-started-web-app"></a>Vytvoření webové aplikace pružiny spouštěcí Začínáme

Následující postup vás provede kroky, které jsou potřebné k vytvoření jednoduché webové aplikace pružiny spouštěcí a otestovat ji místně.

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

1. Klon [pružiny spouštěcí Začínáme] ukázkový projekt do adresáře, kterou jste právě vytvořili; například:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. Změňte adresář na dokončené projektu. například:
   ```
   cd gs-spring-boot
   cd complete
   ```

1. Sestavení na soubor JAR pomocí nástroje Maven; například:
   ```
   mvn package
   ```

1. Po vytvoření webové aplikace, změňte adresář na soubor JAR a spustí webovou aplikaci; například:
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. Test webové aplikace tak, že přejde na adrese http://localhost: 8080 pomocí webového prohlížeče, nebo použijte syntaxi jako v následujícím příkladu, pokud máte curl k dispozici:
   ```
   curl http://localhost:8080
   ```

1. Měli byste vidět zobrazenou následující zprávu: **pozdrav z jara spouštěcí!**

   ![Procházet ukázkové aplikace][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>Vytvoření webové aplikace Azure pro použití s Javou

Následující postup vás provede kroky k vytvoření webové aplikace Azure, nakonfigurujte požadovaná nastavení pro jazyk Java a konfigurovat přihlašovací údaje serveru FTP.

1. Vyhledejte [portál Azure] a přihlaste se.

1. Po přihlášení ke svému účtu na portálu Azure klikněte na ikonu nabídky **App Services**:
   
   ![portál Azure][AZ01]

1. Když **App Services** stránky se zobrazí, klikněte na tlačítko **+ přidat** k vytvoření nové aplikace služby.

   ![Vytvoření služby App Service][AZ02]

1. Když se zobrazí v seznamu šablon webové aplikace, klikněte na odkaz pro základní webové aplikace Microsoft.

   ![Šablony webové aplikace][AZ03]

1. Pokud stránka informace o šabloně webové aplikace se zobrazí, klikněte na tlačítko **vytvořit**.

   ![Vytvoření webové aplikace][AZ04]

1. Zadejte jedinečný název pro vaši webovou aplikaci a zadejte jakákoli další nastavení a potom **vytvořit**.

   ![Vytvoření nastavení webové aplikace][AZ05]

1. Po vytvoření webové aplikace, klikněte na ikonu nabídky **App Services**a potom klikněte na nově vytvořené webové aplikace:

   ![Seznam webových aplikací][AZ06]

1. Když webové aplikace se zobrazí, zadejte verzi Javy pomocí následujících kroků:

   a. Klikněte **nastavení aplikace** položku nabídky.

   b. Zvolte **Java 8** pro verzi Javy.

   c. Zvolte **nejnovější** pro podverzi Javy.

   d. Zvolte **nejnovější 8.5 Tomcat** pro webový kontejner. (Tento kontejner ve skutečnosti se nepoužijí; Azure pomocí kontejneru z vaší aplikace pružiny spouštěcí.)

   e. Klikněte na **Uložit**.

   ![Nastavení aplikace][AZ07]

1. Zadejte přihlašovací údaje nasazení FTP pomocí následujících kroků:

   a. Klikněte **přihlašovací údaje nasazení** položku nabídky.

   b. Zadejte uživatelské jméno a heslo.

   c. Klikněte na **Uložit**.

   ![Zadejte přihlašovací údaje nasazení.][AZ08]

1. Načíst informace o připojení FTP pomocí následujících kroků:

   a. Klikněte **přihlašovací údaje nasazení** položku nabídky.

   b. Úplné uživatelské jméno FTP a adresu URL zkopírovat a uložit pro další části tohoto kurzu.

   ![Adresy URL FTP a přihlašovací údaje][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a>Nasazení webové aplikace pružiny spouštěcí do Azure

Následující postup vás provede kroky nasazení spouštěcí pružiny webové aplikace do Azure.

1. Otevřít textového editoru, například Windows Poznámkový blok, vložte následující text do nového dokumentu a pak soubor uložte jako *web.config*:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. Po uložení *web.config* souboru k vašemu systému, připojí se k vaší webové aplikace přes FTP pomocí adresy URL, uživatelské jméno a heslo z předchozí části tohoto kurzu. Například:
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. Změnit vzdáleného adresáře do kořenové složky vaší webové aplikace (což je v */web/wwwroot*), pak zkopírujte soubor JAR z vaší aplikace pružiny spouštěcí a *web.config* z dříve. Například:
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. Po nasazení vaší JAR a *web.config* soubory do vaší webové aplikace, budete muset restartovat webovou aplikaci pomocí portálu Azure:

   ![][AZ10]

1. Test webové aplikace tak, že přejde k adrese URL webové aplikace pomocí webového prohlížeče, nebo použijte syntaxi jako v následujícím příkladu, pokud máte curl k dispozici:
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. Měli byste vidět zobrazenou následující zprávu: **pozdrav z jara spouštěcí!**

   ![Procházet ukázkové aplikace][SB02]

## <a name="next-steps"></a>Další kroky

Další informace o používání pružiny spuštění aplikace v Azure najdete v následujících článcích:

* [Nasazení aplikace spouštěcí Spring v systému Linux v Azure Container Service](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [Nasazení aplikace spouštěcí pružiny na Cluster Kubernetes v Azure Container Service](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

Další informace o depoying webové aplikace do Azure pomocí FTP, najdete v tématu [nasazení vaší aplikace do Azure App Service pomocí FTP nebo S].

Další podrobnosti o ukázkový projekt pružiny spouštěcí najdete v tématu [pružiny spouštěcí Začínáme].

Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu **pružiny Initializr** v https://start.spring.io/.

Další informace o konfigurování dalších nastavení pro webové aplikace najdete v tématu [konfigurace webových aplikací ve službě Azure App Service].

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[konfigurace webových aplikací ve službě Azure App Service]: /azure/app-service/web-sites-configure
[nasazení vaší aplikace do Azure App Service pomocí FTP nebo S]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[ Pramenité spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí Začínáme]: https://github.com/spring-guides/gs-spring-boot
[Pružiny Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
