---
title: "Správa integrace účet artefaktu metadat - Azure Logic Apps | Microsoft Docs"
description: "Přidat nebo načíst metadata artefaktů z účtů integrace pro Azure Logic Apps"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 28bb8296ddd820ec5aa9793dc0928b4b1e67bf6f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>Spravovat artefaktu metadat v účtech integrace pro logic apps

Můžete definovat vlastní metadata pro artefakty v účty pro integraci a načtení tohoto metadat při běhu pro svou aplikaci logiky. Například můžete zadat metadata pro artefakty jako partnery, smlouvy, schémat a mapy – všechny ukládání metadat pomocí páry klíč hodnota. V současné době artefakty nelze vytvořit metadat prostřednictvím uživatelského rozhraní, ale rozhraní REST API můžete použít k vytvoření metadat. Chcete-li přidat metadata při vytváření nebo vyberte partnera, smlouvy nebo schématu na portálu Azure, zvolte **upravit jako JSON**. K načtení metadat artefaktů v aplikace logiky, můžete použít funkci integrace vyhledávání artefaktů účtu.

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Přidat metadata do artefakty na účty pro integraci

1. Vytvoření [integrace účet](logic-apps-enterprise-integration-create-integration-account.md).

2. Například přidejte artefakt ke svému účtu integrace, [partnera](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [smlouvy](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), nebo [schématu](logic-apps-enterprise-integration-schemas.md).

3.  Vyberte artefaktu, zvolte **upravit jako JSON**a zadejte podrobnosti o metadata.

    ![Zadejte metadat](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Načtení metadat z artefaktů pro logic apps

1. Vytvoření [aplikace logiky](logic-apps-create-a-logic-app.md).

2. Vytvoření [odkaz z aplikace logiky k vašemu účtu integrace](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app). 

3. Návrhář aplikace na základě logiky, přidejte aktivační událost jako *požadavku* nebo *HTTP* do aplikace logiky.

4.  Zvolte **další krok** > **přidat akci**. Vyhledejte *integrace* , můžete najít a pak vyberte **integrace účet – integrace vyhledávání artefaktů účtu**.

    ![Vyberte integrační účet artefaktů vyhledávání](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Vyberte **typu artefaktu**a zadejte **artefaktů název**.

    ![Vyberte typ artefaktů a zadejte název artefaktů](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Příklad: Načtení partnera metadat

Metadata partnera má tyto `routingUrl` podrobnosti:

![Vyhledejte "routingURL" metadata partnera.](media/logic-apps-enterprise-integration-metadata/image6.png)

1. V aplikaci logiky přidat aktivační událost, **integrace účet – integrace vyhledávání artefaktů účtu** akce pro váš partner a **HTTP**.

    ![Přidání do aplikace logiky aktivační událost, artefaktů vyhledávání a "HTTP"](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Chcete-li získat identifikátor URI, přejděte do zobrazení kódu pro svou aplikaci logiky. Vaše definici aplikace logiky by měl vypadat jako tento ukázkový:

    ![Hledání vyhledávání](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a>Další kroky
* [Další informace o smlouvy](logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  
