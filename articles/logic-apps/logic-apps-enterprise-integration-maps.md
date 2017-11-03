---
title: "Transformace XML pomocí map XSLT - Azure Logic Apps | Microsoft Docs"
description: "Přidat, že XSLT mapuje transformace dat XML s Azure Logic Apps a Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 4445a84a6c6425110e7d705019a28b5cc5447046
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="add-maps-for-xml-data-transform"></a>Přidat mapy pro transformaci dat XML

Integrace organizace používá pro transformaci dat XML mezi formáty mapy. Mapu je dokument XML, který definuje data v dokumentu, který by měl být převedeny do jiného formátu. 

## <a name="why-use-maps"></a>Proč používat mapy?

Předpokládejme, že pravidelně zobrazit B2B objednávky nebo faktury z zákazník, který používá formát YYYMMDD mezi daty. Ale ve vaší organizaci, ukládat data ve formátu MMDDYYY. Můžete použít mapu, která *transformace* formát data YYYMMDD do MMDDYYY před uložením podrobnosti objednávky nebo faktury v databázi zákazníků aktivity.

## <a name="how-do-i-create-a-map"></a>Vytvoření mapy

Můžete vytvořit projekty BizTalk integrace s [Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o integračního balíčku enterprise") pro Visual Studio 2015. Potom můžete vytvořit soubor mapa integrace pro, který umožňuje vizuálně položky mezi dva soubory schématu XML mapy. Po sestavení tohoto projektu, budete mít k dokumentu XSLT.

## <a name="how-do-i-add-a-map"></a>Jak přidat mapu?

1. Na portálu Azure vyberte **Procházet**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Do vyhledávacího pole filtru zadejte **integrace**, pak vyberte **účty pro integraci** ze seznamu výsledků.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Vyberte účet integrace, ve které chcete přidat mapy.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Vyberte **mapy** dlaždici.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Po otevření okna mapy, zvolte **přidat**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Zadejte **název** pro mapu. Pro nahrání souboru s mapováním, zvolte ikonu složky na pravé straně **mapy** textové pole. Po dokončení procesu nahrávání, zvolte **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Po Azure přidá ke svému účtu integrace mapy, obdržíte zprávu na obrazovce, která uvádí, zda byl přidán souboru mapy, nebo ne. Poté, co se vám tato zpráva, vyberte **mapy** dlaždici, aby se dala zobrazit nově přidané mapy.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a>Jak upravit mapu?

Musíte nahrát nový soubor mapy s změny, které chcete. Nejprve si můžete stáhnout mapy pro úpravy.

Pokud chcete nahrát nový mapu, která nahradí existující mapu, postupujte podle těchto kroků.

1. Vyberte **mapy** dlaždici.

2. Po otevření okna mapy, vyberte, kterou chcete upravit mapování.

3. Na **mapy** okně zvolte **aktualizace**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. V dialogu pro výběr souborů vyberte soubor mapy, který chcete nahrát a pak vyberte **otevřete**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Postup odstranění mapy?

1. Vyberte **mapy** dlaždici.

2. Po otevření okna mapy, vyberte mapování, které chcete odstranit.

3. Zvolte **odstranit**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Potvrďte, že chcete odstranit mapy.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Další kroky
* [Další informace o integračního balíčku Enterprise](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
* [Další informace o smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  
* [Další informace o transformací](logic-apps-enterprise-integration-transform.md "Další informace o integraci transformací enterprise")  

