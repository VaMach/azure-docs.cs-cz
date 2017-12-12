---
title: "Test jednotky jednostránkové aplikace Azure AD B2C | Microsoft Docs"
description: "Test jednotky přihlášení, registrace, upravit profil a resetovat heslo uživatele cesty pomocí prostředí testovací Azure AD B2C"
services: active-directory-b2c
documentationcenter: 
author: saraford
manager: mtillman
editor: PatAltimore
ms.assetid: 5a8a46af-28bb-4b70-a7f0-01a5240d0255
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: ba8ee4657309ab2a541f4c7b3fd4879542eee63c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="test-drive-a-single-page-application-configured-with-azure-ad-b2c"></a>Vyzkoušejte jednostránkové aplikace konfigurovaná na Azure AD B2C

## <a name="about-this-sample"></a>O této ukázce

Azure Active Directory B2C nabízí cloudové správy identit a udržovat vaše aplikace, obchodní a zákazníky chráněné.  Tento rychlý start využívá k předvedení ukázkové jednostránkové aplikace:

* Pomocí **zaregistrovat nebo přihlásit** zásad k vytvoření nebo Přihlaste se pomocí zprostředkovatele identity sociálních nebo místní účet pomocí e-mailovou adresu. 
* **Volání rozhraní API** načíst zobrazovaný název ze Azure AD B2C zabezpečené prostředků.

## <a name="prerequisites"></a>Požadavky

* Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:
    - **Vývoj pro ASP.NET a web**

* Instalovat [Node.js](https://nodejs.org/en/download/)

* Sociální účet ze sítě Facebook, Google, Microsoft nebo Twitteru. Pokud nemáte účet sociálních, je třeba zadat platnou e-mailovou adresu.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

[Stáhnout nebo naklonovat ukázkovou aplikaci](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) z Githubu.

## <a name="run-the-sample-application"></a>Spuštění ukázkové aplikace

Chcete tuto ukázku spustit z příkazového řádku Node.js: 

```
cd active-directory-b2c-javascript-msal-singlepageapp
npm install && npm update
node server.js
```

V okně konzoly zobrazuje číslo portu pro webovou aplikaci spuštěné v počítači.

```
Listening on port 6420...
```

Otevřete `http://localhost:6420` ve webovém prohlížeči pro přístup k webové aplikaci.


![Ukázkovou aplikaci v prohlížeči](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

## <a name="create-an-account"></a>Vytvoření účtu

Klikněte **přihlášení** tlačítko Spustit Azure AD B2C **zaregistrovat nebo přihlásit** pracovního postupu. Když vytváříte účet, můžete použít existující účet zprostředkovatele sociální identity nebo e-mailový účet.

### <a name="sign-up-using-a-social-identity-provider"></a>Zaregistrujte si pomocí zprostředkovatele identity sociálních

Postup registrace pomocí zprostředkovatele identity sociálních klikněte na tlačítko zprostředkovatele identity, kterou chcete použít. Pokud byste radši chtěli použít e-mailovou adresu, přejít na [zaregistrovat pomocí e-mailovou adresu](#sign-up-using-an-email-address) části.

![Přihlásit nebo zaregistrovat zprostředkovatele](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

Budete muset ověřit pomocí účtu sociálních přihlašovací údaje a aplikaci autorizovat pro čtení informací z účtu sociálních (přihlásit). Poskytnutím přístupu aplikace může načíst informace o profilu z účtu sociálních třeba název a města. 

![Ověřování a autorizaci pomocí sociálních účtu](media/active-directory-b2c-quickstarts-spa/twitter-authenticate-authorize-spa.png)

Vaše nové podrobnosti účtu profilu jsou předem vyplněny informace z vašeho sociálních účtu. 

![Nové podrobnosti účtu registrační profil](media/active-directory-b2c-quickstarts-spa/new-account-sign-up-profile-details-spa.png)

Aktualizujte pole Zobrazovaný název, funkce a města a klikněte na tlačítko **pokračovat**.  Hodnoty, které zadáte, se používají pro svůj profil účtu uživatele Azure AD B2C.

Úspěšně jste vytvořili nový uživatelský účet Azure AD B2C, který používá zprostředkovatele identity. 

Další krok: [volání prostředek](#call-a-resource) části.

### <a name="sign-up-using-an-email-address"></a>Registrace pomocí e-mailovou adresu

Pokud si zvolíte účet sociálních nechcete použít k ověření, můžete vytvořit uživatelský účet Azure AD B2C pomocí platné e-mailovou adresu. Místní uživatelský účet Azure AD B2C použije Azure Active Directory jako zprostředkovatele identity. Chcete-li používat e-mailovou adresu, klikněte na tlačítko **nemáte účet? Zaregistrujte si teď** odkaz.

![Přihlásit nebo zaregistrovat pomocí e-mailu](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-email-spa.png)

Zadejte platnou e-mailovou adresu a klikněte na tlačítko **Odeslat ověřovací kód**. Platné e-mailová adresa se vyžaduje pro příjem ověřovací kód z Azure AD B2C. 

Zadejte ověřovací kód odběru e-mailem a klikněte na tlačítko **ověřit kód**.

Přidat informace z vašeho profilu a klikněte na **vytvořit**.

![Zaregistrovat nový účet pomocí e-mailu](media/active-directory-b2c-quickstarts-spa/sign-up-new-account-profile-email-web.png)

Úspěšně jste vytvořili nový účet místního uživatele Azure AD B2C.

## <a name="call-a-resource"></a>Volání prostředku

Jakmile se přihlásíte, můžete kliknout na **volání webového rozhraní API** tlačítko tak, aby měl název zobrazení vrácená z volání webového rozhraní API jako objekt JSON. 

![Odpověď webové rozhraní API](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

## <a name="next-steps"></a>Další kroky

Dalším krokem je vytvoření vlastního klienta Azure AD B2C a konfigurace vzorku, který se spustí pomocí vašeho klienta. 

> [!div class="nextstepaction"]
> [Vytvoření klienta Azure Active Directory B2C na portálu Azure](active-directory-b2c-get-started.md)