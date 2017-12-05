---
title: "Upravit rozhraní API pomocí portálu Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak upravit rozhraní API pomocí rozhraní API správy (APIM)."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/08/2017
ms.author: apimpm
ms.openlocfilehash: 362c36181da706e3fe0a27cc5ab262271c2a1e57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="edit-an-api"></a>Upravit rozhraní API

Kroky v tomto kurzu ukazují, jak upravit rozhraní API pomocí rozhraní API správy (APIM). 

+ Můžete provést přidáním, odstranění, přejmenování operace v instanci APIM. 
+ Můžete upravit vaše rozhraní API swaggeru.

## <a name="prerequisites"></a>Požadavky

+ [Vytvoření instance služby Azure API Management](get-started-create-service-instance.md)
+ [Import a publikování vašeho prvního rozhraní API](import-and-publish.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="edit-an-api-in-apim"></a>Upravit rozhraní API v APIM

![Upravit rozhraní api](./media/edit-api/edit-api001.png)

1. Klikněte **rozhraní API** kartě.
2. Vyberte jednu z rozhraní API, která jste předtím naimportovali.
3. Vyberte **návrhu** kartě.
4. Vyberte operaci, který chcete upravit.
5. Pokud chcete přejmenovat operaci, vyberte **tužky** v **front-endu** okno.

## <a name="update-the-swagger"></a>Aktualizace swagger

Backbend rozhraní API z portálu Azure můžete aktualizovat pomocí následujících kroků:

1. Vyberte **všechny operace**
2. Klikněte na tlačítko tužky v **front-endu** okno.

    ![Upravit rozhraní api](./media/edit-api/edit-api002.png)

    Zobrazí se vaše rozhraní API swaggeru.

    ![Upravit rozhraní api](./media/edit-api/edit-api003.png)

3. Aktualizujte swagger.
4. Stiskněte klávesu **Uložit**.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Ukázky zásad APIM](policy-samples.md)
> [transformace a chránit publikované rozhraní API](transform-api.md)