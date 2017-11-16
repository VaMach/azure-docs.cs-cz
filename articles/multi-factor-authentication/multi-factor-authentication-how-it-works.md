---
title: "Azure Multi-Factor Authentication – jak to funguje"
description: "Azure Multi-Factor Authentication pomáhá chránit přístup k datům a aplikacím a současně plní požadavky uživatelů na jednoduchý proces přihlašování. Poskytuje dodatečné zabezpečení vyžadováním druhou podobu ověřování a zajišťuje silné ověřování přes celou řadu možností snadno ověření."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 1f3495b038171edd713678aef49be9401ab458dd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a>Jak funguje Azure Multi-Factor Authentication
Zabezpečení dvoustupňové ověření spočívá v jeho vrstveného přístupu. Porušení zabezpečení několika faktory ověřování uvede významné výzvu pro útočníky. I v případě, že útočník dokázal další heslo uživatele, je zbytečné bez nutnosti důvěryhodné zařízení u sebe. 

![Ověření](./media/multi-factor-authentication-how-it-works/howitworks.png)

Azure Multi-Factor Authentication pomáhá chránit přístup k datům a aplikacím a současně plní požadavky uživatelů na jednoduchý proces přihlašování.  Poskytuje dodatečné zabezpečení vyžadováním druhou podobu ověřování a zajišťuje silné ověřování přes celou řadu možností snadno ověření.


## <a name="methods-available-for-two-step-verification"></a>Dostupné metody pro dvoustupňové ověření
Když se uživatel přihlásí, je uživateli odeslána dalšího ověření.  Následuje seznam metod, které lze použít pro tento druhý ověření.

| Metoda ověření | Popis |
| --- | --- |
| Telefonní hovor |Volání je umístěn na registrované Telefon uživatele. Uživatel zadá kód PIN, pokud potřeby pak stiskne klávesu #. |
| Textová zpráva |Odeslána textová zpráva na mobilní telefon uživatele s šestimístný kód. Uživatel zadá tento kód na stránce přihlášení. |
| Oznámení mobilní aplikace |Žádost o ověření posílá Smartphone uživatele. Uživatel zadá kód PIN, v případě potřeby pak vybere **ověřte** na mobilní aplikaci. |
| Ověřovací kód z mobilní aplikace |Mobilní aplikace, která běží na uživatele Smartphone, zobrazí ověřovací kód, který změní každých 30 sekund. Najde poslední kód a přejde na stránku přihlášení uživatele. |
| Tokeny OATH třetích stran | Azure Multi-Factor Authentication Server může být nastaven na přijímání metody ověřování třetích stran. |

Azure Multi-Factor Authentication poskytuje metody volitelný ověření pro cloud a serveru. Můžete zvolit, které metody jsou k dispozici pro vaše uživatele: telefonní hovor, text, oznámení aplikaci nebo aplikaci kódy. Další informace najdete v tématu [volitelný ověření metody](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Další kroky

- Přečtěte si informace o různých [verze a spotřeba metody pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

- Vyberte, jestli chcete nasadit Azure MFA [v cloudu nebo místně](multi-factor-authentication-get-started.md)

- Přečtěte si odpovědi pro [nejčastější dotazy](multi-factor-authentication-faq.md)