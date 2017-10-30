---
title: "Přidání existujícího předplatného Azure do adresáře služby Azure AD | Dokumentace Microsoftu"
description: "Přidání existujícího předplatného do adresáře služby Azure AD"
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
ms.date: 10/17/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: abf207a3ceec708a828170936f7dc7948ccf34a9
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="how-to-add-an-azure-subscription-to-azure-active-directory"></a>Přidání předplatného Azure do služby Azure Active Directory
V tomto článku najdete informace o vztahu mezi předplatným Azure a službou Azure Active Directory (Azure AD) a zjistíte, jak do svého adresáře služby Azure AD přidat existující předplatné. Vaše předplatné Azure má s Azure AD vztah důvěryhodnosti. To znamená, že adresáři svěřuje ověřování uživatelů, služeb a zařízení. Několik předplatných může důvěřovat stejnému adresáři, ale každé předplatné důvěřuje jenom jednomu adresáři. 

Vztah důvěryhodnosti, který má předplatné s adresářem, se liší od vztahu, který má předplatné se ostatními prostředky v Azure (webové stránky, databáze apod.). Pokud platnost předplatného vyprší, zastaví se i přístup k těmto dalším prostředkům přidruženým k předplatnému. Adresář služby Azure AD ale v Azure zůstane a vy k němu můžete přidružit jiné předplatné a spravovat adresář pomocí tohoto nového předplatného.

Všichni uživatelé mají jeden domovský adresář, který je ověřuje, ale mohou být také hosty v dalších adresářích. Ve službě Azure AD se vám zobrazí adresáře, kterých je váš uživatelský účet členem nebo hostem.

## <a name="to-add-an-existing-subscription-to-your-azure-ad-directory"></a>Přidání existujícího předplatného do adresáře služby Azure AD
Musíte se přihlásit pomocí účtu, který existuje v aktuálním adresáři, ke kterému je předplatné přidruženo, i v adresáři, do kterého chcete předplatné přidat. 

1. Přihlaste se k [Centru účtů Azure](https://account.azure.com/Subscriptions) pomocí účtu, který je správcem účtu předplatného, jehož vlastnictví chcete převést.
2. Zkontrolujte, že uživatel, který se má stát vlastníkem předplatného, je v cílovém adresáři.
3. Klikněte na **Převod vlastnictví předplatného**.
4. Zadejte příjemce. Příjemce automaticky dostane e-mail s odkazem pro akceptaci.
5. Příjemce klikne na odkaz a postupuje podle pokynů, včetně zadání platebních údajů. Pokud je příjemce úspěšný, předplatné je převedeno. 
6. Výchozím adresářem předplatného se stane adresář, ve kterém je příslušný uživatel.

Další informace najdete v popisu [přenosu vlastnictví předplatného Azure na jiný účet](../billing/billing-subscription-transfer.md).

## <a name="next-steps"></a>Další kroky
* Další informace o postupu při změně správců pro předplatné Azure najdete v tématu [Přenos vlastnictví předplatného služby Azure na jiný účet](../billing/billing-subscription-transfer.md)
* Další informace o tom, jak se přístup k prostředkům řídí ve službě Microsoft Azure, najdete v části [Principy přístupu k prostředkům ve službě Azure](active-directory-understanding-resource-access.md)
* Další informace o tom, jak přiřadit role ve službě Azure AD, najdete v tématu [Přiřazení rolí správce ve službě Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
