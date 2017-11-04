



Pro účely porovnávání výpočetního síly (výkonu procesoru) jednotlivých SKU v Azure jsme vytvořili koncept výpočetní jednotky Azure – ACU. To vám umožní snadno zjistit, jaká jednotka SKU nejlépe vyhoví vašim požadavkům na výkon.  Jednotka ACU je aktuálně stanovená tak, že malý virtuální počítač (Standard_A1) má 100 ACU a ostatní jednotky SKU jsou pak ohodnoceny podle relativního výsledku standardního srovnávacího testu. 

> [!IMPORTANT]
> ACU je jenom informativní údaj.  Výsledek v případě konkrétních úloh může být odlišný. 
> 
> 

<br>

| Rodina SKU | ACU \ virtuální procesor |
| --- | --- |
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 |
| [A1-A4](../articles/virtual-machines/windows/sizes-general.md) |100 |
| [A5-A7](../articles/virtual-machines/windows/sizes-general.md) |100 |
| [A1_v2-A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 |
| [A2m_v2-A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 |
| [A8-A11](../articles/virtual-machines/windows/sizes-hpc.md) |225* |
| [D1-D14](../articles/virtual-machines/windows/sizes-general.md) |160 |
| [D1_v2-D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210 - 250* |
| [DS1-DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 |
| [DS1_v2-DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* |
| [D_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160–190* ** |
| [Ds_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160–190* ** |
| [E_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160–190* ** |
| [Es_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160–190* ** |
| [F2s_v2 F72s_v2](../articles/virtual-machines/windows/sizes-compute.md) |195-210* |
| [F1-F16](../articles/virtual-machines/windows/sizes-compute.md) |210-250* |
| [F1s-F16s](../articles/virtual-machines/windows/sizes-compute.md) |210-250* |
| [G1-G5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* |
| [GS1-GS5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* |
| [H](../articles/virtual-machines/windows/sizes-hpc.md) |290 - 300* |
| [L4s-L32s](../articles/virtual-machines/windows/sizes-storage.md) |180 - 240* |
| [M](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) | 160–180** |

Hodnoty ACU s hvězdičkou označují použití technologie Intel® Turbo, která může zvýšit frekvenci procesoru podle aktuální potřeby.  Množství nárůst se může lišit v závislosti na velikosti virtuálního počítače, úlohy a dalších úlohách spuštěných na stejném hostiteli.

** Vlákna typu Hyper. 
