
>[!NOTE]
>Pro Log Analytics se dřív používal název Operational Insights.
>
>

Pro prostředky Log Analytics na jedno předplatné platí tato omezení:

| Prostředek | Výchozí omezení | Komentáře
| --- | --- | --- |
| Počet bezplatných pracovních prostorů na předplatné | 10 | Toto omezení nejde zvýšit. |
| Počet placených pracovních prostorů na předplatné | Není dostupné. | Jste omezení počtem prostředků v rámci skupiny prostředků a počtem skupin prostředků na předplatné. | 


Na pracovní prostory Log Analytics se vztahují na následující omezení:

|  | Free | Standard | Premium | Standalone | OMS |
| --- | --- | --- | --- | --- | --- |
| Objem shromážděných dat za den |500 MB<sup>1</sup> |Žádný |Žádný | Žádný | Žádný
| Doba uchování dat |7 dní |1 měsíc |12 měsíců | 1 měsíc<sup>2</sup> | 1 měsíc<sup>2</sup>|

<sup>1</sup> Když zákazník dosáhne svého denního limitu přenosu dat (500 MB), analýza dat se zastaví a bude pokračovat na začátku dalšího dne. Den vychází ze standardu UTC.

<sup>2</sup> Dobu uchování dat pro cenové plány Standalone a OMS je možné prodloužit na 730 dnů.

| Kategorie | Omezení | Komentáře
| --- | --- | --- |
| Rozhraní API kolekce dat | Maximální velikost pro jeden příspěvek je 30 MB.<br>Maximální velikost pro hodnoty pole je 32 kB. | Rozdělte větší svazky na více příspěvků.<br>Pole delší než 32 kB se oříznou. |
| Search API | 5000 záznamů vrácených pro neagregovaná data<br>500000 záznamů vrácených pro agregovaná data | Agregovaná data znamenají vyhledávání, které zahrnuje příkaz `measure`.
 


<!--HONumber=Feb17_HO3-->


