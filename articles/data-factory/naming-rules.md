---
title: "Pravidla pro pojmenovávání entit služby Azure Data Factory | Microsoft Docs"
description: "Popisuje pravidla pojmenování entit služby Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 8f0207853097a3d31e06a722025f3964678414bf
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - pravidla po pojmenování
Následující tabulka obsahuje pravidla pojmenování artefaktů služby Data Factory.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verze 1 služby Data Factory, který je je všeobecně dostupná (GA), najdete v části [pojmenování pravidla v objektu pro vytváření dat version1](v1/data-factory-naming-rules.md).

| Name (Název) | Jedinečnost názvu | Ověřovací kontroly |
|:--- |:--- |:--- |
| Data Factory |Jedinečná napříč Microsoft Azure. Názvy jsou velká a malá písmena, který je `MyDF` a `mydf` odkazovat na stejné služby data factory. |<ul><li>Každý objekt pro vytváření dat je vázaný na přesně jedno předplatné.</li><li>Názvy objektů musí začínat písmenem nebo číslicí a může obsahovat pouze písmena, číslice a pomlčky (-) znaků.</li><li>Každý znak pomlčka (-) musí být okamžitě a následnou písmenem nebo číslem. Po sobě jdoucí pomlčky nejsou povolené v názvech kontejneru.</li><li>Název může být 3 až 63 znaků dlouhý.</li></ul> |
| Propojených služeb/tabulek/kanálů |Jedinečný s ve službě data factory. Názvy jsou velká a malá písmena. |<ul><li>Maximální počet znaků v názvu tabulky: 260.</li><li>Názvy objektů musí začínat písmenem, číslo nebo podtržítko (_).</li><li>Nejsou povolené tyto znaky: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Skupina prostředků |Jedinečná napříč Microsoft Azure. Názvy jsou velká a malá písmena. |<ul><li>Maximální počet znaků: 1 000.</li><li>Název může obsahovat písmena, číslice a tyto znaky: "-", "_",","a"."</li></ul> |

## <a name="next-steps"></a>Další kroky
Informace o vytvoření objektů pro vytváření dat pomocí následujících podrobné insturctions v [rychlý start: Vytvořte objekt pro vytváření dat](quickstart-create-data-factory-powershell.md) článku. 