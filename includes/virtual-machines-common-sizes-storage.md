Úložiště nabízejí disku vysoké propustnosti a vstupně-výstupní operace optimalizované velikosti virtuálních počítačů a jsou ideální pro velké objemy dat, SQL a NoSQL databáze. Tento článek obsahuje informace o počtu Vcpu, datové disky a síťové adaptéry, jakož i výkon sítě a propustnost úložiště pro každou velikost v této skupině. 

Řada Ls-series nabízí až 32 virtuálních procesorů a využívá [řadu procesorů Intel® Xeon® E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Řada Ls-series má stejný výkon procesoru jako řady G/GS-series a 8 GiB paměti na virtuální procesor.  

## <a name="ls-series"></a>Řada Ls

ACU: 180–240
 
| Velikost          | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | NA / NA (0)          | 5 000 / 125                               | 2 / 4 000       | 
| Standard_L8s  | 8    | 64   | 1 388 | 16             | NA / NA (0)          | 10 000 / 250                              | 4 / 8 000  | 
| Standard_L16s | 16   | 128  | 2 807 | 32             | NA / NA (0)          | 20 000 / 500                              | 8 / 6 000–16 000 &#8224; | 
| Standard_L32s* | 32 | 256  | 5,630 | 64             | NA / NA (0)          | 40 000 / 1 000                            | 8 / 20 000 | 
 

Maximální propustnost disku dostupná pro virtuální počítače řady Ls-series může být omezená počtem, velikostí a prokládáním připojených disků. Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/virtual-machines/windows/premium-storage.md). 

*Instance je izolovaná na hardwaru vyhrazeném pro jediného zákazníka.

