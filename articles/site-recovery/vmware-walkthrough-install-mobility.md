---
title: "Nainstalujte službu Mobility u VMware do Azure replikace | Microsoft Docs"
description: "Tento článek popisuje, jak nainstalovat agenta služby Mobility pro VMware do Azure s replikací pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: bc520bd2ea54208889861a7a3b275e3008a05d53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-install-the-mobility-service"></a>Krok 10: Nainstalujte službu Mobility


Tento článek popisuje postup konfigurace nastavení zdrojové a cílové při replikaci na lokální virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

Služba Mobility zaznamenává datové zápisy na počítači a předává je na procesní server. Musí být nainstalován na každý počítač, který chcete replikovat do Azure.

Můžete nainstalovat ručně službu Mobility, pomocí nabízené instalace z procesového serveru Site Recovery, pokud je zapnutá replikace, nebo pomocí nástroje System Center Configuration Manager. Pokud používáte nabízenou instalaci, služba je nainstalovaná ve virtuálním počítači, pokud je zapnutá replikace.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>Ruční instalace

1. Zkontrolujte [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) pro ruční instalaci.
2. Postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) pro ruční instalaci pomocí portálu.
3. Pokud preferujete instalaci z příkazového řádku, postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-the-process-server"></a>Instalace z procesového serveru

Pokud chcete nabízená instalace služby Mobility z procesového serveru, když povolíte replikaci pro virtuální počítač, je třeba účet, který lze použít procesní server přístup k virtuálnímu počítači. Účet se používá pouze pro nabízené instalace.

1. Měli byste mít [vytvořili účet](vmware-walkthrough-prepare-vmware.md) který lze použít pro nabízenou instalaci. Potom zadejte účet, který chcete použít při konfiguraci nastavení zdroje během nasazování Site Recovery.
2. Potom postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Pokud chcete k nabízení služby Mobility na virtuální počítače se systémem Windows nebo Linux.

## <a name="other-methods"></a>Ostatní metody

Další informace o [instalaci služby Mobility, pomocí nástroje Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), nebo pomocí [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 11: povolení replikace](vmware-walkthrough-enable-replication.md)
