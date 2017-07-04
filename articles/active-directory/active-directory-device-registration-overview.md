---
title: "Registrace zařízení ve službě Azure Active Directory – přehled | Dokumentace Microsoftu"
description: "Registrace zařízení služby Azure Active Directory je základem pro scénáře přístupu podmíněného zařízením. Když je zařízení registrováno, poskytne mu registrace zařízení služby Azure Active Directory identitu, která se používá k ověření zařízení při přihlášení uživatele."
services: active-directory
keywords: "registrace zařízení, povolení registrace zařízení, registrace zařízení a MDM"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 1e92c1a2-01b8-4225-950b-373cd601b035
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: d19956e4964f57251f51eb8ffe5041c6a49da1a7
ms.contentlocale: cs-cz
ms.lasthandoff: 03/10/2017


---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Začínáme s registrací zařízení ve službě Azure Active Directory
Registrace zařízení služby Azure Active Directory je základem pro scénáře přístupu podmíněného zařízením. Když je zařízení registrováno, poskytne mu registrace zařízení služby Azure Active Directory identitu, která se používá k ověření zařízení při přihlášení uživatele. Ověřené zařízení a jeho atributy pak lze použít k vynucení zásad podmíněného přístupu u aplikací, které jsou hostovány na cloudu a v místním úložišti.

Při kombinaci s řešením správy mobilních zařízení (MDM), jako je například Microsoft Intune, budou atributy zařízení ve službě Azure Active Directory aktualizovány o další informace o zařízení. To vám umožňuje vytvořit pravidla podmíněného přístupu, která vynucují, aby přístup měla pouze taková zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů.  Další informace o registraci zařízení v Microsoft Intune najdete v tématu [Registrace zařízení pro správu v Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Scénáře pro registraci zařízení ve službě Azure Active Directory
Registrace zařízení služby Azure Active Directory zahrnuje podporu pro zařízení se systémy iOS, Android a Windows. Jednotlivé scénáře použití registrace zařízení služby Azure AD mohou mít konkrétněji určené požadavky a podporované platformy. Jedná se o následující scénáře:

* **Podmíněný přístup k aplikacím, které jsou hostovány na místním úložišti**: Registrovaná zařízení se zásadami přístupu můžete použít u aplikací, které jsou konfigurovány k použití služby AD FS a systému Windows Server 2012 R2. Další informace o nastavení podmíněného přístupu pro místní úložiště naleznete v tématu [Nastavení místního podmíněného přístupu pomocí registrace zařízení služby Azure Active Directory](active-directory-device-registration-on-premises-setup.md).
* **Podmíněný přístup pro aplikace služby Office 365 s Microsoft Intune**: Správci IT mohou poskytnout zásady podmíněného přístupu zařízení, aby zabezpečili podnikové prostředky a zároveň umožnili pracovníkům s vhodnými zařízeními přístup ke službám. Další informace najdete v tématu [Zásady podmíněného přístupu zařízení pro služby Office 365](active-directory-conditional-access-device-policies.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Nastavení registrace zařízení ve službě Azure Active Directory
Na webu Azure Portal musíte povolit registraci zařízení služby Azure AD, aby mohla mobilní zařízení zjistit službu pomocí známých záznamů DNS. Záznamy DNS vaší společnosti musíte nakonfigurovat tak, aby zařízení se systémy Windows 10, Windows 8.1, Windows 7, Android a iOS mohla službu zjistit a použít.
Registrovaná zařízení si můžete zobrazit a povolit nebo zakázat prostřednictvím portálu správce služby Azure Active Directory.

> [!NOTE]
> Nejnovější pokyny k nastavení automatické registrace zařízení najdete v tématu [Nastavení automatické registrace zařízení s Windows připojených k doméně pomocí služby Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
> 
> 

### <a name="enable-azure-active-directory-device-registration-service"></a>Povolení registrace zařízení ve službě Azure Active Directory
1. Přihlaste se k portálu Microsoft Azure jako správce.
2. V levém podokně vyberte **Active Directory**.
3. Na kartě **Adresář** vyberte adresář.
4. Vyberte kartu **Konfigurace**.
5. Přejděte na oddíl **Zařízení**.
6. V poli **UŽIVATELÉ, KTEŘÍ MOHOU PŘIPOJIT ZAŘÍZENÍ K PRACOVIŠTI** vyberte **VŠICHNI**.
7. Zadejte maximální počet zařízení, která chcete povolit pro jednoho uživatele.

> [!NOTE]
> Registrace ke službám Microsoft Intune nebo Správa mobilních zařízení (MDM) pro Office 365 vyžaduje připojení k pracovišti. Pokud jste nakonfigurovali některou z těchto služeb, je zvolena možnost VŠICHNI a tlačítko ŽÁDNÍ je deaktivováno.
> 
> 

Ve výchozím nastavení služby není povoleno dvoufaktorové ověřování. Dvoufaktorové ověřování ale doporučujeme při registraci zařízení.

* Předtím, než pro tuto službu vyžádáte dvoufaktorové ověřování, musíte nakonfigurovat poskytovatele dvoufaktorového ověřování ve službě Azure Active Directory a uživatelské účty pro službu Multi-Factor Authentication. Více informací naleznete v článku [Přidání služby Multi-Factor Authentication ve službě Azure Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).
* Pokud používáte službu AD FS a systém Windows Server 2012 R2, musíte modul dvoufaktorového ověřování nakonfigurovat ve službě AD FS. Více informací naleznete v článku [Použití služby Multi-Factor Authentication ve službě Active Directory Federation Services](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Konfigurace zjišťování registrace zařízení ve službě Azure Active Directory
Zařízení se systémy Windows 7 a Windows 8.1 bude registrace zařízení zjišťovat tak, že zkombinují název uživatelského účtu se známým názvem serveru registrace zařízení.

Je nutné vytvořit záznam DNS CNAME, který odkazuje na záznam A přidružený k vaší službou registrace zařízení služby Azure Active Directory. Záznam CNAME musí použít známou předponu enterpriseregistration, za kterou následuje přípona UPN, kterou používají uživatelské účty ve vaší organizaci. Pokud vaše organizace používá více přípon UPN, je nutné v záznamech DNS vytvořit více záznamů CNAME.

Pokud například v organizaci používáte dvě přípony UPN s názvy @contoso.com a @region.contoso.com, musíte vytvořit následující záznamy DNS.

| Záznam | Typ | Adresa |
| --- | --- | --- |
| enterpriseregistration.contoso.com |CNAME |enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com |CNAME |enterpriseregistration.windows.net |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Zobrazení a správa objektů zařízení ve službě Azure Active Directory
1. Pomocí portálu správce Azure můžete zařízení zobrazit, zablokovat a odblokovat. Zařízení, které je zablokováno, nebude mít přístup k aplikacím, které jsou nakonfigurovány tak, aby k nim měla přístup pouze registrovaná zařízení.
2. Přihlaste se na webu Microsoft Azure Portal jako správce.
3. V levém podokně vyberte **Active Directory**.
4. Vyberte svůj adresář.
5. Vyberte kartu **Uživatelé**. Vyberte uživatele, jehož zařízení si chcete zobrazit.
6. Vyberte kartu **Zařízení**.
7. Z rozevírací nabídky vyberte **Registrovaná zařízení**.
8. Zde si můžete zobrazit, zablokovat nebo odblokovat zařízení registrovaná uživateli.

## <a name="additional-topics"></a>Další témata
Zařízení se systémy Windows 7 a Windows 8.1 připojená k doméně můžete zaregistrovat pomocí registrace zařízení služby Azure AD. Následující témata poskytují další informace o požadavcích a krocích potřebných ke konfiguraci registrace zařízení u zařízení s Windows 7 a Windows 8.1.

* [Automatická registrace zařízení ve službě Azure Active Directory u zařízení se systémem Windows připojených k doméně](active-directory-conditional-access-automatic-device-registration.md)
* [Automatická registrace zařízení ve službě Azure Active Directory u zařízení se systémem Windows 10 připojených k doméně](active-directory-azureadjoin-devices-group-policy.md)


