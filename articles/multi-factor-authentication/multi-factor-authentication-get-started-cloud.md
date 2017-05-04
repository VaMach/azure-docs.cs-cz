---
title: "Začínáme s ověřováním Azure Multi-Factor Authentication v cloudu | Dokumentace Microsoftu"
description: "Toto je stránka ověřování Azure Multi-Factor Authentication, která popisuje, jak začít s Azure MFA v cloudu."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 554931e96e073ec2f2f68df2297e1ee21f5eda87
ms.lasthandoff: 04/27/2017


---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Začínáme s ověřováním Azure Multi-Factor Authenticatin v cloudu
V tomto článku se dozvíte, jak začít používat Azure Multi-Factor Authentication v cloudu.

> [!NOTE]
> V následující dokumentaci naleznete informace o tom, jak povolit uživatelům používání **portálu Azure Classic**. Pokud hledáte informace o nastavení ověřování Azure Multi-Factor Authentication pro uživatele O365, najdete je v tématu [Nastavení ověřování Multi-Factor Authentication pro Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA v cloudu](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a>Požadavek
[Zaregistrujte si předplatné Azure](https://azure.microsoft.com/pricing/free-trial/) – pokud ještě nemáte předplatné Azure, musíte se zaregistrovat. Pokud právě začínáte a používáte Azure MFA, můžete použít zkušební verzi předplatného

## <a name="enable-azure-multi-factor-authentication"></a>Povolení služby Azure Multi-Factor Authentication
Pokud vaši uživatelé mají licence, které zahrnují službu Azure Multi-Factor Authentication, pro zapnutí Azure MFA není potřeba nic dělat. Můžete začít s vyžadováním dvoustupňového ověřování u jednotlivých uživatelů. Licence, které umožňují Azure MFA, jsou:
- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

Pokud nemáte některou z těchto tří licencí nebo nemáte dostatek licencí pro pokrytí všech uživatelů, nic se neděje. Stačí jenom přidat další krok a v adresáři [vytvořit poskytovatele Multi-Factor Auth](multi-factor-authentication-get-started-auth-provider.md).

## <a name="turn-on-two-step-verification-for-users"></a>Zapnutí dvoustupňového ověřování pro uživatele
Pokud chcete začít požadovat dvoustupňové ověřování pro uživatele, změňte stav uživatele ze zakázaného na povolený.  Další informace týkající se stavů uživatele najdete v části [Stavy uživatele v Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md).

Pomocí následujícího postupu povolte MFA pro uživatele.

### <a name="to-turn-on-multi-factor-authentication"></a>Zapnutí Multi-Factor Authentication
1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com).
2. Na levé straně klikněte na službu **Active Directory**.
3. Pod oblastí Adresáře klikněte na adresář pro uživatele, který si přejete povolit.
   ![Kliknutí na Adresář](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Nahoře klikněte na tlačítko **Uživatelé**.
5. V dolní části stránky, klikněte na tlačítko **Spravovat Multi-Factor Auth**. Otevře se nová karta prohlížeče.
   ![Kliknutí na Adresář](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Najděte uživatele, u kterého chcete povolit dvoustupňové ověřování. Možná bude třeba změnit zobrazení v horní části. Ujistěte se, že je stav **Zakázáno.**
   ![Povolení uživatele](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7. **Zaškrtněte** políčko vedle svého názvu.
8. Na pravé straně klikněte na tlačítko **Povolit**.
   ![Povolení uživatele](./media/multi-factor-authentication-get-started-cloud/user1.png)
9. Klikněte na tlačítko **Povolit Multi-Factor Auth**.
   ![Povolení uživatele](./media/multi-factor-authentication-get-started-cloud/enable2.png)
10. Všimněte si, že se stav uživatele změnil ze **Zakázáno** na **Povoleno**.
    ![Povolení uživatelů](./media/multi-factor-authentication-get-started-cloud/user.png)

Až uživatele povolíte, měli byste je upozornit e-mailem. Při dalším pokusu o přihlášení se jim zobrazí výzva k přihlášení účtu k dvoustupňovému ověřování. Až začnou dvoustupňové ověřování používat, budou také muset nastavit hesla pro aplikace, aby se jim nestalo, že pro ně budou uzamknuty aplikace nezaložené na prohlížeči.

## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>Automatizace PowerShellu k automatizaci zapnutí dvoustupňového ověřování
Ke změně [stavu](multi-factor-authentication-whats-next.md) pomocí [Azure AD PowerShellu](/powershell/azure/overview) můžete použít následující.  Můžete změnit `$st.State` na jeden z následujících stavů:

* Povoleno
* Vynuceno
* Zakázáno  

> [!IMPORTANT]
> Nedoporučujeme vám přesouvat uživatele přímo ze stavu Zakázáno do stavu Vynuceno. Aplikace nezaložené na prohlížeči přestanou fungovat, protože uživatel neabsolvoval registraci MFA a nezískal [heslo aplikace](multi-factor-authentication-whats-next.md#app-passwords). Pokud máte aplikace nezaložené na prohlížeči a potřebujete hesla aplikace, doporučujeme vám přejít ze stavu Zakázáno do Povoleno. To vám umožní zaregistrovat a získat hesla aplikací pro uživatele. Potom můžete uživatele převést do stavu Vynuceno.

Použití prostředí PowerShell představuje možnost pro hromadné povolení uživatelů. Aktuálně neexistuje žádná funkce hromadného povolení na portálu Azure a je nutné vybrat jednotlivé uživatele zvlášť. To může být poměrně náročný úkol, pokud máte spoustu uživatelů. Vytvořením skriptu PowerShellu pomocí následujícího postupu můžete projít seznam uživatelů a povolit je.

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


Další informace najdete v části [Stavy uživatele v Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md).

## <a name="next-steps"></a>Další kroky
Teď, když máte nastavené Azure Multi-Factor Authentication v cloudu, můžete konfigurovat a nastavit nasazení. Další podrobnosti najdete v části [Konfigurace Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md).


