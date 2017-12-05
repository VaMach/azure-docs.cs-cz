---
title: "Azure zásady správy rozhraní API-ukázka - vygenerovat sdílený přístupový podpis | Microsoft Docs"
description: "Azure API management zásad ukázka - ukazuje, jak vygenerovat sdílený přístupový podpis pomocí výrazů a předání požadavku do úložiště Azure zásadám přepisování uri..."
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
ms.openlocfilehash: e1f17f9f4e17a3eebb55e4ec1905aec19a2165a5
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="generate-shared-access-signature"></a>Vygenerovat sdílený přístupový podpis

Tento článek ukazuje Azure API management zásad ukázka, které ukazuje, jak vygenerovat [sdíleného přístupového podpisu](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1) pomocí výrazů a předání požadavku do úložiště Azure zásadám přepisování uri. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

