
Grafický procesor optimalizované virtuálních počítačů velikosti jsou k dispozici jeden nebo více grafickými procesory NVIDIA specializované virtuálních počítačů. Tyto velikosti jsou navrženy pro úlohy náročné na výkon, velmi náročná na výkon grafiky a vizualizace. Tento článek obsahuje informace o počet a typ grafickými procesory, Vcpu, datové disky a síťové adaptéry a také úložiště propustnost a šířku pásma sítě pro každou velikost v této skupině. 

* **NC NCv2 a ND** velikosti jsou optimalizované pro aplikace náročné na výkon a velmi náročná na výkon sítě a algoritmy, včetně aplikací na základě CUDA a OpenCL a simulace, AI a hloubkové učení. 
* **VS** velikosti jsou optimalizované a určené pro vzdálené vizualizace, streamování, herní, kódování a využitím architektury například OpenGL a DirectX scénáře VDI.  


## <a name="nc-instances"></a>Instance NC

Instance NC jsou zapnuté podle [NVIDIA tesla – měrná K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) karty. Uživatel může zpracujte prostřednictvím rychlejší dat s využitím CUDA pro zkoumání aplikace energie, havárií simulace, ray trasovaná vykreslování, hloubkové učení a další. Konfigurace NC24r poskytuje nízkou latencí a vysokou propustností síťové rozhraní, které jsou optimalizované pro úzce párované paralelní výpočetních úloh.


| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | GPU | Maximální počet datových disků |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 24 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 |

1 GPU = polovina karty K80.

*Podpora RDMA

## <a name="ncv2-instances"></a>NCv2 instancí

Instance NCv2 jsou nové generace počítačů NC-series, používá technologii [P100 tesla – měrná NVIDIA](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) grafickými procesory. Tyto grafickými procesory můžete zadat více než 2 x výpočetní výkon aktuální NC-series. Zákazníci mohou využít výhod těchto aktualizované grafickými procesory pro tradiční úlohy HPC jako modelování zásob, DNA sekvencování, bílkovin analýzy, simulací typu Monte Carlo a dalších. Podobně jako NC-series poskytuje NCv2-series konfigurace s nízkou latencí a vysokou propustností síťové rozhraní, které jsou optimalizované pro úzce párované paralelní výpočetních úloh.

> [!IMPORTANT]
> Pro tuto řadu velikost virtuálních procesorů (základní) kvóty ve vašem předplatném původně nastavena na 0 v každé oblasti. [Požádat o zvýšení kvóty virtuální procesory](../articles/azure-supportability/resource-manager-core-quotas-request.md) pro tuto řadu v [dostupné oblasti](https://azure.microsoft.com/regions/services/).
>

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | GPU | Maximální počet datových disků |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6_v2 |6 |112 | 336 | 1 | 12 |
| Standard_NC12_v2 |12 |224 | 672 | 2 | 24 |
| Standard_NC24_v2 |24 |448 | 1344 | 4 | 32 |
| Standard_NC24r_v2 * |24 |1448 | 1344 | 4 | 32 |

Grafický procesor 1 = P100 karet.

*Podpora RDMA

## <a name="nd-instances"></a>ND instancí

Virtuální počítače a series jsou nové přidání do rodiny GPU určený pro AI a hloubkové Learning úlohy. Nabízí vynikající výkon pro školení a odvozená. Jsou zapnuté a instance podle [P40 tesla – měrná NVIDIA](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) grafickými procesory. Tyto instance poskytnout vynikající výkon pro jednoduchou přesností operace s plovoucí desetinnou, pro zatížení AI využitím kognitivní nástrojů Microsoft, TensorFlow, Caffe a jiné architektury. Řada ND-Series také nabízí mnohem větší paměť GPU (24 GB) a umožňuje zpracovat mnohem větší modely neuronových sítí. Podobně jako NC-series, a series poskytuje konfigurace s sekundární nízkou latencí, Vysoká propustnost sítě prostřednictvím RDMA a připojení InfiniBand, takže je možné spustit ve velkém měřítku školení úlohy pokrývání uzlů mnoho grafickými procesory.

> [!IMPORTANT]
> Pro tuto řadu velikost virtuálních procesorů (základní) kvóta na oblasti v rámci vašeho předplatného zpočátku nastavena na 0. [Požádat o zvýšení kvóty virtuální procesory](../articles/azure-supportability/resource-manager-core-quotas-request.md) pro tuto řadu v [dostupné oblasti](https://azure.microsoft.com/regions/services/).
>

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | GPU | Maximální počet datových disků |
| --- | --- | --- | --- | --- | --- |
| Standard_ND6 |6 |112 | 336 | 1 | 12 |
| Standard_ND12 |12 |224 | 672 | 2 | 24 |
| Standard_ND24 |24 |448 | 1344 | 4 | 32 |
| Standard_ND24r * |24 |1448 | 1344 | 4 | 32 |

Grafický procesor 1 = P40 karet.

*Podpora RDMA

## <a name="nv-instances"></a>Instance NV



Instance vs jsou zapnuté podle [NVIDIA tesla – měrná M60 ](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) grafickými procesory a mřížky NVIDIA technologie pro plochu accelerated aplikace a virtuální plochy kde Zákazníci mohou vizualizovat svoje data nebo simulace. Uživatelé mohou k vizualizaci své grafiky náročné pracovní postupy v rámci instancí vs získat nadřízená grafiky schopnosti a navíc spouštět úlohy a jednoduchou přesností například kódování a vykreslování. 

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | GPU | Maximální počet datových disků |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 24 |
| Standard_NV12 |12 |112 |680 | 2 | 48 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 |

1 GPU = polovina karty M60.


