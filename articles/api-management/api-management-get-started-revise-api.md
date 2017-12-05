---
title: "Revize můžete změnit pomocí pevné bezpečně v Azure API Management | Microsoft Docs"
description: "Postupujte podle kroků tohoto kurzu se dozvíte, jak provádět změny pevné pomocí revize ve službě API Management."
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
ms.openlocfilehash: 50d7ac17faebb34f1a1f9a3259aa0196950391d9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Provedení změn pevné bezpečně použijte revize
Pokud vaše rozhraní API je připraven k odeslání a začne být používají vývojáři, musíte obvykle postará v provádění změn pro toto rozhraní API a současně nechcete přerušit volající rozhraní API. Je také užitečné umožnit vývojáři vědět o provedené změny. Jsme to udělat pomocí Azure API Management **revize**. Další informace najdete v tématu [verzích & revize](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) a [Správa verzí rozhraní API pomocí Azure API Management](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Přidat novou revizi
> * Změnit pevné vaší revize
> * Zkontrolujte aktuální vaší revize a přidání položky protokolu změn
> * Procházet portál pro vývojáře a podívejte se změny protokol změn

![Protokol změn na portálu pro vývojáře](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Požadavky

+ Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).
+ Navíc kurzu: [importu a publikování vašeho prvního rozhraní API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-revision"></a>Přidat novou revizi

1. Vyberte **rozhraní API** stránky.
2. Vyberte **konference API** ze seznamu rozhraní API (nebo jiných rozhraní API, do které chcete přidat revize).
3. Klikněte **revize** kartě v nabídce v horní části stránky.
4. Vyberte **+ přidat revize**

    > [!TIP]
    > Můžete také **přidat revize** v místní nabídce (**...** ) rozhraní API.
    
    ![Revize nabídce v horní části obrazovky](media/api-management-getstarted-revise-api/TopMenu.PNG)

5. Zadejte popis pro novou revizi, abyste mějte na paměti, čemu se bude používat pro.
6. Vyberte **vytvořit**
7. Teď se vytvoří nová revize.

    > [!NOTE]
    > Původní rozhraní API zůstane v **revize 1**. Toto je revize, které vaši uživatelé nadále volat, dokud se nerozhodnete aby různých revize se aktuální.

## <a name="make-non-breaking-changes-to-your-revision"></a>Změnit pevné vaší revize

1. Vyberte **konference API** ze seznamu rozhraní API.
2. Vyberte **návrhu** karty v horní části obrazovky.
3. Všimněte si, že **revize selektor** (přímo nad karty Návrh) ukazuje vaše aktuální revize jako **revize 2**.

    > [!TIP]
    > Pomocí modulu pro výběr revize přepínat mezi jednotlivými verzemi, které chcete pracovat.

4. Vyberte **+ operace přidání**.
5. Nastavit nové operaci být **POST**a název a zobrazovaný název operace jako **testování**
6. **Uložit** novou operaci.
7. Nyní jsme provedli změnu **revize 2**. Použití **revize selektor** v horní části stránky a přepněte zpět na **revize 1**.
8. Všimněte si, že vaše nové operaci se nezobrazí v **revize 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Zkontrolujte aktuální vaší revize a přidání položky protokolu změn
1. Vyberte **revize** kartě v nabídce v horní části stránky.

    ![V nabídce revize na obrazovce revize.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
1. Otevřete místní nabídky (**...** ) pro **revize 2**.
2. Vyberte **změnit na aktuální**. Zkontrolujte **Post do veřejného změnit protokolu pro toto rozhraní API**, pokud chcete odesílat poznámky o této změně.
3. Vyberte **Post do veřejného změnu protokolu pro toto rozhraní API**
4. Zadejte popis pro vaše změny, která vývojářům zobrazit, například **testování revize. Přidání nové "test" operace.**
5. **Revize 2** je aktuální.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Procházet portál pro vývojáře a podívejte se změny protokol změn
1. Na portálu Azure vyberte **rozhraní API**
2. Vyberte **portál pro vývojáře** v hlavní nabídce.
3. Vyberte **rozhraní API**a potom vyberte **konference API**.
4. Všimněte si nové **testování** operace je nyní k dispozici.
5. Vyberte **historie změn rozhraní API** uvedený pod název rozhraní API.
6. Všimněte si, že položky protokolu vaše změny se zobrazí v tomto seznamu.

    ![Portál pro vývojáře](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Přidat novou revizi
> * Změnit pevné vaší revize
> * Zkontrolujte aktuální vaší revize a přidání položky protokolu změn
> * Procházet portál pro vývojáře a podívejte se změny protokol změn

Přechodu na další kurz:

> [!div class="nextstepaction"]
> [Publikovat více verzí rozhraní API](api-management-get-started-publish-versions.md)