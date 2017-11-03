---
title: "Nakonfigurujte zásady podmíněného přístupu na základě zařízení služby Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat zásady podmíněného přístupu na základě zařízení služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a57edd30975ec0e943fd84b2c66137d328a89b8b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Nakonfigurujte zásady podmíněného přístupu na základě zařízení služby Azure Active Directory

S [podmíněného přístupu Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), můžete upřesnit jak oprávněným uživatelům můžete přístup k prostředkům. Například můžete omezit přístup k určitým prostředkům důvěryhodná zařízení. Zásady podmíněného přístupu, která vyžaduje důvěryhodné zařízení je také označován jako zásady podmíněného přístupu podle zařízení.

Toto téma poskytuje informace o tom, jak nakonfigurovat zásady podmíněného přístupu na základě zařízení pro aplikace Azure AD připojen. 


## <a name="before-you-begin"></a>Než začnete

Podmíněný přístup využívající zařízení ties **podmíněného přístupu Azure AD** a **správy zařízení služby Azure AD společně**. Pokud nejste obeznámeni s jedním z těchto oblastí ještě, měli byste si přečíst v následujících tématech, nejdřív:

- **[Podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  – Toto téma poskytuje koncepční přehled podmíněného přístupu a souvisejících technologiím.

- **[Úvod do správy zařízení v Azure Active Directory](device-management-introduction.md)**  – Toto téma poskytuje přehled různých možností budete muset připojit zařízení s Azure AD. 


## <a name="trusted-devices"></a>Důvěryhodná zařízení

První mobilní, cloudové první světě Azure Active Directory umožňuje jednotné přihlašování k zařízení, aplikacím a službám odkudkoli. Pro některé prostředky ve vašem prostředí, udělení přístupu ti správní uživatelé nemusí být dostatečně funkční. Kromě ti správní uživatelé může také vyžadovat důvěryhodné zařízení, který se má použít pro přístup k prostředkům. Ve vašem prostředí, můžete definovat, co je důvěryhodné zařízení na základě následující součásti:

- [Platformy zařízení](active-directory-conditional-access-azure-portal.md#device-platforms) na zařízení
- Jestli je zařízení kompatibilní
- Jestli je zařízení připojené k doméně 

[Platformy zařízení](active-directory-conditional-access-azure-portal.md#device-platforms) je charakterizovaná operačního systému, který běží na vašem zařízení. V zásadách podmíněného přístupu na základě zařízení můžete omezit přístup k určitým prostředkům specifické platformy zařízení.



V zásadách podmíněného přístupu podle zařízení můžete vyžadovat důvěryhodných zařízení označen jako kompatibilní.

![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/24.png)

Zařízení může být označen jako kompatibilní v adresáři podle:

- Intune 
- Mobilní zařízení třetích stran spravováno systém, který spravuje zařízení s Windows 10 prostřednictvím integrace Azure AD 
 
  

Jenom zařízení, které jsou připojené ke službě Azure AD může být označen jako kompatibilní. Pro připojení zařízení k Azure Active Directory, máte následující možnosti: 

- Zaregistrovat Azure AD
- Připojené k Azure AD
- Hybridní připojený k Azure AD

    ![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/26.png)

Pokud budete mít místní službě Active Directory (AD) nároky, můžete zvážit zařízení, které nejsou připojené ke službě Azure AD ale připojený do služby AD jako důvěryhodný.

![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a>Další kroky

Před konfigurací zásad podmíněného přístupu na základě zařízení ve vašem prostředí, měli byste podniknout podívejte se na [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md).

