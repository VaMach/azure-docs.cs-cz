---
title: "Revize se používá k aktualizaci vašeho rozhraní API v Azure API Management | Microsoft Docs"
description: "Postupujte podle kroků tohoto kurzu se dozvíte, jak provádět změny pevné pomocí revize ve službě API Management."
services: api-management
documentationcenter: 
author: mattfarm
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2017
ms.author: apimpm
ms.openlocfilehash: 0d67166a16ae4d640080ad83e7625e594b0dc246
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="make-non-breaking-changes-safely-using-revisions"></a>Proveďte změny pevné bezpečně pomocí revize
Tento kurz popisuje, jak bezpečně provést změny vašeho rozhraní API a komunikaci změnu vaší vývojářům.

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto kurzu, musíte k vytvoření služby API Management a existujícího rozhraní API můžete změnit (místo konferenční API).

## <a name="about-revisions"></a>O revize
Pokud vaše rozhraní API je připraven k odeslání a začne být používají vývojáři, musíte obvykle postará v provádění změn pro toto rozhraní API - tak, aby přerušit volající rozhraní API. Je také užitečné umožnit vývojáři vědět o provedené změny. Jsme to udělat pomocí Azure API Management **revize**.

## <a name="walkthrough"></a>Názorný postup
V tomto návodu jsme přidat novou revizi, operace přidejte do ní, ujistěte se, že revizí aktuální – vytvoření položky protokolu změnit, pokud k tomu.

## <a name="add-a-new-revision"></a>Přidat novou revizi
1. Vyhledejte **rozhraní API** stránku v rámci služby API Management na portálu Azure.
2. Vyberte **konference API** rozhraní API seznamu, pak vyberte **revize** kartě v nabídce v horní části stránky.
3. Vyberte **+ přidat revize**

    > [!TIP]
    > Můžete také **přidat revize** v místní nabídce (**...** ) na volání rozhraní API.
![Revize nabídce v horní části obrazovky](media/api-management-getstarted-revise-api/TopMenu.PNG)

4. Zadejte popis pro novou revizi, abyste mějte na paměti, čemu se bude používat pro.
5. Vyberte **vytvořit**
6. Teď se vytvoří nová revize.

    > [!NOTE]
    > Můžete původní API zůstane v **revize 1**. Toto je revize, které vaši uživatelé budou nadále volat, dokud se nerozhodnete aby různých revize se aktuální.

## <a name="make-non-breaking-changes-to-your-revision"></a>Změnit pevné vaší revize
1. Vyberte **návrhu** karty v horní části obrazovky.
2. Všimněte si, že **revize selektor** (přímo nad karty Návrh) ukazuje vaše aktuální revize jako **revize 2**.

    > [!TIP]
    > Pomocí modulu pro výběr revize přepínat mezi jednotlivými verzemi, které chcete pracovat.

3. Vyberte **+ operace přidání**.
4. Nastavit nové operaci být **POST**a název a zobrazovaný název operace jako **zpětné vazby**
5. **Uložit** novou operaci.
6. Nyní jsme provedli změnu **revize 2**. Použití **revize selektor** v horní části stránky a přepněte zpět na **revize 1**.
7. Všimněte si, že vaše nové operaci se nezobrazí v **revize 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Zkontrolujte aktuální vaší revize a přidání položky protokolu změn
1. Vyberte **revize** kartě v nabídce v horní části stránky.
![V nabídce revize na obrazovce revize.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
2. Otevřete místní nabídky (**...** ) pro **revize 2**.
3. Vyberte **změnit na aktuální**.
![Zkontrolujte aktuální revize a odeslání na protokol změn](media/api-management-getstarted-revise-api/MakeCurrent.PNG)
4. Vyberte **Post do veřejného změnu protokolu pro toto rozhraní API**
5. Zadejte popis pro vaše změny, která vývojářům zobrazit, například **"Přidat nové operaci zpětné vazby."**
6. **Revize 2** je aktuální.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Procházet portál pro vývojáře a podívejte se změny protokol změn
1. Vyberte **portál pro vývojáře** v hlavní nabídce.
2. Vyberte **rozhraní API**a potom vyberte **konference API**.
3. Všimněte si nové **zpětné vazby** operace je nyní k dispozici.
4. Vyberte **historie změn rozhraní API** uvedený pod název rozhraní API.
5. Všimněte si, že položky protokolu vaše změny se zobrazí v tomto seznamu.
![Protokol změn na portálu pro vývojáře](media/api-management-getstarted-revise-api/ChangeLogDevPortal.PNG)

## <a name="next-steps"></a>Další kroky
[Publikovat verzi API pomocí Azure API Management](#api-management-getstarted-publish-versions.md)