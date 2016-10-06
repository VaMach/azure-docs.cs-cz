<properties 
    pageTitle="Azure Multi-Factor Authentication – začínáme"
    description="Zvolte pro vás ideální řešení zabezpečení multi-factor authentication položením otázky, co se pokoušíte zabezpečit a kde se nachází vaši uživatelé.  Pak vyberte cloud, server MFA nebo AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>


#Výběr  multi-factor authentication řešení pro vás

Jelikož existuje několik typů Azure  Multi-Factor Authentication, musíme určit několik věcí, aby bylo možné zjistit, která verze je ta správná pro vaše použití.  Jedná se o:

-   [Co se pokouším zabezpečit](#what-am-i-trying-to-secure)
-   [Kde se nachází uživatelé](#where-are-the-users-located)

V následujících částech naleznete pokyny k zodpovězení každé z těchto otázek.

## Co se pokouším zabezpečit?

Aby bylo možné zjistit správné řešení multi-factor authentication, nejprve musíme odpovědět na otázku, co se pokoušíte zabezpečit pomocí druhé metody ověřování.  Jedná se o aplikaci, která je v Azure?  Nebo jde například o systém vzdáleného přístupu.  Určením toho, co se pokoušíme zabezpečit, zjistíme odpověď na otázku, kde musí být povoleno ověřování multi-factor authentication.  


Co se pokoušíte zabezpečit| Multi-Factor Authentication v cloudu|Server Multi-Factor Authentication
------------- | :-------------: | :-------------: |
Aplikace Microsoft první strany|* |* |
Aplikace Saas v galerii aplikací|* |* |
Aplikace služby IIS publikované prostřednictvím proxy aplikace Azure AD|* |* |
Aplikace služby IIS nepublikované prostřednictvím proxy aplikace Azure AD | |* |
Vzdálený přístup, jako je například síť VPN, RDG| |* |



## Kde se nachází uživatelé

V závislosti na tom, kde se nachází naši uživatelé můžeme určit správné řešení, které je třeba chcete, ať už jde o multi-factor authentication v cloudu nebo místní využití serveru MFA.



Umístění uživatele| Řešení
------------- | :------------- |
Azure Active Directory| Multi-Factor Authentication v cloudu|
Azure AD a místní AD využívající federaci se službou AD FS| Jak MFA v cloudu, tak i server MFA jsou dostupné možnosti
Azure AD a místní AD pomocí DirSync Azure AD Sync Azure AD Connect – bez synchronizace hesla|Jak MFA v cloudu, tak i server MFA jsou dostupné možnosti
Azure AD a místní AD pomocí DirSync Azure AD Sync Azure AD Connect – se synchronizací hesla|Multi-Factor Authentication v cloudu
Místní služby Active Directory|Server Multi-Factor Authentication

V následující tabulce je uvedeno porovnání funkcí, které jsou s Multi-Factor Authentication v cloudu a se serverem Multi-Factor Authentication.

 | Multi-Factor Authentication v cloudu | Server Multi-Factor Authentication
------------- | :-------------: | :-------------: |
Oznámení mobilní aplikace jako druhý faktor | ● | ● |
Kód ověření mobilní aplikace jako druhý faktor | ● | ●
Telefonní hovor jako druhý faktor | ● | ●
Jednosměrné služby SMS jako druhý faktor | ● | ●
Obousměrné služby SMS jako druhý faktor |  | ●
Tokeny hardwaru jako druhý faktor |  | ●
Hesla aplikací pro klienty, kteří nepodporují MFA | ● |  
Kontrola správce nad metodami ověřování | ● | ●
Režim kódu PIN |  | ●
Výstraha podvodů | ● | ●
Sestavy MFA | ● | ●
Jednorázové potlačení |  | ●
Vlastní přivítání pro telefonní hovory | ● | ●
Přizpůsobitelné ID volajícího pro telefonní hovory | ● | ●
Důvěryhodné IP adresy | ● | ●
Zapamatovat MFA pro důvěryhodná zařízení  | ● |  
Podmíněný přístup | ● | ●
Mezipaměť |  | ●

Teď, když jsme určili, zda chcete použít cloudové  multi-factor authentication nebo server multi-factor authentication na místě, můžete začít s nastavováním a používáním Azure Multi-Factor Authentication.   **Vyberte ikonu, která představuje váš scénář!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Ověření](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>



<!--HONumber=Sep16_HO4-->


