---
title: "Vytvoření první webové aplikace v Javě v Azure během pěti minut | Dokumentace Microsoftu"
description: "Přečtěte si, jak snadné je spouštět webové aplikace ve službě App Service, na příkladu nasazení jednoduché aplikace v Javě."
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
ms.date: 04/17/2017
ms.author: cephalin;robmcm
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 75e51ca45a899c6b6fa123346aa3c5860fd1600d
ms.contentlocale: cs-cz
ms.lasthandoff: 05/08/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Vytvoření první webové aplikace v Javě v Azure během pěti minut

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Tento Rychlý start vám pomůže nasadit první webovou aplikaci v Javě do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během pár minut. Až budete s tímto kurzem hotovi, budete mít jednoduchou webovou aplikaci založenou na Javě spuštěnou v cloudu.

![Přechod do webové aplikace](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="before-you-begin"></a>Než začnete

Tento kurz ukazuje, jak pomocí integrovaného vývojového prostředí Eclipse pro vývojáře v jazyce Java EE sestavit webovou aplikaci v Javě a nasadit ji do Azure. Pokud ještě Eclipse nemáte nainstalované, můžete si ho zdarma stáhnout z webu http://www.eclipse.org/.

Pro zjednodušení procesu publikování webových aplikací v Javě do Azure se bude v postupech v tomto kurzu používat sada [Azure Toolkit pro Eclipse](/azure/azure-toolkit-for-eclipse). Pokyny k instalaci této sady nástrojů najdete v tématu [Instalace sady Azure Toolkit pro Eclipse](/azure/azure-toolkit-for-eclipse-installation).

> [!NOTE]
>
> K dokončení kroků v tomto kurzu můžete použít také [IntelliJ IDEA](https://www.jetbrains.com/idea/) od JetBrains. Několik kroků se u tohoto vývojového prostředí může mírně lišit, ačkoli existuje také sada [Azure Toolkit pro IntelliJ](/azure/azure-toolkit-for-intellij), kterou můžete použít ke zjednodušení procesu publikování pro toto integrované vývojové prostředí.
>

K dokončení kroků v tomto kurzu budete také potřebovat předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Vytvoření dynamického webového projektu v Eclipse

V Eclipse vyberte nabídku **File** (Soubor), pak **New** (Nový) a **Dynamic Web Project** (Dynamický webový projekt).

V dialogovém okně **New Dynamic Web Project** (Nový dynamický webový projekt) pojmenujte projekt **MyFirstJavaOnAzureWebApp** a vyberte **Finish** (Dokončit).
   
![Dialogové okno Dynamic Web Project (Dynamický webový projekt)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

> [!NOTE]
>
> Pokud máte nainstalované místní běhové prostředí, jako je [Apache Tomcat](https://tomcat.apache.org/), můžete ho zadat do pole **Target runtime** (Cílové běhové prostředí).
>

Po vytvoření dynamického webového projektu přidejte novou stránku JSP tak, že v zobrazení Project Explorer (Průzkumník projektů) rozbalíte svůj projekt, kliknete pravým tlačítkem myši na složku **WebContent**, kliknete na **New** (Nový) a pak kliknete na **JSP File** (Soubor JSP).

![Nabídka New JSP File (Nový soubor JSP)](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

Až se zobrazí dialogové okno New JSP File (Nový soubor JSP), pojmenujte soubor **index.jsp**, jako nadřazenou složku ponechte **MyFirstJavaOnAzureWebApp/WebContent** a pak klikněte na **Next** (Další).

![Dialogové okno New JSP File (Nový soubor JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Na druhé stránce dialogového okna New JSP File (Nový soubor JSP) pojmenujte soubor **index.jsp**, jako nadřazenou složku ponechte **MyFirstJavaOnAzureWebApp/WebContent** a pak klikněte na **Finish** (Dokončit).

![Dialogové okno New JSP File (Nový soubor JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-2.png)

Když se v Eclipse otevře vaše nová stránka, nahraďte stávající část `<body></body>` následujícím kódem:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Uložte provedené změny stránky.

## <a name="publish-your-web-app-to-azure"></a>Publikování webové aplikace do Azure

K nasazení vaší webové aplikace do Azure využijete několik funkcí, které poskytuje sada Azure Toolkit pro Eclipse.

Pokud chcete zahájit proces publikování, použijte některou z následujících metod:

* Klikněte pravým tlačítkem myši na svůj projekt v zobrazení **Project Explorer** (Průzkumník projektů) v Eclipse, klikněte na **Azure** a pak klikněte na **Publikovat jako webovou aplikaci Azure**.

   ![Místní nabídka Publikovat jako webovou aplikaci Azure](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

* Klikněte na ikonu **Publish** (Publikovat) na panelu nástrojů Eclipse a pak klikněte na **Publikovat jako webovou aplikaci Azure**.

   ![Rozevírací nabídka Publikovat jako webovou aplikaci Azure](./media/app-service-web-get-started-java/publish-as-azure-web-app-drop-down-menu.png)

Pokud jste se ještě nepřihlásili ke svému účtu Azure, budete vyzváni k přihlášení. Chcete-li tak učinit, proveďte následující kroky:

1. Existují dvě různé možnosti přihlášení k účtu Azure – pro účely tohoto kurzu zvolte **Interactive** (Interaktivní).

   ![Dialogové okno Azure Sign In (Přihlášení do Azure)](./media/app-service-web-get-started-java/azure-signin-dialog-box.png)

1. Zadejte své přihlašovací údaje Azure a pak klikněte na **Přihlásit**.

   ![Dialogové okno Přihlášení do Azure](./media/app-service-web-get-started-java/azure-login-dialog-box.png)

1. Zvolte svá předplatná Azure a pak klikněte na **Vybrat**.

   ![Dialogové okno Přihlášení do Azure](./media/app-service-web-get-started-java/select-azure-subscriptions-dialog-box.png)

> [!NOTE]
>
> Podrobné pokyny k typům přihlášení **Interactive** (Interaktivní) a **Automated** (Automatizované) najdete v článku [Pokyny k přihlášení do Azure pro sadu Azure Toolkit pro Eclipse](https://go.microsoft.com/fwlink/?linkid=846174).
>

Jakmile budete přihlášeni ke svému účtu Azure, zobrazí se dialogové okno **Nasadit webovou aplikaci**. Pokud publikujete webovou aplikaci do Azure poprvé, neměli byste v seznamu vidět žádné služby App Services. Pokud je to váš případ, nebo pokud chcete vytvořit novou službu App Service, jako další krok vytvoříte službu App Service. Chcete-li to provést, klikněte na **Vytvořit**.

![Dialogové okno Nasadit webovou aplikaci](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

Až se zobrazí dialogové okno **Vytvořit službu App Service**, budete muset zadat tyto počáteční údaje:

* Jedinečný název pro vaši webovou aplikaci, který se použije jako adresa DNS vaší webové aplikace, například pro **MyJavaWebApp** to bude *myjavawebapp.azurewebsites.net*.

* Jaký webový kontejner bude vaše webová aplikace používat, například **Newest Tomcat 8.5** (Nejnovější Tomcat 8.5).

* Vaše předplatné Azure.

   ![Dialogové okno Vytvořit službu App Service](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

Pokud nemáte žádné existující plány služby App Service, nebo pokud chcete vytvořit nový plán služby, budete muset zadat následující informace:

* Jedinečný název pro nový plán služby – tento název se zobrazí při budoucím publikování webových aplikací pomocí sady Azure Toolkit a bude uveden na webu [Azure Portal](https://portal.azure.com), když budete spravovat svůj účet.

* Zeměpisné umístění, ve kterém se plán služby vytvoří.

* Cenová úroveň pro plán služby.

   ![Vytvoření plánu služby App Service](./media/app-service-web-get-started-java/create-app-service-plan.png)

Dále klikněte na kartu **Skupina prostředků**. Pokud nemáte žádné existující skupiny prostředků, nebo pokud chcete vytvořit novou, budete muset zadat jedinečný název nové skupiny prostředků, jinak zvolte existující skupinu prostředků z rozevírací nabídky.

![Vytvoření plánu služby App Service](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

Nakonec klikněte na kartu **JDK**. Uvidíte několik možností, které vývojářům umožňují zadat sady JDK (Java Development Kit) jiných výrobců nebo vlastní, ale pro účely tohoto kurzu vyberte **Výchozí** a pak klikněte na **Vytvořit**.

![Vytvoření plánu služby App Service](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)

Sada Azure Toolkit začne vytvářet novou službu App Service a zobrazí dialogové okno s průběhem zpracování.

![Indikátor průběhu vytváření služby App Service](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

Jakmile bude nová služba App Service vytvořena, jako poslední možnost je třeba vybrat, jestli se má webová aplikace nasadit do kořenového adresáře nového webu. Například pokud máte službu App Service na adrese *wingtiptoys.azurewebsites.net* a nezvolíte nasazení do kořenového adresáře, nasadí se vaše webová aplikace **MyFirstJavaOnAzureWebApp** do *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Nasazení webové aplikace do kořenového adresáře](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Po dokončení všech předchozích kroků klikněte na **Nasadit** a publikujte svou webovou aplikaci do Azure.

![Nasazení webové aplikace do Azure](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

Blahopřejeme! Úspěšně jste nasadili svou webovou aplikaci do Azure! Nyní můžete zobrazit náhled vaší webové aplikace na webu Azure:

![Přechod do webové aplikace](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="updating-your-web-app"></a>Aktualizace webové aplikace

Jakmile webovou aplikaci úspěšně nasadíte do Azure, je její aktualizace mnohem jednodušší; následující kroky vás provedou procesem publikování změn do webové aplikace.

Nejprve změňte ukázkový kód JSP z předchozí části tak, aby se název nahrazoval aktuálním datem:

```jsp
<%@ page
    language="java"
    contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"
    import="java.text.SimpleDateFormat"
    import="java.util.Date" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<% SimpleDateFormat date = new SimpleDateFormat("yyyy/MM/dd"); %>
<title><% out.println(date.format(new Date())); %></title>
</head>
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
</html>
```

Po uložení změn klikněte pravým tlačítkem myši na svůj projekt v zobrazení **Project Explorer** (Průzkumník projektů) v Eclipse, klikněte na **Azure** a pak klikněte na **Publikovat jako webovou aplikaci Azure**.

![Publikování aktualizované webové aplikace](./media/app-service-web-get-started-java/publish-updated-web-app-context-menu.png)

Až se zobrazí dialogové okno **Nasadit webovou aplikaci**, objeví se v něm služba App Service z předchozí části. K aktualizaci webové aplikace stačí zvýraznit službu App Service a kliknutím na **Nasadit** publikovat změny.

![Nasazení webové aplikace do Azure](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

> [!NOTE]
>
> Pokud nasazujete webovou aplikaci do kořenového adresáře služby App Service, budete muset znovu zaškrtnout políčko **Nasadit do kořenového adresáře** při každém publikování změn.
>

Po publikování změn si všimněte, že se název stránky v prohlížeči změnil na dnešní datum.

![Přechod do webové aplikace](./media/app-service-web-get-started-java/browse-web-app-2.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud chcete odstranit webovou aplikaci, použijte **Azure Explorer**, který je součástí sady nástrojů Azure. Pokud zobrazení **Azure Explorer** ještě v Eclipse nevidíte, zobrazte ho pomocí následujících kroků:

1. Klikněte na **Window** (Okno), **Show View** (Zobrazit zobrazení) a pak klikněte na **Other** (Ostatní).

   ![Nabídka Show View (Zobrazit zobrazení)](./media/app-service-web-get-started-java/show-azure-explorer-view-1.png)

2. Až se objeví dialogové okno **Show View** (Zobrazit zobrazení), vyberte **Azure Explorer** a klikněte na **OK**.

   ![Dialogové okno Show View (Zobrazit zobrazení)](./media/app-service-web-get-started-java/show-azure-explorer-view-2.png)

Pokud chcete odstranit webovou aplikaci ze zobrazení Azure Explorer, je potřeba rozbalit uzel **Webové aplikace**, kliknout pravým tlačítkem myši na webovou aplikaci a vybrat **Odstranit**.

![Odstranění webové aplikace](./media/app-service-web-get-started-java/delete-web-app-context-menu.png)

Po zobrazení výzvy k odstranění webové aplikace klikněte na **OK**.

## <a name="next-steps"></a>Další kroky

Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [Sada Azure Toolkit pro Eclipse (Tento článek)](../azure-toolkit-for-eclipse.md)
  * [Novinky v sadě Azure Toolkit pro Eclipse](../azure-toolkit-for-eclipse-whats-new.md)
  * [Instalace sady Azure Toolkit pro Eclipse](../azure-toolkit-for-eclipse-installation.md)
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse](https://go.microsoft.com/fwlink/?linkid=846174)
* [Sada Azure Toolkit pro IntelliJ](../azure-toolkit-for-intellij.md)
  * [Novinky v sadě Azure Toolkit pro IntelliJ](../azure-toolkit-for-intellij-whats-new.md)
  * [Instalace sady Azure Toolkit pro IntelliJ](../azure-toolkit-for-intellij-installation.md)
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ](https://go.microsoft.com/fwlink/?linkid=846179)

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/) a [Java Tools for Visual Studio Team Services](https://java.visualstudio.com/) (Nástroje Java pro Visual Studio Team Services).

