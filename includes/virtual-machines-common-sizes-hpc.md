<!-- A-series - compute-intensive instances, H-series -->

Velikosti A8-A11 a H-series se také označují jako *náročné na výpočetní výkon*. Hardware pro hostování těchto velikostí je navržený a optimalizovaný pro úlohy náročné na výpočty a síťový provoz, včetně clusterovaného vysokovýkonného výpočetního prostředí (HPC), například pro modelování a simulace. Řady A8-A11-series používají Intel Xeon E5-2670 @ 2,6 GHz a H-series používá Intel Xeon E5-2667 v3 @ 3,2 GHz. 

Virtuální počítače Azure H-series jsou další generací počítačů s vysokým výpočetním výkonem a jsou zaměřené na výpočetně vysoce náročné úlohy, jako je např. molekulární modelování, výpočty dynamiky tekutin a podobně. Tyto virtuální počítače s 8 a 16 virtuálními procesory jsou postavené na technologii procesorů Intel Haswell E5-2667 V3, které nabízejí paměti DDR4 a dočasné úložiště založené na jednotkách SSD. 

Řada H-series nabízí kromě značného výkonu procesorů i řadu možností přímého přístupu do paměti vzdáleného počítače (RDMA) s nízkou latencí díky technologii FDR InfiniBand, a množství konfigurací paměti pro podporu požadavků na vysoce paměťově náročné výpočetní operace.



## <a name="h-series"></a>H-series

ACU: 290–300

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost disku: IOPS | Maximální počet síťových karet |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16 × 500 |2  |
| Standard_H16 |16 |112 |2000 |32 |32 × 500 |4 |
| Standard_H8m |8 |112 |1000 |16 |16 × 500 |2  |
| Standard_H16m |16 |224 |2000 |32 |32 × 500 |4  |
| Standard_H16r* |16 |112 |2000 |32 |32 × 500 |4  |
| Standard_H16mr* |16 |224 |2000 |32 |32 × 500 |4 |

* Pro aplikace MPI umožňuje síť FDR InfiniBand vyhrazenou síť back-end s přímým přístupem do paměti vzdáleného počítače (RDMA), která poskytuje mimořádně nízkou latenci a velkou šířku pásma.

<br>



## <a name="a-series---compute-intensive-instances"></a>A-series – Instance náročné na výpočetní výkon

ACU: 225

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (HDD): GiB | Max. datových disků | Maximální propustnost datového disku: IOPS | Maximální počet síťových karet|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16×500 |2 |
| Standard_A9* |16 |112 |382 |16 |16×500 |4 |
| Standard_A10 |8 |56 |382 |16 |16×500 |2  |
| Standard_A11 |16 |112 |382 |16 |16×500 |4 |

* Pro aplikace MPI umožňuje síť FDR InfiniBand vyhrazenou síť back-end s přímým přístupem do paměti vzdáleného počítače (RDMA), která poskytuje mimořádně nízkou latenci a velkou šířku pásma.

<br>



