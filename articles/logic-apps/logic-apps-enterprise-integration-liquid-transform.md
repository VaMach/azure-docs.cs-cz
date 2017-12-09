---
title: "Převést data JSON s kapaliny transformací - Azure Logic Apps | Microsoft Docs"
description: "Vytvořte transformací nebo mapy pro pokročilé transformace JSON pomocí Logic Apps a kapaliny šablony."
services: logic-apps
documentationcenter: 
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: LADocs; divswa
ms.openlocfilehash: cd177b1ebcb5d236ce265dc153ee6a02125a69df
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="advanced-json-transformations-using-liquid-template"></a>Pokročilé transformace JSON pomocí kapaliny šablony
Azure Logic Apps podporuje základní transformace JSON prostřednictvím nativní akce operace dat jako vytvářené nebo analyzovat JSON. Služba Logic Apps teď také podporuje rozšířené transformace JSON s kapaliny šablony. [Kapaliny](https://shopify.github.io/liquid/) je šablona open-source jazyk pro flexibilní webové aplikace.
 
V tomto článku Další informace o použití kapaliny mapy nebo šablony, která může podporovat složitější transformace JSON, jako je například iterací, řízení toky, proměnné a tak dále. Musí definovat JSON pro mapování JSON s kapaliny mapy a uložení této mapy ve vašem účtu integrace, před provedením kapaliny transformaci v aplikaci logiky.

## <a name="prerequisites"></a>Požadavky
Tady jsou požadavky pro použití kapaliny akce:

* Předplatné Azure. Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/). Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* Základní znalosti o [postup vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

* Základní [integrace účet](logic-apps-enterprise-integration-create-integration-account.md).


## <a name="create-and-add-liquid-template-or-map-to-integration-account"></a>Vytvoření a přidání kapaliny šablony nebo účet integrace

1. Vytvoření šablony kapaliny ukázka v tomto příkladu. Kapaliny Šablona definuje, jak k transformaci vstupu JSON podle postupu popsaného tady:

   ```
   {%- assign deviceList = content.devices | Split: ', ' -%}
    {
        "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
        "firstNameUpperCase": "{{content.firstName | Upcase}}",
        "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
        "devices" : [
        {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
        {%- endfor -%}
        ]
    }
    ```
    > [!NOTE]
    > Pokud vaše kapaliny šablona používá [filtry](https://shopify.github.io/liquid/basics/introduction/#filters), tyto filtry musí začínat velká písmena. 

2. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

3. V hlavní nabídce Azure, zvolte **všechny prostředky**. 

4. Do vyhledávacího pole zadejte svůj účet integrace. Vyberte svůj účet.

   ![Vyberte účet integrace](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

5.  Na dlaždici integrace účet vyberte **mapy**.

   ![Vyberte mapy](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

6. Zvolte **přidat** a uveďte následující informace pro mapu:
  * **Název**: název pro mapu, která je "JsontoJsonTemplate" v tomto příkladu.
  * **Typ mapování**: typ pro mapu. Pro formát JSON k transformaci JSON, je nutné vybrat **kapaliny**.
  * **Mapa**: existující kapaliny šablony nebo mapovat soubor pro transformaci, která je "SimpleJsonToJsonTemplate.liquid" v tomto příkladu. Nástroje pro výběr souborů můžete použít k vyhledání tohoto souboru.

    ![Přidání šablony kapaliny](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)

    
## <a name="add-the-liquid-action-to-transform-json-in-your-logic-app"></a>Přidat kapaliny akce k transformaci JSON v aplikaci logiky

1. [Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).

2. Přidat [aktivační událost požadavku](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) do aplikace logiky.

3. Zvolte **+ nový krok > Přidat akci**. Vyhledejte *kapaliny* do vyhledávacího pole. Vyberte **kapaliny - transformace JSON do formátu JSON**.

  ![Hledání. akce kapaliny](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. V **obsah** vyberte **textu** z dynamického obsahu seznamu, nebo seznam parametrů, podle toho, co se zobrazí. 
  
  ![Vyberte textu](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Z **mapy** rozevíracího seznamu vyberte kapaliny šablony, což je JsonToJsonTemplate v tomto příkladu.

  ![Vyberte mapy](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Pokud je seznam prázdný, aplikace logiky pravděpodobně není propojený s vaším účtem integrace. Odkaz svou aplikaci logiky integrace účet, který má kapaliny šablony nebo mapy, postupujte takto:

   1. V nabídce aplikace logiky, vyberte **nastavení pracovních postupů**. 
   2. Z **vyberte účet, integrace** seznam, vyberte svůj účet integrace a zvolit **Uložit**.

     ![Aplikace logiky odkaz na účet integrace](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)


## <a name="test-your-logic-app"></a>Testování aplikace logiky

   POST vstupu JSON do aplikace logiky z [Postman](https://www.getpostman.com/postman) nebo podobného nástroje. Transformovaný výstup JSON ze svou aplikaci logiky vypadá v tomto příkladu:
  
   ![Příklad výstupu](./media/logic-apps-enterprise-integration-liquid-transform/example-output.png)


## <a name="next-steps"></a>Další kroky
* [Další informace o integračního balíčku Enterprise](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
* [Další informace o mapování](../logic-apps/logic-apps-enterprise-integration-maps.md "Další informace o enterprise integrace mapy")  

