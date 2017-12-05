---
title: "Zásady správy Azure API ukázkový – autorizace na základě deklarací JWT Access | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak k autorizaci přístupu pro konkrétní metody HTTP do rozhraní API založené na deklaracích identity JWT."
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
ms.openlocfilehash: 15bd99eeda35161b15e110c3bde0b0bb08fe6d64
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="authorize-access-based-on-jwt-claims"></a>Autorizace přístupu na základě deklarací JWT

Tento článek ukazuje rozhraní API služby Azure správy zásad vzorku, který ukazuje, jak k autorizaci přístupu pro konkrétní metody HTTP do rozhraní API založené na deklaracích identity JWT. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Pre-authorize requests based on HTTP method with validate-jwt.policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

