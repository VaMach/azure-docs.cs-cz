---
title: "Postup vytvoření a publikování produktu v Azure API Management"
description: "Naučte se vytvářet a publikovat produkty ve službě Azure API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 89e1115291fbb2ba3499801981b70e10eb23eb94
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="create-and-publish-a-product"></a>Vytvoření a publikování produktu  

Ve službě Azure API Management produkt obsahuje jeden nebo více rozhraní API a také kvóty využití a podmínky použití. Po publikování produktu vývojáři můžou přihlásit k produktu a začít používat rozhraní API produktu.  

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření a publikování produktu
> * Přidání rozhraní API do produktu

![Přidání produktu](media/api-management-howto-add-products/added-product.png)

## <a name="prerequisites"></a>Požadavky

+ Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).
+ Navíc kurzu: [importu a publikování vašeho prvního rozhraní API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-and-publish-a-product"></a>Vytvoření a publikování produktu

1. Klikněte na **produkty** v nabídce na levé straně zobrazíte **produkty** stránky.
2. Klikněte na tlačítko **+ produktu**.

    ![Přidání produktu](media/api-management-howto-add-products/add-product.png)

    Když přidáte produktu, budete muset zadat následující informace: 

    |Name (Název)|Popis|
    |---|---|
    |Zobrazované jméno|Název, jako je má být zobrazen v **portál pro vývojáře**.|
    |Name (Název)|Popisný název produktu.|
    |Popis|**Popis** pole umožňuje poskytují podrobné informace o produktu, například jeho účel, poskytuje přístup k rozhraní API a další užitečné informace.|
    |Stav|Stiskněte klávesu **publikováno** Pokud chcete publikovat produktu. Předtím, než je možné volat rozhraní API v produktu, musí být publikován produktu. Ve výchozím nastavení jsou nové produkty jsou publikování a jsou viditelné pouze pro **správci** skupiny.|
    |Vyžaduje schválení|Zkontrolujte **vyžadovat schválení předplatného** Pokud chcete, aby správci zkontrolovat a následně přijímal nebo odmítal předplatné pokusy o tohoto produktu. Pokud pole není zaškrtnuto, pokusy o odběru jsou schvalovat automaticky. |
    |Limit počtu předplatného|Chcete-li omezit počet více souběžných předplatných, zadejte limitu předplatného. |
    |Právní podmínky|Můžete zahrnout podmínky použití pro produkt, které odběratele musí přijmout, abyste mohli používat produkt.|
    |Rozhraní API|Produkty jsou přidružení jeden nebo více rozhraní API. Může obsahovat několik rozhraní API a poskytněte jim pro vývojáře prostřednictvím portálu pro vývojáře. <br/> Během vytváření produktů můžete přidat existujícího rozhraní API. Můžete přidat rozhraní API do produktu později, buď z produktů **nastavení** stránky nebo při vytváření rozhraní API.|<br/>Vývojáři musí nejdřív přihlásit k odběru produktu získat přístup k rozhraní API. Pokud se přihlášení k odběru získají předplatné klíč, který je vhodný pro jakéhokoli rozhraní API v produktu.<br/> Pokud jste vytvořili instanci APIM, jste správcem již, takže jsou přihlášení k odběru každého produktu ve výchozím nastavení.|

3. Klikněte na tlačítko **Uložit** k vytvoření nového produktu.

### <a name="add-more-configurations"></a>Přidat další konfigurace

Můžete pokračovat v konfiguraci produktu po uložení výběrem **nastavení** kartě. 

Zobrazit nebo přidat odběratele z produktu **odběry** kartě.

Nastavit viditelnost produktu pro vývojáře nebo hosta z **řízení přístupu** kartě.

## <a name="add-apis"></a>Přidat rozhraní API pro určitý produkt

Produkty jsou přidružení jeden nebo více rozhraní API. Může obsahovat několik rozhraní API a poskytněte jim pro vývojáře prostřednictvím portálu pro vývojáře. Během vytváření produktů můžete přidat existujícího rozhraní API. Můžete přidat rozhraní API do produktu později, buď z produktů **nastavení** stránky nebo při vytváření rozhraní API.

Vývojáři musí nejdřív přihlásit k odběru produktu získat přístup k rozhraní API. Pokud se přihlášení k odběru získají předplatné klíč, který je vhodný pro jakéhokoli rozhraní API v produktu. Pokud jste vytvořili instanci APIM, jste správcem již, takže jsou přihlášení k odběru každého produktu ve výchozím nastavení.

### <a name="add-an-api-to-an-existing-product"></a>Přidání rozhraní API pro stávající produkt

1. Vyberte produkt.
2. Vyberte kartu rozhraní API.
3. Klikněte na tlačítko **+ API**.
4. Zvolte rozhraní API a klikněte na **vytvořit**.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření a publikování produktu
> * Přidání rozhraní API do produktu

Přechodu na další kurz:

> [!div class="nextstepaction"]
> [Vytvoření prázdné rozhraní API a model odpovědi rozhraní API](mock-api-responses.md)
