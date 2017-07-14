---
title: "Vytvoření první webové Java aplikace ve službě Azure"
description: "Nasazením základní Java aplikace se naučíte, jak spouštět webové aplikace ve službě App Service."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: a805d92fbe1043b9143140bdbfb8626362aa8bb5
ms.contentlocale: cs-cz
ms.lasthandoff: 06/20/2017

---
# Vytvoření první webové Java aplikace ve službě Azure
<a id="create-your-first-java-web-app-in-azure" class="xliff"></a>

Funkce [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů. Tento kurz Rychlý start ukazuje, jak nasadit webovou Java aplikaci do služby App Service pomocí [integrovaného vývojového prostředí Eclipse pro vývojáře na platformě Java EE](http://www.eclipse.org/).

![Hello Azure! Ukázková webová aplikace](./media/app-service-web-get-started-java/browse-web-app-1.png)

## Požadavky
<a id="prerequisites" class="xliff"></a>

K provedení kroků v tomto kurzu Rychlý start je potřeba nainstalovat:

* Bezplatné [integrované vývojové prostředí Eclipse pro vývojáře na platformě Java EE](http://www.eclipse.org/downloads/). Tento kurz Rychlý start používá Eclipse Neon.
* [Sadu nástrojů Azure pro Eclipse](/azure/azure-toolkit-for-eclipse-installation).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Vytvoření dynamického webového projektu v Eclipse
<a id="create-a-dynamic-web-project-in-eclipse" class="xliff"></a>

V prostředí Eclipse vyberte **File** (Soubor) > **New** (Nový) > **Dynamic Web Project** (Dynamický webový projekt).

V dialogovém okně **New Dynamic Web Project** (Nový dynamický webový projekt) pojmenujte projekt **MyFirstJavaOnAzureWebApp** a vyberte **Finish** (Dokončit).
   
![Dialogové okno New Dynamic Web Project (Nový dynamický webový projekt)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### Přidání stránky JSP
<a id="add-a-jsp-page" class="xliff"></a>

Pokud se nezobrazí Project Explorer (Prohlížeč projektu), obnovte zobrazení.

![Pracovní prostor Java EE pro Eclipse](./media/app-service-web-get-started-java/pe.png)

V prohlížeči Project Explorer rozbalte projekt **MyFirstJavaOnAzureWebApp**.
Klikněte pravým tlačítkem na **WebContent** (Webový obsah) a potom vyberte **New** (Nový) > **JSP File** (Soubor JSP).

![Nabídka pro nový soubor JSP v prohlížeči Project Explorer](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

V dialogovém okně **New JSP File** (Nový soubor JSP):

* Soubor pojmenujte **index.jsp**.
* Vyberte **Finish** (Dokončit).

  ![Dialogové okno New JSP File (Nový soubor JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

V souboru index.jsp nahraďte element `<body></body>` následujícím kódem:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Uložte změny.

## Publikování webové aplikace do služby Azure
<a id="publish-the-web-app-to-azure" class="xliff"></a>

V prohlížeči Project Explorer klikněte pravým tlačítkem na projekt a potom vyberte **Azure** > **Publish as Azure Web App** (Publikovat jako webovou aplikaci Azure).

![Místní nabídka Publish as Azure Web App (Publikovat jako webovou aplikaci Azure)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

V dialogovém okně **Azure Sign In** (Přihlášení k Azure) ponechejte možnost **Interactive** (Interaktivní) a pak vyberte **Sign in** (Přihlásit).

Postupujte podle pokynů k přihlášení.

### Dialogové okno Deploy Web App (Nasazení webové aplikace)
<a id="deploy-web-app-dialog-box" class="xliff"></a>

Jakmile budete přihlášeni ke svému účtu Azure, zobrazí se dialogové okno **Deploy Web App** (Nasazení webové aplikace).

Vyberte **Create** (Vytvořit).

![Dialogové okno Deploy Web App (Nasazení webové aplikace)](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### Dialogové okno Create App Service (Vytvoření služby App Service)
<a id="create-app-service-dialog-box" class="xliff"></a>

Dialogové okno **Create App Service** (Vytvoření služby App Service) se zobrazí s výchozími hodnotami. Číslo **170602185241** zobrazené na následujícím obrázku se bude ve vašem dialogovém okně lišit.

![Dialogové okno Create App Service (Vytvoření služby App Service)](./media/app-service-web-get-started-java/cas1.png)

V dialogovém okně **Create App Service** (Vytvoření služby App Service):

* Ponechejte vygenerovaný název webové aplikace. Tento název musí být v rámci služby Azure jedinečný. Název je součástí adresy URL webové aplikace. Příklad: Pokud je název webové aplikace **MyJavaWebApp**, bude adresa URL *myjavawebapp.azurewebsites.net*.
* Ponechejte výchozí webový kontejner.
* Vyberte předplatné služby Azure.
* Na kartě **App service plan** (Plán služby App Service):

  * **Create new** (Vytvořit nový): Ponechejte výchozí hodnotu, kterou je název plánu služby App Service.
  * **Location** (Umístění): Vyberte **West Europe** (Západní Evropa) nebo umístění ve vaší blízkosti.
  * **Pricing tier** (Cenová úroveň): Vyberte možnost Free. Informace o funkcích najdete na stránce [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).

   ![Dialogové okno Create App Service (Vytvoření služby App Service)](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### Karta Resource group (Skupina prostředků)
<a id="resource-group-tab" class="xliff"></a>

Vyberte kartu **Resource group** (Skupina prostředků). Ponechte výchozí vygenerovanou hodnotu pro skupinu prostředků.

![Karta Resource group (Skupina prostředků)](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Vyberte **Create** (Vytvořit).

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

Sada nástrojů Azure vytvoří webovou aplikaci a zobrazí dialogové okno s ukazatelem průběhu.

![Dialogové okno Create App Service Progress (Průběh vytváření plánu služby App Service)](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### Dialogové okno Deploy Web App (Nasazení webové aplikace)
<a id="deploy-web-app-dialog-box" class="xliff"></a>

V dialogovém okně **Deploy Web App** (Nasazení webové aplikace) vyberte **Deploy to root** (Nasadit do kořene). Pokud máte službu App Service na adrese *wingtiptoys.azurewebsites.net* a nezvolíte nasazení do kořene, nasadí se vaše webová aplikace **MyFirstJavaOnAzureWebApp** do *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Dialogové okno Deploy Web App (Nasazení webové aplikace)](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Dialogové okno zobrazuje vybrané hodnoty pro Azure, JDK a webový kontejner.

Výběrem možnosti **Deploy** (Nasadit) publikujte webovou aplikaci do služby Azure.

Po dokončení publikování vyberte odkaz **Publikováno** v dialogovém okně **Protokol aktivit v Azure**.

![Dialogové okno Protokol aktivit v Azure](./media/app-service-web-get-started-java/aal.png)

Blahopřejeme! Úspěšně jste nasadili svou webovou aplikaci do služby Azure. 

![Hello Azure! Ukázková webová aplikace](./media/app-service-web-get-started-java/browse-web-app-1.png)

## Aktualizace webové aplikace
<a id="update-the-web-app" class="xliff"></a>

Změňte zprávu v ukázkovém kódu JSP na jinou.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Uložte změny.

V prohlížeči Project Explorer klikněte pravým tlačítkem na projekt a potom vyberte **Azure** > **Publish as Azure Web App** (Publikovat jako webovou aplikaci Azure).

Zobrazí se dialogové okno **Deploy Web App** (Nasazení webové aplikace), ve kterém se zobrazí aplikační služba, kterou jste předtím vytvořili. 

> [!NOTE]
> Při každém publikování vyberte **Deploy to root** (Nasadit do kořene).
>

Vyberte webovou aplikaci a pak vyberte **Deploy** (Nasadit). Tím změny publikujete.

Když se zobrazí odkaz **Publikováno**, vyberte ho. Tím přejdete do webové aplikace a budete moct vidět změny.

## Správa webové aplikace
<a id="manage-the-web-app" class="xliff"></a>

Pokud chcete zobrazit webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

V levé nabídce vyberte **Skupiny prostředků**.

![Navigace na skupiny prostředků na portálu](media/app-service-web-get-started-java/rg.png)

Vyberte skupinu prostředků. Stránka zobrazuje prostředky, které jste vytvořili v tomto kurzu Rychlý start.

![Skupina prostředků myResourceGroup](media/app-service-web-get-started-java/rg2.png)

Vyberte webovou aplikaci (**webapp-170602193915** na předchozím obrázku).

Zobrazí se stránka **Přehled**. Tato stránka poskytuje přehled toho, jak si aplikace vede. Tady můžete provádět základní úkoly správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Záložky na levé straně okna obsahují další možnosti konfigurace, které můžete otevřít. 

![Stránka služby App Service na webu Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## Další kroky
<a id="next-steps" class="xliff"></a>

> [!div class="nextstepaction"]
> [Mapování vlastní domény](app-service-web-tutorial-custom-domain.md)

