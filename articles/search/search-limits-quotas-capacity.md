---
title: "Omezení ve službě Azure Search služby | Microsoft Docs"
description: "Omezení služby používá pro plánování kapacity a maximální limit na požadavky a odpovědi pro službu Azure Search."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 857a8606-c1bf-48f1-8758-8032bbe220ad
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/07/2017
ms.author: heidist
ms.openlocfilehash: 60e63401e3915e62e1ec5ac03cd548c291580b24
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="service-limits-in-azure-search"></a>Omezení služby ve službě Azure Search
Maximální omezuje na úložiště, úlohy a počty indexů, dokumenty a další objekty závisí na tom, zda jste [zřízení Azure Search](search-create-service-portal.md) na **volné**, **základní**, nebo **Standardní** cenová úroveň.

* **Volné** je víceklientské sdílené služby, která se dodává s předplatným Azure. Jedná se o možnost bez dalších nákladů pro existující odběratele, který umožňuje vyzkoušet službu před registrací vyhrazených prostředcích.
* **Základní** poskytuje vyhrazený výpočetní prostředky pro úlohy v produkčním prostředí v menším měřítku.
* **Standardní** běží na vyhrazené počítače s další kapacitou úložiště a zpracování na všech úrovních. Standard je rozdělena na čtyři úrovně: S1, S2, S3 a S3 (S3 HD) s vysokou hustotou.

> [!NOTE]
> Služba se zřídí v konkrétní úroveň. Pokud potřebujete přejít vrstev získat větší kapacitu, je třeba zřídit nové služby (neexistuje žádné místní upgrade). Další informace najdete v tématu [zvolte SKU nebo vrstvě](search-sku-tier.md). Další informace o úpravě kapacity v rámci služby jste už zřízené, najdete v části [škálovat prostředek úrovně pro dotaz a indexování úlohy](search-capacity-planning.md).
>

## <a name="per-subscription-limits"></a>Za limity předplatného
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="per-service-limits"></a>Omezení jednotlivých služeb
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="per-index-limits"></a>Za index omezení
Existuje souvislosti mezi limity pro indexy a omezení na indexery. Zadaný limit 200 indexy, maximální limit pro indexery je také 200 pro stejnou službu.

| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Index: maximální počet polí na index |1000 |100 <sup>1</sup> |1000 |1000 |1000 |1000 |
| Index: maximální vyhodnocování profily pro jednotlivé indexu |100 |100 |100 |100 |100 |100 |
| Index: maximální funkce jeden profil |8 |8 |8 |8 |8 |8 |
| Indexery: maximální indexování zatížení na vyvolání |10 000 dokumentů |Omezeno pouze maximální dokumenty |Omezeno pouze maximální dokumenty |Omezeno pouze maximální dokumenty |Omezeno pouze maximální dokumenty |NENÍ K DISPOZICI <sup>2</sup> |
| Indexery: maximální dobu běhu | 1-3 minuty, <sup>3</sup> |24 hodin |24 hodin |24 hodin |24 hodin |NENÍ K DISPOZICI <sup>2</sup> |
| Indexer objektů blob: velikost maximální objektu blob, MB |16 |16 |128 |256 |256 |NENÍ K DISPOZICI <sup>2</sup> |
| Indexer objektů blob: maximální počet znaků z objektu blob extrahovat obsahu |32,000 |64,000 |4 miliony |4 miliony |4 miliony |NENÍ K DISPOZICI <sup>2</sup> |

<sup>1</sup> úroveň basic je určená jenom SKU s nižší limit 100 polí na index.

<sup>2</sup> S3 HD aktuálně nepodporuje indexery. Pokud máte naléhavá potřeba pro tuto funkci, kontaktujte podporu Azure.

<sup>3</sup> maximální dobu spuštění indexeru pro úroveň Free je 3 minut zdroje blob a pro všechny ostatní zdroje dat 1 minuta.

## <a name="document-size-limits"></a>Omezení velikosti dokumentu
| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Velikosti jednotlivých dokumentů na Index rozhraní API |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |

Odkazuje na dokument maximální velikost při volání rozhraní API Index. Velikost dokument je ve skutečnosti omezení velikosti obsahu žádosti Index rozhraní API. Vzhledem k tomu, že můžete předat dávky více dokumentů do indexu API najednou, omezení velikosti ve skutečnosti závisí na tom, kolik dokumenty jsou v dávce. Dokument maximální velikost dávky s jedním dokumentem, je 16 MB JSON.

Snížení velikosti dokumentu, nezapomeňte vyloučit-dotazovatelný data z požadavku. Obrázky a další binární data nejsou přímo dotazovatelný a by neměly být uloženy v indexu. K integraci dat bez dotazovatelné výsledky hledání, definujte není prohledávatelné pole, které obsahuje adresu URL odkaz na prostředek.

## <a name="workload-limits-queries-per-second"></a>Omezení zatížení (dotazy za sekundu)
| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| QPS |Není k dispozici |Přibližně 3 na repliku |Přibližně 15 na repliku |Přibližně 60 na repliku |Více než 60 na repliku |Více než 60 na repliku |

Dotazy na za sekundu (QPS) je přibližný podle heuristiky, pomocí úloh simulované a skutečnou zákazníků odvození odhadované hodnoty. Určit přesnou propustnost QPS se liší v závislosti na vašich dat a povaha dotazu.

I když jsou výše uvedeného hrubý odhady, skutečná rychlost je obtížné určit, zejména v bezplatné sdílené služby kde propustnost je založena na dostupnou šířku pásma a soutěž o systémové prostředky. V úrovni Free výpočetní a úložnou kapacitu jsou sdíleny více odběrateli, takže QPS pro vaše řešení bude vždy lišit v závislosti na tom, kolik dalších zatížení jsou spuštěné ve stejnou dobu.

Na standardní úrovni můžete odhadnout QPS přesněji vzhledem k tomu, že budete mít kontrolu nad více parametrů. Najdete v tématu osvědčené postupy [spravovat řešení vyhledávání](search-manage.md) pokyny o tom, jak vypočítat QPS pro zatížení.

## <a name="api-request-limits"></a>Omezení žádostí o rozhraní API
* Maximálně 16 MB každý požadavek <sup>1</sup>
* Maximální délka adresy URL 8 KB
* Maximální 1000 dokumentů na jednu dávku indexu nahrávání, sloučí nebo odstraní
* Maximální 32 polí v klauzuli $orderby
* Maximální hledání termín velikost je 32 766 bajtů (32 KB minus 2 bajtů) textu ve formátu UTF-8

<sup>1</sup> ve službě Azure Search je text požadavku podléhá horní limit 16 MB, nastavení praktické omezení na obsah jednotlivých polí nebo kolekce, které nejsou v opačném případě omezené teoretické omezení (viz [podporované dat typy](https://msdn.microsoft.com/library/azure/dn798938.aspx) Další informace o omezení a pole složení).

## <a name="api-response-limits"></a>Omezení odpovědi rozhraní API
* Maximální 1000 dokumenty vrácené na stránku výsledků hledání
* Maximální 100 návrhy vrátí na žádost navrhnout rozhraní API

## <a name="api-key-limits"></a>Omezení klíč rozhraní API
Klíče API Key se používají pro ověřování služby. Existují dva typy. Klíče správce se zadaným v hlavičce žádosti a udělit přístup úplné pro čtení a zápis ke službě. Klíče dotazu jsou jen pro čtení, zadaná na adresu URL a obvykle se distribuují klientským aplikacím.

* Maximálně 2 klíče správce pro službu
* Maximální počet klíčů 50 dotaz na službu
