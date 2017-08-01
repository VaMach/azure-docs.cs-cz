
<!-- A-series, Av2-series, D-series, Dv2-series, DS-series*, DSv2-series* -->

- Virtuální počítače A-series a Av2-series je možné nasadit na celou řadu typů hardwaru a procesorů. Velikost je omezována v závislosti na použitém hardwaru a nabízí konzistentní výkon procesoru pro spuštěné instance bez ohledu na hardware. Pokud chcete zjistit fyzický hardware, na kterém je virtuální počítač dané velikosti nasazený, zadejte dotaz na virtuální hardware přímo z virtuálního počítače.

- Virtuální počítače D-series slouží ke spouštění aplikací, které potřebují vyšší výpočetní výkon a krátkodobý vysoký diskový výkon. Virtuální počítače D-series mají rychlejší procesory, vyšší poměr paměti na jádro a jednotky SSD pro dočasný disk. Podrobnosti najdete v oznámení na blogu Azure: [Nové velikosti virtuálních počítačů D-Series](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

- Virtuální počítače Dv2-series, nástupce původní řady D-series, mají výkonnější procesor. Procesor v Dv2-series je asi o 35 % rychlejší než procesor v D-series. Využívá nejnovější generaci procesorů Intel Xeon® E5-2673 v3 (Haswell) s frekvencí 2,4 GHz, kterou může technologie Intel Turbo Boost 2.0 zvýšit až na 3,1 GHz. Řada Dv2-series má stejnou konfiguraci paměti a disku jako řada D.

- Velikosti úrovně Basic jsou primárně určeny pro úlohy související s vývojem a další aplikace, pro které se nevyžaduje vyrovnávání zatížení, automatické škálování nebo používání virtuálních počítačů s vysokými nároky na paměť. Informace o velikostech virtuálních počítačů vhodnějších pro produkční aplikace najdete v tématu (Velikosti virtuálních počítačů)[virtual-machines-size-specs.md] a informace o cenách virtuálních počítačů najdete v tématu [Ceny virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="dsv3-series"></a>Dsv3-series

ACU: 160–190

Velikosti Dsv3-series jsou založeny na procesorech 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell), jejichž frekvence se s technologií Intel Turbo Boost 2.0 může zvýšit až na 3,5 GHz, a používají službu Storage úrovně Premium. Velikosti Dsv3-series nabízejí kombinaci procesoru, paměti a místního disku pro většinu provozních úloh.


| Velikost             | Virtuální procesory | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|------------------|-----------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2s_v3  | 2         | 8           | 16             | 4              | 4 000 / 32 (50)                                                       | 3 200 / 48                                | 2 / střední                                   |
| Standard_D4s_v3  | 4         | 16          | 32             | 8              | 8 000 / 64 (100)                                                      | 6 400 / 96                                | 2 / střední                                   |
| Standard_D8s_v3  | 8         | 32          | 64             | 16             | 16 000 / 128 (200)                                                    | 12 800 / 192                              | 4 / vysoká                                       |
| Standard_D16s_v3 | 16        | 64          | 128            | 32             | 32 000 / 256 (400)                                                    | 25 600 / 384                              | 8 / vysoká                                       |


## <a name="dv3-series"></a>Dv3-series

ACU: 160–190

Velikosti Dv3-series jsou založeny na procesorech 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell) a s technologií Intel Turbo Boost 2.0 se jejich frekvence může zvýšit až na 3,5 GHz. Velikosti Dv3-series nabízejí kombinaci procesoru, paměti a místního disku pro většinu provozních úloh.

Úložiště datových disků se účtuje nezávisle na virtuálních počítačích. Pokud chcete použít disky Storage úrovně Premium, použijte velikosti Dsv3. Měřiče cen a účtování pro velikosti Dsv3 jsou stejné jako pro Dv3-series. 


| Velikost             | Virtuální procesory | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|------------------|-----------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2_v3  | 2         | 8           | 16             | 4              | 4 000 / 32 (50)                                                       | 3 200 / 48                                | 2 / střední                                   |
| Standard_D4_v3  | 4         | 16          | 32             | 8              | 8 000 / 64 (100)                                                      | 6 400 / 96                                | 2 / střední                                   |
| Standard_D8_v3  | 8         | 32          | 64             | 16             | 16 000 / 128 (200)                                                    | 12 800 / 192                              | 4 / vysoká                                       |
| Standard_D16_v3 | 16        | 64          | 128            | 32             | 32 000 / 256 (400)                                                    | 25 600 / 384                              | 8 / vysoká                                       |


## <a name="dsv2-series"></a>DSv2-series

ACU: 210–250

| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4 000 / 32 (43) |3 200 / 48 |2 / 750 |
| Standard_DS2_v2 |2 |7 |14 |4 |8 000 / 64 (86) |6 400 / 96 |2 / 1 500 |
| Standard_DS3_v2 |4 |14 |28 |8 |16 000 / 128 (172) |12 800 / 192 |4 / 3 000 |
| Standard_DS4_v2 |8 |28 |56 |16 |32 000 / 256 (344) |25 600 / 384 |8 / 6 000 |
| Standard_DS5_v2 |16 |56 |112 |32 |64 000 / 512 (688) |51 200 / 768 |8 / 6 000–12 000 &#8224;|



## <a name="dv2-series"></a>Dv2-series

ACU: 210–250

| Velikost              | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Maximální propustnost místního disku: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1_v2    | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 2 / 2×500                         | 2 / 750                 |
| Standard_D2_v2    | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / 1 500                     |
| Standard_D3_v2    | 4         | 14          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / 3 000                     |
| Standard_D4_v2    | 8         | 28          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / 6 000                     |
| Standard_D5_v2    | 16        | 56          | 800            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / 6 000–12 000 &#8224;          |


<br>

## <a name="ds-series"></a>DS-series
| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Max. datových disků | Maximální propustnost disku s mezipamětí: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4 000 / 32 (43) |3 200 / 32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |4 |8 000 / 64 (86) |6 400 / 64 |2 / 1 000 |
| Standard_DS3 |4 |14 |28 |8 |16 000 / 128 (172) |12 800 / 128 |4 / 2 000 |
| Standard_DS4 |8 |28 |56 |16 |32 000 / 256 (344) |25 600 / 256 |8 / 4 000 |

<br>
## <a name="d-series"></a>D-series 

ACU: 160

| Velikost         | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Maximální propustnost místního disku: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 2 / 2×500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / 1 000                     |
| Standard_D3  | 4         | 14          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / 2 000                     |
| Standard_D4  | 8         | 28          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / 4 000                     |

<br>


## <a name="av2-series"></a>Av2-series

ACU: 100

| Velikost            | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | Maximální propustnost místního disku: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) | 
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000 / 20 / 10                                           | 2 / 2×500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000 / 40 / 20                                           | 4 / 4×500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000 / 80 / 40                                           | 8 / 8×500               | 4 / 1 000                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000 / 160 / 80                                          | 16 / 16×500             | 8 / 2 000                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000 / 40 / 20                                           | 4 / 4×500               | 2 / 500                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000 / 80 / 40                                           | 8 / 8×500               | 4 / 1 000                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000 / 160 / 80                                          | 16 / 16×500             | 8 / 2 000                     |

<br>

## <a name="a-series"></a>A-Series

ACU: 50–100

| Velikost | Procesorová jádra | Paměť: GiB | Místní HDD: GiB | Max. datových disků | Maximální propustnost datového disku: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0* |1 |0,768 |20 |1 |1×500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2×500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4 |4×500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8×500 |2 / 1 000 |
| Standard_A4 |8 |14 |605 |16 |16×500 |4 / 2 000 |
| Standard_A5 |2 |14 |135 |4 |4×500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8×500 |2 / 1 000 |
| Standard_A7 |8 |56 |605 |16 |16×500 |4 / 2 000 |
<br>

* Velikost A0 využívá fyzický hardware na maximum. Pouze pro tuto konkrétní velikost můžou výkon vašich spuštěných úloh ovlivnit jiná zákaznická nasazení. Relativní výkon je popsaný níže jako očekávaný základ, přičemž se dá očekávat variabilita přibližně 15 procent.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Velikosti Standard A0–A4 při použití rozhraní příkazového řádku a PowerShellu
V klasickém modelu nasazení se některé názvy virtuálních počítačů s prostředím příkazového řádku a PowerShellu trochu liší:

* Standard_A0 je ExtraSmall 
* Standard_A1 je Small
* Standard_A2 je Medium
* Standard_A3 je Large
* Standard_A4 je ExtraLarge

## <a name="basic-a"></a>Basic A

|Velikost – Velikost\Název |Procesorová jádra|Memory (Paměť)|Síťové karty (Max.)|Max. velikost dočasného disku |Max. počet datových disků (každý o velikosti 1 023 GB)|Max. IOPS (300 na disk)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1×300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2×300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4×300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8×300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16×300|
