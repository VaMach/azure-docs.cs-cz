---
title: "Vytvoření služby Azure Search na portálu | Microsoft Docs"
description: "Zřídit služby Azure Search na portálu."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/07/2017
ms.author: heidist
ms.openlocfilehash: eaf317b42026298cc42edcc907bc48169f869460
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="create-an-azure-search-service-in-the-portal"></a>Vytvoření služby Azure Search v portálu.

Naučte se vytvořit nebo zřídit služby Azure Search na portálu. Prostředí PowerShell pokyny najdete v tématu [spravovat Azure Search pomocí prostředí PowerShell](search-manage-powershell.md).

## <a name="subscribe-free-or-paid"></a>Přihlášení k odběru (volné nebo placené)

[Otevřít bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) a použijte bezplatný kredit k vyzkoušení placené služby Azure. Až kredity vyčerpáte, si účet nechat a dál používat bezplatné služby Azure, jako jsou weby. Platební karty se nikdy účtovat Pokud explicitně změnit nastavení a požádejte o nestrhne.

Alternativně [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure. 

## <a name="find-azure-search"></a>Najít vyhledávání systému Azure
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. Kliknutím na znaménko plus ("+") v levém horním rohu.
3. Vyberte **Web + mobilní** > **služba Azure Search**.

![](./media/search-create-service-portal/find-search3.png)

## <a name="name-the-service-and-url-endpoint"></a>Název služby a koncový bod adresy URL

Název služby je součástí adresy URL koncového bodu, které rozhraní API jsou vydávány volání: `https://your-service-name.search.windows.net`. Zadejte název vaší služby v **URL** pole. 

Požadavky na název služby:
   * Musí být jedinečný v rámci oboru názvů search.windows.net
   * 2 až 60 znaků.
   * Používat malá písmena, číslice nebo spojovníky ("-")
   * Vyhněte se pomlčky ("-") v první 2 znaky nebo jako poslední jeden znak
   * Žádné po sobě jdoucí pomlčky ("--") kdekoli

## <a name="select-a-subscription"></a>Vyberte předplatné.
Pokud máte více než jedno předplatné, vyberte ten, který má také služby úložiště dat nebo souboru. Vyhledávání systému Azure můžete automaticky rozpoznat úložiště Azure Table a objektů Blob, databáze SQL a Azure Cosmos DB pro indexování prostřednictvím [ *indexery*](search-indexer-overview.md), ale pouze pro služby ve stejném předplatném.

## <a name="select-a-resource-group"></a>Vybrat skupinu prostředků
Skupina prostředků je kolekce služeb Azure a prostředky, použít společně. Pokud používáte Azure Search při indexování databázi SQL, pak obě služby by měl být například součástí stejné skupiny prostředků.

> [!TIP]
> Odstranění skupiny prostředků se odstraní taky služby v něm. Pro projekty prototypu využívá více služeb třeba umisťovat všechny ve stejné skupině prostředků usnadňuje čištění po dokončení projektu. 

## <a name="select-a-hosting-location"></a>Vyberte umístění pro hostování 
Jako služba Azure může být hostovaný Azure Search v datových centrech po celém světě. Všimněte si, že [ceny se může lišit](https://azure.microsoft.com/pricing/details/search/) zeměpisných údajů.

## <a name="select-a-pricing-tier-sku"></a>Vyberte cenovou úroveň (SKU)
[Služba Azure Search je aktuálně k dispozici v několika cenových úrovní](https://azure.microsoft.com/pricing/details/search/): volné, Basic nebo Standard. Každá úroveň má svou vlastní [kapacity a omezení](search-limits-quotas-capacity.md). V tématu [zvolte cenovou úroveň nebo SKU](search-sku-tier.md) pokyny.

V tomto návodu jsme zvolili na plán úrovně Standard pro naši službu.

Po vytvoření služby nelze změnit cenovou úroveň. Pokud budete později potřebovat vyšší nebo nižší úrovně, budete muset znovu vytvořit službu.

## <a name="create-your-service"></a>Vytvoření služby

Nezapomeňte připnout služby pro snadný přístup k řídicímu panelu při každém přihlášení.

![](./media/search-create-service-portal/new-service3.png)

## <a name="scale-your-service"></a>Škálování služby
To může trvat několik minut pro vytvoření služby (15 minut nebo déle v závislosti na vrstvě). Po zřízení služby, je možné škálovat tak, aby vyhovovala vašim potřebám. Protože jste zvolili na plán úrovně Standard pro služby Azure Search, můžete škálovat služby v dvěma rozměry: repliky a oddíly. Jste nastavili základní vrstvě, můžete použít pouze repliky. Pokud jste zřídili bezplatnou službou, škálování není k dispozici.

***Oddíly*** povolit služby k ukládání a hledání v další dokumenty.

***Repliky*** povolit služby pro zpracování vyšší zatížení vyhledávací dotazy.

> [!Important]
> Služba musí mít [2 repliky smlouva SLA jen pro čtení a 3 repliky pro čtení/zápisu SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Přejděte na stránku služby search na portálu Azure.
2. V levém navigačním podokně vyberte **nastavení** > **škálování**.
3. Pomocí slidebar přidejte repliky nebo oddíly.

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> Každá úroveň má jiný [omezení](search-limits-quotas-capacity.md) na celkový počet jednotek vyhledávání povolené v jedné službě (repliky * oddíly = celkový počet jednotek vyhledávání).

## <a name="when-to-add-a-second-service"></a>Pokud chcete přidat druhý službu

Většina zákazníků použít pouze jednu službu zřídí v vrstvy, která poskytuje [pravým vyrovnávání prostředků](search-sku-tier.md). Jedna služba může být hostitelem více indexů, podléhají [maximální limit vrstvy vyberete](search-capacity-planning.md), s každou indexem izolované z jiné. Ve službě Azure Search požadavků můžete pouze přesměrováni na jeden index, což minimalizuje načítání náhodnému nebo záměrnému dat z jiných indexy v stejnou službu.

I když většina zákazníků používat jenom jedna služba, redundance služby může být nutné v případě provozních požadavků zahrnují následující:

+ Zotavení po havárii (data center výpadek). Služba Azure Search neposkytuje rychlých převzetí služeb při selhání v případě výpadku. Doporučení a pokyny najdete v tématu [služby správy](search-manage.md).
+ Vaše šetření víceklientský modelování má zjistíte, že další služby optimální návrhu. Další informace najdete v tématu [návrhu víceklientský](search-modeling-multitenant-saas-applications.md).
+ Pro globální nasazené aplikace může vyžadovat instanci Azure Search v několika oblastech pro zajištění minimální latence mezinárodní provoz vaší aplikace.

> [!NOTE]
> Ve službě Azure Search nelze oddělit indexování a dotazování zatížení; Proto by nikdy vytvořit více služeb pro oddělenou úlohy. Index je vždy dotaz na službu ve kterém it vytvořil (můžete v jedné službě vytvoření indexu a zkopírujte ho do jiného.).
>

Druhý služby není nutné pro zajištění vysoké dostupnosti. Vysoká dostupnost pro dotazy se dosáhne, když používáte 2 nebo více replik v rámci stejné služby. Aktualizace repliky jsou sekvenční, což znamená, že je alespoň jeden funkční při aktualizaci služby se nasazuje. Další informace o provozu najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Další kroky
Po zřízení služby Azure Search, budete chtít [definujte index](search-what-is-an-index.md) vám umožní nahrát a hledání vaše data. 

> [!div class="nextstepaction"]
> [Jak používat Azure Search v rozhraní .NET](search-howto-dotnet-sdk.md)
