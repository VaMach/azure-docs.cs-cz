Služba Storage je omezená místem na disku nebo vynuceným omezením *maximálního počtu* indexů nebo dokumentů – podle toho, co nastane dříve.

| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Smlouva SLA |Ne <sup>1</sup> |Ano |Ano |Ano |Ano |Ano |
| Úložiště na oddíl |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Oddíly na službu |neuvedeno |1 |12 |12 |12 |3 <sup>2</sup> |
| Velikost oddílu |Není k dispozici |2 GB |25 GB |100 GB |200 GB |200 GB |
| Repliky |neuvedeno |3 |12 |12 |12 |12 |
| Maximální počet indexů |3 |5 <sup>3</sup>|50 |200 |200 |1 000 na oddíl nebo 3 000 na službu |
| Maximální počet indexerů |3 |5 <sup>3</sup>|50 |200 |200 |Nepodporuje indexery |
| Maximální počet zdrojů dat |3 |5 <sup>3</sup>|50 |200 |200 |Nepodporuje indexery |
| Maximální dokumenty <sup>3</sup> |10 000 |1 milion |15 milionů na oddíl nebo 180 milionů na službu |60 milionů na oddíl nebo 720 milionů na službu |120 milionů na oddíl nebo 1,4 miliard na službu |1 milion na index nebo 200 milionů na oddíl |

<sup>1</sup> volné vrstvy a preview funkce není součástí smlouvy o úrovni služeb (SLA). Pro všechny fakturovatelné úrovně SLA projeví při zřizování dostatečná redundance pro vaši službu. Dvě nebo více replik jsou požadovány pro SLA dotazu (načíst). Tři nebo více replik jsou požadovány pro dotazy a indexování smlouvy o úrovni služeb (pro čtení a zápis). Počet oddílů není k posouzení SLA. 

<sup>2</sup> S3 HD má vynucené omezení na 3 oddíly, což je méně než omezení počtu oddílů pro S3. Nižší omezení počtu oddílů je nastaveno z důvodu, že počet indexů pro S3 HD je podstatně vyšší. Vzhledem k tomu, že existují omezení jak výpočetních prostředků (úložiště a zpracování), tak obsahu (indexy a dokumenty), omezení obsahu bude dosaženo dříve.

>[!Important]
> **<sup>3</sup>**  počínaje pozdní 2017 je nově vytvořený služby vyhledávání systému Azure byly zřízené pomocí výkonnější základní konfigurace hardwaru, které umožňují některá omezení změnit v některých oblastech (Brazílie – Jih, Střední Kanada Střed, východní USA, střed USA – sever, Severní Evropa, střed USA – Jih, jihovýchodní Asie, Spojené království – Jih, západní Evropa a západní USA):
>
>* Základní a úroveň Standard vyhledávacích služeb vytvořených po pozdní 2017 nemají žádné limity na počet dokumentů; na tyto služby se vynucují jenom limity úložiště. 
>* Pro služby S3 s vysokou hustotou vytvořené po pozdní 2017 200 milionů dokumentů na oddíl odebral ale 1 milion dokumentů na index limit zůstanou.
>* Základní služby vytvořené po pozdní 2017 mít vyšší limit 15 indexy, indexery a zdroje dat.
>
>Chcete-li se dozvědět více o tom, které omezení použít do existující služby, pomocí portálu Azure zobrazíte informace o limitu na stránce Přehled služby.