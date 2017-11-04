---
title: "Test jednotky webové aplikace služby Azure AD B2C | Microsoft Docs"
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
ms.author: saraford
ms.openlocfilehash: 2a51f15fd38a901548dcf4c56922f9715c328463
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-web-application-configured-with-azure-ad-b2c"></a>Vyzkoušejte webová aplikace konfigurovaná na Azure AD B2C

Azure Active Directory B2C nabízí cloudové správy identit a udržovat vaše aplikace, obchodní a zákazníky chráněné.  Tento rychlý start používá ukázkovou aplikaci seznamu úkolů k předvedení:

* Pomocí **zaregistrovat nebo přihlásit** zásad k vytvoření nebo Přihlaste se pomocí zprostředkovatele identity sociálních nebo místní účet pomocí e-mailovou adresu. 
* Volání rozhraní API zabezpečené pomocí Azure AD B2C můžete vytvářet a upravovat položkami seznamu úkolů.

## <a name="prerequisites"></a>Požadavky

* Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:
    - **Vývoj pro ASP.NET a web**

* Sociální účet ze sítě Facebook, Google, Microsoft nebo Twitteru. Pokud nemáte účet sociálních, je třeba zadat platnou e-mailovou adresu.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

[Stáhnout nebo naklonovat ukázkovou aplikaci](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) z Githubu.

## <a name="run-the-app-in-visual-studio"></a>Spusťte aplikaci v sadě Visual Studio

Ve složce projektu ukázkové aplikace otevřete `B2C-WebAPI-DotNet.sln` řešení v sadě Visual Studio. 

Řešení se skládá ze dvou projektů:

* **TaskWebApp** – ASP.NET MVC webové aplikace, kde uživatel může spravovat jejich položky seznamu úkolů.  
* **TaskService** – rozhraní ASP.NET Web API back-end, který spravuje všechny operace CRUD provést na položky seznamu úkolů uživatele. Webové aplikace volá toto rozhraní API a zobrazí výsledky.

Pro tento rychlý start, budete muset spustit i `TaskWebApp` a `TaskService` projekty ve stejnou dobu. 

1. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění...** . 
2. Vyberte **více projektů po spuštění** přepínač.
3. Změna **akce** pro oba projekty tak, aby **spustit**. Klikněte na **OK**.

![Nastavit spouštěcí stránku v sadě Visual Studio](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Vyberte **ladění > Spustit ladění** sestavení a spuštění aplikace. Každá aplikace se otevře v jeho vlastní kartu prohlížeče:

* `https://localhost:44316/`– Tato stránka je webová aplikace ASP.NET. Můžete pracovat přímo pomocí této aplikace v rychlý start.
* `https://localhost:44332/`– Tato stránka je webové rozhraní API, která volá webové aplikace ASP.NET.

## <a name="create-an-account"></a>Vytvoření účtu

Klikněte na tlačítko **zaregistrovat / přihlásit** odkaz ve webové aplikaci ASP.NET spustit **zaregistrovat nebo přihlásit** pracovního postupu. Když vytváříte účet, můžete použít existující účet zprostředkovatele sociální identity nebo e-mailový účet.

![Ukázka ASP.NET webové aplikace](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Zaregistrujte si pomocí zprostředkovatele identity sociálních

Postup registrace pomocí zprostředkovatele identity sociálních klikněte na tlačítko zprostředkovatele identity, kterou chcete použít. Pokud byste radši chtěli použít e-mailovou adresu, přejít na [zaregistrovat pomocí e-mailovou adresu](#sign-up-using-an-email-address) části.

![Přihlásit nebo zaregistrovat zprostředkovatele](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

Budete muset ověřit pomocí účtu sociálních přihlašovací údaje a aplikaci autorizovat pro čtení informací z účtu sociálních (přihlásit). Poskytnutím přístupu aplikace může načíst informace o profilu z účtu sociálních třeba název a města. 

![Ověřování a autorizaci pomocí sociálních účtu](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Dokončete proces přihlášení pro zprostředkovatele identity. Klikněte například na **přihlášení** tlačítko Twitter.

Vaše nové podrobnosti účtu profilu jsou předem vyplněny informace z vašeho sociálních účtu.

![Nové podrobnosti účtu registrační profil](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

Aktualizujte pole Zobrazovaný název, funkce a města a klikněte na tlačítko **pokračovat**.  Hodnoty, které zadáte, se používají pro svůj profil účtu uživatele Azure AD B2C.

Úspěšně jste vytvořili nový uživatelský účet Azure AD B2C, který používá zprostředkovatele identity. 

Další krok: [přechod na zobrazení vaší deklarace identity](#view-your-claims) části.

### <a name="sign-up-using-an-email-address"></a>Registrace pomocí e-mailovou adresu

Pokud si zvolíte účet sociálních nechcete použít k ověření, můžete vytvořit uživatelský účet Azure AD B2C pomocí platné e-mailovou adresu. Místní uživatelský účet Azure AD B2C použije Azure Active Directory jako zprostředkovatele identity. Chcete-li používat e-mailovou adresu, klikněte na tlačítko **nemáte účet? Zaregistrujte si teď** odkaz.

![Přihlásit nebo zaregistrovat pomocí e-mailu](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-email-web.png)

Zadejte platnou e-mailovou adresu a klikněte na tlačítko **Odeslat ověřovací kód**. Platné e-mailová adresa se vyžaduje pro příjem ověřovací kód z Azure AD B2C. 

Zadejte ověřovací kód odběru e-mailem a klikněte na tlačítko **ověřit kód**.

Přidat informace z vašeho profilu a klikněte na **vytvořit**.

![Zaregistrovat nový účet pomocí e-mailu](media/active-directory-b2c-quickstarts-web-app/sign-up-new-account-profile-email-web.png)

Úspěšně jste vytvořili nový účet místního uživatele Azure AD B2C.

## <a name="reset-your-password"></a>Resetování hesla

Pokud jste vytvořili účtu pomocí e-mailovou adresu, Azure AD B2C obsahuje funkci dovolit uživatelům resetovat jejich hesla. Chcete-li upravit profil, který jste vytvořili, klikněte na název profilu v panelu nabídek a vyberte **resetovat heslo**.

Ověřit e-mailovou adresu ho a klikněte na **Odeslat ověřovací kód**. Ověřovací kód je zaslána e-mailovou adresu.

Zadejte ověřovací kód, který jste obdrželi v e-mailu a klikněte na tlačítko **ověřit kód**.

Po ověření e-mailovou adresu, klikněte na tlačítko **pokračovat**.

Zadejte nové heslo a klikněte na tlačítko **pokračovat**.

## <a name="view-your-claims"></a>Zobrazit vaše deklarace identity

Klikněte na tlačítko **deklarace identity** v panelu nabídek webové aplikace zobrazíte deklarace identity přidružené k vaší poslední akci. 

![Zaregistrovat nový účet pomocí e-mailu](media/active-directory-b2c-quickstarts-web-app/view-claims-sign-up-web.png)

V tomto příkladu se poslední akce pro *přihlásit nebo zaregistrovat* prostředí. Upozornění **typ deklarace identity** `http://schemas.microsoft.com/claims/authnclassreference` je `b2c_1_susi` označující poslední akci byla registrace nebo přihlášení. Pokud se poslední akce resetování hesla **typ deklarace identity** by `b2c_1_reset`.

## <a name="edit-your-profile"></a>Úpravy profilu

Azure Active Directory B2C poskytuje funkce, aby uživatelé mohli aktualizaci jejich profilů. V panelu nabídek webové aplikace klikněte na název profilu a vyberte **upravit profil** upravit profil, který jste vytvořili.

![Úprava profilu](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Změna vaše **zobrazovaný název** a **města**.  Klikněte na tlačítko **pokračovat** pro váš profil aktualizovat.

![Aktualizovat profil](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

Všimněte si vaše aktualizace název zobrazovaný v pravé horní části stránky po změně názvu. 

Klikněte na tlačítko **deklarace identity**. Změny můžete provedeny **zobrazovaný název** a **města** se projeví v deklaracích.

![Zobrazení deklarace identity](media/active-directory-b2c-quickstarts-web-app/view-claims-update-web.png)

 Upozornění **typ deklarace identity** `http://schemas.microsoft.com/claims/authnclassreference` se aktualizovala na `b2c_1_edit_profile` znamenající provést poslední akci byla úpravy profilu. Všimněte si také, název a Město jsou nové hodnoty *Sara S.* a *Praha*.

## <a name="access-a-resource"></a>Přístup k prostředku

Klikněte na tlačítko **seznam úkolů** zadat a upravit položky seznamu úkolů. Webové aplikace ASP.NET obsahuje přístupový token v požadavku na webové rozhraní API prostředků požadavku na oprávnění k provádění operací na položky seznamu úkolů uživatele. 

Zadejte text v **nová položka** textové pole. Klikněte na tlačítko **přidat** volat Azure AD B2C zabezpečené webového rozhraní API, která přidá položku seznamu úkolů.

![Přidat položku seznamu úkolů](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

## <a name="other-scenarios"></a>Další scénáře

Scénáře pro testování jednotky jsou následující:

* Odhlaste se od aplikace a klikněte na tlačítko **seznam úkolů**. Všimněte si, jak budete vyzváni k přihlášení a položky seznamu jsou nastavené jako trvalé. 
* Vytvořte nový účet pomocí jiného typu účtu. Například použijte poskytovatele sociálních identity, pokud jste vytvořili dříve pomocí e-mailovou adresu účtu.

## <a name="next-steps"></a>Další kroky

Dalším krokem je vytvoření vlastního klienta Azure AD B2C a konfigurace vzorku, který se spustí pomocí vašeho klienta. 

> [!div class="nextstepaction"]
> [Vytvoření klienta Azure Active Directory B2C na portálu Azure](active-directory-b2c-get-started.md)