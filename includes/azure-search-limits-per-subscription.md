Můžete vytvořit více služeb v rámci předplatného, každé z nich zřízené v konkrétní úroveň, omezena pouze počet služeb v každé úrovni povoleno. Například můžete vytvořit až 12 services na úroveň Basic a jiné 12 services na vrstvě S1 v rámci stejného předplatného. Další informace o úrovních najdete v tématu [zvolte SKU nebo vrstvy pro službu Azure Search](../articles/search/search-sku-tier.md).

Omezení maximální služby může být vyvolána při požadavku. Pokud potřebujete další služby v rámci stejného předplatného, kontaktujte podporu Azure.

| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD <sup>1</sup> |
| --- | --- | --- | --- | --- | --- | --- |
| Maximální služby |1 |12 |12 |6 |6 |6 |
| Maximální měřítko v SU <sup>2</sup> |NENÍ K DISPOZICI <sup>3</sup> |3 SU <sup>4</sup> |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> S3 HD nepodporuje [indexery](../articles/search/search-indexer-overview.md) v tuto chvíli. 

<sup>2</sup> jednotek služby vyhledávání (SU) jsou fakturace jednotek, které jsou přidělené jako buď *repliky* nebo *oddílu*. Potřebujete i prostředky pro úložiště, indexování a operace dotazů. Další informace o tom, jak se vypočítávají jednotek služby vyhledávání plus graf platné kombinace, které zůstávají v rámci maximální limit najdete v tématu [škálovat prostředek úrovně pro úlohy dotazovací a indexovací](../articles/search/search-capacity-planning.md). 

<sup>3</sup> Free je založen na sdílené prostředky využívané víc odběratelům. Na této úrovni neexistují vyhrazené prostředky pro jednotlivé odběratele. Z tohoto důvodu maximální měřítko je označeno není k dispozici.

<sup>4</sup> basic má pevnou jeden oddíl. Na této úrovni další služby SUs slouží pro přidělení více replik pro úlohy vyšší dotazu.

