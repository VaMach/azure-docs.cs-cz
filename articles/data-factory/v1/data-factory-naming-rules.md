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
ms.date: 10/15/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 2f4979a07db794f012ba602ab65ac54872003027
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - pravidla po pojmenování
> [!NOTE]
> Tento článek se týká verze 1 služby Data Factory, která je obecně dostupná (GA). Pokud používáte verze 2 služby Data Factory, který je ve verzi preview, najdete v části [pojmenování pravidla v datové továrně verze 2](../naming-rules.md).

Následující tabulka obsahuje pravidla pojmenování artefaktů služby Data Factory.

| Name (Název) | Jedinečnost názvu | Ověřovací kontroly |
|:--- |:--- |:--- |
| Data Factory |Jedinečná napříč Microsoft Azure. Názvy jsou velká a malá písmena, který je `MyDF` a `mydf` odkazovat na stejné služby data factory. |<ul><li>Každý objekt pro vytváření dat je vázaný na přesně jedno předplatné.</li><li>Názvy objektů musí začínat písmenem nebo číslicí a může obsahovat pouze písmena, číslice a pomlčky (-) znaků.</li><li>Každý znak pomlčka (-) musí být okamžitě a následnou písmenem nebo číslem. Po sobě jdoucí pomlčky nejsou povolené v názvech kontejneru.</li><li>Název může být 3 až 63 znaků dlouhý.</li></ul> |
| Propojených služeb/tabulek/kanálů |Jedinečný s ve službě data factory. Názvy jsou velká a malá písmena. |<ul><li>Maximální počet znaků v názvu tabulky: 260.</li><li>Názvy objektů musí začínat písmenem, číslo nebo podtržítko (_).</li><li>Nejsou povolené tyto znaky: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Skupina prostředků |Jedinečná napříč Microsoft Azure. Názvy jsou velká a malá písmena. |<ul><li>Maximální počet znaků: 1 000.</li><li>Název může obsahovat písmena, číslice a tyto znaky: "-", "_",","a"."</li></ul> |

