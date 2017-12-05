---
title: "Azure zásady správy rozhraní API-ukázka - obsah odpovědi filtr | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak filtrovat elementy data z datové části odpovědi na základě produktu přidružený k požadavku."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 2acaad9e97f18cca22893c8c41ead4533ac5ae85
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="filter-response-content"></a>Filtrování obsahu odpovědi

Tento článek ukazuje rozhraní API služby Azure správy zásad vzorku, který ukazuje, jak filtrovat elementy data z datové části odpovědi na základě produktu přidružený k požadavku. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **odchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Filter response content based on product name.policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

