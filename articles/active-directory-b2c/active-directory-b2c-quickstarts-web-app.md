---
title: "Test jednotky webové aplikace Azure AD B2C povoleno | Microsoft Docs"
description: "Test jednotky přihlášení, registrace, upravit profil a resetovat heslo uživatele cesty pomocí prostředí testovací Azure AD B2C"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: patricka
ms.openlocfilehash: 07f2c21409176d30f4570e267a4472745f843f85
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="test-drive-an-azure-ad-b2c-enabled-web-app"></a>Webové aplikace s povoleným test jednotky Azure AD B2C

Azure Active Directory B2C nabízí cloudové správy identit a udržovat vaše aplikace, obchodní a zákazníky chráněné. Tento rychlý start používá ukázkovou aplikaci seznamu úkolů k předvedení:

> [!div class="checklist"]
> * Přihlaste se pomocí vlastní přihlašovací stránku.
> * Přihlášení pomocí zprostředkovatele identity sociálních.
> * Vytváření a správě vašeho profilu účtu a uživatele Azure AD B2C.
> * Volání webového rozhraní API, které jsou zabezpečené službou Azure AD B2C.

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) s **ASP.NET a webové vývoj** zatížení. 
* Sociální účet ze sítě Facebook, Google, Microsoft nebo Twitteru.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

[Stáhnout nebo naklonovat ukázkovou aplikaci](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) z Githubu.

## <a name="run-the-app-in-visual-studio"></a>Spusťte aplikaci v sadě Visual Studio

Ve složce projektu ukázkové aplikace otevřete `B2C-WebAPI-DotNet.sln` řešení v sadě Visual Studio. 

Řešení je ukázková aplikace seznamu úkolů skládající se ze dvou projektů:

* **TaskWebApp** – ASP.NET MVC webové aplikace, kde uživatel může spravovat jejich položky seznamu úkolů.  
* **TaskService** – rozhraní ASP.NET Web API back-end, který spravuje operace provést na položky seznamu úkolů uživatele. Webová aplikace volání této webové rozhraní API a zobrazí výsledky.

Pro tento rychlý start, budete muset spustit i `TaskWebApp` a `TaskService` projekty ve stejnou dobu. 

1. V nabídce sady Visual Studio, vyberte **projekty > nastavit projekty po spuštění...** . 
2. Vyberte **více projektů po spuštění** přepínač.
3. Změna **akce** pro oba projekty tak, aby **spustit**. Klikněte na **OK**.

![Nastavit spouštěcí stránku v sadě Visual Studio](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Vyberte **ladění > Spustit ladění** sestavení a spuštění aplikace. Každá aplikace se otevře v jeho vlastní kartu prohlížeče:

`https://localhost:44316/`– Tato stránka je webová aplikace ASP.NET. Můžete pracovat přímo pomocí této aplikace v rychlý start.
`https://localhost:44332/`– Tato stránka je webové rozhraní API, která volá webové aplikace ASP.NET.

## <a name="create-an-account"></a>Vytvoření účtu

Klikněte na tlačítko **zaregistrovat / přihlásit** odkaz ve webové aplikaci ASP.NET spustit **zaregistrovat nebo přihlásit** pracovního postupu. Když vytváříte účet, můžete použít existující účet zprostředkovatele sociální identity nebo e-mailový účet. Pro tento rychlý start používejte účet sociálních identity zprostředkovatele ze sítě Facebook, Google, Microsoft nebo Twitteru.

![Ukázka ASP.NET webové aplikace](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Zaregistrujte si pomocí zprostředkovatele identity sociálních

Postup registrace pomocí zprostředkovatele identity sociálních klikněte na tlačítko zprostředkovatele identity, kterou chcete použít. 

![Přihlásit nebo zaregistrovat zprostředkovatele](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

Budete muset ověřit pomocí účtu sociálních přihlašovací údaje a aplikaci autorizovat pro čtení informací z účtu sociálních (přihlásit). Poskytnutím přístupu aplikace může načíst informace o profilu z účtu sociálních třeba název a města. 

Dokončete proces přihlášení pro zprostředkovatele identity. Například pokud jste zvolili Twitter, zadejte své přihlašovací údaje služby Twitter a klikněte na tlačítko **přihlášení**.

![Ověřování a autorizaci pomocí sociálních účtu](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Jsou předem vyplněny informace z vašeho účtu sociálních nové Podrobnosti profilu účtu Azure AD B2C.

Aktualizujte pole Zobrazovaný název, funkce a města a klikněte na tlačítko **pokračovat**.  Hodnoty, které zadáte, se používají pro svůj profil účtu uživatele Azure AD B2C.

![Nové podrobnosti účtu registrační profil](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

Máte úspěšně:

> [!div class="checklist"]
> * Ověřit pomocí zprostředkovatele identity.
> * Vytvořit uživatelský účet Azure AD B2C. 

## <a name="edit-your-profile"></a>Úpravy profilu

Azure Active Directory B2C poskytuje funkce, aby uživatelé mohli aktualizaci jejich profilů. V panelu nabídek webové aplikace klikněte na název profilu a vyberte **upravit profil** upravit profil, který jste vytvořili.

![Úprava profilu](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Změna vaše **zobrazovaný název** a **města**.  Klikněte na tlačítko **pokračovat** pro váš profil aktualizovat.

![Aktualizovat profil](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

Všimněte si, že název zobrazovaný v pravé horní části stránky se zobrazí aktualizovaná název. 

## <a name="access-a-secured-web-api-resource"></a>Přístup k zabezpečeným webové rozhraní API prostředků

Klikněte na tlačítko **seznam úkolů** zadat a upravit položky seznamu úkolů. Webové aplikace ASP.NET obsahuje přístupový token v požadavku na webové rozhraní API prostředků požadavku na oprávnění k provádění operací na položky seznamu úkolů uživatele. 

Zadejte text v **nová položka** textové pole. Klikněte na tlačítko **přidat** volat Azure AD B2C zabezpečené webového rozhraní API, která přidá položku seznamu úkolů.

![Přidat položku seznamu úkolů](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

Úspěšně jste použili účtu uživatele Azure AD B2C Chcete-li provést autorizovaným volání webového rozhraní API Azure AD B2C zabezpečené.

## <a name="next-steps"></a>Další kroky

Ukázka používá v tento rychlý start slouží k akci scénáře Azure AD B2C, včetně:

* Vytvoření nové místní účet pomocí e-mailovou adresu.
* Resetování hesla místní účet.

Pokud jste připraveni pustíte do vytvoření vlastního klienta Azure AD B2C a nakonfigurovat vzorku, který se spustí pomocí vlastního klienta, vyzkoušejte následující kurzu.

> [!div class="nextstepaction"]
> [Vytvoření webové aplikace ASP.NET s Azure Active Directory B2C profil registrace, přihlášení, úpravy a resetování hesla](active-directory-b2c-devquickstarts-web-dotnet-susi.md)