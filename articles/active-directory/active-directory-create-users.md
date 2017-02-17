---
title: "Přidání nových uživatelů do služby Azure Active Directory | Dokumentace Microsoftu"
description: "Vysvětluje, jak ve službě Azure Active Directory přidat nové uživatele nebo změnit informace o uživatelích."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/10/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 8f171456f74925eba16ebd6f56ced558a11f9d99
ms.openlocfilehash: 6c366a2b7e5017bbe67cf1dbefc0baec6cf1aed3


---
# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Přidání nových uživatelů nebo uživatelů s účty Microsoft do služby Azure Active Directory
Vyplňte svůj adresář uživateli, které nově přidáte. Tento článek vysvětluje, jak pro vaši organizaci přidat nové uživatele a jak přidat uživatele, kteří mají účty Microsoft. Další informace o přidávání uživatelů z dalších adresářů ve službě Azure Active Directory nebo přidávání uživatelů z partnerských společností najdete v článku o [přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory](active-directory-create-users-external.md). Přidaní uživatelé nemají ve výchozím nastavení oprávnění správce, ale příslušné role jim můžete kdykoli přiřadit.

## <a name="add-a-user"></a>Přidání uživatele
1. Přihlaste se k portálu [Azure Classic](https://manage.windowsazure.com) prostřednictvím účtu, který má k adresáři oprávnění globálního správce.
2. Vyberte **Active Directory** a potom vyberte název adresáře své organizace.
3. Vyberte kartu **Uživatelé** a na panelu příkazů vyberte **Přidat uživatele**.
4. Na stránce **Informace o uživateli** vyberte v části **Typ uživatele** jedno z následujících:

   * **Nový uživatel v organizaci** – přidá do vašeho adresáře nový uživatelský účet.
   * **Uživatel s existujícím účtem Microsoft** – přidá do vašeho adresáře existující účet Microsoft uživatele (například účet Outlook).
5. V závislosti na tom, jaký **Typ uživatele** jste vybrali, zadejte buď uživatelské jméno (pro nového uživatele), nebo e-mailovou adresu (pro uživatele s účtem Microsoft).
6. Na stránce **Profil** uživatele zadejte jeho jméno, příjmení a uživatelské jméno a ze seznamu **Role** vyberte roli uživatele. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md). Určete, jestli u uživatele chcete **Povolit službu Multi-Factor Authentication**.
7. Na stránce **Získat dočasné heslo** vyberte **Vytvořit**.

> [!IMPORTANT]
> Pokud vaše organizace používá více než jednu doménu, měli byste vědět o následujících problémech týkajících se přidávání uživatelských účtů:
>
> * Pokud chcete přidat uživatelské účty s totožným hlavním názvem uživatele (UPN) pro všechny domény, přidejte například geoffgrisso@contoso.onmicrosoft.com, **jako první** a **až potom** geoffgrisso@contoso.com.
> * **Nepřidávejte** geoffgrisso@contoso.com předtím, než přidáte geoffgrisso@contoso.onmicrosoft.com. Správné pořadí je důležité a náprava chyby může být náročná.
>
>

## <a name="change-user-information"></a>Změna informací o uživateli
S výjimkou ID objektu můžete změnit jakýkoli atribut uživatele.

1. Otevřete svůj adresář.
2. Vyberte kartu **Uživatelé** a pak vyberte zobrazované jméno uživatele, jehož informace chcete změnit.
3. Proveďte změny a klikněte na **Uložit**.

Pokud je uživatel, jehož informace chcete změnit, synchronizovaný s místní službou Active Directory, nemůžete tento postup použít. Ke změně informací o uživateli použijte nástroje pro správu vaší místní služby Active Directory.

## <a name="guest-user-management-and-limitations"></a>Správa a omezení uživatele typu host
Účty hostů jsou uživatelé z jiných adresářů, kteří byli do vašeho adresáře pozváni a mají přístup k dokumentům SharePointu, aplikacím a dalším prostředkům služby Azure. Účet hosta má ve vašem adresáři základní atribut UserType (Typ uživatele) nastavený na hodnotu Guest (Host). Běžní uživatelé (konkrétně členové vašeho adresáře) mají atribut UserType nastavený na hodnotu Member (Člen).

Hosté mají ve vašem adresáři omezená oprávnění. Hostům jejich oprávnění neumožňují získávat podrobnější informace o ostatních uživatelích v adresáři. Uživatelé typu host však mohou interagovat s uživateli a skupinami přiřazenými k prostředkům, na kterých pracují. Uživatel typu host může:

* Vidět ostatní uživatele a skupiny přidružené k předplatnému Azure, ke kterému je přiřazený
* Vidět členy skupin, do kterých patří
* Vyhledávat ostatní uživatele v adresáři v případě, že zná jejich úplné e-mailové adresy
* Vidět omezenou sadu atributů uživatelů, které vyhledává – omezeno na zobrazované jméno, e-mailovou adresu, hlavní název uživatele (UPN) a miniaturu fotografie
* Získat seznam ověřených domén v adresáři
* Dávat souhlas aplikacím, udělovat jim stejným přístup, jako mají členové vašeho adresáře

## <a name="set-guest-user-access-policies"></a>Nastavení zásad přístupu uživatele typu host
Karta **Konfigurace** v adresáři zahrnuje i možnosti řízení přístupu uživatelů typu host. Tyto možnosti může změnit pouze globální správce adresáře prostřednictvím portálu Azure Classic. V současné době není k dispozici žádná metoda změny pomocí PowerShellu ani rozhraní API.

Pokud chcete na portálu Azure Classic otevřít kartu **Konfigurace**, vyberte **Active Directory** a pak vyberte název adresáře.

![Karta Konfigurace ve službě Azure Active Directory][1]

Potom můžete upravit možnosti řízení přístupu pro uživatele typu host.

![možnosti řízení přístupu pro uživatele typu host][2]

## <a name="whats-next"></a>Kam dál
* [Přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory](active-directory-create-users-external.md)
* [Správa služby Azure AD](active-directory-administer.md)
* [Správa hesel ve službě Azure AD](active-directory-manage-passwords.md)
* [Správa skupin ve službě Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png



<!--HONumber=Feb17_HO2-->


