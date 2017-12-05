---
title: "Zásady správy Azure API ukázkový – přidání možností do back-end službu | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak pro přidání možností do back-end službu. Například přijměte název místě, ne na zeměpisnou šířku a délku předpovědi počasí rozhraní API."
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
ms.openlocfilehash: d500df0f0e48134ac9c1397795d65706d2e56ff9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="add-capabilities-to-a-backend-service"></a>Přidávání funkcí do back-end službu

Tento článek ukazuje Azure API management zásad ukázka, které ukazuje, jak pro přidání možností do back-end službu. Například přijměte název místě, ne na zeměpisnou šířku a délku předpovědi počasí rozhraní API. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

