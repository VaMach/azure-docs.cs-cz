---
title: "Nastavit zdroje a cíle pro replikaci technologie Hyper-V do Azure (bez System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky nastavit zdroj a cíl nastavení pro replikaci virtuálních počítačů technologie Hyper-V do úložiště Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: b38eb3a011d46f2239891ea1d1bcac2a4059a866
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-replication-to-azure"></a>Krok 8: Nastavte zdroj a cíl pro replikaci technologie Hyper-V do Azure.

Tento článek popisuje, jak nakonfigurovat nastavení zdrojové a cílové během replikovat místní virtuální počítače Hyper-V (bez System Center VMM) do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavte web Hyper-V, nainstalujte zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure na hostitele Hyper-V a zaregistrujte lokality v trezoru.

1. V **Příprava infrastruktury**, klikněte na tlačítko **zdroj**. Chcete-li přidat nový web technologie Hyper-V jako kontejner pro hostitele Hyper-V nebo clusterů, klikněte na tlačítko **serveru technologie Hyper-V +**.

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. V **web vytvořit Hyper-V**, zadejte název pro tuto lokalitu. Pak klikněte na **OK**. Nyní, vyberte lokalitu, jste vytvořili a klikněte na tlačítko **+ technologie Hyper-V Server** přidání serveru do lokality.

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. V **přidat Server** > **typ serveru**, zkontrolujte, zda **technologie Hyper-V server** se zobrazí.

    - Ujistěte se, že server Hyper-V, které chcete přidat v souladu se [požadavky](#on-premises-prerequisites)a bude schopen získat přístup k zadané adresy URL.
    - Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery. Je-li spustit tento soubor k instalaci zprostředkovatele a agenta služeb zotavení na každém hostiteli technologie Hyper-V.

    ![Nastavení zdroje](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-the-provider-and-agent"></a>Nainstaluje zprostředkovatele a agenta

1. Spusťte instalační soubor na každém hostiteli, které jste přidali do lokality Hyper-V zprostředkovatele. Pokud instalujete na clusteru s podporou technologie Hyper-V, spusťte instalační program na každém uzlu clusteru. Instalace a registrace každého uzlu clusteru technologie Hyper-V zajistí, že jsou chráněné virtuální počítače, i v případě, že migraci mezi uzly.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V okně **Instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**.
4. V **nastavení trezoru**, klikněte na tlačítko **Procházet** a vyberte soubor klíče trezoru, který jste si stáhli. Zadejte předplatné Azure Site Recovery, název trezoru a Web Hyper-V, do které patří server technologie Hyper-V.

    ![Registrace serveru](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. V **nastavení proxy serveru**, určete, jak se zprostředkovatel, který běží na hostitelích technologie Hyper-V připojuje k Azure Site Recovery přes internet.

    * Pokud chcete, aby se zprostředkovatel připojil přímo, vyberte **Připojit k Azure Site Recovery přímo bez proxy serveru**.
    * Pokud váš stávající proxy server vyžaduje ověření, nebo pokud chcete používat vlastní proxy server pro připojení poskytovatele, vyberte **připojit k Azure Site Recovery pomocí proxy serveru**.
    * Pokud používáte proxy server:
        - Zadejte adresu, port a přihlašovací údaje
        - Zajistěte, aby k adresám URL popsaným v [požadavky](#prerequisites) jsou povoleny prostřednictvím proxy serveru.

    ![Internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. Po dokončení instalace, klikněte na tlačítko **zaregistrovat** zaregistrujte server v trezoru.

    ![Umístění instalace](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. Po dokončení registrace pomocí Azure Site Recovery je načíst metadata ze serveru technologie Hyper-V a server se zobrazí v **infrastruktura Site Recovery** > **hostitelů Hyper-V**.


## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Zadejte účet úložiště Azure pro replikaci a síť Azure, ke které se virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**.
2. Vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit virtuální počítače Azure po převzetí služeb při selhání. Vyberte model nasazení, který chcete použít pro virtuální počítače v Azure (klasický nebo prostředek management).

3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    - Pokud nemáte účet úložiště, klikněte na tlačítko **+ úložiště** vytvořit vložený založené na správci prostředků účtu. 
    - Pokud nemáte síť Azure, klikněte na tlačítko **+ síť** vytvořit vložený sítě pomocí Správce prostředků.






## <a name="next-steps"></a>Další kroky

Přejděte na [krok 9: nastavení zásad replikace](hyper-v-site-walkthrough-replication.md)
