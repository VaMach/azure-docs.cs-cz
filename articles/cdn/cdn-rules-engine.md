---
title: "Potlačení chování HTTP pomocí stroj pravidel Azure CDN | Microsoft Docs"
description: "Stroj pravidel umožňuje přizpůsobit způsob zpracování požadavků HTTP pomocí Azure CDN, třeba blokování doručování určité typy obsahu, definovat zásady ukládání do mezipaměti a změnit hlavičky protokolu HTTP."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: abfe283476206b181018d187675b47112dc5ad2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a>Potlačení chování HTTP pomocí stroj pravidel Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Přehled
Stroj pravidel umožňuje přizpůsobit způsob zpracování požadavků HTTP, jako je například blokování doručování určité typy obsahu, definování zásady ukládání do mezipaměti a úpravy hlaviček protokolu HTTP.  V tomto kurzu se ukazují, že vytvoření pravidla, která se změní chování ukládání do mezipaměti CDN prostředků.  Také obsahu videa v není k dispozici "[viz také](#see-also)" oddílu.

   > [!TIP] 
   > Odkaz na syntaxi podrobně, najdete v části [referenční dokumentace pravidel modul](cdn-rules-engine-reference.md).
   > 


## <a name="tutorial"></a>Kurz
1. Okno profil CDN, klikněte **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    Otevře se na portálu pro správu CDN.
2. Klikněte na **HTTP velké** kartě, za nímž následuje **stroj pravidel**.
   
    Zobrazí se možnosti pro nové pravidlo.
   
    ![Nové možnosti pravidlo CDN](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > Pořadí, ve kterém jsou uvedeny víc pravidel, ovlivňuje způsob zpracování. Následující pravidlo může přepsat akce zadané předchozí pravidlem.
   > 
   > 
3. Zadejte název do pole **název nebo popis** textové pole.
4. Určete typ požadavky, bude pravidlo platí pro.  Ve výchozím nastavení **vždy** je vybraná podmínka shodu.  Budete používat **vždy** pro účely tohoto kurzu, takže políčko nechte který vybrali.
   
   ![Stav shody CDN](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > Existuje mnoho typů odpovídal podmínky, které jsou k dispozici v rozevírací nabídce.  Aktuálně vybrané podmínky podrobně vysvětluje kliknutím na ikonu blue informační nalevo od podmínky shody.
   > 
   >  Úplný seznam podmíněné výrazy podrobně najdete v tématu [pravidla modul podmíněné výrazy](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Úplný seznam podmínek shodu podrobně, najdete v části [podmínky odpovídají stroj pravidel](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
5. Klikněte  **+**  vedle položky **funkce** přidat nové funkce.  V rozevírací nabídce na levé straně vyberte **Force interní Max-Age**.  Do textového pole, která se zobrazí, zadejte **300**.  Ponechejte zbývající výchozí hodnoty.
   
   ![Funkce CDN](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Jako s shodu podmínky, kliknutím na ikonu blue informační nalevo od novou funkci se zobrazí podrobnosti o této funkci.  U **Force interní Max-Age**, jsme jsou přepsání assetu **Cache-Control** a **Expires** hlavičky řídit, kdy bude hraničního uzlu CDN aktualizace asset z tohoto počátku.  Náš příklad 300 sekund znamená, že CDN hraničního uzlu po dobu 5 minut před aktualizací asset z jeho počátku mezipaměti asset.
   > 
   > Úplný seznam funkcí podrobně, najdete v části [podrobnosti o funkcích stroj pravidel](cdn-rules-engine-reference-features.md).
   > 
   > 
6. Klikněte **přidat** tlačítko pro uložení nové pravidlo.  Nové pravidlo je nyní čeká na schválení. Jakmile byla schválena, stav se změní z **čekající XML** k **Active XML**.
   
   > [!IMPORTANT]
   > Změny pravidel může trvat až 90 minut rozšíří v rámci CDN.
   > 
   > 

## <a name="see-also"></a>Viz také
* [Přehled Azure CDN](cdn-overview.md)
* [Referenční dokumentace pravidel modulu](cdn-rules-engine-reference.md)
* [Stav shody motoru pravidla](cdn-rules-engine-reference-match-conditions.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkce modulu pravidla](cdn-rules-engine-reference-features.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel](cdn-rules-engine.md)
* [Azure pátek: Azure CDN výkonné nové prémiové funkce](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)