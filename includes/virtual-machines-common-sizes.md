


Azure nabízí na výběr několik standardních velikostí. Tady je několik aspektů, které je třeba při volbě velikosti zvážit:

* Virtuální počítače D-series slouží ke spouštění aplikací, které potřebují vyšší výpočetní výkon a krátkodobý vysoký diskový výkon. Virtuální počítače D-series mají rychlejší procesory, vyšší poměr paměti na jádro a jednotky SSD pro dočasný disk. Podrobnosti najdete v oznámení na blogu Azure: [Nové velikosti virtuálních počítačů D-Series](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).
* Virtuální počítače Dv2-series, nástupce původní řady D-series, mají výkonnější procesor. Procesor v Dv2-series je asi o 35 % rychlejší než procesor v D-series. Využívá nejnovější generaci procesorů Intel Xeon® E5-2673 v3 (Haswell) s frekvencí 2,4 GHz, kterou může technologie Intel Turbo Boost 2.0 zvýšit až na 3,1 GHz. Řada Dv2-series má stejnou konfiguraci paměti a disku jako řada D.
* Řada F-series využívá procesory Intel Xeon® E5-2673 v3 (Haswell) s frekvencí 2,4 GHz, kterou může technologie Intel Turbo Boost 2.0 zvýšit až na 3,1 GHz. Výkon procesoru je tedy stejný jako u virtuálních počítačů Dv2-series.  Za nižší ceníkové sazby za hodinu nabízí řada F-series nejlepší poměr cena–výkon v portfoliu Azure, pokud jde o množství výpočetních jednotek Azure (ACU) na jádro. 
  
    F-series také zavádí nové standardní pojmenování velikostí virtuálních počítačů Azure. Pro tuto řadu a pro budoucí velikosti virtuálních počítačů označuje číslo za názvem rodiny počet procesorových jader. Další možnosti, například optimalizace pro službu Storage úrovně Premium, budou označeny písmeny následujícími za počtem jader. Tento formát označení se bude používat pro virtuální počítače vydané v budoucnu, nebude se však vztahovat na velikosti dříve vydaných virtuálních počítačů.
* Virtuální počítače G-series nabízejí nejvíce paměti a spouštějí se na hostitelích s procesory Intel Xeon E5 V3.
* Virtuální počítače DS-series, DSv2-series, Fs-series a GS-series můžou používat službu Storage úrovně Premium, která poskytuje vysoký výkon a nízkou latenci pro úlohy náročné na V/V operace. Tyto virtuální počítače hostují virtuální disky na jednotkách SSD a nabízejí také místní mezipaměť disku SSD. V některých oblastech je dostupná služba Storage úrovně Premium. Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/storage/storage-premium-storage.md).
*   Virtuální počítače A-series a Av2-series je možné nasadit na celou řadu typů hardwaru a procesorů. Velikost je omezována v závislosti na použitém hardwaru a nabízí konzistentní výkon procesoru pro spuštěné instance bez ohledu na hardware. Pokud chcete zjistit fyzický hardware, na kterém je virtuální počítač dané velikosti nasazený, zadejte dotaz na virtuální hardware přímo z virtuálního počítače.
* Velikost A0 využívá fyzický hardware na maximum. Pouze pro tuto konkrétní velikost můžou výkon vašich spuštěných úloh ovlivnit jiná zákaznická nasazení. Relativní výkon je popsaný níže jako očekávaný základ, přičemž se dá očekávat variabilita přibližně 15 procent.

Velikost virtuálního počítače má vliv na cenu. Velikost také určuje kapacitu zpracování, paměti a úložiště virtuálního počítače. Náklady na úložiště se počítají samostatně na základě využitých stránek v účtu úložiště. Podrobnosti najdete v článcích [Podrobnosti o cenách služby Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/) a [Ceny za Azure Storage](https://azure.microsoft.com/pricing/details/storage/). 

Následující aspekty mohou pomoci při rozhodování o velikosti:

* Velikosti A8-A11 a H-series se také označují jako *náročné na výpočetní výkon*. Hardware pro hostování těchto velikostí je navržený a optimalizovaný pro úlohy náročné na výpočty a síťový provoz, včetně clusterovaného vysokovýkonného výpočetního prostředí (HPC), například pro modelování a simulace. Řady A8-A11-series používají Intel Xeon E5-2670 @ 2,6 GHz a H-series používá Intel Xeon E5-2667 v3 @ 3,2 GHz. Podrobné informace o použití těchto velikostí najdete v článku [Virtuální počítače H-series a A-series pro výpočetně náročné aplikace](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
* Řady Dv2, D, G a jejich protějšky DS/GS jsou ideální pro aplikace, které vyžadují rychlejší procesory, vyšší výkon místních disků nebo mají vysoké nároky na paměť.  Nabízejí výkonnou kombinaci pro mnoho podnikových aplikací.
* Virtuální počítače F-series jsou skvělou volbou pro úlohy, které potřebují vysoký výkon procesoru, ale ne tolik paměti nebo prostoru na místním disku SSD na jádro.  Úlohy jako například analýzy, herní servery, webové servery nebo dávkové zpracování můžou řadu F-series s výhodou využít.
* Někteří z fyzických hostitelů v datových centrech Azure nemusí podporovat větší velikosti virtuálních počítačů, například A5–A11. Výsledkem pokusu o změnu velikosti existujícího virtuálního počítače, o vytvoření nového virtuálního počítače ve virtuální síti vytvořené před 16. dubnem 2013 nebo pokusu o přidání nového virtuálního počítače do cloudové služby v takovém případě může být chybová zpráva **Nepovedlo se nakonfigurovat virtuální počítač <machine name>** nebo **Nepodařilo se vytvořit virtuální počítač <machine name>**. Pomoc při řešení této chyby v jednotlivých scénářích nasazení najdete v článku [Chyba: „Nepovedlo se nakonfigurovat virtuální počítač“](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) na fóru podpory.  
* Vaše předplatné může také omezovat počet jader, které můžete v určitých rodinách velikostí nasadit. S požadavkem na zvýšení této kvóty se obraťte na podporu Azure.

## <a name="performance-considerations"></a>Otázky výkonu
Pro účely porovnávání výpočetního síly (výkonu procesoru) jednotlivých SKU v Azure jsme vytvořili koncept výpočetní jednotky Azure – ACU. To vám umožní snadno zjistit, jaká jednotka SKU nejlépe vyhoví vašim požadavkům na výkon.  Jednotka ACU je aktuálně stanovená tak, že malý virtuální počítač (Standard_A1) má 100 ACU a ostatní jednotky SKU jsou pak ohodnoceny podle relativního výsledku standardního srovnávacího testu. 

> [!IMPORTANT]
> ACU je jenom informativní údaj.  Výsledek v případě konkrétních úloh může být odlišný. 
> 
> 

<br>

| Rodina SKU | ACU na jádro |
| --- | --- |
| [A0](#a-series) |50 |
| [A1-A4](#a-series) |100 |
| [A5-A7](#a-series) |100 |
| [A1_v2-A8_v2](#av2-series) |100 |
| [A2m_v2-A8m_v2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1-D14](#d-series) |160 |
| [D1_v2-D15_v2](#dv2-series) |210 - 250* |
| [DS1-DS14](#ds-series) |160 |
| [DS1_v2-DS15_v2](#dsv2-series) |210-250* |
| [F1-F16](#f-series) |210-250* |
| [F1s-F16s](#fs-series) |210-250* |
| [G1-G5](#g-series) |180 - 240* |
| [GS1-GS5](#gs-series) |180 - 240* |
| [H](#h-series) |290 - 300* |
| [L4s-L32s](#l-series) |180 - 240* |

Hodnoty ACU s hvězdičkou označují použití technologie Intel® Turbo, která může zvýšit frekvenci procesoru podle aktuální potřeby.  Množství nárůst se může lišit v závislosti na velikosti virtuálního počítače, úlohy a dalších úlohách spuštěných na stejném hostiteli.

## <a name="size-tables"></a>Tabulky velikostí
Následující tabulky pro jednotlivé velikosti virtuálních počítačů uvádějí poskytované kapacity.

* Kapacita úložiště je v jednotkách GiB, tj. 1024^3 bajtů. Při porovnávání diskové kapacity měřené v GB (1000^3 B) s kapacitou měřenou v GiB (1024^3) pamatujte, že údaj v GiB je číselně menší. Například 1023 GiB = 1098,4 GB
* Propustnost disku se měří v počtu V/V operací za sekundu (IOPS) a v MB/s, kde 1 MB/s = 10^6 bajtů/s.
* Disky pro ukládání dat můžou fungovat v režimu s mezipamětí, nebo bez ní.  Pro diskové operace s mezipamětí je možné nastavit mezipaměť na hostiteli **jen na čtení** nebo **na čtení i zápis**.  Pro diskové operace bez mezipaměti je mezipaměť na hostiteli nastavená na **žádná**.
* Maximální šířka pásma sítě je maximální přenosová kapacita vyhrazená a přidělená danému typu virtuálního počítače. Maximální šířka pásma slouží pro orientaci při výběru správného typu virtuálního počítače, aby byla zajištěna dostatečná síťová kapacita. Při přechodu mezi nízkou, střední, vysokou a velmi vysokou šířkou pásma se bude odpovídajícím způsobem měnit propustnost. Skutečný výkon sítě bude záviset na mnoha faktorech včetně zatížení aplikací a sítě a síťového nastavení aplikace.

## <a name="a-series"></a>A-Series
| Velikost | Procesorová jádra | Paměť: GiB | Místní HDD: GiB | Max. datových disků | Maximální propustnost datového disku: IOPS | Max. počet NIC / Šířka pásma sítě |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 |1 |0,768 |20 |1 |1×500 |1 / malá |
| Standard_A1 |1 |1,75 |70 |2 |2×500 |1 / střední |
| Standard_A2 |2 |3,5 |135 |4 |4×500 |1 / střední |
| Standard_A3 |4 |7 |285 |8 |8×500 |2 / vysoká |
| Standard_A4 |8 |14 |605 |16 |16×500 |4 / vysoká |
| Standard_A5 |2 |14 |135 |4 |4×500 |1 / střední |
| Standard_A6 |4 |28 |285 |8 |8×500 |2 / vysoká |
| Standard_A7 |8 |56 |605 |16 |16×500 |4 / vysoká |

<br>

## <a name="a-series---compute-intensive-instances"></a>A-series – Instance náročné na výpočetní výkon
Informace o použití těchto velikostí najdete v článku [Virtuální počítače H-series a A-series pro výpočetně náročné aplikace](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Velikost | Procesorová jádra | Paměť: GiB | Místní HDD: GiB | Max. datových disků | Maximální propustnost datového disku: IOPS | Max. počet NIC / Šířka pásma sítě |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16×500 |2 / vysoká |
| Standard_A9* |16 |112 |382 |16 |16×500 |4 / velmi vysoká |
| Standard_A10 |8 |56 |382 |16 |16×500 |2 / vysoká |
| Standard_A11 |16 |112 |382 |16 |16×500 |4 / velmi vysoká |

*Podpora RDMA

<br>

## <a name="av2-series"></a>Av2-series

| Velikost            | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Maximální propustnost místního disku: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet NIC / Šířka pásma sítě |
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000 / 20 / 10                                           | 2 / 2×500                         | 1 / střední                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000 / 40 / 20                                           | 4 / 4×500                         | 2 / střední                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000 / 80 / 40                                           | 8 / 8×500                         | 4 / vysoká                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000 / 160 / 80                                          | 16 / 16×500                       | 8 / vysoká                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000 / 40 / 20                                           | 4 / 4×500                         | 2 / střední                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000 / 80 / 40                                           | 8 / 8×500                         | 4 / vysoká                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000 / 160 / 80                                          | 16 / 16×500                       | 8 / vysoká                     |

## <a name="d-series"></a>D-series

| Velikost         | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Maximální propustnost místního disku: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet NIC / Šířka pásma sítě |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 2 / 2×500                         | 1 / střední                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / vysoká                     |
| Standard_D3  | 4         | 14          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / vysoká                     |
| Standard_D4  | 8         | 28          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / vysoká                     |
| Standard_D11 | 2         | 14          | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / vysoká                     |
| Standard_D12 | 4         | 28          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / vysoká                     |
| Standard_D13 | 8         | 56          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / vysoká                     |
| Standard_D14 | 16        | 112         | 800            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / velmi vysoká                |
<br>

## <a name="dv2-series"></a>Dv2-series


| Velikost              | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Maximální propustnost místního disku: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet NIC / Šířka pásma sítě |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1_v2    | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 2 / 2×500                         | 1 / střední                 |
| Standard_D2_v2    | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / vysoká                     |
| Standard_D3_v2    | 4         | 14          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / vysoká                     |
| Standard_D4_v2    | 8         | 28          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / vysoká                     |
| Standard_D5_v2    | 16        | 56          | 800            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / velmi vysoká           |
| Standard_D11_v2   | 2         | 14          | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / vysoká                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / vysoká                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / vysoká                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / velmi vysoká           |
| Standard_D15_v2** | 20        | 140         | 1&000;          | 60000 / 937 / 468                                        | 40 / 40×500                       | 8 / velmi vysoká*          |

*V některých oblastech je k dispozici pro velikost Standard_D15_v2 urychlení sítě. Další informace jejím využití a dostupnosti najdete v článku [Urychlení sítě je ve verzi Preview](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) a [Urychlení sítě pro virtuální počítače](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md).

*Instance je izolovaná na hardwaru vyhrazeném pro jediného zákazníka.

<br>

## <a name="ds-series"></a>DS-series*
| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet NIC / Šířka pásma sítě |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4 000 / 32 (43) |3 200 / 32 |1 / střední |
| Standard_DS2 |2 |7 |14 |4 |8 000 / 64 (86) |6 400 / 64 |2 / vysoká |
| Standard_DS3 |4 |14 |28 |8 |16 000 / 128 (172) |12 800 / 128 |4 / vysoká |
| Standard_DS4 |8 |28 |56 |16 |32 000 / 256 (344) |25 600 / 256 |8 / vysoká |
| Standard_DS11 |2 |14 |28 |4 |8 000 / 64 (72) |6 400 / 64 |2 / vysoká |
| Standard_DS12 |4 |28 |56 |8 |16 000 / 128 (144) |12 800 / 128 |4 / vysoká |
| Standard_DS13 |8 |56 |112 |16 |32 000 / 256 (288) |25 600 / 256 |8 / vysoká |
| Standard_DS14 |16 |112 |224 |32 |64 000 / 512 (576) |51 200 / 512 |8 / velmi vysoká |

MB/s = 10^6 bajtů za sekundu a GiB = 1024^3 bajtů.

*Maximální propustnost disku (IOPS nebo MB/s) dostupná pro virtuální počítače DS-series může být omezená počtem, velikostí a prokládáním připojených disků.  Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/storage/storage-premium-storage.md).

<br>

## <a name="dsv2-series"></a>DSv2-series*
| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet NIC / Šířka pásma sítě |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4 000 / 32 (43) |3 200 / 48 |1 střední |
| Standard_DS2_v2 |2 |7 |14 |4 |8 000 / 64 (86) |6 400 / 96 |2 vysoká |
| Standard_DS3_v2 |4 |14 |28 |8 |16 000 / 128 (172) |12 800 / 192 |4 / vysoká |
| Standard_DS4_v2 |8 |28 |56 |16 |32 000 / 256 (344) |25 600 / 384 |8 / vysoká |
| Standard_DS5_v2 |16 |56 |112 |32 |64 000 / 512 (688) |51 200 / 768 |8 / velmi vysoká |
| Standard_DS11_v2 |2 |14 |28 |4 |8 000 / 64 (72) |6 400 / 96 |2 vysoká |
| Standard_DS12_v2 |4 |28 |56 |8 |16 000 / 128 (144) |12 800 / 192 |4 / vysoká |
| Standard_DS13_v2 |8 |56 |112 |16 |32 000 / 256 (288) |25 600 / 384 |8 / vysoká |
| Standard_DS14_v2 |16 |112 |224 |32 |64 000 / 512 (576) |51 200 / 768 |8 / velmi vysoká |
| Standard_DS15_v2*** |20 |140 |280 |40 |80 000 / 640 (720) |64 000 / 960 |8 / velmi vysoká** |

MB/s = 10^6 bajtů za sekundu a GiB = 1024^3 bajtů.

*Maximální propustnost disku (IOPS nebo MB/s) dostupná pro virtuální počítače DSv2-series může být omezená počtem, velikostí a prokládáním připojených disků.  Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/storage/storage-premium-storage.md).

**V některých oblastech je k dispozici pro velikost Standard_DS15_v2 urychlení sítě. Další informace jejím využití a dostupnosti najdete v článku [Urychlení sítě je ve verzi Preview](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) a [Urychlení sítě pro virtuální počítače](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md).

***Instance je izolovaná na hardwaru vyhrazeném pro jediného zákazníka.
<br>

## <a name="f-series"></a>F-series

| Velikost         | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Maximální propustnost místního disku: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet NIC / Šířka pásma sítě |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 2 / 2×500                         | 1 / střední                 |
| Standard_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / vysoká                     |
| Standard_F4  | 4         | 8           | 64             | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / vysoká                     |
| Standard_F8  | 8         | 16          | 128            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / vysoká                     |
| Standard_F16 | 16        | 32          | 256            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / velmi vysoká           |
<br>

## <a name="fs-series"></a>Fs-series*
| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet NIC / Šířka pásma sítě |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4 000 / 32 (12) |3 200 / 48 |1 / střední |
| Standard_F2s |2 |4 |8 |4 |8 000 / 64 (24) |6 400 / 96 |2 / vysoká |
| Standard_F4s |4 |8 |16 |8 |16 000 / 128 (48) |12 800 / 192 |4 / vysoká |
| Standard_F8s |8 |16 |32 |16 |32 000 / 256 (96) |25 600 / 384 |8 / vysoká |
| Standard_F16s |16 |32 |64 |32 |64 000 / 512 (192) |51 200 / 768 |8 / velmi vysoká |

MB/s = 10^6 bajtů za sekundu a GiB = 1024^3 bajtů.

*Maximální propustnost disku (IOPS nebo MB/s) dostupná pro virtuální počítače Fs-series může být omezená počtem, velikostí a prokládáním připojených disků.  Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/storage/storage-premium-storage.md).

<br>

## <a name="g-series"></a>G-series

| Velikost         | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Maximální propustnost místního disku: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet NIC / Šířka pásma sítě |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000 / 93 / 46                                           | 4 / 4 × 500                       | 1 / vysoká                     |
| Standard_G2  | 4         | 56          | 768            | 12000 / 187 / 93                                         | 8 / 8 × 500                       | 2 / vysoká                     |
| Standard_G3  | 8         | 112         | 1&536;          | 24000 / 375 / 187                                        | 16 / 16 × 500                     | 4 / velmi vysoká                |
| Standard_G4  | 16        | 224         | 3&072;          | 48000 / 750 / 375                                        | 32 / 32 × 500                     | 8 / velmi vysoká           |
| Standard_G5* | 32        | 448         | 6&144;          | 96000 / 1500 / 750                                       | 64 / 64 × 500                     | 8 / velmi vysoká           |

*Instance je izolovaná na hardwaru vyhrazeném pro jediného zákazníka.
<br>

## <a name="gs-series"></a>GS-series*
| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet NIC / Šířka pásma sítě |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_GS1 |2 |28 |56 |4 |10 000 / 100 (264) |5 000 / 125 |1 / vysoká |
| Standard_GS2 |4 |56 |112 |8 |20 000 / 200 (528) |10 000 / 250 |2 / vysoká |
| Standard_GS3 |8 |112 |224 |16 |40 000 / 400 (1 056) |20 000 / 500 |4 / velmi vysoká |
| Standard_GS4 |16 |224 |448 |32 |80 000 / 800 (2 112) |40 000 / 1 000 |8 / velmi vysoká |
| Standard_GS5** |32 |448 |896 |64 |160 000 / 1 600 (4 224) |80 000 / 2 000 |8 / velmi vysoká |

MB/s = 10^6 bajtů za sekundu a GiB = 1024^3 bajtů.

*Maximální propustnost disku (IOPS nebo MB/s) dostupná pro virtuální počítače GS-series může být omezená počtem, velikostí a prokládáním připojených disků. Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/storage/storage-premium-storage.md). 

*Instance je izolovaná na hardwaru vyhrazeném pro jediného zákazníka.
<br>

## <a name="h-series"></a>H-series
Virtuální počítače Azure H-series jsou další generací počítačů s vysokým výpočetním výkonem a jsou zaměřené na výpočetně vysoce náročné úlohy, jako je např. molekulární modelování, výpočty dynamiky tekutin a podobně. Tyto virtuální počítače s 8 a 16 jádry jsou postavené na technologii procesorů Intel Haswell E5-2667 V3, které nabízejí paměti DDR4 a místní úložiště SSD. 

Řada H-series nabízí kromě značného výkonu procesorů i řadu možností přímého přístupu do paměti vzdáleného počítače (RDMA) s nízkou latencí díky technologii FDR InfiniBand, a množství konfigurací paměti pro podporu požadavků na vysoce paměťově náročné výpočetní operace.

Informace o použití těchto velikostí najdete v článku [Virtuální počítače H-series a A-series pro výpočetně náročné aplikace](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku: IOPS | Max. počet NIC / Šířka pásma sítě |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16 × 500 |2 / vysoká |
| Standard_H16 |16 |112 |2000 |32 |32 × 500 |4 / velmi vysoká |
| Standard_H8m |8 |112 |1000 |16 |16 × 500 |2 / vysoká |
| Standard_H16m |16 |224 |2000 |32 |32 × 500 |4 / velmi vysoká |
| Standard_H16r* |16 |112 |2000 |32 |32 × 500 |4 / velmi vysoká |
| Standard_H16mr* |16 |224 |2000 |32 |32 × 500 |4 / velmi vysoká |

*Podpora RDMA

<br>


## <a name="ls-series"></a>Ls-series* 

Řada Ls je optimalizována pro úlohy, které vyžadují místní úložiště s nízkou latencí, jako jsou databáze NoSQL (např. Cassandra, MongoDB, Cloudera a Redis). Řada Ls nabízí až 32 procesorových jader a využívá [řadu procesorů Intel® Xeon® E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). To je stejný výkon procesoru jako řady G/GS a má 8 GiB paměti na procesorové jádro.  

 
| Velikost          | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet NIC / Šířka pásma sítě | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | NA / NA (0)          | 5 000 / 125                               | 2 / vysoká       | 
| Standard_L8s  | 8    | 64   | 1&388; | 16             | NA / NA (0)          | 10 000 / 250                              | 4 / velmi vysoká  | 
| Standard_L16s | 16   | 128  | 2&807; | 32             | NA / NA (0)          | 20 000 / 500                              | 8 / velmi vysoká | 
| Standard_L32s** | 32   | 256  | 5,630 | 64             | NA / NA (0)          | 40 000 / 1 000                            | 8 / velmi vysoká | 
 
MB/s = 10^6 bajtů za sekundu a GiB = 1024^3 bajtů. 

*Maximální propustnost disku (IOPS nebo MB/s) dostupná pro virtuální počítače Ls-series může být omezená počtem, velikostí a prokládáním připojených disků. Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/storage/storage-premium-storage.md). 

*Instance je izolovaná na hardwaru vyhrazeném pro jediného zákazníka.



## <a name="n-series"></a>N-series
Velikosti NC a NV se také označují jako instance s podporou grafického procesoru. Jsou to specializované virtuální počítače s grafickými kartami NVIDIA, optimalizované pro různé scénáře a případy použití. Velikosti NV jsou optimalizované a navržené pro vzdálené vizualizace, streamování, hry, kódování a VDI díky využití architektur typu OpenGL a DirectX. Velikosti NC jsou vhodnější pro výpočetně a síťově náročné úlohy a algoritmy včetně aplikací a simulací využívajících technologie CUDA a OpenCL. 


### <a name="nv-instances"></a>Instance NV
Instance NV jsou osazeny grafickými kartami NVIDIA Tesla M60 a NVIDIA GRID pro urychlení desktopových aplikací i virtuálních desktopů, na kterých budou zákazníci vizualizovat svoje data nebo simulace. Uživatelé budou moci vizualizovat své graficky náročné pracovní postupy na instancích NV a využít tak jejich špičkové grafické možnosti, a navíc spouštět úlohy s jednoduchou přesností, například kódování a vykreslování. Tesla M60 má 4096 jader CUDA v provedení dual-GPU a dokáže zpracovat až 36 datových proudů 1080p H.264. 


| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 |
| Standard_NV12 |12 |112 |680 | 2 |
| Standard_NV24 |24 |224 |1440 | 4 |

1 GPU = polovina karty M60.

**Podporované operační systémy**

* Windows Server 2016, Windows Server 2012 R2 – viz [Instalace ovladačů v instancích N-series s Windows](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md)

### <a name="nc-instances"></a>Instance NC
Instance NC jsou osazeny kartami NVIDIA Tesla K80. Uživatelé se můžou díky technologii CUDA prokousávat daty daleko rychleji, například v aplikacích pro výzkum energie, simulace srážek, vykreslování sledováním paprsku, hloubkové učení a dalších. Tesla K80 má 4992 jader CUDA v provedení dual-GPU a nabízí výkon až 2,91 Teraflops v dvojité přesnosti a až 8,93 Teraflops v jednoduché přesnosti.

| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 |

1 GPU = polovina karty K80.

*Podpora RDMA

**Podporované operační systémy**

* Windows Server 2016, Windows Server 2012 R2 – viz [Instalace ovladačů v instancích N-series s Windows](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md)
* Ubuntu 16.04 LTS – viz [Instalace ovladačů v instancích N-series s Linuxem](../articles/virtual-machines/virtual-machines-linux-n-series-driver-setup.md)

<br>

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Poznámky: Standardní instance A0–A4 využívají příkazový řádek a PowerShell
V klasickém modelu nasazení se některé názvy virtuálních počítačů s prostředím příkazového řádku a PowerShellu trochu liší:

* Standard_A0 je ExtraSmall 
* Standard_A1 je Small
* Standard_A2 je Medium
* Standard_A3 je Large
* Standard_A4 je ExtraLarge

## <a name="next-steps"></a>Další kroky
* Další informace o [Limitech, kvótách a omezeních předplatného a služeb Azure](../articles/azure-subscription-service-limits.md)
* Další informace o [Virtuálních počítačích H-series a A-series pro výpočetně náročné aplikace](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (HPC)

