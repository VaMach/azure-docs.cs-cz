---
title: "Jak funguje virtuální počítač Azure replikace mezi oblastmi Azure ve službě Azure Site Recovery?  | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů Azure mezi oblastmi Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: 7a3e1eb129db530295cbf28bbce1dbe48575d50d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a>Jak funguje replikace virtuálního počítače Azure ve službě Site Recovery?


Tento článek popisuje součástech a procesech součástí replikaci a obnovení virtuálních počítačů Azure (VM) z jedné oblasti na jiný pomocí [Azure Site Recovery](site-recovery-overview.md) služby.

>[!NOTE]
>Azure replikace virtuálního počítače se službou Site Recovery je aktuálně ve verzi preview.

Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architectural-components"></a>Komponenty architektury

Následující obrázek poskytuje podrobný pohled prostředí virtuálního počítače Azure v určité oblasti (v tomto příkladu umístění východní USA). V prostředí virtuálního počítače Azure:
- Aplikace může mít spuštěný na virtuálních počítačích s disky, které šíří mezi různými účty úložiště.
- Virtuální počítače můžou být součástí jedné nebo více podsítí v rámci virtuální sítě.

![prostředí zákazníka](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Další informace o požadavcích pro nasazení a požadavky [matici podpory](site-recovery-support-matrix-azure-to-azure.md).

## <a name="replication-process"></a>Proces replikace

### <a name="step-1"></a>Krok 1

Když povolíte replikaci virtuálního počítače Azure na portálu Azure, prostředky uvedené v následující obrázek a tabulka se automaticky vytvoří v cílové oblasti. Ve výchozím nastavení prostředky jsou vytvořeny na základě nastavení oblasti zdroje. Nastavení cílového podle potřeby můžete přizpůsobit. [Další informace](site-recovery-replicate-azure-to-azure.md).

![Povolit replikaci proces, krok 1](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

**Prostředek** | **Podrobnosti**
--- | ---
**Cílová skupina prostředků** | Skupinu prostředků, do které patří replikované virtuální počítače po převzetí služeb při selhání.
**Cílová virtuální síť** | Virtuální síť, ve kterém jsou replikované virtuální počítače umístěné po převzetí služeb při selhání. Mapování sítě se vytvoří mezi zdrojovými a cílovými virtuální sítě a naopak.
**Účty úložiště mezipaměti** | Předtím, než změny na zdrojové virtuální počítače jsou replikovány do cílového účtu úložiště, jsou sledovány a odešle na účet úložiště mezipaměti v cílovém umístění. Tím se zajistí minimální dopad na produkční aplikace běžící na virtuálním počítači.
**Cílové úložiště účty**  | Účty úložiště v cílovém umístění, do které se replikují data.
**Cílové skupiny dostupnosti**  | Sady dostupnosti, které jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.

### <a name="step-2"></a>Krok 2

Jak je zapnutá replikace, rozšíření Site Recovery Mobility service se automaticky nainstaluje na virtuálním počítači. Dojde k následujícímu:

1. Virtuální počítač není zaregistrována Site Recovery.

2. Pro virtuální počítač je nakonfigurován průběžnou replikaci. Datové zápisy na disky virtuálních počítačů se přenášejí nepřetržitě k účtu úložiště mezipaměti ve zdrojovém umístění.

   ![Povolit replikaci proces, krok 2](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > Site Recovery nikdy potřebuje příchozí připojení k virtuálnímu počítači. Virtuální počítač potřebuje pouze odchozí připojení k Site Recovery service adresy URL nebo IP adresy, Office 365 ověřování adresy URL nebo IP adres a adres IP účet úložiště mezipaměti. Další informace najdete v tématu [pokyny sítě pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md) článku.

## <a name="continuous-replication-process"></a>Proces průběžná replikace

Po průběžné replikace funguje, se k účtu úložiště mezipaměti okamžitě přenesou zápisy na disk. Site Recovery zpracovává data a odešle ji do cílového účtu úložiště. Po zpracování dat, body obnovení jsou generovány v cílový účet úložiště každých několik minut.

## <a name="failover-process"></a>Proces převzetí služeb při selhání

Při zahájení převzetí služeb při selhání, virtuální počítače jsou vytvořeny v cílové skupině prostředků, cílová virtuální síť, cílové podsíti a skupina dostupnosti cíl. Při selhání můžete použít jakýkoli bod obnovení.

![Proces převzetí služeb při selhání](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Další kroky

- Další informace o [sítě](site-recovery-azure-to-azure-networking-guidance.md) pro replikaci virtuálního počítače Azure.
- Postupujte podle návodu k [replikovat virtuální počítače Azure.](site-recovery-azure-to-azure.md)
