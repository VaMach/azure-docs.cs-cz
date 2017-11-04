---
title: "Nastavení prostředí pro zdroj (VMware do Azure) | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit prostředí místní spuštění replikace virtuálních počítačů VMware do Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 49cc01e54158b68e440f1efbfe2f8b0d076a5692
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Nastavení prostředí pro zdroj (VMware do Azure)
> [!div class="op_single_selector"]
> * [Z VMware do Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fyzické do Azure](./site-recovery-set-up-physical-to-azure.md)

Tento článek popisuje, jak nastavit v místním prostředí k zahájení replikace virtuálních počítačů spuštěných na VMware do Azure.

## <a name="prerequisites"></a>Požadavky

Článek předpokládá, že jste již vytvořili:
- Trezor služeb zotavení v [portál Azure](http://portal.azure.com "portál Azure").
- Vyhrazený účet do systému VMware vCenter, který lze použít pro [automatického zjišťování](./site-recovery-vmware-to-azure.md).
- Virtuální počítač, na které se mají nainstalovat konfigurační server.

## <a name="configuration-server-minimum-requirements"></a>Minimální požadavky na konfiguraci serveru
Následující tabulka uvádí minimální hardwaru, softwaru a požadavky sítě pro konfigurační server.

> [!IMPORTANT]
> Pokud nasazujete konfigurační Server pro ochranu virtuálních počítačů VMware, doporučujeme jej jako nasadíte **vysoce dostupné (HA)** virtuálního počítače.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Servery proxy server HTTPS nejsou podporovány konfigurační server.

## <a name="choose-your-protection-goals"></a>Volba cílů ochrany

1. V portálu Azure přejděte do **služeb zotavení** okno trezoru a vyberte svůj trezor.
2. V nabídce prostředků úložiště, přejděte na **Začínáme** > **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. V **cíl ochrany**, vyberte **do Azure**a zvolte **Ano, s hypervisoru VMware vSphere**. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí
Nastavení zdrojového prostředí zahrnuje dva hlavní činnosti:

- Nainstalujte a zaregistrujte konfigurační server pomocí Site Recovery.
- Zjistit virtuální počítače na místní připojením Site Recovery na vaše místní VMware vCenter nebo vSphere EXSi hostitele.

### <a name="step-1-install-and-register-a-configuration-server"></a>Krok 1: Instalace a zaregistrujte konfigurační server

1. Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**. V **připravit zdroj**, pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server** aby vám ho přidal.

    ![Nastavení zdroje](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. Na **přidat Server** okno, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.
5. Stáhnout registrační klíč trezoru Když spustíte instalační program Unified musíte registrační klíč. Klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Na počítači, který používáte jako konfigurační server, spusťte **Unified instalace nástroje Azure Site Recovery** instalace konfigurační server, procesový server a hlavní cílový server.

#### <a name="run-azure-site-recovery-unified-setup"></a>Spuštění Azure Site Recovery sjednocený instalační program

> [!TIP]
> Konfigurace serveru registrace selže, pokud čas počítače systémové hodiny se liší od místního času ve více než pět minut. Synchronizovat systémových hodin s [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) před zahájením instalace.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Konfigurační server lze nainstalovat pomocí příkazového řádku. Další informace najdete v tématu [instalace serveru konfigurace pomocí nástroje příkazového řádku](http://aka.ms/installconfigsrv).

#### <a name="add-the-vmware-account-for-automatic-discovery"></a>Přidejte účet VMware pro automatické zjišťování

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>Krok 2: Přidání systému vCenter
Povolit Azure Site Recovery se zjistit virtuální počítače spuštěné v místním prostředí, kterou potřebujete připojit VMware vCenter Server nebo hostitelů vSphere ESXi službou Site Recovery.

Vyberte **+ vCenter** zahájíte připojení serveru VMware vCenter server nebo hostiteli ESXi VMware vSphere.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Běžné problémy
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Další kroky
[Nastavení cílového prostředí](./site-recovery-prepare-target-vmware-to-azure.md) v Azure.
