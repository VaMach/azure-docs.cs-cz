<properties 
    pageTitle="Začínáme s ověřováním Azure Multi-Factor Authentication a službami Active Directory Federation Services" 
    description="Toto je stránka vícefaktorového ověřování Azure, která popisuje, jak začít s vícefaktorovým ověřováním Azure a službami AD FS." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>

# Začínáme s ověřováním Azure Multi-Factor Authentication a službami Active Directory Federation Services



<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Pokud má vaše organizace federované místní služby Active Directory s Azure Active Directory pomocí služby AD FS, jsou k dispozici následující 2 možnosti používání ověřování Azure Multi-Factor Authentication.

- Zabezpečení cloudových prostředků pomocí ověřování Azure Multi-Factor Authentication nebo služeb Active Directory Federation Services 
- Zabezpečení cloudu a místních prostředků pomocí ověřování Azure Multi-Factor Authentication Server 

Následující tabulka shrnuje zkušeností ověřování mezi zabezpečením prostředků pomocí Azure Multi-Factor Authentication a AD FS

|Zkušenosti s ověřováním – aplikace založené na prohlížeči|Zkušenosti s ověřováním – aplikace nezaložené na prohlížeči
:------------- | :------------- | :------------- |
Zabezpečení prostředků Azure AD pomocí Azure Multi-Factor Authentication |<li>1. faktor ověřování se provádí místně pomocí služby AD FS.</li> <li>2. faktor je metoda založená na telefonu prováděná pomocí cloudového ověřování.</li>|Koncoví uživatelé mohou k přihlášení používat hesla aplikací.
Zabezpečení prostředků Azure AD pomocí Active Directory Federation Services |<li>1. faktor ověřování se provádí místně pomocí služby AD FS.</li><li>2. faktor je prováděn v místě dodržením deklarace identity.</li>|Koncoví uživatelé mohou k přihlášení používat hesla aplikací.

Upozornění s hesly aplikací pro federované uživatele: 

- Heslo aplikace se ověřuje pomocí cloudového ověřování a tak obchází federace. Federace se aktivně používá pouze při nastavování hesla aplikace.
- Místní nastavení řízení přístupu klienta není dodrženo heslem aplikace.
- U hesla aplikace ztratíte schopnost ověřování přihlašování v místě.
- Zakázání/odstranění účtu může trvat až 3 hodiny pro dirsync, přičemž dojde ke zpoždění zakázání/odstranění hesla aplikace v cloudové identitě.

Informace o nastavení ověřování Azure Multi-Factor Authentication nebo serveru Azure Multi-Factor Authentication se službou AD FS naleznete v následujících tématech:

- [Zabezpečené cloudové prostředky pomocí ověřování Azure Multi-Factor Authentication a AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
- [Zabezpečený cloud a místní prostředky pomocí ověřování Azure Multi-Factor Authentication Server s Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Zabezpečení cloudu a místních prostředků pomocí ověřování Azure Multi-Factor Authentication Server s AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)







 



<!--HONumber=Aug16_HO4-->


