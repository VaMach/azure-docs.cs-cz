---
title: "Publikování verze rozhraní API pomocí Azure API Management | Microsoft Docs"
description: "Postupujte podle kroků tohoto kurzu se dozvíte, jak publikovat více verzí ve službě API Management."
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
ms.openlocfilehash: d63bdd3110f5c5db3e7bfec424644fdbc8d8d90c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="publish-multiple-versions-of-your-api"></a>Publikovat více verzí rozhraní API 

Existují časy, kdy je nepraktické tak, aby měl volající všechny na používání rozhraní API přesně stejnou verzi. Někdy budete chtít publikování nové nebo jiné funkce rozhraní API pro některé uživatele, zatímco jiné chcete přilepit s rozhraním API, která aktuálně funguje pro ně. Volající chcete provést upgrade na novější verzi, chtějí mohli učinit pomocí snadno pochopit přístup.  Jsme můžete to provést pomocí **verze** v Azure API Management. Další informace najdete v tématu [verzích & revize](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Přidat novou verzi do existujícího rozhraní API
> * Vyberte verzi schématu
> * Přidat verze pro určitý produkt
> * Procházet portál pro vývojáře verzi

![Verze zobrazený na portálu pro vývojáře](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Požadavky

+ Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).
+ Navíc kurzu: [importu a publikování vašeho prvního rozhraní API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-version"></a>Přidat novou verzi

![Rozhraní API kontextovou nabídku - přidat verze](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Vyberte **konference API** ze seznamu rozhraní API.
2. Vyberte příslušnou kontextovou nabídku (**...** ) vedle sebe.
3. Vyberte **+ přidat verze**.

    > [!TIP]
    > Verze může být rovněž povolena při prvním vytvoření nové rozhraní API – vyberte **verze toto rozhraní API?** na **přidat rozhraní API** obrazovky.

## <a name="choose-a-versioning-scheme"></a>Volba schématu Správa verzí

Azure API Management můžete zvolit způsob, ve kterém můžete zadat, která verze rozhraní API chtějí volajícím povolit. To uděláte tak, že zvolíte **schéma správy verzí**. Toto schéma může být buď **cestu, Hlavička nebo dotaz řetězec**. V našem příkladu používáme cestu.

![Přidat verze obrazovky](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Nechte **cesta** vybraný jako vaše **schéma správy verzí**.
2. Přidat **v1** jako vaše **identifikátor verze**.

    > [!TIP]
    > Pokud vyberete **záhlaví** nebo **řetězec dotazu** jako schéma Správa verzí, budete muset zadat další hodnotu – název hlavičky, nebo parametr řetězce dotazu.

3. Zadejte popis, nechcete-li.
4. Vyberte **vytvořit** nastavit novou verzi.
5. Pod **Big API konference** v rozhraní API seznamu, uvidíte teď dvě odlišné API - **původní**, a **v1**.

    ![Verze uvedené v části rozhraní API na portálu Azure](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Pokud přidáte na verzi neobsahují rozhraní API, vždy vytvoříme **původní** pro vás - reagovat na výchozí adresy URL. Tím se zajistí, že nejsou žádné existující volající přerušený proces přidávání na verzi. Pokud vytvoříte nové rozhraní API s verzemi, které jsou povolené na začátku, původní se nevytvoří.

6. Nyní můžete upravovat a konfigurovat **v1** jako rozhraní API, které jsou oddělené na **původní**. Změny jednu verzi nemají vliv jiné.

## <a name="add-the-version-to-a-product"></a>Přidat verze pro určitý produkt

Pro volající zobrazíte nové verze, musí být přidaný do **produktu** (produkty nedědí z nadřazeného verzí).

1. Vyberte **produkty** na stránce Správa služby.
2. Vyberte **neomezená**.
3. Vyberte **rozhraní API**.
4. Vyberte **Přidat**.
5. Vyberte **konference rozhraní API, verze v1**.
6. Vraťte se na stránku Správa služeb a vyberte **rozhraní API**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Procházet portál pro vývojáře verzi

1. Vyberte **portál pro vývojáře** v hlavní nabídce.
2. Vyberte **rozhraní API**, Všimněte si, že **konference API** ukazuje **původní** a **v1** verze.
3. Vyberte **v1**.
4. Upozornění **adresa URL požadavku** první operace v seznamu. Ukazuje, že cesta adresy URL rozhraní API obsahuje **v1**.

    ![Rozhraní API kontextovou nabídku - přidat verze](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Přidat novou verzi do existujícího rozhraní API
> * Vyberte verzi schématu 
> * Přidat verze pro určitý produkt
> * Procházet portál pro vývojáře verzi

Přechodu na další kurz:

> [!div class="nextstepaction"]
> [Upgrade a škálování](upgrade-and-scale.md)