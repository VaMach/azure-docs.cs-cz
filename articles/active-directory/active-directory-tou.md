---
title: Funkce Azure Active Directory Terms of Use | Dokumentace Microsoftu
description: "Funkce Azure AD Terms of Use umožní vám a vaší společnosti poskytnout podmínky použití uživatelům služby Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: b6318b419a0ea87fd1fb56656b1161909876f338
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-terms-of-use-feature-preview"></a>Funkce Azure Active Directory Terms of Use (Preview)
Azure AD Terms of Use poskytuje organizacím jednoduchý způsob předkládání informací koncovým uživatelům.  Zajistí, že se uživatelům zobrazí příslušná právní omezení pro zákonné požadavky nebo požadavky dodržování předpisů.

K předkládání obsahu funkce Azure AD Terms of Use využívá formát PDF.   Tento soubor PDF může mít libovolný obsah, jako jsou například stávající smluvní dokumenty, a umožňuje od koncových uživatelů při přihlášení získat jejich souhlas.  Můžete využít podmínky použití pro aplikace, skupiny uživatelů nebo pokud máte více sad podmínek použití pro různé účely.

Zbývající část tohoto dokumentu popisuje, jak funkci Azure AD Terms of Use začít využívat.  

## <a name="why-use-azure-ad-terms-of-use"></a>Pro používat funkci Azure AD Terms of Use
Je pro vás obtížné získat od zaměstnanců nebo hostů souhlas s vašimi podmínkami použití, než získají přístup? Potřebujete pomoc při zjišťování, kdo souhlasil nebo nesouhlasil s vašimi firemními podmínkami použití?  Azure AD Terms of Use poskytuje organizacím jednoduchý způsob předkládání informací koncovým uživatelům.  Zajistí, že se jim zobrazí příslušná právní omezení pro zákonné požadavky nebo požadavky dodržování předpisů.

Funkci Azure AD Terms of Use je možné využít v následujících scénářích:
-   Obecné podmínky použití pro všechny uživatele ve vaší organizaci.
-   Konkrétní podmínky použití založené na atributech uživatelů (např. doktoři vs. sestry nebo místní vs. zahraniční pracovníci, realizované prostřednictvím [dynamických skupin](https://azure.microsoft.com/updates/azure-active-directory-dynamic-membership-for-groups)).
-   Konkrétní podmínky použití založené na přístupu k aplikacím s velkým vlivem na chod firmy, jako je například Salesforce.


## <a name="prerequisites"></a>Požadavky
Při konfiguraci funkce Azure AD Terms of Use použijte následující kroky:

1. Přihlaste se k Azure AD jako globální správce, správce zabezpečení nebo správce podmíněného přístupu pro adresář, pro který chcete funkci Azure AD Terms of Use nakonfigurovat.
2. Zkontrolujte, že tento adresář má předplatné Azure AD Premium P1, P2, EMS E3 nebo EMS E5.  Pokud ne, [získejte předplatné služby Azure AD Premium](active-directory-get-started-premium.md) nebo [začněte se zkušební verzí](https://azure.microsoft.com/trial/get-started-active-directory/).
3. Zobrazte si řídicí panel funkce Azure AD Terms of Use na [https://aka.ms/catou](https://aka.ms/catou).



## <a name="add-company-terms-of-use"></a>Přidání firemních podmínek použití
Jakmile dokončíte vaše podmínky použití, přidejte je pomocí následujícího postupu.

### <a name="to-add-terms-of-use"></a>Přidání podmínek použití
1. Přejděte na řídicí panel na [https://aka.ms/catou](https://aka.ms/catou).
2. Klikněte na Přidat.</br>
![Přidání podmínek použití](media/active-directory-tou/tou2.png)
3. Zadejte **název** pro podmínky použití.
4. Zadejte **zobrazovaný název**.  Tato hlavička se uživatelům zobrazí při přihlášení.
5. Pomocí **Procházet** přejděte k souboru PDF s finálními podmínkami použití a vyberte ho.  Doporučená velikost písma je 24.
6. Nahrané podmínky můžete **vynutit** použitím šablony nebo vlastních zásad podmíněného přístupu.  Vlastní zásady podmíněného přístupu umožňují využít detailní podmínky použití (až na úroveň konkrétních cloudových aplikací nebo skupin uživatelů).  Další informace najdete v tématu o [konfiguraci zásad podmíněného přístupu](active-directory-conditional-access-best-practices.md).
7. Klikněte na možnost **Vytvořit**.
8. Pokud jste vybrali vlastní šablonu podmíněného přístupu, zobrazí se nová obrazovka, která vám umožní přizpůsobit zásady CA.
7. Teď byste měli vidět vaše nové podmínky použití.</br>

![Přidání podmínek použití](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Odstranění podmínek použití
K odebrání nebo odstranění starých podmínek použití můžete použít tento postup:

### <a name="to-delete-terms-of-use"></a>Odstranění podmínek použití
1. Přejděte na řídicí panel na [https://aka.ms/catou](https://aka.ms/catou).
2. Vyberte podmínky použití, které chcete odebrat.
3. Klikněte na **Odstranit**.
4. Vaše nové podmínky použití by se teď neměly zobrazovat.


## <a name="audit-terms-of-use"></a>Audit podmínek použití
Funkce Azure AD Terms of Use nabízí snadné auditování, takže se můžete přesvědčit, kdo a kdy přijal vaše podmínky použití.  Pokud chcete začít s auditováním použijte následující postup:

### <a name="to-audit-terms-of-use"></a>Audit podmínek použití
1. Přejděte na řídicí panel na [https://aka.ms/catou](https://aka.ms/catou).
2. Klikněte na Událost auditu.</br>
![Událost auditu](media/active-directory-tou/tou8.png)
3.  Na obrazovce protokolů auditu Azure AD můžete pomocí rozevíracích seznamů filtrovat informace a najít tak konkrétní údaje protokolu auditu.
![Událost auditu](media/active-directory-tou/tou9.png)
4.  Tyto informace můžete také stáhnout jako soubor .csv a použít místně.

## <a name="what-users-see"></a>Co se zobrazí uživatelům
Příslušným uživatelům se po vytvoření a vynucení podmínek použití zobrazí následující obrazovky.  Uvidí je během svého přihlašování.
-   Osvědčeným postupem je nastavit velikost písma v souboru PDF na 24.
![Událost auditu](media/active-directory-tou/tou10.png)
-   Takto se tato obrazovka zobrazí na mobilních zařízeních:</br></br>
![Událost auditu](media/active-directory-tou/tou11.png)

## <a name="additional-information"></a>Další informace
Následující informace byste měli znát. Mohou vám být při používání podmínek použití užitečné.


Příslušní uživatelé se musí odhlásit a znovu přihlásit, aby vyhověli novým zásadám:
 - pokud jsou u podmínek použití povolené zásady podmíněného přístupu
 - nebo pokud se vytvoří druhé podmínky použití.

Důvodem je to, že zásady podmíněného přístupu vstupují v platnost okamžitě. Když k tomu dojde, správci se začnou zobrazovat „smutné cloudy“ nebo „potíže s tokenem Azure AD“. Správce se musí odhlásit a znovu přihlásit, aby tyto nové zásady splnil.





## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Otázka: Jak poznám, kdy/jestli uživatel přijal podmínky použití?**</br>
Odpověď: Vyjádření souhlasu uživatele s podmínkami použití se zapíše do protokolu auditu. Můžete prohledat protokol auditu Azure AD a prohlédnout si výsledky.  

**Otázka: Pokud změníte podmínky použití, je potřeba, aby je uživatelé přijali znovu?**</br>
Odpověď: Ano, správce může změnit podmínky použití. Vyžaduje to opakované přijetí nových podmínek.

**Otázka: Podporují se podmínky použití ve více jazycích?**</br>
Odpověď: Ne, aktuálně není možné mít několik jazyků v jedněch podmínkách použití.  Můžete ale využít nastavení rozsahu pro skupinu (například podmínky použití pro Francii jsou jiné než podmínky použití pro Spojené království). 

**Otázka: Kdy se podmínky použití aktivují?**</br>
Odpověď: Podmínky použití se aktivují během přihlašování.

**Otázka: Na které aplikace je možné podmínkami použití cílit?**</br>
Odpověď: Můžete vytvořit zásady podmíněného přístupu pro podnikové aplikace s využitím moderního ověřování.  Další informace najdete v tématu věnovaném [podnikovým aplikacím](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal).

**Otázka: Je možné pro konkrétního uživatele nebo aplikaci přidat několikero podmínek použití?**</br>
Odpověď: Ano, a to tak, že vytvoříte několik zásad podmíněného přístupu, které cílí na tyto skupiny nebo aplikace. Pokud uživatel spadá do rozsahu několika podmínek použití, musí je odsouhlasit postupně.
 
**Otázka: Co se stane, když uživatel odmítne podmínky použití?**</br>
Odpověď: Uživatel bude mít k zablokovaný přístup k aplikaci. Aby uživatel získal přístup, musel by se musel znovu přihlásit a s těmito podmínkami souhlasit.