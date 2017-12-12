---
title: "Microsoft Azure Multi-Factor Authentication uživatele stavy"
description: "Další informace o stavu uživatele v Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: b7fb5135ac1ae776851d79f936eb860b4b121c71
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Jak vyžadovat dvoustupňové ověřování pro uživatele nebo skupinu

Existují dva přístupy pro vyžádání dvoustupňové ověřování. První možností je umožnit každý jednotlivý uživatel pro Azure Multi-Factor Authentication (MFA). Pokud uživatelé jsou povolené jednotlivě, fungují vždy dvoustupňové ověření (s několika výjimkami, jako při přihlášení z důvěryhodných IP adres nebo pokud je zapnuta funkce zapamatovaných zařízeních). Druhou možností je nastavit zásady podmíněného přístupu, která vyžaduje dvoustupňové ověřování za určitých podmínek.

>[!TIP] 
>Vyberte jednu z těchto metod tak, aby vyžadovala dvoustupňové ověření, nikoli oba dva. Povolení uživatele pro Azure MFA potlačí všechny zásady podmíněného přístupu.

## <a name="which-option-is-right-for-you"></a>Možnosti, kterou je pro vás nejvhodnější

**Povolení Azure MFA změnou stavů uživatele** je tradiční přístup pro vyžádání dvoustupňové ověřování. Funguje pro oba Azure MFA v cloudu a Azure MFA serveru. Všichni uživatelé, které vám umožňují mít stejné prostředí, která je k provedení dvoustupňové ověřování při každém přihlášení. Povolení uživatele potlačí všechny zásady podmíněného přístupu, které může mít vliv na tohoto uživatele. 

**Povolení Azure MFA se zásadami podmíněného přístupu** je flexibilnější přístup pro vyžádání dvoustupňové ověřování. Ho fungovat pouze pro Azure MFA v cloudu, ale a podmíněného přístupu je [placené funkce služby Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Můžete vytvořit zásady podmíněného přístupu, které platí pro skupiny, jakož i jednotlivé uživatele. S vysokým rizikem skupiny je možné přidělit další omezení než nízkým rizikem skupiny nebo dvoustupňové ověřování můžete vyžaduje se jenom pro vysoce rizikové cloudové aplikace a pro ty, které jsou nízkým rizikem přeskočeno. 

Obě tyto možnosti vyzvat uživatele k registraci pro Azure Multi-Factor Authentication při prvním přihlášení po zapnutí požadavky. Obě možnosti také pracovat konfigurovat [nastavení ověřování Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md)

## <a name="enable-azure-mfa-by-changing-user-status"></a>Povolit Azure MFA změnou stavu uživatele

Uživatelské účty v Azure Multi-Factor Authentication mají následující tři jedinečné stavy:

| Status | Popis | Neprohlížečové aplikace vliv | Aplikace prohlížeče vliv | Moderní ověřování vliv |
|:---:|:---:|:---:|:--:|:--:|
| Zakázáno |Výchozího stavu pro nového uživatele, která nejsou zaregistrovaná Azure Multi-Factor Authentication (MFA). |Ne |Ne |Ne |
| Povoleno |Uživatel byl zaregistrován ke službě Azure MFA, ale není registrován. Se zobrazí výzva k registraci při příštím přihlášení. |Ne.  Budou nadále fungovat až do dokončení procesu registrace. | Ano. Po vypršení platnosti relace, se bude vyžadovat registrace MFA.| Ano. Jakmile vyprší platnost přístupového tokenu, se bude vyžadovat registrace MFA. |
| Vynuceno |Uživatel byl zaregistrován a dokončil proces registrace pro Azure MFA. |Ano.  Aplikace potřebujete hesla aplikace. |Ano. MFA vyžaduje na přihlášení. | Ano. MFA vyžaduje na přihlášení. |

Stav uživatele, odráží jestli správce zapsal je v Azure MFA, a zda jejich dokončit proces registrace.

Všichni uživatelé spustí *zakázáno*. Při zápisu uživatele v Azure MFA, jejich změny stavu *povoleno*. Při povolení uživatelé přihlásit a dokončit proces registrace, jejich stav se změní na *vynucené*.  

### <a name="view-the-status-for-a-user"></a>Zobrazení stavu pro uživatele

Pomocí následujících kroků pro přístup ke stránce, kde můžete zobrazit a spravovat stavů uživatele:

1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**.
   ![Vyberte služby Multi-Factor Authentication](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. Otevře se nová stránka, která zobrazuje stavů uživatele.
   ![Stav uživatele služby Multi-Factor authentication – snímek obrazovky](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Změnit stav pro uživatele

1. Pomocí předchozího postupu se na stránku uživatelů služby Multi-Factor authentication.
2. Vyhledejte uživatele, který chcete povolit pro Azure MFA. Možná bude třeba změnit zobrazení v horní části. 
   ![Najít uživatele – snímek obrazovky](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Zaškrtněte políčko vedle jména.
4. Na pravé straně v části Rychlé kroky, zvolte **povolit** nebo **zakázat**.
   ![Povolit vybraného uživatele – snímek obrazovky](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >*Povolit* uživatelé automaticky přepnout *vynucené* při registraci pro Azure MFA. Neměňte ručně vynucené stavu uživatele. 

5. Potvrďte výběr v místním okně, které se otevře. 

Po povolení uživatelů, je vhodné seznámit e-mailem. Sdělte jim, že budete vyzváni k registraci při příštím přihlášení. Navíc pokud vaše organizace používá neprohlížečové aplikace, které nepodporují moderní ověřování, že budete muset vytvořit hesla aplikací. Můžete použít také odkaz na našem [Průvodce pro koncové uživatele Azure MFA](./end-user/multi-factor-authentication-end-user.md) pomáhá jim začít pracovat.

### <a name="use-powershell"></a>Použití prostředí PowerShell
Chcete-li změnit stav stavu uživatele pomocí [Azure AD PowerShell](/powershell/azure/overview), změňte `$st.State`. Existují tři možné stavy:

* Povoleno
* Vynuceno
* Zakázáno  

Nelze přesunout přímo na uživatele *vynucené* stavu. Aplikace nezaložené na prohlížeči přestanou fungovat, protože uživatel neabsolvoval registraci MFA a nezískal [heslo aplikace](multi-factor-authentication-whats-next.md#app-passwords). 

Pomocí prostředí PowerShell je vhodný, když potřebujete hromadné povolení uživatelů. Vytvořte Powershellový skript, který prochází seznam uživatelů a umožňuje jim:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Zde naleznete příklad:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Povolit Azure MFA se zásadami podmíněného přístupu

Podmíněný přístup je placené funkce služby Azure Active Directory, s mnoha možností možné konfigurace. Tyto kroky provede jeden způsob, jak vytvořit zásady. Další informace najdete v tématu o [podmíněného přístupu v Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **podmíněného přístupu**.
3. Vyberte **nové zásady**.
4. V části **přiřazení**, vyberte **uživatelů a skupin**. Použití **zahrnout** a **vyloučit** karty se můžete určit, kteří uživatelé a skupiny bude spravovat zásady.
5. V části **přiřazení**, vyberte **cloudových aplikací**. Vybrat možnost zahrnutí **všech cloudových aplikací**.
6. V části **přístup k ovládacím prvkům**, vyberte **Grant**. Zvolte **vyžadovat vícefaktorové ověřování**.
7. Zapnout **povolit zásady** k **na** a pak vyberte **Uložit**.

Další možnosti v zásadách podmíněného přístupu umožňují zadat přesně v případě, že by měl být požadované dvoustupňové ověřování. Například můžete dokonce vytvářet zásady, které stavy: když dodavatelů došlo k pokusu o přístup k vaší aplikace Nákup z nedůvěryhodné sítě na zařízení, které nejsou připojené k doméně, vyžaduje dvoustupňové ověřování. 

## <a name="next-steps"></a>Další kroky

- Získat tipy pro [osvědčené postupy pro podmíněný přístup](../active-directory/active-directory-conditional-access-best-practices.md)

- Správa nastavení služby Multi-Factor Authentication pro [uživatelům a jejich zařízení](multi-factor-authentication-manage-users-and-devices.md)
