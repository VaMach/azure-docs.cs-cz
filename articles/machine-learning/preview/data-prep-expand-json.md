---
title: "Rozbalte položku pomocí Azure Machine Learning Workbench transformace JSON"
description: "V referenčním dokumentu pro transformaci, rozbalte položku JSON."
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 57f461f9a39e8069654985d86bfe3ae23dd0855a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="expand-json-transformation"></a>Rozbalte JSON transformace
**Rozbalte JSON** transformace umožňuje uživatelům rozbalte jako existující sloupec, který obsahuje platný text JSON do více sloupců.

## <a name="how-to-perform-this-transformation"></a>Jak provádět Tato transformace

Postupujte podle těchto kroků k provedení této transformace:
1. Vyberte zdrojový sloupec, který obsahuje JSON text.
2. Vyberte **rozbalte JSON** z **transformuje** nabídky. Nebo klikněte pravým tlačítkem na záhlaví sloupce, zdroje a vyberte **rozbalte JSON** v místní nabídce. 
3. Klikněte na **OK**. 
 
Vedle zdrojového sloupce jsou přidány sloupce. Tyto sloupce obsahují vlastnosti z další úroveň hierarchie v textu JSON. Vlastnost klíče, pokud existuje, je použít k vytvoření názvu odpovídajícího sloupce. Vnořené vlastnosti se zachovají jako text JSON pro tohoto uživatele můžete interaktivně rozbalit nebo zahodit podle potřeby.

## <a name="examples"></a>Příklady

Zdroj požadován *zákazníka* je rozšířena do dvou sloupců *Customer.Name* a *Customer.Phone*.

| Zákazníka                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {"Název": "Carrie Dodson", "Phone": "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {"Název": "Leonardem Robledo", "Phone": "456 7890 123"} | Leonardem Robledo | 456-7890-123   |

