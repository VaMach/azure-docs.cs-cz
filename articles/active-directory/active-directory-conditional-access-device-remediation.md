<properties
    pageTitle="Možnosti uživatele při řešení problémů s přístupem k aplikacím v Azure AD s ochranou přístupu podmíněnou zařízením | Microsoft Azure"
    description="Toto téma vám pomůže určit, jestli existuje nápravný postup, jehož provedením získáte přístup k požadované aplikaci."
    services="active-directory"
    keywords="přístup podmíněný zařízením, registrace zařízení, povolení registrace zařízení, registrace zařízení a MDM"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="markvi"/>


# Možnosti uživatele při řešení problémů s přístupem k aplikacím v Azure AD s ochranou přístupu podmíněnou zařízením

Při přístupu k aplikaci, například k Office 365 nebo SharePoint Online, se zobrazila stránka s informací o odepření přístupu.  
Co se s tím dá dělat?

Tento průvodce vám pomůže určit, jestli existuje nápravný postup, jehož provedením získáte přístup k požadované aplikaci.



Na jaké platformě běží vaše zařízení?
Odpověď na tuto otázku vás nasměruje na správnou část tohoto tématu:
 

-   Zařízení s Windows

-   Zařízení s iOS (iPhone nebo iPad)

-   Zařízení s Androidem

## Přístup ze zařízení s Windows

Pokud vaše zařízení používá systém Windows 10, Windows 8.1, Windows 8.0, Windows 7, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2, zvolte příslušnou příčinu označením stránky, která se zobrazila při pokusu o přístup k aplikaci.

### Zařízení není registrované

Pokud není zařízení registrované ve službě Azure Active Directory a aplikace je chráněná zásadami, které rozlišují zařízení, může se zobrazit stránka s následujícím obsahem:

![Scénář](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")

 

Pokud je zařízení připojené k doméně služby Active Directory vaší organizace, můžete vyzkoušet následující postup:

1.  Přihlaste se k Windows pomocí svého pracovního účtu (účet služby Active Directory).

2.  Připojte se k podnikové síti prostřednictvím sítě VPN nebo přímého přístupu.

3.  Po připojení uzamkněte relaci Windows pomocí kláves Windows + L.

4.  Odemkněte relaci Windows zadáním přihlašovacích údajů ke svému pracovnímu účtu.

5.  Počkejte zhruba minutu a zkuste aplikaci znovu spustit.

6.  Pokud se zobrazí stejná stránka, obraťte se na správce a sdělte mu podrobnosti, které získáte po kliknutí na odkaz Podrobnosti.

Pokud není zařízení připojené k doméně a používá Windows 10, máte dvě možnosti: 

1. Spusťte službu Azure AD Join
2. Přidejte svůj pracovní nebo školní účet do Windows. 

Informace o rozdílech mezi těmito dvěma účty najdete v článku [Používání zařízení s Windows 10 na pracovišti](active-directory-azureadjoin-windows10-devices.md).

Pokud chcete spustit službu Azure AD Join, postupujte následovně (neplatí pro Windows Phone):

**Windows 10 Anniversary Update**   

1.  Spusťte aplikaci Nastavení.

2.  Přejděte na Účty a potom na Přístup do práce nebo do školy.

3.  Klikněte na Připojit.

4.  V dolní části stránky zvolte možnost Připojit toto zařízení k Azure AD.

5.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a postupujte podle pokynů až do dokončení.

6.  Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu.

7.  Zkuste aplikaci znovu spustit.




**Windows 10 November 2015 Update**


1.  Spusťte aplikaci Nastavení.

2.  Přejděte na Systém a potom na Informace o systému.
    
3.  Klikněte na Připojit se k Azure AD.

4.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a postupujte podle pokynů až do dokončení.

5.  Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu (účet Azure AD).

6.  Zkuste aplikaci znovu spustit.


Pokud chcete přidat svůj pracovní nebo školní účet, postupujte následovně:

**Windows 10 Anniversary Update**   

1.  Spusťte aplikaci Nastavení.

2.  Přejděte na Účty a potom na Přístup do práce nebo do školy.

3.  Klikněte na Připojit.

4.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a postupujte podle pokynů až do dokončení.

5.  Zkuste aplikaci znovu spustit.    


**Windows 10 November 2015 Update**
    
1.  Spusťte aplikaci Nastavení.
2.  Přejděte na Účty a potom na Vaše účty.
3.  Klikněte na Přidat pracovní nebo školní účet.
4.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a postupujte podle pokynů až do dokončení.
5.  Zkuste aplikaci znovu spustit.

Pokud není zařízení připojené k doméně a používá Windows 8.1, můžete pomocí následujících kroků provést připojení k pracovišti zaregistrovat se do služby Microsoft Intune:

1.  Spusťte nastavení počítače.

2.  Přejděte na Sítě a klikněte na Pracoviště.

3.  Klikněte na Připojit.

4.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a postupujte podle pokynů až do dokončení.

5.  Klikněte na Zapnout.

6.  Počkejte na dokončení.

7.  Zkuste aplikaci znovu spustit.


## Nepodporovaný prohlížeč

Po aplikaci spouštíte z následujících prohlížečů, zobrazí se vám podobná stránka:

1.  Chrome, Firefox nebo jiný prohlížeč kromě prohlížeče Microsoft Edge nebo Microsoft Internet Explorer ve Windows 10 nebo Windows Serveru 2016.

2.  Firefox ve Windows 8.1, Windows 7, Windows Serveru 2012 R2, Windows Serveru 2012 nebo Windows Serveru 2008 R2.
 

![Scénář](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")


Jedinou možností nápravy je použít prohlížeč, který platforma vašeho zařízení podporuje.

## Přístup ze zařízení s iOS

Pokyny pro iPhony a iPady připravujeme.

## Přístup ze zařízení s Androidem

Pokyny pro telefony a tablety s Androidem připravujeme.


## Další kroky

[Podmíněný přístup ke službě Azure Active Directory](active-directory-conditional-access.md)




<!--HONumber=Aug16_HO4-->


