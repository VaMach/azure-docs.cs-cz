---
title: "Nastavit zdroje a cíle replikace VMware do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky nastavit zdroj a cíl nastavení pro replikaci virtuálních počítačů VMware do úložiště Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 94b629a62c3a54eee69ee397b2f27e3f20b753d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-vmware-replication-to-azure"></a>Krok 8: Nastavení zdroje a cíle replikace VMware do Azure

Tento článek popisuje postup konfigurace nastavení zdrojové a cílové při replikaci na lokální virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavení konfigurace serveru, registrace v trezoru a vyhledání virtuálních počítačů.

1. Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.
2. Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.
3. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.
5. Stáhnout registrační klíč trezoru Tuto funkci potřebujete po spuštění Unified instalace. Klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a>Zaregistrujte konfigurační server v trezoru

Předtím, než můžete spustit, a potom spuštěním Unified instalačního programu nainstalujte konfigurační server, procesový server a hlavní cílový server, postupujte takto.
    - Vám zajistí rychlý přehled video

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Na serveru, konfigurace virtuálního počítače, zkontrolujte, jestli se systémové hodiny synchronizované se [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Měla by se shodovat. Pokud je 15 minut před nebo za instalace může selhat.
    - Spusťte instalační program jako místní správce na serveru konfigurace virtuálního počítače.
    - Ujistěte se, že je povolená TLS 1.0 ve virtuálním počítači.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Konfigurační server lze také nainstalovat [z příkazového řádku](http://aka.ms/installconfigsrv).



## <a name="connect-to-vmware-servers"></a>Připojení k serverům VMware

Povolit Azure Site Recovery se zjistit virtuální počítače spuštěné v místním prostředí, kterou potřebujete připojit VMware vCenter Server nebo hostitelů vSphere ESXi službou Site Recovery. Než začnete, vezměte na vědomí následující:

- Pokud přidáte vCenter server nebo hostitelů vSphere pro obnovení lokality pomocí účtu bez oprávnění správce na serveru, že účet potřebuje těchto oprávnění povoleno:
    - Datacenter, úložiště, složka, hostitele, sítě, prostředků, virtuální počítač, vSphere distribuované přepínače.
    - VCenter server potřebuje oprávnění úložišť zobrazení.
- Když přidáte servery VMware server pro obnovení lokality, může trvat 15 minut nebo déle se objevily na portálu.

### <a name="add-the-account-for-automatic-discovery"></a>Přidejte účet pro automatické zjišťování

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a>Nastavit připojení

Připojení k serverům následujícím způsobem:

1. Vyberte **+ vCenter** zahájíte připojení serveru VMware vCenter server nebo hostiteli ESXi VMware vSphere.
2. V části **Přidat server vCenter** zadejte popisný název pro hostitele vSphere nebo server vCenter, a poté zadejte IP adresu nebo plně kvalifikovaný název domény serveru.
3. Pokud vaše servery VMware nejsou konfigurované k naslouchání požadavkům na jiném portu, ponechte port 443. Vyberte účet, který se má připojit k serveru VMware vCenter nebo vSphere ESXi. Klikněte na **OK**.
4. Site Recovery se připojuje k VMware servery pomocí zadané nastavení a vyhledá virtuální počítače.

> [!NOTE]
> Pokud přidáváte server nebo hostitele pomocí účtu, který nemá oprávnění správce na serveru vCenter nebo hostitele, ujistěte se, že má účet oprávnění tyto povoleno: Datacenter, úložiště, složky, hostitele, sítě, prostředků, virtuální počítač a vSphere distribuované přepínače. Kromě toho serveru VMware vCenter server potřebuje oprávnění k zobrazení úložiště povolené.


## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Než nastavíte cílovém prostředí, ujistěte se, že máte účet úložiště Azure a virtuální sítě nastavit.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![cíl](./media/vmware-walkthrough-source-target/gs-target.png)
4. Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, abyste mohli vytvořit účet správce prostředků nebo sítě vložené.

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 9: nastavení zásad replikace](vmware-walkthrough-replication.md)
