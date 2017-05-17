---
title: "Nastavení služby Azure AD Join pro uživatele | Dokumentace Microsoftu"
description: "Vysvětluje, jak mohou správci nastavit službu Azure AD Join pro místní adresář a registraci zařízení."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 0480c4f89b6036897bc8d7eff81b56262ea8806c
ms.openlocfilehash: e4e0fa77552c4df2ea5bb9ddae916e7c661824d1
ms.contentlocale: cs-cz
ms.lasthandoff: 02/23/2017


---
# <a name="setting-up-azure-ad-join-in-your-organization"></a>Nastavení služby Azure AD Join v organizaci
Předtím, než budete moct nastavit službu Azure Active Directory Join (Azure AD Join), musíte synchronizovat místní adresáře uživatelů s cloudem nebo ručně vytvořit spravované účty v Azure AD.

Podrobný návod, jak synchronizovat místní uživatele se službou Azure AD, najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

Pokud vás zajímá, jak ve službě Azure AD ručně vytvořit a spravovat uživatele, podívejte se na článek o [správě uživatelů ve službě Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Nastavení registrace zařízení
1. Přihlaste se do portálu Azure v roli správce.
2. V levém podokně vyberte **Active Directory**.
3. Na kartě **Adresář** vyberte adresář.
4. Vyberte kartu **Konfigurace**.
5. Přejděte do části **Zařízení**.
6. Na kartě **Zařízení** nastavte následující:  
   * **MAXIMÁLNÍ POČET ZAŘÍZENÍ NA UŽIVATELE**: Nastavte maximální počet zařízení, která uživatel ve službě Azure AD může mít.  Pokud uživatel dosáhne této kvóty, nebude už moct přidávat další zařízení, dokud neodebere některá z existujících.
   * **PŘI PŘIPOJOVÁNÍ ZAŘÍZENÍ VYŽADOVAT VÍCEFAKTOROVÉ OVĚŘENÍ**: Nastavte, jestli musí uživatelé pro připojení zařízení poskytnout druhý ověřovací faktor. Další informace o ověřování Azure Multi-Factor Authentication najdete v článku [Začínáme s ověřováním Azure Multi-Factor Authentication v cloudu](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).
   * **UŽIVATELÉ, KTEŘÍ MOHOU PŘIPOJOVAT ZAŘÍZENÍ KE SLUŽBĚ AZURE AD**: Vyberte uživatele a skupiny, kteří mají povoleno připojovat zařízení ke službě Azure AD.
   * **DALŠÍ SPRÁVCI PŘIPOJENÝCH ZAŘÍZENÍ VE SLUŽBĚ AZURE AD**: U verzí Azure AD Premium a Enterprise Mobility Suite (EMS) můžete nastavit, kteří uživatelé budou mít u zařízení práva místního správce. Globální správci a vlastníci zařízení mají oprávnění místního správce automaticky.

<center>![Nastavení registrace zařízení](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center>

Jakmile pro uživatele nastavíte službu Azure AD Join, budou se moct ke službě Azure AD připojovat prostřednictvím svých firemních nebo osobních zařízení.

Pokud chcete uživatelům umožnit, aby si nastavili službu Azure AD Join, můžete použít následující tři scénáře:

* Uživatelé připojí zařízení, které vlastní firma, ke službě Azure AD přímo.
* Uživatelé připojí zařízení, které vlastní firma, k místní doméně služby Active Directory a potom rozšíří připojení zařízení na službu Azure AD.
* Uživatelé na svá osobní zařízení přidají pracovní nebo školní účty.

## <a name="additional-information"></a>Další informace
* [Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)


