<properties
    pageTitle="Řešení potíží: Odsud se tam nelze dostat | Microsoft Azure"
    description="Toto téma vám pomůže určit správný postup odstranění problému, jehož provedením získáte přístup k aplikaci."
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
    ms.date="08/23/2016"
    ms.author="markvi"/>


# Řešení potíží: Odsud se tam nelze dostat

Při přístupu k aplikaci, například SharePoint Online, se zobrazila stránka s informací o odepření přístupu.  
Co se s tím dá dělat?

Tento průvodce vám pomůže určit dostupný postup odstranění problému, jehož provedením získáte přístup k aplikaci.



Na jaké platformě běží vaše zařízení?
Odpověď na tuto otázku vás nasměruje na správnou část tohoto tématu:


-   Zařízení s Windows
-   Zařízení s iOS (iPhone nebo iPad)
-   Zařízení s Androidem

## Přístup ze zařízení s Windows

Pokud vaše zařízení běží na systému Windows 10, Windows 8.1, Windows 8.0, Windows 7, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2, zvolte příslušnou příčinu určením stránky, která se zobrazila při pokusu o přístup k aplikaci.

### Zařízení není registrované

Pokud zařízení registrované ve službě Azure Active Directory a aplikace je chráněná zásadami na základě zařízení, může se zobrazit stránka s následujícím obsahem:

![Zprávy „Odsud se tam nelze dostat“ pro neregistrovaná zařízení](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")



Pokud je zařízení připojené k doméně služby Active Directory ve vaší organizaci, můžete vyzkoušet následující postup:

1.  Ujistěte se, že jste přihlášeni k Windows pomocí svého pracovního účtu (účet služby Active Directory).
2.  Připojte se k podnikové síti prostřednictvím sítě VPN nebo technologie DirectAccess.
3.  Jakmile budete připojeni, uzamkněte relaci Windows stisknutím kláves Windows + L.
4.  Odemkněte relaci Windows zadáním přihlašovacích údajů ke svému pracovnímu účtu.
5.  Počkejte zhruba minutu a zkuste aplikaci znovu spustit.
6.  Pokud se zobrazí stejná stránka, obraťte se na svého správce a sdělte mu podrobnosti, které získáte po kliknutí na odkaz **Podrobnosti**.

Pokud zařízení není připojené k doméně a běží na systému Windows 10, máte dvě možnosti:

- Spusťte službu Azure AD Join.
- Přidejte svůj pracovní nebo školní účet do Windows.

Informace o rozdílech mezi těmito dvěma účty najdete v článku [Používání zařízení s Windows 10 na pracovišti](active-directory-azureadjoin-windows10-devices.md).

Chcete-li spustit službu Azure AD Join, postupujte následovně (neplatí pro Windows Phone):

**Windows 10 Anniversary Update**

1.  Otevřete aplikaci **Nastavení**.
2.  Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.
3.  Klikněte na **Připojit**.
4.  V dolní části stránky klikněte na **Připojit toto zařízení k Azure AD**.
5.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování, a postupujte podle pokynů až do dokončení.
6.  Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu.
7.  Zkuste znovu spustit aplikaci.




**Windows 10 November 2015 Update**


1.  Otevřete aplikaci **Nastavení**.
2.  Klikněte na **Systém**  >  **O systému**.
3.  Klikněte na **Připojit se k Azure AD**.
4.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování, a postupujte podle pokynů až do dokončení.
5.  Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu (účet Azure AD).
6.  Zkuste znovu spustit aplikaci.

Pokud chcete přidat svůj pracovní nebo školní účet, postupujte následovně:

**Windows 10 Anniversary Update**

1.  Otevřete aplikaci **Nastavení**.
2.  Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.
3.  Klikněte na **Připojit**.
4.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování, a postupujte podle pokynů až do dokončení.
5.  Zkuste znovu spustit aplikaci.


**Windows 10 November 2015 Update**

1.  Otevřete aplikaci **Nastavení**.
2.  Klikněte na **Účty**  >  **Vaše účty**.
3.  Klikněte na **Přidat pracovní nebo školní účet**.
4.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování, a postupujte podle pokynů až do dokončení.
5.  Zkuste znovu spustit aplikaci.

Pokud zařízení není připojené k doméně a běží na systému Windows 8.1, můžete pomocí následujícího postupu provést Workplace Join (připojení k pracovišti) a zaregistrovat se do Microsoft Intune:

1.  Otevřete **Nastavení počítače**.
2.  Klikněte na **Síť**  >  **Pracoviště**.
3.  Klikněte na **Připojit**.
4.  Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování, a postupujte podle pokynů až do dokončení.
5.  Klikněte na **Zapnout**.
6.  Počkejte na dokončení.
7.  Zkuste znovu spustit aplikaci.


## Nepodporovaný prohlížeč

Podobná stránka se vám zobrazí, pokud aplikaci spouštíte z následujících prohlížečů:

- Chrome, Firefox nebo jiný prohlížeč kromě prohlížeče Microsoft Edge nebo Microsoft Internet Explorer ve Windows 10 nebo Windows Server 2016.
- Firefox ve Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.

![Zpráva „Odsud se tam nelze dostat“ pro nepodporované prohlížeče](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")


Jedinou možností odstranění problému je použít prohlížeč, který aplikace pro platformu vašeho zařízení podporuje.

## Přístup ze zařízení s iOS
Pokyny pro iPhony a iPady připravujeme.

## Přístup ze zařízení s Androidem
Pokyny pro telefony a tablety s Androidem připravujeme.

## Další kroky

[Podmíněný přístup ke službě Azure Active Directory](active-directory-conditional-access.md)



<!--HONumber=ago16_HO5-->


