Následující tabulka popisuje všechny hlavní kvót, omezení, výchozí hodnoty a omezení ve službě Azure Scheduler.

| Prostředek | Popis limit |
| --- | --- |
| **Velikost úlohy** |Maximální velikost je 16 kB. Pokud PUT nebo opravy výsledkem úlohu větší než těchto mezních hodnot, vrátí se 400 stavový kód chybný požadavek. |
| **Velikost adresy URL žádosti** |Maximální velikost adrese URL žádosti je 2048 znaků. |
| **Velikost agregační záhlaví** |Velikost maximální agregační záhlaví je 4096 znaků. |
| **Počet záhlaví** |Hlavička maximální počet je 50 hlavičky. |
| **Velikost obsahu** |Text maximální velikost je 8192 znaků. |
| **Interval opakování** |Maximální počet opakování rozpětí je 18 měsíců. |
| **Čas spuštění** |Maximální počet "čas spuštění" je 18 měsíců. |
| **Historie úlohy** |Maximální odpovědi, které jsou uloženy v historii úlohy je 2048 bajtů. |
| **Frekvence** |Kvóta maximální frekvence výchozí je 1 hodina bezplatnou kolekci úloh a 1 minuta v kolekci standardní úlohy. Maximální frekvence se dá nakonfigurovat na kolekci úloh nižší než maximální. Všechny úlohy v kolekci úloh jsou omezené hodnotu nastavenou v kolekci úloh. Pokud se pokusíte vytvořit úlohu s frekvencí vyšší než maximální frekvence na kolekci úloh se nezdaří žádost s 409 konflikt stavový kód. |
| **Úlohy** |Výchozí kvótu maximální počet úloh je 5 úloh v bezplatnou kolekci úloh a 50 úloh v kolekci standardní úlohy. Maximální počet úloh je možné konfigurovat na kolekci úloh. Všechny úlohy v kolekci úloh jsou omezené hodnotu nastavenou v kolekci úloh. Pokud budete chtít vytvořit více úloh než kvóta maximální úlohy, požadavek selže s 409 konflikt stavový kód. |
| **Kolekce úloh** |Maximální počet kolekce úloh podle předplatného je 200 000. |
| **Uchování historie úlohy** |Historie úlohy se uchovávají po dobu až 2 měsíců nebo maximálně posledních 1000 spuštěních. |
| **Uchování dokončené a chybný úlohy** |Dokončené a chybný úlohy jsou uchovávat 60 dnů. |
| **Časový limit** |Je statický časový limit (ne konfigurovat) požadavku 60 sekund pro akce HTTP. Delší spuštění operací postupujte podle asynchronní protokoly HTTP; například vrátit 202 okamžitě však pokračovat v práci na pozadí. |

