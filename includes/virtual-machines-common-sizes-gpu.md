
Grafický procesor optimalizované virtuálních počítačů velikosti jsou pro speciální virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa. K dispozici jeden nebo více grafickými procesory. Tento článek obsahuje informace o počtu Vcpu, datové disky a síťové adaptéry, jakož i výkon sítě a propustnost úložiště pro každou velikost v této skupině. 


Velikosti NC a NV se také označují jako instance s podporou grafického procesoru. Jedná se o velikosti specializované virtuálních počítačů, které zahrnují karty společnosti NVIDIA, optimalizované pro různé scénáře a případy použití. Velikosti NV jsou optimalizované a navržené pro vzdálené vizualizace, streamování, hry, kódování a scénáře VDI díky využití architektur typu OpenGL a DirectX. Velikosti NC jsou vhodnější pro výpočetně a síťově náročné úlohy a algoritmy včetně aplikací a simulací využívajících technologie CUDA a OpenCL. 


Instance NV jsou osazeny grafickými kartami NVIDIA Tesla M60 a NVIDIA GRID pro urychlení desktopových aplikací i virtuálních klientů, na kterých můžou zákazníci vizualizovat svoje data nebo simulace. Uživatelé mohou k vizualizaci své grafiky náročné pracovní postupy v rámci instancí vs získat nadřízená grafiky schopnosti a navíc spouštět úlohy a jednoduchou přesností například kódování a vykreslování. Tesla M60 má 4096 jader CUDA v provedení dual-GPU a dokáže zpracovat až 36 datových proudů 1080p H.264. 

Instance NC jsou osazeny kartami NVIDIA Tesla K80. Uživatelé se můžou díky technologii CUDA prokousávat daty daleko rychleji, například v aplikacích pro výzkum energie, simulace srážek, vykreslování sledováním paprsku, hloubkové učení a dalších. Tesla K80 má 4992 jader CUDA v provedení dual-GPU a nabízí výkon až 2,91 Teraflops v dvojité přesnosti a až 8,93 Teraflops v jednoduché přesnosti.

## <a name="nv-instances"></a>Instance NV

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | GPU | Maximální počet datových disků |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 24 |
| Standard_NV12 |12 |112 |680 | 2 | 48 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 |

1 GPU = polovina karty M60.

## <a name="nc-instances"></a>Instance NC

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | GPU | Maximální počet datových disků |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 24 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 |

1 GPU = polovina karty K80.

*Podpora RDMA


