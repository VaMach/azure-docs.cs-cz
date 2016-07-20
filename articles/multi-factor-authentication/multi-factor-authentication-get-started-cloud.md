<properties 
    pageTitle="Začínáme s ověřováním Microsoft Azure Multi-Factor Authenticatin v cloudu" 
    description="Toto je stránka ověřování Azure Multi-Factor Authentication, která popisuje, jak začít s Azure MFA v cloudu." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/12/2016" 
    ms.author="billmath"/>

# Začínáme s ověřováním Azure Multi-Factor Authenticatin v cloudu
V následujícím článku se dozvíte, jak začít používat Azure Multi-Factor Authentication v cloudu.

> [AZURE.NOTE]  V následující dokumentaci naleznete informace o tom, jak povolit uživatelům používání **portálu Azure Classic**. Pokud hledáte informace o nastavení ověřování Azure Multi-Factor Authentication pro uživatele O365, naleznete je v tématu [Nastavení ověřování Multi-Factor Authentication pro Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA v cloudu](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## Předpoklady
Než povolíte ověřování Azure Multi-Factor Authentication pro uživatele, je třeba splnit následující předpoklady. 




- [Zaregistrujte si předplatné Azure](https://azure.microsoft.com/pricing/free-trial/) – pokud ještě nemáte předplatné Azure, musíte se zaregistrovat. Pokud právě začínáte a používáte Azure MFA, můžete použít zkušební verzi předplatného
2. [Vytvořte poskytovatele ověřování Multi-Factor Auth](multi-factor-authentication-get-started-auth-provider.md) a přiřaďte ho do adresáře nebo [přiřaďte licence uživatelům](multi-factor-authentication-get-started-assign-licenses.md) 

> [AZURE.NOTE]  Licence jsou k dispozici pro uživatele, kteří mají Azure MFA, Azure AD Premium nebo Enterprise Mobility Suite (EMS).  MFA je zahrnuto v Azure AD Premium a EMS. Pokud máte dostatek licencí, nemusíte vytvářet poskytovatele ověřování. 
        

## Zapnutí ověřování Multi-Factor Authentication pro uživatele
Pokud chcete zapnout ověřování Multi-Factor Authentication pro uživatele, jednoduše změňte stav uživatele ze zakázaného na povolený.  Další informace týkající se stavů uživatele naleznete v části [Stavy uživatele v Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md)

Pomocí následujícího postupu povolte MFA pro uživatele.

### Zapnutí Multi-Factor Authentication
--------------------------------------------------------------------------------
1.  Přihlaste se jako správce do **portálu Azure Classic**.
2.  Na levé straně klikněte na službu **Active Directory**.
3.  Pod oblastí **adresáře** klikněte na adresář pro uživatele, který si přejete povolit.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Nahoře klikněte na tlačítko **Uživatelé**.
5.  V dolní části stránky, klikněte na tlačítko **Spravovat Multi-Factor Auth**.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Otevře se nová karta prohlížeče.  Najděte uživatele, u kterého chcete povolit Multi-Factor Authentication. Možná bude třeba změnit zobrazení v horní části. Ujistěte se, zda je stav **zakázáno.**
![Povolte uživatele](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  **Zaškrtněte** políčko vedle svého názvu.
7.  Na pravé straně klikněte na tlačítko **Povolit**. 
![Povolit uživatele](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Klikněte na tlačítko **Povolit Multi-Factor Auth**.
![Povolit uživatele](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Měli byste zaznamenat, že se stav uživatele změnil ze **Zakázáno** na **Povoleno**.
![Povolit uživatele](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Po povolení uživatelů je doporučujeme upozornit e-mailem.  Ten by je měl také informovat o způsobu, jak mohou používat své aplikace s výjimkou prohlížeče, aby nedošlo k uzamknutí.


## Automatizace zapnutí Multi-Factor Authentication pomocí prostředí PowerShell

Chcete-li změnit [stav](multi-factor-authentication-whats-next.md) pomocí [Azure AD PowerShell](powershell-install-configuremd), můžete použít následující.  Můžete změnit `$st.State` na jeden z následujících stavů:


- Povoleno
- Vynuceno
- Zakázáno  

> [AZURE.IMPORTANT]  Upozorňujeme, že pokud přejdete přímo ze stavu Zakázáno do stavu Vynuceno, zastaralí klienti ověřování přestanou pracovat, protože uživatel neprošel registrací MFA a nezískal [heslo aplikace](multi-factor-authentication-whats-next.md#app-passwords).  Pokud máte zastaralé klienty ověřování a potřebujete hesla aplikace, pak doporučujeme přejít ze stavu Zakázáno do Povoleno.  To vám umožní uživatelům zaregistrovat a získat jejich hesla aplikací.   
        
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Použití prostředí PowerShell představuje možnost pro hromadné povolení uživatelů.  Aktuálně neexistuje žádná funkce hromadného povolení na portálu Azure a je nutné vybrat jednotlivé uživatele zvlášť.  To může být poměrně náročný úkol, pokud máte mnoho uživatelů.  Vytvořením skriptu prostředí PowerShell pomocí výše uvedeného můžete projít seznam uživatelů a povolit je.  Zde naleznete příklad:
    
    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Další informace týkající se stavů uživatele naleznete v části [Stavy uživatele v Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md)

## Další kroky
Nyní, když máte nastavené Multi-Factor Authentication v cloudu, můžete konfigurovat a nastavit nasazení.  Viz [Konfigurace Azure Multi-Factor Authentication.]



<!--HONumber=Jun16_HO2-->


