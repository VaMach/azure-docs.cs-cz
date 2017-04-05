Velikosti NC a NV se také označují jako instance s podporou grafického procesoru. Jsou to specializované virtuální počítače s grafickými kartami NVIDIA, optimalizované pro různé scénáře a případy použití. Velikosti NV jsou optimalizované a navržené pro vzdálené vizualizace, streamování, hry, kódování a VDI díky využití architektur typu OpenGL a DirectX. Velikosti NC jsou vhodnější pro výpočetně a síťově náročné úlohy a algoritmy včetně aplikací a simulací využívajících technologie CUDA a OpenCL. 


Instance NV jsou osazeny grafickými kartami NVIDIA Tesla M60 a NVIDIA GRID pro urychlení desktopových aplikací i virtuálních desktopů, na kterých budou zákazníci vizualizovat svoje data nebo simulace. Uživatelé budou moci vizualizovat své graficky náročné pracovní postupy na instancích NV a využít tak jejich špičkové grafické možnosti, a navíc spouštět úlohy s jednoduchou přesností, například kódování a vykreslování. Tesla M60 má 4096 jader CUDA v provedení dual-GPU a dokáže zpracovat až 36 datových proudů 1080p H.264. 

Instance NC jsou osazeny kartami NVIDIA Tesla K80. Uživatelé se můžou díky technologii CUDA prokousávat daty daleko rychleji, například v aplikacích pro výzkum energie, simulace srážek, vykreslování sledováním paprsku, hloubkové učení a dalších. Tesla K80 má 4992 jader CUDA v provedení dual-GPU a nabízí výkon až 2,91 Teraflops v dvojité přesnosti a až 8,93 Teraflops v jednoduché přesnosti.

## <a name="nv-instances"></a>Instance NV

| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 |
| Standard_NV12 |12 |112 |680 | 2 |
| Standard_NV24 |24 |224 |1440 | 4 |

1 GPU = polovina karty M60.

**Podporované operační systémy**

* Windows Server 2016, Windows Server 2012 R2 – viz [Instalace ovladačů v instancích N-series s Windows](../articles/virtual-machines/windows/n-series-driver-setup.md)

## <a name="nc-instances"></a>Instance NC

| Velikost | Procesorová jádra | Paměť: GiB | Místní SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 |

1 GPU = polovina karty K80.

*Podpora RDMA

**Podporované operační systémy**

* Windows Server 2016, Windows Server 2012 R2 – viz [Instalace ovladačů v instancích N-series s Windows](../articles/virtual-machines/windows/n-series-driver-setup.md)
* Ubuntu 16.04 LTS – viz [Instalace ovladačů v instancích N-series s Linuxem](../articles/virtual-machines/linux/n-series-driver-setup.md)

<br>

