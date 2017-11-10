
Paměťově optimalizovaná nabídka velikosti virtuálních počítačů vysoký poměr paměti procesor, který se výborně hodí pro servery relační databáze, středních a velkých mezipaměti a analýzy v paměti. Tento článek obsahuje informace o počtu Vcpu, datové disky a síťové adaptéry, jakož i výkon sítě a propustnost úložiště pro každou velikost v této skupině. 

* Řada M-series nabízí nejvyšší počet virtuálních procesorů (až 128 virtuálních procesorů) a největší paměť (až 2,0 TiB) ze všech virtuálních počítačů v cloudu.  Je ideální pro extrémně velké databáze nebo další aplikace, kterým přináší výhody vysoký počet virtuálních procesorů a velké množství paměti.

* Řady Dv2-series, D-series, G-series a jejich protějšky DS/GS jsou ideální pro aplikace, které vyžadují rychlejší virtuální procesory, vyšší výkon dočasného úložiště nebo mají vysoké nároky na paměť.  Nabízejí výkonnou kombinaci pro mnoho podnikových aplikací.

* Virtuální počítače D-series slouží ke spouštění aplikací, které potřebují vyšší výpočetní výkon a krátkodobý vysoký diskový výkon. Virtuální počítače řady D-series mají rychlejší procesory, vyšší poměr paměti na virtuální procesor a jednotky SSD (solid-state-drive) pro dočasné úložiště. Podrobnosti najdete v oznámení na blogu Azure: [Nové velikosti virtuálních počítačů D-Series](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Virtuální počítače Dv2-series, nástupce původní řady D-series, mají výkonnější procesor. Procesor v Dv2-series je asi o 35 % rychlejší než procesor v D-series. Využívá nejnovější generaci procesorů Intel Xeon® E5-2673 v3 (Haswell) s frekvencí 2,4 GHz, kterou může technologie Intel Turbo Boost 2.0 zvýšit až na 3,1 GHz. Řada Dv2-series má stejnou konfiguraci paměti a disku jako řada D.


## <a name="esv3-series"></a>ESv3-series

ACU: 160–190

Instance ESv3-series jsou založené na procesorech 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell), jejichž frekvence se s technologií Intel Turbo Boost 2.0 může zvýšit až na 3,5 GHz, a používají službu Storage úrovně Premium. Instance Ev3-series jsou ideální pro podnikové aplikace s vysokými nároky na paměť.


| Velikost             | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3  | 2      | 16          | 32             | 4              | 4 000 / 32 (50)                                                       | 3 200 / 48                                | 2 / střední                                   |
| Standard_E4s_v3  | 4      | 32          | 64             | 8              | 8 000 / 64 (100)                                                      | 6 400 / 96                                | 2 / střední                                   |
| Standard_E8s_v3  | 8      | 64          | 128            | 16             | 16 000 / 128 (200)                                                    | 12 800 / 192                              | 4 / vysoká                                       |
| Standard_E16s_v3 | 16     | 128         | 256            | 32             | 32 000 / 256 (400)                                                    | 25 600 / 384                              | 8 / vysoká                                       |
| Standard_E32s_v3 | 32     | 256         | 512            | 32             | 64 000 / 512 (800)                                                    | 51 200 / 768                              | 8 / velmi vysoká                             |
| Standard_E64s_v3 | 64     | 432         | 864            | 32             | 128 000 / 1 024 (1 600)                                                   | 80 000 / 1 200                             | 8 / velmi vysoká                             |



## <a name="ev3-series"></a>Ev3-series

ACU: 160–190 

Instance Ev3-series jsou založené na procesorech 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell) a s technologií Intel Turbo Boost 2.0 se jejich frekvence může zvýšit až na 3,5 GHz. Instance Ev3-series jsou ideální pro podnikové aplikace s vysokými nároky na paměť.

Úložiště datových disků se účtuje nezávisle na virtuálních počítačích. Pokud chcete použít disky Storage úrovně Premium, použijte velikosti ESv3. Měřiče cen a účtování pro velikosti ESv3 jsou stejné jako pro Ev3-series. 


| Velikost            | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Max. počet NIC / Šířka pásma sítě |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3 000 / 46 / 23                                               | 2 / střední                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6 000 / 93 / 46                                               | 2 / střední                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12 000 / 187 / 93                                             | 4 / vysoká                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24 000 / 375 / 187                                            | 8 / vysoká                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48 000 / 750 / 375                                            | 8 / velmi vysoká           |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96 000 / 1 000 / 500                                           | 8 / velmi vysoká           |


## <a name="m-series"></a>M-series*

ACU: 160–180

| Velikost            | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M64s  | 64   | 1024        | 2 048           | 32             | 80 000 / 800 (6 348)       | 40 000 / 1 000                            | 8 / 16 000          |
| Standard_M64ms  | 64   | 1 792        | 2 048           | 32             | 80 000 / 800 (6 348)       | 40 000 / 1 000                            | 8 / 16 000          |
| Standard_M128s** | 128  | 2 048        | 4 096           | 64             | 160 000 / 1 600 (12 696) | 80 000 / 2 000                            | 8 / 25 000          |


* Virtuální počítače M-series se vyznačují technologií Intel® Hyper-Threading.

** Pro více než 64 virtuálních procesorů se vyžaduje použití některého z těchto podporovaných hostovaných operačních systémů: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 a Red Hat Enterprise Linux nebo CentOS 7.3 s LIS 4.2.1. 

<br>

## <a name="gs-series"></a>GS-series*

ACU: 180–240

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_GS1 |2 |28 |56 |4 |10 000 / 100 (264) |5 000 / 125 |2 / 2 000 |
| Standard_GS2 |4 |56 |112 |8 |20 000 / 200 (528) |10 000 / 250 |2 / 4 000 |
| Standard_GS3 |8 |112 |224 |16 |40 000 / 400 (1 056) |20 000 / 500 |4 / 8 000 |
| Standard_GS4 |16 |224 |448 |32 |80 000 / 800 (2 112) |40 000 / 1 000 |8 / 6 000–16 000 &#8224; |
| Standard_GS5** |32 |448 |896 |64 |160 000 / 1 600 (4 224) |80 000 / 2 000 |8 / 20 000 |

*Maximální propustnost disku (IOPS nebo MB/s) dostupná pro virtuální počítače GS-series může být omezená počtem, velikostí a prokládáním připojených disků. Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/virtual-machines/windows/premium-storage.md). 

*Instance je izolovaná na hardwaru vyhrazeném pro jediného zákazníka.


<br>

## <a name="g-series"></a>G-series

ACU: 180–240

| Velikost         | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000 / 93 / 46                                           | 4 / 4 × 500                       | 2 / 2 000                     |
| Standard_G2  | 4         | 56          | 768            | 12000 / 187 / 93                                         | 8 / 8 × 500                       | 2 / 4 000                     |
| Standard_G3  | 8         | 112         | 1 536          | 24000 / 375 / 187                                        | 16 / 16 × 500                     | 4 / 8 000                |
| Standard_G4  | 16        | 224         | 3 072          | 48000 / 750 / 375                                        | 32 / 32 × 500                     | 8 / 6 000–16 000 &#8224;          |
| Standard_G5* | 32        | 448         | 6 144          | 96000 / 1500 / 750                                       | 64 / 64 × 500                     | 8 / 20 000           |

*Instance je izolovaná na hardwaru vyhrazeném pro jediného zákazníka.
<br>


## <a name="dsv2-series"></a>DSv2-series*

ACU: 210–250

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2 |2 |14 |28 |4 |8 000 / 64 (72) |6 400 / 96 |2 / 1 500 |
| Standard_DS12_v2 |4 |28 |56 |8 |16 000 / 128 (144) |12 800 / 192 |4 / 3 000 |
| Standard_DS13_v2 |8 |56 |112 |16 |32 000 / 256 (288) |25 600 / 384 |8 / 6 000 |
| Standard_DS14_v2 |16 |112 |224 |32 |64 000 / 512 (576) |51 200 / 768 |8 / 6 000–12 000 &#8224; |
| Standard_DS15_v2** |20 |140 |280 |40 |80 000 / 640 (720) |64 000 / 960 |8 / 20 000***

*Maximální propustnost disku (IOPS nebo MB/s) dostupná pro virtuální počítače DSv2-series může být omezená počtem, velikostí a prokládáním připojených disků.  Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/virtual-machines/windows/premium-storage.md).

** Instance je izolovaný uzel, který zaručuje, že váš virtuální počítač je jediným virtuálním počítačem na našem uzlu Intel Haswell.

*** 25 000 Mb/s s akcelerovanými síťovými službami.

<br>

## <a name="dv2-series"></a>Dv2-series

ACU: 210–250

| Velikost              | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / 1 500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / 3 000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / 6 000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / 6 000–12 000 &#8224;          |
| Standard_D15_v2* | 20        | 140         | 1 000          | 60000 / 937 / 468                                        | 40 / 40×500                       | 8 / 20 000** |

* Instance je izolovaný uzel, který zaručuje, že váš virtuální počítač je jediným virtuálním počítačem na našem uzlu Intel Haswell.

** 25 000 Mb/s s akcelerovanými síťovými službami.

<br>

## <a name="ds-series"></a>DS-series*

ACU: 160

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |4 |8 000 / 64 (72) |6 400 / 64 |2 / 1 000 |
| Standard_DS12 |4 |28 |56 |8 |16 000 / 128 (144) |12 800 / 128 |4 / 2 000 |
| Standard_DS13 |8 |56 |112 |16 |32 000 / 256 (288) |25 600 / 256 |8 / 4 000 |
| Standard_DS14 |16 |112 |224 |32 |64 000 / 512 (576) |51 200 / 512 |8 / 6 000–8 000 &#8224; |

*Maximální propustnost disku (IOPS nebo MB/s) dostupná pro virtuální počítače DS-series může být omezená počtem, velikostí a prokládáním připojených disků.  Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/virtual-machines/windows/premium-storage.md).


## <a name="d-series"></a>D-series

ACU: 160

| Velikost         | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / 1 000                     |
| Standard_D12 | 4         | 28          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / 2 000                     |
| Standard_D13 | 8         | 56          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / 4 000                     |
| Standard_D14 | 16        | 112         | 800            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / 6 000–8 000 &#8224;                |

<br>

