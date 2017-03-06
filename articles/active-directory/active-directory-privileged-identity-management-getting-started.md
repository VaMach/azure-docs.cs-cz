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
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: b9a3b64d9de48f17a295ca7a9ea58cf26e8f83ed
ms.openlocfilehash: 6945529b8189c270ca6e5c41be0cd0d9caa37cb4
ms.lasthandoff: 02/28/2017

---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Začátek práce s aplikací Azure AD Privileged Identity Management
Pomocí aplikace Azure Active Directory (AD) Privileged Identity Management můžete spravovat, řídit a sledovat přístup v rámci organizace. To zahrnuje přístup k prostředkům v Azure AD a dalších online službách Microsoftu jako Office 365 nebo Microsoft Intune.

Tento článek vysvětluje, jak přidat aplikaci Azure AD PIM do řídicího panelu webu Azure Portal.

## <a name="add-the-privileged-identity-management-application"></a>Přidání aplikace Privileged Identity Management
Před použitím aplikace Azure AD Privileged Identity Management musíte tuto aplikaci přidat do řídicího panelu webu Azure Portal.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce adresáře.
2. Pokud má vaše organizace víc než jeden adresář, vyberte své uživatelské jméno v pravém horním rohu webu Azure Portal. Vyberte adresář, kde budete PIM používat.
3. Vyberte **Další služby** a pomocí pole Filtr najděte **Azure AD Privileged Identity Management**.
4. Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**. Aplikace Privileged Identity Management se otevře.

Pokud jste první, kdo bude ve vašem adresáři používat aplikaci Azure AD Privileged Identity Management, [průvodce zabezpečením](active-directory-privileged-identity-management-security-wizard.md) vás provede počátečním přiřazením. Automaticky se pak stanete prvním **správcem zabezpečení** a **správcem privilegovaných rolí** adresáře. Pouze správce privilegovaných rolí má přístup k této aplikaci za účelem správy přístupu pro další správce.  

## <a name="navigate-to-your-tasks"></a>Přechod k úkolům
Po nastavení Azure AD Privileged Identity Management se vždy, když aplikaci otevřete, zobrazí navigační okno. Pomocí tohoto okna provádějte úkoly správy identity.

![Úkoly nejvyšší úrovně v PIM – snímek obrazovky](./media/active-directory-privileged-identity-management-getting-started/pim_tasks.png)

* **Aktivovat moje role** – přejdete k seznamu rolí, které vám jsou přiřazeny. Tam můžete aktivovat role, ke kterým máte oprávnění.
* **Spravovat privilegované role** – zobrazí řídicí panel pro správce privilegovaných rolí, odkud se dá spravovat přiřazení rolí, měnit nastavení aktivace rolí, začít kontrolu přístupu atd. Možnosti na tomto řídicím panelu se zobrazují jen správcům privilegovaných rolí.
* **Zkontrolovat privilegovaný přístup** – přejdete ke kontrolám přístupu, které je potřeba dokončit, ať už kontrolujete přístup sobě nebo někomu jinému. 

## <a name="next-steps"></a>Další kroky
[Přehled aplikace Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) obsahuje podrobné informace o tom, jak můžete spravovat přístup pro správu ve vaší organizaci.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

