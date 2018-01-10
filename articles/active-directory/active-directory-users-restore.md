---
title: "Obnovení odstraněného uživatele v Azure Active Directory | Microsoft Docs"
description: "Jak obnovit odstraněné uživatele, zobrazit obnovitelné uživatele nebo trvale odstranit uživatele v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/08/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: c3b7550c2aea0e8bcb7998e0e8c732894b500403
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="restore-a-deleted-user-in-azure-active-directory"></a>Obnovení odstraněného uživatele v Azure Active Directory

Tento článek obsahuje pokyny k obnovení nebo trvale odstranit dříve odstraněné uživatele. Když odstraníte uživatele ve službě Azure Active Directory (Azure AD), je 30 dní od data odstranění uchovávat Odstraněný uživatel. Během této doby se dají obnovovat uživatel a jeho vlastnosti. 

## <a name="required-permissions"></a>Požadovaná oprávnění
Tato oprávnění jsou dostatečná k obnovení uživatele.

Role  | Oprávnění 
--------- | ---------
Správce společnosti<p>Podpora partnerů úrovně 1<p>Podpora partnerů úrovně 2<p>Správce uživatelských účtů | Můžete obnovit odstranění uživatelé 
Správce společnosti<p>Podpora partnerů úrovně 1<p>Podpora partnerů úrovně 2<p>Správce uživatelských účtů | Může trvale odstranit uživatele

## <a name="how-to-restore-a-deleted-user"></a>Obnovení odstraněného uživatele

Na portálu Azure můžete obnovit odstraněné uživatele i trvale odstranit odstraněný uživatel.

1. V [centra pro správu Azure AD](https://aad.portal.azure.com), vyberte **uživatelů a skupin** &gt; **všichni uživatelé**. 
2. V části **zobrazit**, filtrovat stránku zobrazíte **uživatelé v poslední době odstraňovali**. 
3. Vyberte jeden nebo více odstraněných uživatelů.
4. Vyberte **obnovení uživatele** nebo **trvale odstranit**.

## <a name="next-steps"></a>Další postup
Tyto články poskytují další informace o správu uživatelů Azure Active Directory.

* [Rychlé spuštění: Přidání nebo odstranění uživatelů do Azure Active Directory](add-users-azure-active-directory.md)
* [Správa uživatelských profilů](active-directory-users-profile-azure-portal.md)
* [Přidat uživatele typu Host z jiného adresáře](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Přiřazení uživatele k roli ve službě Azure AD](active-directory-users-assign-role-azure-portal.md)
