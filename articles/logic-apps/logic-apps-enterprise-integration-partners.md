---
title: "Vytvořte partnery pro business-to-business (B2B) zprávy – Azure Logic Apps | Microsoft Docs"
description: "Informace o postupu přidání partnery ke svému účtu integrace s Logic Apps a Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 950cb449b53f400f0f0f860caf5415bbb5212269
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Přidat nebo aktualizovat partnery ve smlouvách business-to-business v pracovním postupu

Partneři jsou entity, které se účastní transakce (B2B) business-to-business výměnu zpráv mezi sebou. Před vytvořením partnery, které představují jste a jiné organizaci v tyto transakce, musí oba sdílet informace, které identifikuje a ověří zprávy odeslané podle sebe navzájem. Po popisují tyto podrobnosti a budete připravení zahájit relaci vaší firmy, můžete vytvořit partnerům ve vašem účtu integrace představují oba.

## <a name="what-roles-do-partners-have-in-your-integration-account"></a>Ve vašem účtu integrace jaké role mají partnery?

Zadat podrobnosti o zprávy vyměňují mezi partnery, vytvořte smlouvy mezi těmito partnery. Ale předtím, než vytvoříte smlouvu, je třeba přidat alespoň dva partneři ke svému účtu integrace. Vaše organizace musí být součástí smlouvy jako **hostitele partnera**. Druhého partnera nebo **hosta partnera** představuje organizace, která výměny zpráv ve vaší organizaci. Partner hostů může být jiné společnosti nebo i oddělení ve vaší vlastní organizaci.

Po přidání těchto partnerů, můžete vytvořit smlouvu.

Příjem a odesílání nastavení jsou orientované z hlediska hostované partnera. Nastavení škálování v smlouvu například určit, jak hostované partnera obdrží zpráv odeslaných z hosta partnera. Nastavení odesílání na smlouvě, uvádí, jak hostované partnera odešle zprávy do hostovaného partnera.

## <a name="how-to-create-a-partner"></a>Postup vytvoření partnera?

1. Na portálu Azure vyberte **Procházet**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Do vyhledávacího pole filtru zadejte **integrace**, pak vyberte **účty pro integraci** v seznamu výsledků.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Vyberte účet integrace, ve které chcete přidat vaši partneři.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Vyberte **partnery** dlaždici.

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. V okně partnery zvolte **přidat**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Zadejte název svého partnera, a pak vyberte **kvalifikátor**. Nakonec zadejte **hodnotu** k identifikaci dokumenty, které jsou do svých aplikací.

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Chcete-li sledovat průběh pro proces vytváření partnera, vyberte *zvonku* ikonu oznámení.

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. Chcete-li potvrdit, že byly úspěšně přidány nové partnerům, vyberte **partnery** dlaždici.

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    Po výběru dlaždice partnery, se zobrazí také nově přidané partnery v okně partnery.

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a>Úprava existující partnery ve vašem účtu integrace

1. Vyberte **partnery** dlaždici.
2. Po otevření okna partnery, vyberte partnera, se kterým chcete upravit.
3. Na **aktualizace partnera** dlaždici, provedené změny.
4. Jakmile jste hotovi, zvolte **Uložit**, nebo zrušit změny, vyberte **zahodit**.

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a>Postup odstranění partnera

1. Vyberte **partnery** dlaždici.
2. Po otevření okna partnera vyberte partnera, který chcete odstranit.
3. Zvolte **odstranit**.

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Další kroky
* [Další informace o smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  

