---
title: "Azure zásady správy rozhraní API-ukázka – chyby odesílání do Stackify pro protokolování | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak přidat zásadu protokolování chyba k odesílání chyb do Stackify pro protokolování..."
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
ms.openlocfilehash: 34319e43789d0f2c7d3e48a0277926eb598e88fc
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="send-errors-to-stackify-for-logging"></a>Odeslání chyb při Stackify pro protokolování

Tento článek ukazuje ukázkové zásady správy rozhraní API služby Azure, který ukazuje, jak přidat zásadu protokolování chyba k odesílání chyb do Stackify pro protokolování. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **při chybě** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

