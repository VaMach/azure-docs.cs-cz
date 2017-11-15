---
title: "Přidání nebo odstranění uživatelů ve službě Azure Active Directory | Microsoft Docs"
description: "Vysvětluje, jak přidat nové uživatele nebo odstranit existující uživatele v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 0e46ff82c4177de6b33e5df8714318bff83fbb34
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a>Rychlé spuštění: Přidání nových uživatelů do služby Azure Active Directory
Tento článek vysvětluje, jak přidat nové uživatele ve vaší organizaci ve službě Azure Active Directory (Azure AD) jednoho současně pomocí portálu Azure nebo synchronizace účtu místního systému Windows Server AD uživatelská data. 

## <a name="add-cloud-based-users"></a>Přidání cloudových uživatelů
1. Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **Azure Active Directory** a potom **uživatelů a skupin**.
3. Na **uživatelů a skupin**, vyberte **všichni uživatelé**a potom vyberte **nového uživatele**.
   ![Použití příkazu Přidat](./media/add-users-azure-active-directory/add-user.png)
4. Zadejte podrobnosti pro uživatele, jako například **název** a **uživatelské jméno**. Část názvu domény uživatelského jména musí buď musí být počáteční výchozí domény název "[název domény].onmicrosoft.com" nebo ověřené, nefederovaných [vlastní název domény](add-custom-domain.md) například "contoso.com".
5. Zkopírujte nebo jinak Poznámka: hesla generovaného uživatele tak, aby ji mohli nabídnout uživatele po dokončení tohoto procesu.
6. Volitelně můžete otevřít a vyplňte informace v **profil**, **skupiny**, nebo **Directory role** pro uživatele. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles-azure-portal.md).
7. Na **uživatele**, vyberte **vytvořit**.
8. Bezpečně distribuujte vygenerované heslo pro nového uživatele tak, aby uživatel moct přihlásit.

> [!TIP]
> Můžete také synchronizovat data uživatelských účtů z místního systému Windows Server AD. Řešení společnosti Microsoft identity span místní a cloudové schopnosti, vytváření identitu jednoho uživatele pro ověřování a autorizaci pro všechny prostředky, bez ohledu na umístění. Říkáme toto hybridní identita. [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) slouží k integraci místních adresářů se službou Azure Active Directory pro hybridní identity scénáře. To umožní poskytovat společnou identitu pro uživatele pro aplikace Office 365, Azure a SaaS integrované s Azure AD. 

## <a name="delete-users-from-azure-ad"></a>Odstranit uživatele z Azure AD
1. Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **uživatelů a skupin**.
3. Na **uživatelů a skupin** okně vyberte uživatele, kterého chcete odstranit ze seznamu. 
4. V okně pro vybraného uživatele, vyberte **přehled**a potom na panelu příkazů vyberte **odstranit**.
   ![Použití příkazu Přidat](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Další informace 
* [Přidat uživatele typu Host z jiného adresáře](active-directory-b2b-what-is-azure-ad-b2b.md) 

* [Přiřazení uživatele k roli ve službě Azure AD](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a>Další kroky
V tento rychlý start když jste se naučili postup přidání nových uživatelů do Azure AD Premium. 

Na následující odkaz můžete použít k vytvoření nového uživatele ve službě Azure AD na portálu Azure.

> [!div class="nextstepaction"]
> [Přidání uživatelů do Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 