---
title: "Azure Active Directory B2C: Konfigurace služby Twitter | Microsoft Docs"
description: "Zadejte registrace a přihlášení k příjemce s účty služby Twitter v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.openlocfilehash: 82a001dd53cdddcf3b360090f3250af593c96fbb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts"></a>Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s účty služby Twitter.

> [!NOTE]
> Tato funkce je ve verzi preview.
> 

## <a name="create-a-twitter-application"></a>Vytvořit aplikaci služby Twitter.
Pokud chcete používat služby Twitter jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci služby Twitter a zadat se správné parametry. Twitter vývojářského účtu k tomu potřebujete. Pokud nemáte, můžete ho na získat [https://dev.twitter.com/](https://dev.twitter.com/).

1. Přejděte na [Twitter webu pro vývojáře](https://dev.twitter.com/) a přihlaste se pomocí přihlašovacích údajů.
2. Klikněte na tlačítko **Moje aplikace** pod **nástroje & podporu** a pak klikněte na **vytvořit novou aplikaci**. 
3. Ve formuláři, zadejte hodnotu **název**, **popis**, a **webu**.
4. Pro **adresu URL zpětné volání**, zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Nezapomeňte nahradit **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).
5. Zaškrtněte políčko s **vývojáře smlouvy** a klikněte na tlačítko **vytvořit aplikaci služby Twitter**.
6. Po vytvoření aplikace, klikněte na tlačítko **klíče a přístupové tokeny**.
7. Zkopírujte hodnotu **uživatelský klíč** a **uživatelský tajný klíč**. Budete potřebovat oba dva ke konfiguraci služby Twitter jako zprostředkovatel identity ve vašem klientovi.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurace služby Twitter jako zprostředkovatele identity ve vašem klientovi
1. Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.
2. Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.
3. Klikněte na **Přidat** v horní části okna.
4. Zadejte popisný **název** pro konfiguraci poskytovatele identity. Zadejte například "Twitter".
5. Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Twitter**a klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte Twitteru **uživatelský klíč** pro **id klienta** a Twitteru **uživatelský tajný klíč** pro **tajný klíč klienta**.
7. Klikněte na tlačítko **OK**a potom klikněte na **vytvořit** uložte konfiguraci služby Twitter.

