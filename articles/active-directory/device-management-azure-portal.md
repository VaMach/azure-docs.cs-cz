---
title: "Správa zařízení pomocí Azure portal – preview | Microsoft Docs"
description: "Další informace o použití portálu Azure ke správě zařízení."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 4b46e1627a229b0649d9ccd2550cd28fda9849f8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="managing-devices-using-the-azure-portal---preview"></a>Správa zařízení pomocí Azure portal – náhled

>[!NOTE]
>Tato funkce je aktuálně ve verzi public preview. Připravte se na obnovit nebo odeberte všechny změny. Tato funkce je k dispozici v libovolné předplatné služby Azure Active Directory (Azure AD) verzi public Preview. Když je tato funkce obecně dostupná, může vyžadovat některé aspekty funkce však předplatné služby Azure Active Directory premium.


Se správou zařízení ve službě Azure Active Directory (Azure AD) můžete zajistit, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů. 

V tomto tématu:

- Předpokládá, že jste obeznámeni s [Úvod do správy zařízení v Azure Active Directory](device-management-introduction.md)

- Poskytuje informace o správě zařízení pomocí portálu Azure


Ke správě zařízení na portálu Azure, je třeba kliknout na **zařízení** v **spravovat** části **Azure Active Directory** okno.

![Spravovat zařízení s Intune](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a>Konfigurace nastavení zařízení

Ke správě zařízení pomocí portálu Azure, musí být buď zaregistrované, nebo připojený k Azure AD. Jako správce můžete optimalizovat proces registrace a připojení zařízení tak, že nakonfigurujete nastavení zařízení.

![Spravovat zařízení s Intune](./media/device-management-azure-portal/22.png)


V okně Nastavení zařízení můžete konfigurovat:

- **Uživatelé mohou připojit zařízení ke službě Azure AD** – toto nastavení umožňuje vybrat uživatele, kteří mohou připojit zařízení ke službě Azure AD. Výchozí hodnota je **všechny**.

- **Zařízení připojená k další místní správci v Azure AD** – můžete vybrat uživatele, kteří jsou udělena práva místního správce v zařízení. Jsou přidáni uživatelé tady přidat *Správci zařízení* role ve službě Azure AD. Globální správci ve službě Azure AD a vlastníci zařízení jsou udělena práva místního správce ve výchozím nastavení. Tato možnost je k dispozici prostřednictvím produkty, jako je například Azure AD Premium nebo Enterprise Mobility Suite (EMS) funkce edice premium. 

- **Uživatelé mohou registrovat svá zařízení s Azure AD** -je nutné nakonfigurovat tato nastavení mají zařízení zaregistrovat u služby Azure AD. Pokud vyberete **žádné**, zařízení není povoleno zaregistrovat, které nejsou připojené k Azure AD nebo hybridní připojený k Azure AD. Registrace s Microsoft Intune nebo správy mobilních zařízení (MDM) pro Office 365 vyžaduje registraci. Pokud jste nakonfigurovali některou z těchto služeb **všechny** je vybraná a **NONE** není k dispozici...

- **Vyžadovat vícefaktorového ověřování pro připojení zařízení** – můžete zvolit, jestli uživatelé jsou nutné k zajištění druhý faktor ověřování pro připojení zařízení do služby Azure AD. Výchozí hodnota je **ne**. Doporučujeme, abyste při registraci zařízení, které vyžadují služby Multi-Factor authentication. Než povolíte službu Multi-Factor authentication pro tuto službu, je nutné zajistit, že aplikace Multi-Factor authentication je nakonfigurován pro uživatele, kteří registrovat svá zařízení. Další informace o službám různých Azure Multi-Factor authentication, naleznete v části [Začínáme s Azure Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **Maximální počet zařízení** – toto nastavení umožňuje vybrat maximální počet zařízení, které uživatel může mít ve službě Azure AD. Pokud uživatel dosáhne této kvóty, budou se moct přidávat další zařízení, dokud jeden nebo více existující zařízení se odeberou. Uvozovky zařízení se počítá pro všechna zařízení, které jsou připojené k Azure AD nebo dnes zaregistrovat Azure AD. Výchozí hodnota je **20**.

- **Uživatelé můžou synchronizovat nastavení a dat aplikací mezi zařízeními** – ve výchozím nastavení, toto nastavení je **NONE**. Výběr konkrétní uživatele nebo skupiny nebo všechny umožňuje uživatelské nastavení a dat aplikací na synchronizaci přes své zařízení s Windows 10. Další informace o tom, jak funguje synchronizace ve Windows 10.
Tato možnost je k dispozici prostřednictvím produkty, jako je například Azure AD Premium nebo Enterprise Mobility Suite (EMS) premium funkce.
 
    ![Spravovat zařízení s Intune](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a>Vyhledávání zařízení

Jako správce portálu Azure, máte dvě možnosti vyhledávání zaregistrovaná a připojené k zařízení:

- **Všechna zařízení** v **spravovat** části **zařízení** okno  

    ![Všechna zařízení](./media/device-management-azure-portal/41.png)


- **Zařízení** v **spravovat** části **uživatele** okno
 
    ![Všechna zařízení](./media/device-management-azure-portal/43.png)



Obě možnosti můžete získat zobrazení který:


- Umožňuje vyhledat zařízení pomocí zobrazovaný název jako filtr.

- Poskytuje podrobný přehled o zaregistrovaná a připojené k zařízení

- Umožňuje provádět běžné úlohy správy zařízení
   

![Všechna zařízení](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Úlohy správy zařízení

Jako správce můžete spravovat registrovaná nebo připojeného k zařízení. Tato část poskytuje informace o běžných úlohách správy zařízení.


**Spravovat zařízení s Intune** – Pokud jste správce služby Intune, můžete spravovat zařízení, které jsou označeny jako **Microsoft Intune**. Správce můžete zobrazit další zařízení 

![Spravovat zařízení s Intune](./media/device-management-azure-portal/31.png)


**Povolit / zakázat zařízení s Azure AD**

K povolení nebo zakázání zařízení, musíte být globálním správcem ve službě Azure AD. Zakázáním zařízení tomuto zařízení brání přístupu k prostředkům Azure AD.  Chcete-li zakázat zařízení, můžete buďto kliknout na *...* Klikněte na zařízení pro další podrobnosti.

 
![Spravovat zařízení s Intune](./media/device-management-azure-portal/33.png)

Zakázáním zařízení se stavem v změní **povoleno** sloupec, který se **ne**.

![Zakázat zařízení](./media/device-management-azure-portal/32.png)


**Odstranění zařízení s Azure AD** – Pokud chcete odstranit zařízení, musíte být globálním správcem ve službě Azure AD.  
Odstranění zařízení:
 
- Tomuto zařízení brání přístupu k prostředkům Azure AD 

- Odebere všechny podrobnosti, které jsou připojené k zařízení, například, klíče Bitlockeru pro zařízení s Windows  

- Reprezentuje neobnovitelná aktivity a se nedoporučuje, pokud je potřeba

Pokud se zařízení spravuje jiná autoritu pro správu (např. Microsoft Intune), ujistěte se, že má zařízení vymazání / vyřazení před odstraněním zařízení ve službě Azure AD.

Můžete buď vybrat "..." Odstranit zařízení nebo klikněte na zařízení pro další podrobnosti
 
![Odstranění zařízení](./media/device-management-azure-portal/34.png)


**Zobrazení nebo zkopírujte ID zařízení** -ID zařízení můžete použít k ověření informace ID zařízení na zařízení nebo pomocí prostředí PowerShell při řešení potíží. Pro přístup k možnosti kopírování, klikněte na zařízení.

![Zobrazit ID zařízení](./media/device-management-azure-portal/35.png)
  

**Zobrazení nebo kopírování klíče Bitlockeru** – Pokud jste správce, můžete zobrazit a zkopírujte tyto klíče nástroje BitLocker pomoct uživatelům obnovit svá šifrované jednotce. Tyto klíče jsou dostupné pouze pro zařízení s Windows, které jsou zašifrované a jejich klíče uloženého ve službě Azure AD. Při přístupu k podrobnosti o zařízení, můžete zkopírovat tyto klíče.
 
![Klíče Bitlockeru zobrazení](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Protokoly auditu


Aktivity zařízení jsou dostupné přes protokoly aktivity. To zahrnuje aktivity aktivaci služby registrace zařízení nebo uživatele:

- Vytváření zařízení a přidání vlastníků/uživatelů na zařízení

- Změny nastavení zařízení

- Operace zařízení například odstranění nebo aktualizaci zařízení
 
Vašim vstupním bodem k datům auditování je **protokoly auditu** v **aktivity** části **zařízení* okno.

![Protokoly auditu](./media/device-management-azure-portal/61.png)


Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:

- datum a čas výskytu

- cíle

- iniciátoru nebo objektu actor (který) aktivity

- aktivity (co)

![Protokoly auditu](./media/device-management-azure-portal/63.png)

Zobrazení seznamu můžete upravit kliknutím na **Sloupce** na panelu nástrojů.
 
![Protokoly auditu](./media/device-management-azure-portal/64.png)


Abyste omezili zobrazovaná data na úroveň, která vám vyhovuje, můžete filtrovat data přihlašování s využitím následujících polí:

- Catergory
- Typ prostředku aktivity
- Aktivita
- Rozsah dat
- cíl
- Zahájit (objektu Actor)

Kromě filtry můžete vyhledat konkrétní položky.

![Protokoly auditu](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Další kroky

* [Úvod do správy zařízení v Azure Active Directory](device-management-introduction.md)



