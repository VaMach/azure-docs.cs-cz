---
title: "Zkontrolujte architekturu pro replikaci virtuálních počítačů Azure mezi oblastmi Azure | Microsoft Docs"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů Azure mezi oblastmi Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/08/2017
ms.author: raynew
ms.openlocfilehash: 8251534b2e1e0d223f5e1df5dbd33831604615cb
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="azure-to-azure-replication-architecture"></a>Architektura replikace Azure do Azure


Tento článek popisuje architektuře a procesech používá při replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů Azure (VM) mezi oblastí Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) služby.

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

Když povolíte replikaci virtuálního počítače Azure, prostředky vidíte níže se automaticky vytvoří v cílová oblast na základě nastavení oblasti zdroje. Můžete přizpůsobit nastavení cílové prostředky podle potřeby. 

![Povolit replikaci proces, krok 1](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Prostředek** | **Podrobnosti**
--- | ---
**Cílová skupina prostředků** | Skupinu prostředků, do které patří replikované virtuální počítače po převzetí služeb při selhání.
**Cílová virtuální síť** | Virtuální síť, ve kterém jsou replikované virtuální počítače umístěné po převzetí služeb při selhání. Mapování sítě se vytvoří mezi zdrojovými a cílovými virtuální sítě a naopak.
**Účty úložiště mezipaměti** | Předtím, než změny zdrojů virtuální počítače jsou replikovány do cílového účtu úložiště, jsou sledovány a odešle na účet úložiště mezipaměti v cílovém umístění. Tím se zajistí minimální dopad na produkční aplikace běžící na virtuálním počítači.
**Cílové úložiště účty**  | Účty úložiště v cílovém umístění, do které se replikují data.
**Cílové skupiny dostupnosti**  | Sady dostupnosti, které jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.

### <a name="step-2"></a>Krok 2

Jak je zapnutá replikace, rozšíření Site Recovery Mobility service se automaticky nainstaluje na virtuální počítač:

1. Virtuální počítač není zaregistrována Site Recovery.

2. Pro virtuální počítač je nakonfigurován průběžnou replikaci. Datové zápisy na disky virtuálních počítačů se přenášejí nepřetržitě k účtu úložiště mezipaměti ve zdrojovém umístění.

   ![Povolit replikaci proces, krok 2](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

  
 Site Recovery nikdy potřebuje příchozí připojení k virtuálnímu počítači. Pouze odchozí připojení je nutná k obnovení lokality služby adresy URL nebo IP adresy, Office 365 ověřování adresy URL nebo IP adresy a mezipaměti úložiště účet IP adresy.

### <a name="step-3"></a>Krok 3

Po průběžná replikace v průběhu zápisu disku přenesou okamžitě k účtu úložiště mezipaměti. Site Recovery zpracovává data a odešle ji do cílového účtu úložiště. Po zpracování dat, body obnovení jsou generovány v cílový účet úložiště každých několik minut.

## <a name="failover-process"></a>Proces převzetí služeb při selhání

Při zahájení převzetí služeb při selhání, virtuální počítače vytvořené v cílová skupina prostředků, cílová virtuální síť, cílové podsíti a v sadě dostupnosti cíl. Při selhání můžete použít jakýkoli bod obnovení.

![Proces převzetí služeb při selhání](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Další kroky

Podívat se na matici podpory postupujte podle kurzu k povolení replikace virtuálních počítačů Azure v sekundární oblasti.
Spusťte převzetí služeb při selhání a navrácení služeb po obnovení.