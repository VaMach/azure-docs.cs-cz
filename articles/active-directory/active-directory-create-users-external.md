---
title: "Přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory | Dokumentace Microsoftu"
description: "Vysvětluje, jak ve službě Azure Active Directory přidat nové uživatele nebo změnit informace o uživateli, včetně externích uživatelů a hostů."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 564a04ec-53c1-470b-9ab9-f3db57da0a89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/10/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 399230584d01986dd0f793a6ff8245ef2b4f8fb1
ms.contentlocale: cs-cz
ms.lasthandoff: 07/26/2017

---
# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory"></a>Přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory

Tento článek vysvětluje, jak přidat uživatele z dalších adresářů ve službě Azure Active Directory nebo uživatele z partnerských společností. Informace o přidávání nových uživatelů v organizaci a přidávání uživatelů, kteří mají účty Microsoft, najdete v článku [Přidání nových uživatelů do službě Azure Active Directory](active-directory-create-users.md). 

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. Informace o přidání uživatelů typu host pro spolupráci B2B v centru pro správu Azure AD najdete v tématu [Co je spolupráce Azure AD s B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)

Přidaní uživatelé nemají ve výchozím nastavení oprávnění správce, ale příslušné role jim můžete kdykoli přiřadit.

## <a name="add-a-user"></a>Přidání uživatele
1. Přihlaste se k portálu [Azure Classic](https://manage.windowsazure.com) prostřednictvím účtu, který má k adresáři oprávnění globálního správce.
2. Vyberte **Active Directory** a otevřete svůj adresář.
3. Vyberte kartu **Uživatelé** a na panelu příkazů vyberte **Přidat uživatele**.
4. Na stránce **Informace o uživateli** vyberte v části **Typ uživatele** jedno z následujících:

   * **Uživatel z jiného adresáře služby Azure AD** – přidá do vašeho adresáře uživatele, který pochází z jiného adresáře služby Azure AD. Uživatele z jiného adresáře můžete vybrat pouze v případě, že jste také členem daného adresáře.
   * **Uživatelé z partnerských společností** – pokud chcete do svého adresáře pozvat a autorizovat uživatele z partnerských společností. Další informace najdete v článku [Spolupráce B2B ve službě Azure Active Directory](active-directory-b2b-what-is-azure-ad-b2b.md). Budete muset [nahrát soubor .csv s e-mailovými adresami](active-directory-b2b-references-csv-file-format.md).
5. Na stránce **Profil** uživatele zadejte jeho jméno, příjmení a uživatelské jméno a ze seznamu **Role** vyberte roli uživatele. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md). Určete, jestli u uživatele chcete **Povolit službu Multi-Factor Authentication**.
6. Na stránce **Získat dočasné heslo** vyberte **Vytvořit**.

> [!IMPORTANT]
> Pokud vaše organizace používá více než jednu doménu, měli byste vědět o následujících problémech týkajících se přidávání uživatelských účtů:
>
> * Pokud chcete přidat uživatelské účty s totožným hlavním názvem uživatele (UPN) pro všechny domény, přidejte například geoffgrisso@contoso.onmicrosoft.com **jako první** a **až potom** geoffgrisso@contoso.com.
> * **Nepřidávejte** geoffgrisso@contoso.com předtím, než přidáte geoffgrisso@contoso.onmicrosoft.com.
>

Pokud chcete měnit informace u uživatele, jehož identita je synchronizována s místní službou Active Directory, nemůžete k tomu použít portál Azure Classic. Ke změně informací o uživateli použijte nástroje pro správu vaší místní služby Active Directory.

## <a name="add-external-users"></a>Přidání externích uživatelů
Můžete taky přidat uživatele z jiného adresáře služby Azure AD, do kterého patříte, nebo z partnerských společností tím, že nahrajete soubor .csv. Pokud chcete přidat externího uživatele, vyberte v poli **Typ uživatele** možnost **Uživatel z jiného adresáře služby Microsoft Azure AD** nebo **Uživatelé z partnerských společností**.

Uživatelé těchto typů pocházejí z jiných adresářů a přidají se jako **externí uživatelé**. Externí uživatelé mohou spolupracovat s ostatními uživateli v adresáři, aniž by bylo nutné přidávat nové účty a přihlašovací údaje. Externí uživatelé jsou při přihlašování ověřováni pomocí svých domovských adresářů. Toto ověření platí i pro všechny další adresáře, do kterých byli přidáni.

## <a name="external-user-management-and-limitations"></a>Správa a omezení externích uživatelů
Když do svého adresáře přidáte uživatele z jiného adresáře, bude tento uživatel ve vašem adresáři externím uživatelem. Zobrazované jméno a uživatelské jméno, které se pro externího uživatele ve vašem adresáři použije, se zkopíruje z jeho domovského adresáře. Od této chvíle jsou vlastnosti externího uživatelského účtu zcela nezávislé. Změna vlastnosti uživatele v jeho domovském adresáři se do účtu externího uživatele ve vašem adresáři nepřenese.

Jediné propojení, které mezi těmito dvěma účty existuje, spočívá v tom, že se uživatel vždy ověřuje prostřednictvím svého domovského adresáře nebo účtu Microsoft. To je důvod, proč u externího uživatele nevidíte možnost povolení vícefaktorového ověřování nebo resetování hesla. V současné době se při přihlášení uživatele vyhodnocují pouze zásady ověřování jeho domovského adresáře nebo účtu Microsoft.

> [!NOTE]
> Uživatele pořád můžete zakázat ve svém adresáři a zablokovat mu tak přístup do adresáře.
>
>

Externí uživatel ve vašem adresáři zůstane i v případě, že bude odstraněn ze svého domovského adresáře nebo zruší svůj účet Microsoft. Tento uživatel ale nebude mít přístup k prostředkům ve vašem adresáři, protože se nemůže ověřit prostřednictvím domovského adresáře nebo účtu Microsoft.

### <a name="services-that-currently-support-access-by-azure-ad-external-users"></a>Služby, které aktuálně podporují přístup externích uživatelů služby Azure AD
* **Portál Azure Classic**: Umožňuje externímu uživateli, který je správcem více adresářů, spravovat každý z těchto adresářů.
* **SharePoint Online**: Pokud je povoleno externí sdílení, umožňuje externímu uživateli přístup k autorizovaným prostředkům služby SharePoint Online.
* **Dynamics CRM**: Pokud je uživatel licencován prostřednictvím rozhraní PowerShell, umožňuje externímu uživateli přístup k autorizovaným prostředkům služby Dynamics CRM.
* **Dynamics AX**: Pokud je uživatel licencován prostřednictvím rozhraní PowerShell, umožňuje externímu uživateli přístup k autorizovaným prostředkům služby Dynamics AX. Omezení pro [externí uživatele služby Azure AD](#known-limitations-of-azure-ad-external-users) platí pro externí uživatele v aplikaci Dynamics AX také.

## <a name="next-steps"></a>Další kroky
* [Přidání nových uživatelů do služby Azure Active Directory](active-directory-create-users.md)
* [Správa služby Azure AD](active-directory-administer.md)
* [Správa hesel ve službě Azure AD](active-directory-manage-passwords.md)
* [Správa skupin ve službě Azure AD](active-directory-manage-groups.md)

