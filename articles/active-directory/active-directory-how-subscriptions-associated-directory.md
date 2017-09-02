---
title: "Jak je předplatné Azure propojeno se službou Azure Active Directory | Dokumentace Microsoftu"
description: "Popisuje přihlášení do služby Microsoft Azure a související problémy, jako je například vztah mezi předplatným služby Azure a službou Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: b2f8cc774b6cc245a4000e5c6924a8bb817707bc
ms.contentlocale: cs-cz
ms.lasthandoff: 08/24/2017

---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Jak je předplatné Azure propojeno se službou Azure Active Directory
V tomto článku najdete informace o vztahu mezi předplatným služby Azure a službou Azure Active Directory (Azure AD).

## <a name="your-azure-subscriptions-relationship-to-azure-ad"></a>Vztah mezi předplatným Azure a službou Azure AD
Vaše předplatné Azure má s Azure AD vztah důvěryhodnosti. To znamená, že adresáři svěřuje ověřování uživatelů, služeb a zařízení. Několik předplatných může důvěřovat stejnému adresáři, ale každé předplatné důvěřuje jenom jednomu adresáři. 

Vztah důvěryhodnosti, který má předplatné s adresářem, se liší od vztahu, který má předplatné se ostatními prostředky v Azure (webové stránky, databáze apod.). Pokud platnost předplatného vyprší, zastaví se i přístup k těmto dalším prostředkům přidruženým k předplatnému. Adresář ale ve službě Azure zůstane a vy k němu můžete přidružit jiné předplatné a pokračovat ve správě uživatelů adresáře.

![Diagram přidružení předplatných](./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png)

Azure AD nefunguje jako ostatní služby v předplatném Azure. Ostatní služby Azure jsou podřízené předplatnému Azure. Ale služba Azure AD není založená na předplatném. Umožňuje přístup k adresářům na základě přihlášeného uživatele.

Všichni uživatelé mají jeden domovský adresář, který je ověřuje, ale mohou být také hosty v dalších adresářích. V Azure AD se vám zobrazí jenom adresáře, jichž je váš uživatelský účet členem. Adresář se také může synchronizovat s místní službou Active Directory.

## <a name="azure-ad-and-cloud-service-subscriptions"></a>Předplatná cloudových služeb a Azure AD
Služba Azure AD poskytuje základní možnosti správy adresáře a identity pro většinu cloudových služeb Microsoftu, včetně těchto:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Službu Azure AD získáte zdarma při registraci libovolné z těchto cloudových služeb Microsoftu. Pokud chcete do adresáře Azure AD přidat další předplatné Azure, je nutné, abyste byli přihlášení pomocí účtu Microsoft. Pokud se například zaregistrujete do Azure pomocí účtu Microsoft a současně se zaregistrujete do jiné cloudové služby Microsoftu pomocí pracovního nebo školního účtu, máte dvě instance Azure AD:
1. Výchozí adresář pro předplatné Azure. K tomuto adresáři (nebo jiným, které vytvoříte) můžete přidat další předplatné Azure, pokud jste přihlášení pomocí účtu Microsoft, protože vás Azure může ověřit.
2. Domovský adresář pro váš pracovní nebo školní účet. Pokud jste přihlášení do Azure pomocí pracovního nebo školního účtu, nemůžete do stávajícího adresáře přidat předplatné Azure, protože Azure nemůže váš pracovní nebo školní účet přímo ověřit. 
 
Další informace o postupu při změně správců pro předplatné Azure najdete v tématu [Přenos vlastnictví předplatného služby Azure na jiný účet](../billing/billing-subscription-transfer.md)

## <a name="suggestions-to-manage-both-a-subscription-and-a-directory"></a>Návrhy pro správu předplatného i adresáře
Správní role pro předplatné služby Azure spravují prostředky, které jsou svázané s předplatným služby Azure. Tato část vysvětluje rozdíly mezi správci předplatného Azure a správci adresáře Azure AD. Správní role a další návrhy pro jejich využití ke správě předplatného jsou popsané v kapitole [Přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md).

Ve výchozím nastavení máte po registraci přiřazenou roli správce služby. Pokud se ostatní potřebují přihlašovat ke službám a používat je v rámci stejného předplatného, můžete je přidat jako spolusprávce. 

Azure AD má ke správě adresáře a funkcím souvisejícím s identitou k dispozici jinou sadu správních rolí. Globální správce adresáře může do adresáře například přidávat uživatele a skupiny, nebo vyžadovat vícefaktorové ověřování pro uživatele. Uživatel, který vytvoří adresář, se přiřadí k roli globálního správce a může ostatním uživatelům přiřazovat správní role. Správní role služby Azure AD jsou také používány dalšími službami, například Office 365 nebo Microsoft Intune. 

Správci předplatného Azure a správci adresáře Azure AD představují dva samostatné koncepty. 
* Správci předplatného Azure mohu spravovat prostředky v Azure a využívat Azure AD na webu Azure Portal (protože samotný web Azure Portal je prostředek Azure). 
* Správci adresáře mohou spravovat vlastnosti v jenom adresáři Azure AD.

Jedna osoba může mít obě role, ale není to nutné. Uživateli lze přiřadit roli globálního správce adresáře, ale nelze ho přiřadit jako správce nebo spolusprávce služeb v předplatném služby Azure. Uživatel, který není správcem předplatného, se může přihlásit k webu Azure Portal, ale nemůže na tomto portálu spravovat adresáře pro toto předplatné. Tento uživatel může spravovat adresáře pomocí jiných nástrojů, jako je Azure AD PowerShell nebo Centrum pro správu Office 365.

## <a name="next-steps"></a>Další kroky
* Další informace o postupu při změně správců pro předplatné Azure najdete v tématu [Přenos vlastnictví předplatného služby Azure na jiný účet](../billing/billing-subscription-transfer.md)
* Další informace o tom, jak se přístup k prostředkům řídí ve službě Microsoft Azure, najdete v části [Principy přístupu k prostředkům ve službě Azure](active-directory-understanding-resource-access.md)
* Další informace o tom, jak přiřadit role ve službě Azure AD, najdete v tématu [Přiřazení rolí správce ve službě Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG

