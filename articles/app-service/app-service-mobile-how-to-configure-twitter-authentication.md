---
title: "Jak nakonfigurovat ověřování služby Twitter pro vaši aplikaci aplikační služby"
description: "Informace o konfiguraci ověřování služby Twitter pro vaši aplikaci aplikační služby."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: afde020b7817dc58ecea24eb4a09cf93d0986eb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Postup konfigurace aplikace služby App Service pomocí přihlášení služby Twitter.
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Toto téma ukazuje, jak nakonfigurovat služby Azure App Service pomocí služby Twitter jako zprostředkovatel ověřování.

Dokončete postup v tomto tématu, musí mít účet služby Twitter, který má ověřenou e-mailovou adresu a telefonní číslo. Chcete-li vytvořit nový účet služby Twitter, přejděte na <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"></a>Aplikaci zaregistrovat u služby Twitter.
1. Přihlaste se na [portál Azure]a přejděte k vaší aplikaci. Kopie vašeho **URL**. To použijete ke konfiguraci vaší aplikace služby Twitter.
2. Přejděte na [Twitter vývojáři] web, přihlaste se pomocí přihlašovacích údajů účtu služby Twitter a klikněte na **vytvořit novou aplikaci**.
3. Zadejte **název** a **popis** pro novou aplikaci. Vložení ve vaší aplikaci **URL** pro **webu** hodnotu. Potom **adresu URL zpětné volání**, vložte **adresu URL zpětné volání** jste zkopírovali dříve. Toto je vaše mobilní aplikace brána připojena cesta, */.auth/login/twitter/callback*. Například, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Ujistěte se, že používáte schéma HTTPS.
4. V dolní části stránky přečtěte si a přijměte podmínky. Pak klikněte na tlačítko **vytvořit aplikaci služby Twitter**. Takovém postupu zaregistruje aplikace zobrazí podrobnosti o aplikaci.
5. Klikněte na tlačítko **nastavení** zkontrolujte **povolit této aplikace, který se má použít k přihlášení pomocí služby Twitter**, pak klikněte na tlačítko **nastavení aktualizace**.
6. Vyberte **klíče a přístupové tokeny** kartě. Poznamenejte si hodnoty **uživatelský klíč (klíč rozhraní API)** a **uživatelský tajný klíč (tajný klíč rozhraní API)**.
   
   > [!NOTE]
   > Uživatelský tajný klíč je důležitým bezpečnostním pověřením. S kýmkoli sdílet tento tajný klíč nebo distribuovat s vaší aplikací.
   > 
   > 

## <a name="secrets"></a>Twitter přidat informace do vaší aplikace
1. Zpět v [portál Azure], přejděte k vaší aplikaci. Klikněte na tlačítko **nastavení**a potom **ověřování / autorizace**.
2. Pokud ověřování / autorizace funkce není povolena, zapněte přepínač k **na**.
3. Klikněte na tlačítko **Twitter**. Vložte v ID aplikace a tajný klíč aplikace pro hodnoty, které jste získali dříve. Pak klikněte na **OK**.
   
   ![][1]
   
   Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje autorizovaný přístup k obsahu webu a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace.
4. (Volitelné) Chcete-li omezit přístup k webu jenom na uživatele ověřeného službou Twitter, nastavte **akci provést, když požadavek nebude ověřený** k **Twitter**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují do služby Twitter pro ověřování.
5. Klikněte na **Uložit**.

Nyní jste připraveni pro ověřování ve vaší aplikaci pomocí služby Twitter.

## <a name="related-content"></a>Související obsah
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter vývojáři]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[portál Azure]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
