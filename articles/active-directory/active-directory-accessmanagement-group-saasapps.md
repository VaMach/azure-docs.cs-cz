---
title: "Pomocí skupiny pro správu přístupu k aplikacím SaaS | Microsoft Docs"
description: "Jak používat skupin v Azure Active Directory Premium nebo Basic přiřazení přístupu k aplikacím SaaS, které jsou integrované s Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 818f4b515926c35078b3118978f3accbf3bbb65b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Použití skupiny ke správě přístupu k aplikacím SaaS
Pomocí Azure Active Directory (Azure AD) s licencí Azure AD Premium nebo Azure AD Basic, můžete použít skupiny přiřazení přístupu k aplikaci SaaS, která je integrovaná s Azure AD. Například pokud chcete přiřadit přístup pro marketingové oddělení používat pět různých aplikace SaaS, je můžete vytvořit skupinu, která obsahuje uživatelé v marketingovém oddělení a potom přidělit této skupině pro těchto pět aplikace SaaS, které jsou vyžadovány z marketingového oddělení. Tímto způsobem můžete ušetřit čas tím Správa členství z marketingového oddělení na jednom místě. Uživatelé pak přiřazené k aplikaci při jsou přidány jako členové skupiny marketing a jejich přiřazení odebrali z aplikace, když jsou odebrány ze skupiny marketing. Tato funkce slouží se stovkami aplikací, které můžete přidat z v galerii aplikací Azure AD.

> [!IMPORTANT]
> Tato funkce slouží pouze po spuštění zkušební verzi Azure AD Premium nebo zakoupení licence Azure AD Premium nebo Azure AD Basic.
> Vnořené členství ve skupinách momentálně není podporované v případě přiřazování k aplikacím podle skupiny.

**Přiřadit přístup pro uživatele nebo skupinu k aplikaci SaaS**

1. V [centra pro správu Azure AD](https://aad.portal.azure.com), vyberte **podnikové aplikace, které**.
2. Vyberte aplikace, které jste přidali v galerii aplikací ho otevřete.
3. Vyberte **uživatelů a skupin**a potom vyberte **přidat uživatele**.
4. Na **přidat přiřazení**, vyberte **uživatelů a skupin** otevřete **uživatelů a skupin** seznamu výběru.
6. Vyberte počet skupin nebo uživatelů tak, jak chcete, pak klikněte nebo klepněte na **vyberte** je přidejte do **přidat přiřazení** seznamu. Můžete také přiřadit role pro uživatele v této fázi.
7. Vyberte **přiřadit** přiřadit uživatele nebo skupiny do aplikace vybrané rozlehlé sítě.

### <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
