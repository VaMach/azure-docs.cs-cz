---
title: "Azure zásady správy rozhraní API-ukázka - OAuth2 použití k ověřování mezi bránou rozhraní a back-end | Microsoft Docs"
description: "Azure API management zásad ukázka - demonstruje použití OAuth2 pro ověřování mezi bránou rozhraní a back-end. Ukazuje, jak získat přístupový token z AAD a předá je na back-end."
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
ms.openlocfilehash: e0aeec66f23033f916c782c8a895e725b0735b62
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>OAuth2 pro autorizaci použít mezi bránou rozhraní a back-end

Tento článek ukazuje rozhraní API služby Azure správy zásad vzorku, který ukazuje, jak používat OAuth2 pro ověřování mezi bránou rozhraní a back-end. Ukazuje, jak získat přístupový token z AAD a předá je na back-end. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get OAuth2 access token from AAD and forward it to the backend.xml)]

## <a name="next-steps"></a>Další kroky

Další informace o zásadách APIM:

+ [Zásady transformace](../api-management-transformation-policies.md)
+ [Ukázky zásad](../policy-samples.md)

