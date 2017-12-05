---
title: "Azure zásady správy rozhraní API-Ukázka – implementace X-proti útokům CSRF vzor | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak implementovat X proti útokům CSRF vzor používá mnoho rozhraní API. V tomto příkladu je konkrétní k bráně SAP."
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
ms.openlocfilehash: a99ff25f7f8a3851936ba8d28e76eff98b1b0458
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="implement-x-csrf-pattern"></a>Implementace X proti útokům CSRF – vzor

Tento článek ukazuje rozhraní API služby Azure správy zásad vzorku, který ukazuje, jak implementovat X proti útokům CSRF vzor používá mnoho rozhraní API. V tomto příkladu je konkrétní k bráně SAP. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get X-CSRF token from SAP gateway using send request policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

