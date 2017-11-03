---
title: "OpenAPI rozšíření pro vlastní konektory - Azure Logic Apps | Microsoft Docs"
description: "Rozšíření OpenAPI vlastní konektor s pokročilé funkce"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: e8565019bd4631043485224c73c2fe60e633951c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="extend-your-custom-connectors-openapi-with-advanced-functionality"></a>Rozšíření OpenAPI vlastní konektor s pokročilé funkce

Chcete-li vytvořit vlastní konektory pro Azure Logic Apps, Microsoft Flow nebo Microsoft PowerApps, je nutné zadat definiční soubor OpenAPI, což je strojově čitelným dokument bez ohledu na jazyk, který popisuje operace vaše rozhraní API a parametry. Společně s OpenAPI pro funkce na více systémů v poli můžete použít také tato rozšíření OpenAPI při vytváření vlastní konektory pro Logic Apps a toku:

* [`summary`](#summary)
* [`x-ms-summary`](#x-ms-summary)
* [`description`](#description)
* [`x-ms-visibility`](#x-ms-visibility)
* [`x-ms-dynamic-values`](#x-ms-dynamic-values)
* [`x-ms-dynamic-schema`](#x-ms-dynamic-schema)

Zde jsou další informace o těchto rozšířeních:

<a name="summary"></a>

## <a name="summary"></a>Souhrn

Určuje název pro akci (operace). </br>
Platí pro: operace </br>
Doporučená: Použijte *velká na začátku věty* pro `summary`. </br>
Příklad: "Pokud se přidá do kalendáře" nebo "Odeslat e-mailu"

![pro každou operaci "shrnutí"](./media/custom-connector-openapi-extensions/summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "summary": "Send an email",
    /// Other action properties here...
  }
},
```

<a name="x-ms-summary"></a>

## <a name="x-ms-summary"></a>x-ms souhrn

Určuje název entity. </br>
Platí pro: parametry, schéma odpovědi </br>
Doporučená: Použijte *title případ* pro `x-ms-summary`. </br>
Příklad: "ID kalendáře", "Předmět", "Popis události" a tak dále

!["x-ms souhrn" pro každé entity](./media/custom-connector-openapi-extensions/x-ms-summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "parameters": [ 
      {
        /// Other parameters here...
        "x-ms-summary": "Subject",
        /// Other parameters here...
      }
    ]
  }
},
```

<a name="description"></a>

## <a name="description"></a>description

Určuje podrobné vysvětlení operace funkce nebo formátu entity a funkce. </br>
Platí pro: schéma odpověď operace, parametry, </br>
Doporučená: Použijte *velká na začátku věty* pro `description`. </br>
Příklad: "Tato operace se aktivuje při přidání nové události do kalendáře", "Zadejte předmět e-mailu" a tak dále

!["Popis" pro každou operaci nebo entity](./media/custom-connector-openapi-extensions/description.png)

``` json
"actions" {
  "Send_an_email": {
     "description": "Specify the subject of the mail",
     /// Other action properties here...
  }
},
```

<a name="visibility"></a>

## <a name="x-ms-visibility"></a>x-ms přehled

Určuje viditelnost zobrazující se uživatelům pro entitu. </br>
Možné hodnoty: `important`, `advanced`, a`internal` </br>
Platí pro: operace, parametry, schémata

* `important`operace a parametry vždy nejdříve jsou zobrazeny pro uživatele.
* `advanced`operace a parametry jsou skryté v části Další nabídky.
* `internal`operace a parametry jsou skryta uživatele.

> [!NOTE] 
> Pro parametry, které jsou `internal` a `required`, můžete **musí** zadejte výchozí hodnoty pro tyto parametry.

Příklad: **Další informace naleznete v** nabídky a **zobrazit rozšířené možnosti** jsou skrytí nabídky `advanced` operace a parametry.

!["x-ms viditelnost" pro zobrazení nebo skrytí operace a parametry](./media/custom-connector-openapi-extensions/x-ms-visibility.png)

``` json
"actions" {
  "Send_an_email": {
     /// Other action properties here...
     "parameters": [
         {
           "name": "Subject",
           "type": "string",
           "description": "Specify the subject of the mail",
           "x-ms-summary": "Subject",
           "x-ms-visibility": "important",
           /// Other parameter properties here...
         }
     ]
     /// Other action properties here...
  }
},
```

<a name="x-ms-dynamic-values"></a>

## <a name="x-ms-dynamic-values"></a>x-ms dynamické hodnoty

Zobrazuje seznam vyplněná pro uživatele, mohou vybrat vstupní parametry pro operace. </br>
Platí pro: parametry </br>
Použití: Přidat `x-ms-dynamic-values` objektu do definice parametru. Například najdete [OpenAPI ukázka](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

!["x-ms dynamické hodnoty" pro zobrazení seznamy](./media/custom-connector-openapi-extensions/x-ms-dynamic-values.png)

### <a name="properties-for-x-ms-dynamic-values"></a>Vlastnosti pro x-ms dynamické hodnoty

| Name (Název) | Požadované nebo volitelné | Popis | 
| ---- | -------------------- | ----------- | 
| **operationID** | Požaduje se | Operace volání pro naplnění seznamu | 
| **Hodnota cesta** | Požaduje se | Řetězec cesty v objektu uvnitř `value-collection` který odkazuje na hodnotu parametru. </br>Pokud `value-collection` není zadán, a odpověď je vyhodnoceno jako pole. | 
| **Název hodnoty** | Nepovinné | Řetězec cesty v objektu uvnitř `value-collection` který odkazuje hodnotu popis. </br>Pokud `value-collection` není zadán, a odpověď je vyhodnoceno jako pole. | 
| **Kolekce hodnot** | Nepovinné | Řetězec cesty, jehož výsledkem je pole objektů v datové části odpovědi | 
| **Parametry** | Nepovinné | Objekt, jehož vlastnosti zadejte vstupních parametrů požadovaných pro vyvolání operace dynamické hodnoty | 
|||| 

Tady je příklad, který zobrazuje vlastnosti v `x-ms-dynamic-values`:

``` json
"x-ms-dynamic-values": {
  "operationId": "PopulateDropdown",
  "value-path": "name",
  "value-title": "properties/displayName",
  "value-collection": "value",
  "parameters": {
     "staticParameter": "<value>",
     "dynamicParameter": {
        "parameter": "<value-to-pass-to-dynamicParameter>"
     }
  }
}
```

## <a name="example-all-the-openapi-extensions-up-to-this-point"></a>Příklad: Všechny OpenAPI rozšíření v tomto okamžiku

``` json
"/api/lists/{listID-dynamic}": {
    "get": {
        "description": "Get items from a single list - uses dynamic values and outputs dynamic schema",
        "summary": "Gets items from the selected list",
        "operationID": "GetListItems",
        "parameters": [
           {
             "name": "listID-dynamic",
             "type": "string",
             "in": "path",
             "description": "Select the list from where you want outputs",
             "required": true,
             "x-ms-summary": "Select List",
             "x-ms-dynamic-values": {
                "operationID": "GetLists",
                "value-path": "id",
                "value-title": "name"
             }
           }
        ]
    }
}
```

<a name="x-ms-dynamic schema"></a>

## <a name="x-ms-dynamic-schema"></a>x-ms dynamické – schéma

Označuje, že schéma pro aktuální parametr nebo odpovědi je dynamická. Tento objekt můžete vyvolání operace, která je definována hodnota tohoto pole, dynamicky zjištění schématu a zobrazí příslušné uživatelské rozhraní pro shromažďování vstupy uživatele nebo zobrazit dostupná pole. 

Platí pro: parametry, odpovědi

Použití: Přidat `x-ms-dynamic-schema` objekt, který chcete definici parametru nebo odpověď žádost textu. Příklad najdete [OpenAPI ukázka](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

Tento příklad ukazuje, jak se změní vstupní formuláře, na základě položky, které si uživatel vybere ze seznamu rozevíracího seznamu:

!["x-ms-dynamic-schema" pro dynamické parametry](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema.png)

A tento příklad ukazuje, jak změnit výstupy, na základě položky, které si uživatel vybere ze seznamu rozevíracího seznamu. V této verzi si uživatel vybere "Aut":

!["x-ms-dynamic schématu odpověď" pro vybranou položku "Aut"](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output1.png)

V této verzi si uživatel vybere "Jídlo":

!["x-ms-dynamic schématu odpověď" pro vybranou položku "Jídlo"](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output2.png)

### <a name="properties-for-x-ms-dynamic-schema"></a>Vlastnosti pro x-ms dynamické – schéma

| Name (Název) | Požadované nebo volitelné | Popis | 
| ---- | -------------------- | ----------- | 
| **operationID** | Požaduje se | Operace volání pro načítání schématu | 
| **Parametry** | Požaduje se | Objekt, jehož vlastnosti zadejte vstupních parametrů požadovaných pro vyvolání operace dynamické schématu | 
| **Hodnota cesta** |Nepovinné | Řetězec cesty, která odkazuje na vlastnost, která má schéma. </br>Pokud není zadaný, předpokládá se, že odpověď je tak, aby obsahovala schéma ve vlastnostech kořenový objekt. | 
|||| 

Tady je příklad pro dynamický parametr:

``` json
{
  "name": "dynamicListSchema",
  "in": "body",
  "description": "Dynamic schema for items in the selected list",
  "schema": {
    "type": "object",
    "x-ms-dynamic-schema": {
        "operationID": "GetListSchema",
        "parameters": {
          "listID": {
            "parameter": "listID-dynamic"
          }
        },
        "value-path": "items"
    }
  }
}
```

Tady je příklad pro dynamické odpovědi:

``` json
"DynamicResponseGetListSchema": {
   "type": "object",
   "x-ms-dynamic-schema": {
      "operationID": "GetListSchema",
      "parameters": {
         "listID": {
            "parameter": "listID-dynamic"
         }
      },
      "value-path": "items"
    }
}
```

## <a name="next-steps"></a>Další kroky

* [Popisují vaše vlastní rozhraní API a konektorů](../logic-apps/custom-connector-api-postman-collection.md)
* [Služba Logic Apps: Registrace vaší konektoru](../logic-apps/logic-apps-custom-connector-register.md)
* [Postup: Registrace vaší konektoru](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)