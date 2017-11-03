---
title: Test jednotky plochy aplikace Azure AD B2C | Microsoft Docs
description: "Test jednotky přihlášení, registrace, upravit profil a resetovat heslo uživatele cesty pomocí prostředí testovací Azure AD B2C"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 86293627-26fb-4e96-a76b-f263f9a945bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 9653d86dd5635016512bf6e6fafbf7195145ed07
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-desktop-application-configured-with-azure-ad-b2c"></a>Vyzkoušejte desktopová aplikace nakonfigurované v Azure AD B2C

Azure Active Directory B2C nabízí cloudové správy identit a udržovat vaše aplikace, obchodní a zákazníky chráněné.  Tento rychlý start používá ukázkovou aplikaci plochy Windows Presentation Foundation (WPF) za účelem ukázky:

* Pomocí **zaregistrovat nebo přihlásit** zásad k vytvoření nebo Přihlaste se pomocí zprostředkovatele identity sociálních nebo místní účet pomocí e-mailovou adresu. 
* **Volání rozhraní API** načíst zobrazovaný název ze Azure AD B2C zabezpečené prostředků.

## <a name="prerequisites"></a>Požadavky

* Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:
    - **Vývoj aplikací rozhraní .NET**

* Sociální účet ze sítě Facebook, Google, Microsoft nebo Twitteru. Pokud nemáte účet sociálních, je třeba zadat platnou e-mailovou adresu.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

[Stáhnout nebo naklonovat ukázkovou aplikaci](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) z Githubu.

## <a name="run-the-app-in-visual-studio"></a>Spusťte aplikaci v sadě Visual Studio

Ve složce projektu ukázkové aplikace otevřete `active-directory-b2c-wpf.sln` řešení v sadě Visual Studio. 

Vyberte **ladění > Spustit ladění** sestavení a spuštění aplikace. 

## <a name="create-an-account"></a>Vytvoření účtu

Klikněte na tlačítko **přihlášení** spustit **zaregistrovat nebo přihlásit** pracovního postupu. Když vytváříte účet, můžete použít existující účet zprostředkovatele sociální identity nebo e-mailový účet.

![Ukázková aplikace](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Zaregistrujte si pomocí zprostředkovatele identity sociálních

Postup registrace pomocí zprostředkovatele identity sociálních klikněte na tlačítko zprostředkovatele identity, kterou chcete použít. Pokud byste radši chtěli použít e-mailovou adresu, přejít na [zaregistrovat pomocí e-mailovou adresu](#sign-up-using-an-email-address) části.

![Přihlásit nebo zaregistrovat zprostředkovatele](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

Budete muset ověřit pomocí účtu sociálních přihlašovací údaje a aplikaci autorizovat pro čtení informací z účtu sociálních (přihlásit). Poskytnutím přístupu aplikace může načíst informace o profilu z účtu sociálních třeba název a města. 

![Ověřování a autorizaci pomocí sociálních účtu](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

Vaše nové podrobnosti účtu profilu jsou předem vyplněny informace z vašeho sociálních účtu. Upravit podrobnosti, pokud chcete a klikněte na tlačítko **pokračovat**.

![Nové podrobnosti účtu registrační profil](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

Úspěšně jste vytvořili nový uživatelský účet Azure AD B2C, který používá zprostředkovatele identity. Po přihlášení se přístupový token se zobrazí v *Token informace* textové pole. Přístupový token se používá při přístupu k prostředku rozhraní API.

![Autorizační token](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

Další krok: [přejít na upravit svůj profil](#edit-your-profile) části.

### <a name="sign-up-using-an-email-address"></a>Registrace pomocí e-mailovou adresu

Pokud si zvolíte účet sociálních nechcete použít k ověření, můžete vytvořit uživatelský účet Azure AD B2C pomocí platné e-mailovou adresu. Místní uživatelský účet Azure AD B2C použije Azure Active Directory jako zprostředkovatele identity. Chcete-li používat e-mailovou adresu, klikněte na tlačítko **nemáte účet? Zaregistrujte si teď** odkaz.

![Přihlásit nebo zaregistrovat pomocí e-mailu](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-email-wpf.png)

Zadejte platnou e-mailovou adresu a klikněte na tlačítko **Odeslat ověřovací kód**. Platné e-mailová adresa se vyžaduje pro příjem ověřovací kód z Azure AD B2C.

Zadejte ověřovací kód odběru e-mailem a klikněte na tlačítko **ověřit kód**.

Přidat informace z vašeho profilu a klikněte na **vytvořit**.

![Zaregistrovat nový účet pomocí e-mailu](media/active-directory-b2c-quickstarts-desktop-app/sign-up-new-account-profile-email-wpf.png)

Úspěšně jste vytvořili nový účet místního uživatele Azure AD B2C. Po přihlášení se přístupový token se zobrazí v *Token informace* textové pole. Přístupový token se používá při přístupu k prostředku rozhraní API.

![Autorizační token](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="edit-your-profile"></a>Úpravy profilu

Azure Active Directory B2C poskytuje funkce, aby uživatelé mohli aktualizaci jejich profilů. Klikněte na tlačítko **upravit profil** upravit profil, který jste vytvořili.

![Úprava profilu](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

Vyberte zprostředkovatele identity přidružené k účtu, který jste vytvořili. Například pokud jste použili Twitter jako zprostředkovatele identity při vytváření účtu, vyberte služby Twitter. Chcete-li upravit podrobnosti související profilu.

![Vyberte zprostředkovatele spojeného s profilem upravit](media/active-directory-b2c-quickstarts-desktop-app/edit-account-choose-provider-wpf.png)

Změna vaše **zobrazovaný název** nebo **města**. 

![Aktualizovat profil](media/active-directory-b2c-quickstarts-desktop-app/update-profile-wpf.png)

Nový přístupový token se zobrazí v *Token informace* textové pole. Pokud chcete ověřit změny v profilu, zkopírujte a vložte do tokenu decoder https://jwt.ms přístupový token.

![Autorizační token](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="access-a-resource"></a>Přístup k prostředku

Klikněte na tlačítko **volání rozhraní API** vytvořte žádost na do Azure AD B2C zabezpečené https://fabrikamb2chello.azurewebsites.net/hello prostředků. 

![Volání rozhraní API](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

Aplikace obsahuje token přístupu zobrazují v *Token informace* textového pole v požadavku. Rozhraní API odesílá zpět zobrazovaný název obsažených v tokenu přístupu.

## <a name="next-steps"></a>Další kroky

Dalším krokem je vytvoření vlastního klienta Azure AD B2C a konfigurace vzorku, který se spustí pomocí vašeho klienta. 

> [!div class="nextstepaction"]
> [Vytvoření klienta Azure Active Directory B2C na portálu Azure](active-directory-b2c-get-started.md)
