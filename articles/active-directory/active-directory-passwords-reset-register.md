---
title: "Azure AD: Registrace pro samoobslužné resetování hesla | Dokumentace Microsoftu"
description: "Registrace ověřovacích dat pro samoobslužné resetování hesla"
services: active-directory
keywords: "správa hesel služby Active Directory, správa hesel, samoobslužné resetování hesla Azure AD"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/16/2017
ms.author: joflore
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 0f10ad4904b10a9554e949e77f0192edcb8f624f
ms.lasthandoff: 04/12/2017


---
# <a name="register-for-self-service-password-reset"></a>Registrace pro samoobslužné resetování hesla

Pokud správce tuto funkci povolil, jako koncový uživatel můžete resetovat své heslo nebo odemknout svůj účet bez nutnosti kontaktování někoho dalšího, pokud použijete samoobslužné resetování hesla. Před použitím této funkce si budete muset zaregistrovat metody ověřování nebo potvrdit předdefinované metody ověřování, které připravil správce.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Registrace nebo potvrzení ověřovacích dat pro samoobslužné resetování hesla

1. Otevřete v zařízení webový prohlížeč a přejděte na [registrační stránku pro resetování hesla](http://aka.ms/ssprsetup).
2. Zadejte uživatelské jméno a heslo získané od správce.
3. V závislosti na možnostech, které správce schválil, se zobrazí jedna nebo více z následujících položek. Můžete je nakonfigurovat nebo ověřit, že se mají použít, pokud budete potřebovat resetovat heslo.
    * Telefon do kanceláře – Tuto možnost může nastavit pouze správce.
    * Telefon pro ověření – Pro tuto možnost by mělo být nastaveno jiné telefonní číslo, k němuž máte přístup, například mobilní telefon, který může přijmout textovou zprávu nebo volání (správce může jako hodnotu uvést číslo vašeho mobilního telefonu, pokud již má váš souhlas s používáním tohoto údaje).
    * Ověřovací e-mail – Pro tuto možnost by měla být nastavena alternativní e-mailová adresa, k níž máte přístup bez hesla, které potřebujete resetovat.
    * Bezpečnostní otázky – Tato možnost vám poskytne seznam otázek schválených správcem, na které můžete odpovědět. Nesmíte použít stejnou odpověď pro více než jednu otázku.
4. Zadejte a zkontrolujte informace vyžadované správcem. Pokud je k dispozici více než jedna možnost, doporučujeme zaregistrovat si více metod, abyste si zajistili flexibilitu pro případ nedostupnosti některé metody (například při cestování, kdy nemáte přístup k telefonu do kanceláře).

    ![Zaregistrujte si metody ověřování a klikněte na Dokončit][Register].

5. Po dokončení kroku 4 zvolte **Dokončit**. Od této chvíle můžete použít samoobslužné resetování hesla, pokud byste to v budoucnu potřebovali.

Pokud zadáte údaj do pole Telefon pro ověření nebo Ověřovací e-mail, nezobrazí se v globálním adresáři. Tyto údaje se zobrazí jen vám a vašim správcům. Odpovědi na vaše bezpečnostní otázky se zobrazí jen vám.

Správci mohou vyžadovat, abyste metody ověřování po určité době potvrdili a ověřili, že máte stále zaregistrované odpovídající metody.

## <a name="next-steps"></a>Další kroky

* [Postup při změně hesla prostřednictvím samoobslužného resetování hesla](active-directory-passwords-update-your-own-password.md)
* [Registrační stránka pro resetování hesla](http://aka.ms/ssprsetup)
* [Portál pro resetování hesla](https://passwordreset.microsoftonline.com/)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registrační stránka pro resetování hesla se zobrazenými metodami a tlačítkem Dokončit"

