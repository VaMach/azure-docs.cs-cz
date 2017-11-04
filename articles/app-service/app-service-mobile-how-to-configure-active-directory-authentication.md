---
title: "Postup konfigurace ověřování Azure Active Directory pro vaši aplikaci aplikační služby"
description: "Informace o konfiguraci ověřování služby Azure Active Directory pro vaši aplikaci aplikační služby."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 25f0578a9e273c30ecc98af5b66c6dd43305aa03
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Konfigurace aplikace App Service pro použití přihlášení Azure Active Directory
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Toto téma ukazuje, jak nakonfigurovat Azure App Services používat Azure Active Directory k ověřování.

## <a name="express"></a>Konfigurace Azure Active Directory s použitím expresního nastavení
1. V [portál Azure], přejděte k vaší aplikaci. Klikněte na tlačítko **nastavení**a potom **ověřování/autorizace**.
2. Pokud ověřování / autorizace funkce není povolena, zapněte přepínač k **na**.
3. Klikněte na tlačítko **Azure Active Directory**a potom klikněte na **Express** pod **režim správy**.
4. Klikněte na tlačítko **OK** zaregistrovat aplikaci v Azure Active Directory. Tím se vytvoří nové registrace. Pokud chcete místo toho vyberte existující registraci, klikněte na tlačítko **vybrat existující aplikaci** a poté vyhledejte název dříve vytvořenou registrace v rámci vašeho klienta.
   Klikněte na možnost registrace, vyberte ho a klikněte na tlačítko **OK**. Pak klikněte na tlačítko **OK** v okně Nastavení Azure Active Directory.
   Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje autorizovaný přístup k obsahu webu a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace.
5. (Volitelné) Chcete-li omezit přístup k webu jenom na uživatele ověřeného službou Azure Active Directory, nastavte **akci provést, když požadavek nebude ověřený** k **přihlásit se přes Azure Active Directory**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují do Azure Active Directory pro ověřování.
6. Klikněte na **Uložit**.

Nyní jste připraveni pro ověřování ve vaší aplikaci pomocí Azure Active Directory.

## <a name="advanced"></a>(Alternativní metoda) ručně nakonfigurovat upřesňující nastavení Azure Active Directory
Můžete také zadat nastavení konfigurace ručně. Toto je upřednostňovaný řešení Pokud klienta AAD, který si přejete použít se liší od klienta, se kterým jste přihlášení do Azure. Chcete-li dokončit konfiguraci, musíte nejdřív vytvořit registrace v Azure Active Directory a potom je nutné zadat některé podrobnosti registrace do služby App Service.

### <a name="register"></a>Registrace vaší aplikace v Azure Active Directory
1. Přihlaste se na [portál Azure]a přejděte k vaší aplikaci. Kopírovat aplikace **URL**. To bude používat ke konfiguraci vaší aplikace Azure Active Directory.
2. Přejděte na **služby Active Directory**, vyberte **registrace aplikace**, pak klikněte na tlačítko **nové registrace aplikace** v horní části spuštění nové aplikace registrace. 
3. V dialogovém okně Vytvořit aplikaci registrace, zadejte **název** pro vaši aplikaci, vyberte **aplikace webového rozhraní API** zadejte v **přihlašovací adresa URL** pole vložte adresu URL aplikace (z Krok 1). Klikněte na **vytvořit**.
4. Během pár sekund by se zobrazit nové zobrazí registrace aplikace, kterou jste právě vytvořili.
5. Po přidání aplikace, klikněte na název registrace aplikace, klikněte na **nastavení** nahoře, potom klikněte na **vlastnosti** 
6. V **identifikátor ID URI aplikace** pole, vložte adresu URL aplikace (z kroku 1), také v **adresa URL domovské stránky** vložte adresu URL aplikace (z kroku 1), a pak klikněte na tlačítko **uložit**
7. Nyní klikněte na **adresy URL odpovědí**, upravit **adresa URL odpovědi**, vložte adresu URL aplikace (z kroku 1), je třeba změnit protokol, který se zajistěte, aby byla **https://** protocol (ne http://) potom připojen na konec adresy URL, */.auth/login/aad/callback* . (Například `https://contoso.azurewebsites.net/.auth/login/aad/callback`.) Klikněte na **Uložit**.   
8.  V tomto okamžiku zkopírovat **ID aplikace** pro aplikaci. Mějte to pro pozdější použití. Budete potřebovat ke konfiguraci vaší webové aplikace.
9. Zavřete okno Podrobnosti o registraci aplikace. By měla vrátit do souhrnu aplikace registrace Azure Active Directory, klikněte na **koncové body** tlačítko nahoře, zkopírujte **dokument federačních metadat** adresy URL. 
10. Otevřete nové okno prohlížeče a přejděte na adresu URL kliknutím vkládání a přejděte na stránku XML. Na začátek dokumentu bude **EntityDescriptor** elementu, by měl být **entityID** atribut formuláře `https://sts.windows.net/` následuje identifikátor GUID, které jsou specifické pro vašeho klienta (nazývané "ID klienta"). Tato hodnota pro kopírování – bude sloužit jako vaše **URL vystavitele**. Můžete nakonfigurovat aplikace k používání to později.

### <a name="secrets"></a>Informace přidat Azure Active Directory do vaší aplikace
1. Zpět v [portál Azure], přejděte k vaší aplikaci. Klikněte na tlačítko **ověřování/autorizace**. Pokud není povolená funkce ověřování/autorizace, zapněte přepínač k **na**. Klikněte na **Azure Active Directory**, v rámci zprostředkovatele ověřování, ke konfiguraci vaší aplikace. (Volitelné) Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje autorizovaný přístup k obsahu webu a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace. Vyberte **akci provést, když požadavek nebude ověřený**, tuto možnost nastavíte na **přihlásit se přes Azure Active Directory**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují do Azure Active Directory pro ověřování.
2. v konfiguraci ověřování služby Active Directory, klikněte na tlačítko **Upřesnit** pod **režim správy**. Vložte ID aplikace do pole ID klienta (z kroku 8) a vložte do hodnoty URL vystavitele entityId (z kroku 10). Pak klikněte na **OK**.
3. V okně Konfigurace ověřování služby Active Directory klikněte na **Uložit**.

Nyní jste připraveni pro ověřování ve vaší aplikaci pomocí Azure Active Directory.

## <a name="optional-configure-a-native-client-application"></a>(Volitelné) Konfigurovat nativní klientskou aplikaci
Azure Active Directory taky umožňuje registraci nativních klientů, které poskytuje větší kontrolu nad oprávněními mapování. Tuto funkci potřebujete Pokud chcete provést přihlášení pomocí knihovny, jako **Active Directory Authentication Library**.

1. Přejděte na **služby Active Directory** v [portál Azure classic].
2. Vyberte adresář a potom vyberte **aplikace** v horní části. Klikněte na tlačítko **přidat** v dolní části k vytvoření nové aplikace registrace.
3. Klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.
4. V průvodci Přidat aplikaci, zadejte **název** pro aplikaci a klikněte na tlačítko **nativní klientská aplikace** typu. Klikněte na tlačítko Pokračovat.
5. V **identifikátor URI pro přesměrování** zadejte váš web */.auth/login/done* koncový bod, pomocí schéma HTTPS. Tato hodnota by měla být podobná *https://contoso.azurewebsites.net/.auth/login/done*. Pokud se vytváří aplikace pro systém Windows, místo toho použijte [balíček SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) jako identifikátor URI.
6. Po přidání nativní aplikace, klikněte **konfigurace** kartě. Najít **ID klienta** a poznamenejte si tuto hodnotu.
7. Posuňte se dolů na stránce **oprávnění k ostatním aplikacím** části a klikněte na tlačítko **přidat aplikaci**.
8. Vyhledejte aplikaci, který jste dříve zaregistrovali a klikněte na ikonu plus. Klikněte na tlačítko zaškrtnutí zavřete dialogové okno. Pokud webovou aplikaci nelze najít, přejděte do jeho registrace a přidat nové adresa URL odpovědi (například verze protokolu HTTP vaše aktuální adresa URL), klikněte na Uložit a potom opakujte tyto kroky – aplikace by měla zobrazí v seznamu.
9. Na nový záznam, který jste právě přidali, otevřete **delegovaná oprávnění** rozevírací seznam a vyberte **přístup (appName)**. Potom klikněte na **Uložit**.

Nyní máte nakonfigurovaný nativní klientskou aplikaci, který má přístup k aplikaci služby App Service.

## <a name="related-content"></a>Související obsah
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth-config.png



<!-- URLs. -->

[portál Azure]: https://portal.azure.com/
[portál Azure classic]: https://manage.windowsazure.com/
[alternative method]:#advanced
