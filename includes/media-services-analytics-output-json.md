Úloha vytvoří výstupní soubor JSON, který obsahuje metadata o zjištěných a sledovaných strany. Metadat obsahuje souřadnice označující umístění řezy, stejně jako číslo ID vzhled určující sledování to jednotlivých. Čísla ID vzhled jsou náchylné k resetování okolností v případě, že dojde ke ztrátě nebo překryté v rámečku, čelní tučné výsledkem některé jednotlivce získávání přiřazeno více ID.

Výstup JSON obsahuje následující atributy:

| Element | Popis |
| --- | --- |
| Verze |Vztahuje se na verzi rozhraní API Video. |
| Index | (Platí pouze pro Azure Media Redactor) definuje index rámečku aktuální událost. |
| Časová osa |"Rysky" za sekundu videa. |
| Posun |Toto je časové posunutí pro časová razítka. Ve verzi 1.0 rozhraní API, Video bude vždy 0. V budoucích scénáře, které podporujeme, tato hodnota může změnit. |
| kmitočet snímků |Počet snímků za sekundu videa. |
| fragmenty |Metadata se blokové až do různých segmentů názvem fragmenty. Každý fragment obsahuje počáteční, doba trvání, číslo intervalu a událostí. |
| start |Čas zahájení první událost v 'rysky'. |
| Doba trvání |Délka fragment v "rysky". |
| Interval |Interval každý záznam události v rámci fragment v "rysky". |
| stránka events |Každá událost obsahuje řezy zjištěn a sledují v rámci této doby trvání. Jde pole událostí. Vnější pole představuje jeden časový interval. Vnitřní pole se skládá z 0 nebo více událostí, které bylo provedeno v tomto bodě v čase. Prázdný závorky [] znamená, že nebyly zjištěny žádné řezy. |
| id |ID řez, který je sledován. Toto číslo může nechtěně změnit, pokud se stane nezjištěné řez. Daný individuální by mělo mít stejné ID v rámci celkového video, ale to nemůže zaručit z důvodu omezení v algoritmus detekce (NF pásmová atd.) |
| x, y |Vlevo nahoře X a Y souřadnice ohraničujícího pole Normalizovaný rozsahu 0,0 až 1,0 písmo. <br/>-X a Y souřadnice jsou relativní na šířku vždycky, takže pokud máte na výšku video (nebo obráceným v případě iOS), budete muset transponuje souřadnice odpovídajícím způsobem. |
| šířky, výšky |Šířka a výška tučné ohraničujícího pole Normalizovaný rozsahu od 0,0 do 1,0. |
| facesDetected |To se nachází na konci výsledky JSON a shrnuje počet řezy, které algoritmus zjistil při přehrávání videa. Protože ID můžete nechtěně obnovit, pokud se stane nezjištěné řez (např. řez bude mimo obrazovku, vypadá rychle aplikace), toto číslo nemusí vždy rovná true počet řezy ve videu. |

