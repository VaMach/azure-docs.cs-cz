---
title: "Nastavit zdroje a cíle pro fyzický server replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky k zadání nastavení zdroje a cíle replikace fyzických serverů do úložiště Azure se službou Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e89bbf5a2c1d71852e49da43d3106a05ebfc28a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-the-source-and-target-for-physical-server-replication-to-azure"></a>Krok 7: Nastavte zdroj a cíl pro fyzický server replikaci do Azure.

Tento článek popisuje, jak nakonfigurovat nastavení zdrojové a cílové během replikace místní fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavení konfigurace serveru, registrace v trezoru a zjistit počítače.

1. Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.
2. Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.
3. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.
5. Stáhnout registrační klíč trezoru Tuto funkci potřebujete po spuštění Unified instalace. Klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a>Zaregistrujte konfigurační server v trezoru

Předtím, než můžete spustit, a potom spuštěním Unified instalačního programu nainstalujte konfigurační server, procesový server a hlavní cílový server, postupujte takto. Video popisuje nastavení součásti pro virtuální počítač VMware do Azure replikace. Stejný postup je však platný pro fyzický server na Azure replikaci.

- Na serveru, konfigurace virtuálního počítače, zkontrolujte, jestli se systémové hodiny synchronizované se [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Měla by se shodovat. Pokud je 15 minut před nebo za instalace může selhat.
- Spusťte instalační program jako místní správce na serveru, konfigurace.
- Ujistěte se, že je povolená TLS 1.0 ve virtuálním počítači.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Konfigurační server lze také nainstalovat [z příkazového řádku](http://aka.ms/installconfigsrv).




## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Než nastavíte cílovém prostředí, ujistěte se, že máte účet úložiště Azure a virtuální sítě nastavit.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![cíl](./media/physical-walkthrough-source-target/gs-target.png)

4. Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, abyste mohli vytvořit účet správce prostředků nebo sítě vložené.

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 8: nastavení zásad replikace](physical-walkthrough-replication.md)
