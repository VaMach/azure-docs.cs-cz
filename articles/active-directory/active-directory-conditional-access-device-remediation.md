---
title: "Řešení potíží s přístupem k Azure Active Directory | Dokumentace Microsoftu"
description: "Zjistěte, co můžete provést při řešení problémů s přístupem k online prostředkům vaší organizace."
services: active-directory
keywords: "přístup podmíněný zařízením, registrace zařízení, povolení registrace zařízení, registrace zařízení a MDM"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/24/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: fbabf6f2e1e588ba509c4da84ab1700b1b5d4f87
ms.openlocfilehash: ad9f9a8c5b370ffa916b9089ef3ce523fe0266c7


---
# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Řešení potíží s přístupem k Azure Active Directory
Pokoušíte se o přístup k intranetu SharePoint Online vaší organizace a zobrazí se vám chybová zpráva „přístup byl odepřen“. Co se s tím dá dělat?


Tento článek popisuje nápravné kroky, které vám můžou pomoct vyřešit problémy s přístupem k online prostředkům vaší organizace.

Pomoc s řešením problémů s přístupem k Azure Active Directory (Azure AD) najdete v příslušné části článku určeného po platformu vašeho zařízení:

* Zařízení s Windows
* Zařízení s iOS (Brzy zde najdete pomoc pro iPhony a iPady.)
* Zařízení s Androidem (Brzy zde najdete pomoc pro telefony a tablety s Androidem.)

## <a name="access-from-a-windows-device"></a>Přístup ze zařízení s Windows
Pokud vaše zařízení používá některou z následujících platforem, vyhledejte v následujících částech chybovou zprávu, která se zobrazí při pokusu o přístup k aplikaci nebo službě:

* Windows 10
* Windows 8.1
* Windows 8
* Windows 7
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Zařízení není registrované
Pokud zařízení není zaregistrované ve službě Azure AD a aplikace je chráněná zásadami, které jsou založené na konkrétním zařízení, může se zobrazit stránka s některou z těchto chybových zpráv:

![Zprávy „Odsud se tam nelze dostat“ pro neregistrovaná zařízení](./media/active-directory-conditional-access-device-remediation/01.png "Scénář")

Pokud je zařízení připojené k doméně služby Active Directory ve vaší organizaci, zkuste tohle:

1. Přihlaste se k Windows pomocí svého pracovního účtu (účtu služby Active Directory).
2. Připojte se k podnikové síti prostřednictvím virtuální privátní sítě (VPN) nebo technologie DirectAccess.
3. Jakmile budete připojeni, uzamkněte relaci Windows stisknutím kláves Windows + L.
4. Odemkněte relaci Windows zadáním přihlašovacích údajů ke svému pracovnímu účtu.
5. Počkejte zhruba minutu a znovu se pokuste o přístup k aplikaci nebo službě.
6. Pokud se zobrazí stejná stránka, obraťte se na svého správce a sdělte mu podrobnosti, které získáte po kliknutí na odkaz **Podrobnosti**.

Pokud zařízení není připojené k doméně a běží na systému Windows 10, máte dvě možnosti:

* Spusťte službu Azure AD Join
* Přidejte svůj pracovní nebo školní účet do Windows.

Informace o rozdílech mezi těmito dvěma možnostmi najdete v článku [Používání zařízení s Windows 10 na pracovišti](active-directory-azureadjoin-windows10-devices.md).

Spusťte službu Azure AD Join, a to pomocí následujících kroků pro platformu vašeho zařízení. (Na telefonech s Windows není Azure AD Join k dispozici.)

**Windows 10 Anniversary Update**

1. Otevřete aplikaci **Nastavení**.
2. Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.
3. Klikněte na **Připojit**.
4. Klikněte na **Připojit toto zařízení k Azure AD**.
5. Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.
6. Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu.
7. Znovu se pokuste o přístup k aplikaci.

**Windows 10 November 2015 Update**

1. Otevřete aplikaci **Nastavení**.
2. Klikněte na **Systém**  >  **O systému**.
3. Klikněte na **Připojit se k Azure AD**.
4. Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.
5. Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu (účtu Azure AD).
6. Znovu se pokuste o přístup k aplikaci.

Pokud chcete přidat svůj pracovní nebo školní účet, postupujte takto:

**Windows 10 Anniversary Update**

1. Otevřete aplikaci **Nastavení**.
2. Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.
3. Klikněte na **Připojit**.
4. Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.
5. Znovu se pokuste o přístup k aplikaci.

**Windows 10 November 2015 Update**

1. Otevřete aplikaci **Nastavení**.
2. Klikněte na **Účty**  >  **Vaše účty**.
3. Klikněte na **Přidat pracovní nebo školní účet**.
4. Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.
5. Znovu se pokuste o přístup k aplikaci.

Pokud zařízení není připojené k doméně a běží na systému Windows 8.1, můžete pomocí následujících kroků provést Workplace Join (připojení k pracovišti) a zaregistrovat se do Microsoft Intune:

1. Otevřete **Nastavení počítače**.
2. Klikněte na **Síť**  >  **Pracoviště**.
3. Klikněte na **Připojit**.
4. Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.
5. Klikněte na **Zapnout**.
6. Znovu se pokuste o přístup k aplikaci.

### <a name="browser-is-not-supported"></a>Nepodporovaný prohlížeč
Přístup vám může být odepřen, pokud se o přístup k aplikaci nebo službě pokoušíte pomocí některého z těchto prohlížečů:

* Chrome, Firefox nebo jiný prohlížeč než Microsoft Edge nebo Microsoft Internet Explorer ve Windows 10 nebo Windows Serveru 2016
* Firefox ve Windows 8.1, Windows 7, Windows Serveru 2012 R2, Windows Serveru 2012 nebo Windows Serveru 2008 R2

Uvidíte chybovou stránku podobnou této:

![Zpráva „Odsud se tam nelze dostat“ pro nepodporované prohlížeče](./media/active-directory-conditional-access-device-remediation/02.png "Scénář")

Jedinou možností odstranění problému je použít prohlížeč, který aplikace pro platformu vašeho zařízení podporuje.

## <a name="next-steps"></a>Další kroky
[Podmíněný přístup ke službě Azure Active Directory](active-directory-conditional-access.md)




<!--HONumber=Jan17_HO4-->


