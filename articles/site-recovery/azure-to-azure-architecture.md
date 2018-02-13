---
title: "Architektura replikace Azure do Azure ve službě Azure Site Recovery | Microsoft Docs"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů Azure mezi oblastmi Azure pomocí služby Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 409dd26cc1dfcb1c562d175a43e842b213501d03
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="azure-to-azure-replication-architecture"></a>Architektura replikace Azure do Azure


Tento článek popisuje architekturu používá při replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů Azure (VM) mezi oblastí Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) služby.

>[!NOTE]
>Azure replikace virtuálního počítače se službou Site Recovery je aktuálně ve verzi preview.



## <a name="architectural-components"></a>Komponenty architektury

Následující obrázek poskytuje podrobný pohled prostředí virtuálního počítače Azure v určité oblasti (v tomto příkladu umístění východní USA). V prostředí virtuálního počítače Azure:
- Aplikace může mít spuštěný na virtuálních počítačích s disky, které šíří mezi různými účty úložiště.
- Virtuální počítače můžou být součástí jedné nebo více podsítí v rámci virtuální sítě.


**Replikace Azure do Azure**

![prostředí zákazníka](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Proces replikace

### <a name="step-1"></a>Krok 1

Když povolíte replikaci virtuálního počítače Azure, v následujících zdrojích informací jsou automaticky vytvořen ve cílová oblast na základě nastavení oblasti zdroje. Můžete přizpůsobit nastavení cílové prostředky podle potřeby.

![Povolit replikaci proces, krok 1](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Prostředek** | **Podrobnosti**
--- | ---
**Cílová skupina prostředků** | Skupinu prostředků, do které patří replikované virtuální počítače po převzetí služeb při selhání.
**Cílová virtuální síť** | Virtuální síť, ve kterém jsou replikované virtuální počítače umístěné po převzetí služeb při selhání. Mapování sítě se vytvoří mezi zdrojovými a cílovými virtuální sítě a naopak.
**Účty úložiště mezipaměti** | Předtím, než změny zdrojů virtuálního počítače jsou replikovány do cílového účtu úložiště, jsou sledovány a odešle na účet úložiště mezipaměti v umístění zdroje. Tento krok zajistí minimální dopad na produkční aplikace běžící na virtuálním počítači.
**Cílové úložiště účty**  | Účty úložiště v cílovém umístění, do které se replikují data.
**Cílové skupiny dostupnosti**  | Sady dostupnosti, které jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.

### <a name="step-2"></a>Krok 2

Jak je zapnutá replikace, rozšíření Site Recovery Mobility service se automaticky nainstaluje na virtuální počítač:

1. Virtuální počítač není zaregistrována Site Recovery.

2. Pro virtuální počítač je nakonfigurován průběžnou replikaci. Datové zápisy na disky virtuálních počítačů se přenášejí nepřetržitě k účtu úložiště mezipaměti ve zdrojovém umístění.

   ![Povolit replikaci proces, krok 2](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)


 Site Recovery nikdy potřebuje příchozí připojení k virtuálnímu počítači. Pro následující je potřeba jenom odchozí připojení.

 - Adresy URL nebo IP adresy služby Obnovení lokality
 - Office 365 ověřování adresy URL nebo IP adresy
 - Mezipaměť úložiště účet IP adresy

Po povolení konzistence pro víc Virtuálních počítačů v replikační skupině vzájemně komunikovat přes port 20004. Ujistěte se, že neexistuje žádné zařízení brány firewall interní komunikaci mezi virtuálními počítači přes port 20004 blokování.

> [!IMPORTANT]
Pokud chcete virtuální počítače s Linuxem jako součást skupiny replikace, zajistěte, aby že odchozí přenosy na portu 20004 je otevřen ručně podle pokynů na konkrétní verzi systému Linux.

### <a name="step-3"></a>Krok 3

Po průběžná replikace v průběhu zápisu disku přenesou okamžitě k účtu úložiště mezipaměti. Site Recovery zpracovává data a odešle ji do cílového účtu úložiště. Po zpracování dat, body obnovení jsou generovány v cílový účet úložiště každých několik minut.

## <a name="failover-process"></a>Proces převzetí služeb při selhání

Při zahájení převzetí služeb při selhání, virtuální počítače vytvořené v cílová skupina prostředků, cílová virtuální síť, cílové podsíti a v sadě dostupnosti cíl. Při selhání můžete použít jakýkoli bod obnovení.

![Proces převzetí služeb při selhání](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Další postup

[Rychle replikovat](azure-to-azure-quickstart.md) virtuálního počítače Azure v sekundární oblasti.
