---
title: "Azure zásady správy rozhraní API-ukázka - trasy na základě požadavku na velikosti jeho obsahu | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak pro směrování požadavků na základě velikosti své subjekty."
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
ms.openlocfilehash: 0558f82a31015af91df135b6a0d4c4050049d11d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="route-the-request-based-on-the-size-of-its-body"></a>Směrování požadavku na základě velikosti jeho obsahu

Tento článek ukazuje rozhraní API služby Azure správy zásad vzorku, který ukazuje, jak pro směrování požadavků na základě velikosti své subjekty. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Route requests based on size.policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

