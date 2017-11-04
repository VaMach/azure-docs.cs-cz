---
title: "Zaregistrovat Azure Enterprise Agreement náklady za správu Azure | Microsoft Docs"
description: "Použijte k registraci s Azure náklady na správu Cloudyn Enterprise Agreement."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 41a9df712b07253d9f5f9db8542fb9917592320f
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Registrovat smlouvy Enterprise Azure a zobrazení náklady dat

Vaše smlouvy Enterprise Azure slouží k registraci Azure náklady na správu Cloudyn. Registrace poskytuje přístup k portálu Cloudyn. Tento rychlý start podrobnosti o procesu registrace potřebné k vytvoření Cloudyn zkušební verzi předplatného a přihlaste se k portálu Cloudyn. Je také ukazuje, jak spustit hned zobrazení dat náklady.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

- Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="create-a-trial-registration"></a>Vytvořte zkušební registrace

1. Na portálu Azure klikněte na tlačítko **náklady na správu + fakturace** v seznamu služeb.
2. V části **přehled**, klikněte na tlačítko **náklady na správu**  
    ![Náklady na stránce Správa](./media/quick-register-ea/cost-mgt-billing-service.png)
3. Na **náklady na správu** stránky, **přejít na náklady na správu** chcete otevřít stránku registrace Cloudyn v novém okně.
4. Na stránce portálu registrace zkušební verze Cloudyn, zadejte název vaší společnosti a potom vyberte **Azure podnikového zápisu správce**.  
    ![Registrace zkušební verze](./media/quick-register-ea/trial-reg.png)
5. Zadejte klíč rozhraní API registrace podnikového portálu. Pokud nemáte klíč užitečný, klikněte [podnikový portál](https://ea.azure.com) propojení a proveďte následující kroky:
  1. Přihlaste se k webu Azure Enterprise a klikněte na tlačítko **sestavy**, klikněte na tlačítko **přístupový klíč rozhraní API** a poté zkopírujte primární klíč.  
    ![Klíč rozhraní API EA](./media/quick-register-ea/ea-key.png)
  3. Přejděte zpět na stránku registrace a vložte klíč rozhraní API.
6. Souhlas s podmínkami použití a ověřit váš klíč. Klikněte na tlačítko **Další** k autorizaci Cloudyn ke shromažďování dat prostředků Azure. Data shromážděná zahrnuje využití, výkon, fakturace a značky data z vašich předplatných.  
    ![ověření klíče](./media/quick-register-ea/ea-key-validated.png)
7. V části **pozvat ostatní účastníci**, můžete přidat uživatele zadáním jejich e-mailové adresy. Po dokončení klikněte na tlačítko **Další**. Jak dlouho trvá přibližně 2 hodiny pro všechny vaše fakturační údaje, které se přidají do Cloudyn.
8. Klikněte na tlačítko **přejít na Cloudyn** Cloudyn portál otevřít a pak na **správu cloudu účty** stránky, byste měli vidět registrované informace o účtu EA.

Podívejte se na kurz video o registraci Enterprise Agreement, najdete v tématu [jak najít vaše EA registrace ID a klíč rozhraní API pro použití v Azure náklady na správu Cloudyn](https://youtu.be/u_phLs_udig).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Další kroky

V tento rychlý start použít informace o vaší smlouvě Enterprise Azure k registraci s náklady na správu. Můžete také přihlášení k portálu Cloudyn a spuštění zobrazení dat náklady. Další informace o Azure náklady na správu Cloudyn, pokračujte v kurzu pro náklady na správu.

> [!div class="nextstepaction"]
> [Zkontrolujte využití a náklady](./tutorial-review-usage.md)
