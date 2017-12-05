---
title: "Zásady správy Azure API ukázkový - přidat hlavičku předané | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak přidat hlavičku předané v příchozí žádosti o umožňuje rozhraní API pro vytvoření správné adresy URL back-end."
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
ms.openlocfilehash: cc2df914532b6cda37c951b65b243e90b63d57cb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="add-a-forwarded-header"></a>Přidat hlavičku předané

Tento článek ukazuje rozhraní API služby Azure správy zásad vzorku, který ukazuje, jak přidat hlavičku předané v příchozí žádosti o umožňuje rozhraní API pro vytvoření správné adresy URL back-end. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="code"></a>Kód

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)
