---
title: "Začínáme s aplikací Azure AD Privileged Identity Management | Dokumentace Microsoftu"
description: "Přečtěte si, jak spravovat privilegované identity pomocí aplikace Azure Active Directory Privileged Identity Management na webu Azure Portal."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 17cdff033cc3dbb199d11c3b8ac1acbc92499877
ms.contentlocale: cs-cz
ms.lasthandoff: 07/10/2017

---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Začátek práce s aplikací Azure AD Privileged Identity Management
Pomocí aplikace Azure Active Directory (AD) Privileged Identity Management můžete spravovat, řídit a sledovat přístup v rámci organizace. Tento obor zahrnuje přístup k prostředkům ve službě Azure AD a dalších online službách Microsoftu jako Office 365 nebo Microsoft Intune.

Tento článek vysvětluje, jak přidat aplikaci Azure AD PIM do řídicího panelu webu Azure Portal.

## <a name="add-the-privileged-identity-management-application"></a>Přidání aplikace Privileged Identity Management
Před použitím aplikace Azure AD Privileged Identity Management musíte tuto aplikaci přidat do řídicího panelu webu Azure Portal.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce adresáře.
2. Pokud má vaše organizace víc než jeden adresář, vyberte své uživatelské jméno v pravém horním rohu webu Azure Portal. Vyberte adresář, kde chcete PIM používat.
3. Vyberte **Další služby** a pomocí pole Filtr najděte **Azure AD Privileged Identity Management**.
4. Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**. Aplikace Privileged Identity Management se otevře.

Pokud jste první, kdo ve vašem adresáři používá Azure AD Privileged Identity Management, automaticky se vám přiřadí role **Správce zabezpečení** a **Správce privilegovaných rolí** v daném adresáři. Pouze správci privilegovaných rolí můžou spravovat přiřazení rolí uživatelů. Kromě toho můžete spustit [průvodce zabezpečením](active-directory-privileged-identity-management-security-wizard.md), který vás provede počátečním zjišťováním a přiřazováním.

## <a name="navigate-to-your-tasks"></a>Přechod k úkolům
Po nastavení Azure AD Privileged Identity Management se vždy, když aplikaci otevřete, zobrazí navigační okno. Pomocí tohoto okna provádějte úkoly správy identity.

![Úkoly nejvyšší úrovně v PIM – snímek obrazovky](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* **Moje role** – přejdete k seznamu rolí, které vám jsou přiřazeny. V této části můžete aktivovat role, ke kterým máte oprávnění.
* **Schválit žádosti (Preview)** zobrazí seznam žádostí uživatelů ve vašem adresáři o aktivaci, které čekají na schválení. [Další informace](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* **Čekající žádosti (Preview)** zobrazí všechny aktuální žádosti k aktivaci.
* **Zkontrolovat přístup** – přejdete k čekajícím kontrolám přístupu, které je potřeba dokončit, ať už kontrolujete přístup sobě nebo někomu jinému.
* **Role adresáře služby Azure AD** v části Správa je řídicí panel pro správce privilegovaných rolí, odkud se dá spravovat přiřazení rolí, měnit nastavení aktivace rolí, začít kontrolu přístupu atd. Možnosti na tomto řídicím panelu se zobrazují jen správcům privilegovaných rolí.

## <a name="next-steps"></a>Další kroky
[Přehled aplikace Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) obsahuje podrobné informace o tom, jak můžete spravovat přístup pro správu ve vaší organizaci.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

