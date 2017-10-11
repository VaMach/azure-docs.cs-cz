---
title: "Nainstalujte službu Mobility u fyzického serveru na Azure replikaci | Microsoft Docs"
description: "Tento článek popisuje postup instalace agenta služby Mobility na fyzických serverech replikaci do Azure se službou Azure Site Recovery."
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
ms.openlocfilehash: d73267d7a64221a3138af19e9a2d5dd15809b927
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-9-install-the-mobility-service"></a>Krok 9: Instalace služby Mobility


Tento článek popisuje, jak nainstalovat službu mobility při replikaci místní Windows nebo Linuxem fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

Služba Mobility zaznamenává datové zápisy na počítači a předává je na procesní server. Musí být nainstalován na každém serveru, který chcete replikovat do Azure.

Můžete nainstalovat službu Mobility ručně, nebo pomocí nabízené instalace z procesového serveru Site Recovery, pokud je zapnutá replikace, nebo pomocí nástroje, jako je například System Center Configuration Manager. Pokud používáte nabízenou instalaci, služba je nainstalovaná na serveru, když aktivujete replikaci.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>Ruční instalace

1. Zkontrolujte [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) pro ruční instalaci.
2. Postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) pro ruční instalaci pomocí portálu.
3. Pokud preferujete instalaci z příkazového řádku, postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-the-process-server"></a>Instalace z procesového serveru

Pokud chcete nabízená instalace služby Mobility z procesového serveru, když povolíte replikaci pro počítač, je třeba účet, který lze použít procesní server pro přístup k počítači. Účet se používá pouze pro nabízené instalace.

1. Pokud jste nevytvořili účet, to udělat pomocí těchto pokynů:

    - Můžete použít domény nebo místní účet
    - Pro systém Windows Pokud nepoužíváte účet domény, je nutné zakázat řízení vzdáleného přístupu uživatele v místním počítači. Chcete-li to provést, v registru podle **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD **LocalAccountTokenFilterPolicy**, s hodnotou 1.
    - Pokud chcete přidat položku registru pro Windows z rozhraní příkazového řádku, zadejte:

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - Pro systémy Linux musí být účet root na zdrojovém serveru Linux.

2. Potom postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Pokud chcete k nabízení služby Mobility na virtuální počítače se systémem Windows nebo Linux.

## <a name="other-installation-methods"></a>Jiné metody instalace

- [Další informace o](site-recovery-install-mobility-service-using-sccm.md) instalaci služby Mobility, pomocí nástroje Configuration Manager
- [Další informace o](site-recovery-automate-mobility-service-install.md) instalace s Azure Automation DSC.


## <a name="next-steps"></a>Další kroky

Přejděte na [krok 10: povolení replikace](physical-walkthrough-enable-replication.md)
