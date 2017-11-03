---
title: "Zaregistrovat informace o partnerovi CSP pomocí Azure náklady na správu | Microsoft Docs"
description: "Použijte k registraci s Azure náklady na správu Cloudyn vaše informace o partnerovi CSP."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: bcb072a2f2ab8c0e5097fca2c95309464483cb53
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="register-with-the-csp-partner-program-and-view-cost-data"></a>Zaregistrovat se program pro partnery zprostředkovatele kryptografických služeb a náklady dat zobrazení

Jako partner zprostředkovatele kryptografických služeb můžete zaregistrovat s Azure náklady na správu Cloudyn. Registrace poskytuje přístup k portálu Cloudyn. Tento rychlý start podrobnosti o procesu registrace potřebné k vytvoření Cloudyn zkušební verzi předplatného a přihlaste se k portálu Cloudyn. Je také ukazuje, jak spustit hned zobrazení dat náklady.


>[!NOTE]
>K dokončení registrace, musí být správcem partnera program přístup k rozhraní API Center partnera. Konfigurace rozhraní API partnera Center je nutná pro ověřování a přístup k datům. Další informace najdete v tématu [připojit k rozhraní API Center partnera](https://msdn.microsoft.com/library/partnercenter/mt709136.aspx). Kromě toho nepřímých CSP uživatelů lze použít pouze Cloudyn při jejich přímé prodejce CSP zaregistruje se Cloudyn pro poskytnutí přístupu k jejich zákazníků a předplatných.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

- Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="create-a-trial-registration"></a>Vytvořte zkušební registrace

1. Na portálu Azure klikněte na tlačítko **náklady na správu + fakturace** v seznamu služeb.
2. V části **přehled**, klikněte na tlačítko **náklady na správu**  
    ![Náklady na stránce Správa](./media/quick-register-csp/cost-mgt-billing-service.png)
3. Na **náklady na správu** klikněte na **přejít na náklady na správu** chcete otevřít stránku registrace Cloudyn v novém okně.
4. Na stránce portálu registrace zkušební verze Cloudyn, zadejte název vaší společnosti, vyberte **Microsoft CSP partnera Program Správce**a potom klikněte na **Další**.  
5. Zadejte **ID aplikace**, **Commerce ID**, **aplikace tajný klíč**a vyberte **výchozí ceny plánování**. Pokud nemáte informace užitečný, přihlaste se k portálu Partnerské centrum na [https://partnercenter.microsoft.com](https://partnercenter.microsoft.com) se správcem vaší primární účet a proveďte následující kroky:
  1. Přejděte na **řídicí panel** > **nastavení účtu** > **správy aplikací**.
  2. Pokud jste předtím vytvořili webovou aplikaci, tento krok přeskočte. Jinak, klikněte na tlačítko **přidat novou webovou aplikaci** v **webové aplikace** části.
  3. Kopírování **ID aplikace** GUID z webové aplikace.
  4. Kopírování **Commerce ID** GUID z webové aplikace.
  5. Podle potřeby, vyberte jako jeden nebo dva roky, doba platnosti klíče. Vyberte **přidat klíč** a pak zkopírujte a uložte tajná hodnota klíče.  
    ![Zprostředkovatel kryptografických služeb partnerské Centrum](./media/quick-register-csp/csp-partner-center.png)
  6. Přejděte zpět na stránku registrace a vložte informace.  
      ![Přihlašovací údaje účtu zprostředkovatele kryptografických služeb](./media/quick-register-csp/csp-reg.png)
6. Souhlas s podmínkami použití a ověření vašich informací. Klikněte na tlačítko **Další** k autorizaci Cloudyn ke shromažďování dat prostředků Azure. Data shromážděná zahrnuje využití, výkon, fakturace a značky data z vašich předplatných.  
7. V části **pozvat ostatní účastníci**, můžete přidat uživatele zadáním jejich e-mailové adresy. Po dokončení klikněte na tlačítko **Další**. Jak dlouho trvá přibližně 2 hodiny pro všechny vaše fakturační údaje, které se přidají do Cloudyn.
8. Klikněte na tlačítko **přejít na Cloudyn** Cloudyn portál otevřít a pak na **správu cloudu účty** stránky, byste měli vidět registrované informace o účtu CSP.

## <a name="configure-indirect-csp-access-in-cloudyn"></a>Nakonfigurujte nepřímý přístup CSP ve Cloudyn

Ve výchozím nastavení je rozhraní API Center partnera jenom pro přímé CSP dostupné. Přímé poskytovatele CSP však můžete nakonfigurovat přístup pro jejich nepřímých CSP zákazníky nebo partnery, použití skupin entity v Cloudyn.

Pokud chcete povolit přístup pro nepřímou CSP zákazníky nebo partnery, postupujte podle kroků v [vytvořit zkušební registraci](#create-a-trial-registration) nastavit zkušební registrace. Dále proveďte následující kroky k segmentu nepřímých CSP datům pomocí Cloudyn skupiny entit. Pak přiřaďte příslušných uživatelských oprávnění skupinám entity.

1. Vytvoření skupiny entit s informacemi v [vytvořte entity](tutorial-user-access.md#create-entities).
2. Postupujte podle kroků v [přiřazování odběry náklady na entity](https://support.cloudyn.com/hc/en-us/articles/115005139425-Video-Assigning-subscriptions-to-Cost-Entities). Přidružte účet nepřímých CSP zákazníka a jejich předplatná Azure na typ entity, která vytvoříte dříve.
3. Postupujte podle kroků v [vytvořit uživatele s přístupem správce](tutorial-user-access.md#create-a-user-with-admin-access) vytvoření uživatelského účtu s přístupem správce. Potom zkontrolujte, že uživatelský účet má přístup správce ke konkrétní entity, které jste vytvořili dříve pro nepřímou účet.

Nepřímý partnery CSP přihlásit k portálu Cloudyn pomocí účty, které jste vytvořili pro ně.


[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Další kroky

V tento rychlý start používá vaše informace CSP při registraci s náklady na správu. Můžete také přihlášení k portálu Cloudyn a spuštění zobrazení dat náklady. Další informace o Azure náklady na správu Cloudyn, pokračujte v kurzu pro náklady na správu.

> [!div class="nextstepaction"]
> [Zkontrolujte využití a náklady](./tutorial-review-usage.md)
