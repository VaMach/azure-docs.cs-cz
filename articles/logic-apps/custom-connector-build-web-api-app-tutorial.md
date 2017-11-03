---
title: "Vytvoření vlastních konektorů z rozhraní Web API – Azure Logic Apps | Microsoft Docs"
description: "Vytvoření vlastních konektorů z rozhraní Web API"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 22bf335718721d29933557142b436e75778f8c86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-custom-connectors-from-web-apis"></a>Vytvoření vlastních konektorů z rozhraní Web API

Chcete-li vytvořit vlastní konektor, který můžete použít v Azure Logic Apps, Microsoft Flow nebo Microsoft PowerApps, nejprve vytvořit webového rozhraní API, který hostitel s Azure Web Apps, ověřování pomocí služby Azure Active Directory a zaregistrujte se jako konektor s Logic Apps, tok, nebo PowerApps. V tomto kurzu se dozvíte, jak tyto úlohy provedete podle budovy aplikace rozhraní ASP.NET Web API. Najdete v tématu vzorů, které se zobrazí kód pro vaše konektor triggery a akce, můžete struktury [vytvořte vlastní rozhraní API, která můžete volat z pracovních aplikace logiky](../logic-apps/logic-apps-create-api-app.md).

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2013 nebo novější](https://www.visualstudio.com/vs/). Tento kurz používá Visual Studio 2015.

* Kód pro webového rozhraní API. Pokud žádné nemáte, zkuste v tomto kurzu: [Začínáme s ASP.NET Web API 2 (C#)](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).

* Předplatné Azure. Pokud nemáte předplatné, můžete začít s [bezplatný účet Azure](https://azure.microsoft.com/free/). Jinak, zaregistrujte si [předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

## <a name="create-and-deploy-an-aspnet-web-app-to-azure"></a>Vytvoření a nasazení webové aplikace ASP.NET do Azure

Pro tento kurz vytvoření webové aplikace Visual C# ASP.NET. 

1. Otevřete Visual Studio, a potom vyberte **soubor** > **nový projekt**.

   1. Rozbalte položku **nainstalovaná**, přejděte na **šablony** > **Visual C#** > **webové**a vyberte **ASP Webové aplikace .NET**.

   2. Zadejte název projektu, umístění a název řešení pro vaši aplikaci a pak zvolte **OK**.

   Například:

   ![Vytvoření webové aplikace Visual C# ASP.NET](./media/custom-connector-build-web-api-app-tutorial/visual-studio-new-project-aspnet-web-app.png)

2. V **nové webové aplikace ASP.NET** vyberte **webového rozhraní API** šablony. Pokud již není vybrána, vyberte **hostitel v cloudu**. Zvolte **změnit ověřování**.

   ![Vyberte šablonu "Webového rozhraní API", "Hostovat v cloudu", "Změna ověřování"](./media/custom-connector-build-web-api-app-tutorial/visual-studio-web-api-template.png)

3. Vyberte **bez ověřování**a zvolte **OK**. Můžete nastavit ověřování později.

   ![Vyberte možnost "Žádné ověřování"](./media/custom-connector-build-web-api-app-tutorial/visual-studio-change-authentication.png)

4. Když **nové webové aplikace ASP.NET** se bude zobrazovat, vyberte **OK**. 

5. V **vytvořit službu App Service** pole, zkontrolujte hostování nastavení popsané v tabulce, proveďte požadované změny a zvolte **vytvořit**. 

   [Plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) představuje kolekci fyzické prostředky využívané pro hostování aplikací ve vašem předplatném Azure. Další informace o [služby App Service](../app-service/app-service-value-prop-what-is.md).

   ![Vytvoření služby App Service](./media/custom-connector-build-web-api-app-tutorial/visual-studio-create-app-service.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | Vaše Azure pracovní nebo školní účet nebo osobní účet Microsoft | *váš uživatelský účet* | Vyberte uživatelský účet. | 
   | **Název webové aplikace** | *vlastní--api název webové aplikace* nebo výchozí název | Zadejte název pro rozhraní API webové aplikace, která se používá v adrese URL vaší aplikace, například: http://*-api název webové aplikace*. | 
   | **Předplatné** | *Název předplatného Azure* | Vyberte předplatné Azure, které chcete použít. | 
   | **Skupina prostředků** | *Azure--název skupiny prostředků –* | Vyberte existující skupinu prostředků Azure, nebo pokud jste to ještě neudělali, vytvořte skupinu prostředků. <p>**Poznámka:**: Skupina prostředků Azure slouží k uspořádání prostředků Azure ve vašem předplatném Azure. | 
   | **Plán služby App Service** | *Service název plánu služby App* | Vyberte existující plán aplikační služby, nebo pokud jste to ještě neudělali, vytvořte plán. | 
   |||| 

   Pokud vytvoříte plán služby App Service, zadejte tato nastavení:

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Umístění** | *nasazení oblast* | Vyberte oblast pro nasazení aplikace. | 
   | **Velikost** | *Aplikace služby plán velikosti* | Vyberte velikost plán, který určuje náklady a výpočetní kapacitu prostředků pro plán služby. | 
   |||| 

   Chcete-li nastavit žádné další prostředky požadované aplikace, zvolte **Objevte další služby Azure**.

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Typ prostředku** | *Typ prostředku Azure* | Vyberte a nastavit další prostředky požadované aplikace. | 
   |||| 

6. Po Visual Studio nasadí projekt, sestavte kód pro vaši aplikaci.

## <a name="create-an-openapi-swagger-file-that-describes-your-web-api"></a>Vytvořte soubor OpenAPI (Swagger), která popisuje vašeho webového rozhraní API

K připojení aplikace webového rozhraní API k Logic Apps, budete potřebovat [OpenAPI (dříve Swagger) souboru](http://swagger.io/) operace vaše rozhraní API, který popisuje. Můžete napsat vlastní OpenAPI definice rozhraní API pomocí [Swagger online editor](http://editor.swagger.io/), ale tento kurz používá nástroj na Otevřít zdroj s názvem [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle/blob/master/README.md).

1. Pokud jste to ještě neudělali, nainstalujte balíček Swashbuckle Nuget v projektu sady Visual Studio.

   1. V sadě Visual Studio, vyberte **nástroje** > **Správce balíčků NuGet** > 
    **Konzola správce balíčků**.

   2. V **Konzola správce balíčků**, přejděte do adresáře projektu vaší aplikace, pokud existuje ještě nejste (Spustit `Set-Location "project-path"`), a spusťte tento příkaz prostředí PowerShell: 
   
      `Install-Package Swashbuckle`

      Například:

      ![Konzola správce balíčků, instalaci Swashbuckle](./media/custom-connector-build-web-api-app-tutorial/visual-studio-package-manager-install-swashbuckle.png)

   > [!TIP]
   > Pokud spustíte aplikaci po instalaci Swashbuckle, Swashbuckle generuje soubor OpenAPI na této adrese URL: 
   >
   > http://*{your-web-api-app-root-URL}*/swagger/docs/v1
   > 
   > Swashbuckle vytvoří také uživatelské rozhraní na této adrese URL: 
   > 
   > http://*{your-web-api-app-root-URL}*  /swagger

3. Až budete připraveni, publikujte aplikaci webového rozhraní API Azure. Chcete-li publikovat ze sady Visual Studio, klikněte pravým tlačítkem na projekt webové v Průzkumníku řešení, zvolte **publikování...** a postupujte podle pokynů.

   > [!IMPORTANT]
   > Zkontrolujte duplicitní ID operací dokument OpenAPI neplatný. Pokud jste použili šablony ukázka C# šablony *dvakrát opakuje toto ID operace*:`Values_Get` 
   > 
   > Chcete-li tento problém vyřešit, změňte jednu instanci na `Value_Get` a znovu publikovat.

4. Získáte dokument OpenAPI procházením do tohoto umístění: 

   http://*{your-web-api-app-root-URL}*/swagger/docs/v1

   Můžete také stáhnout [ukázka OpenAPI dokumentu](https://pwrappssamples.blob.core.windows.net/samples/webAPI.json) z tohoto kurzu. 
   Ujistěte se, že odeberete komentáře, které od "/ /", než použijete dokumentu.

5. Uložte obsah jako soubor ve formátu JSON. Podle toho, prohlížeč, možná muset zkopírovat a vložit do prázdný textový soubor.

## <a name="next-steps"></a>Další kroky

* [Nastavení ověřování pro vlastní konektory](../logic-apps/custom-connector-azure-active-directory-authentication.md)











