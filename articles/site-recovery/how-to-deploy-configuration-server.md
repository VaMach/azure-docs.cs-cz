---
title: " Nasazení se konfigurační server pro obnovení po havárii VMware s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak nasadit konfigurační server pro obnovení po havárii VMware s Azure Site Recovery"
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: anoopkv
ms.openlocfilehash: e257ede08ac46ad863b4883b10399058e6f59f1f
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="deploy-a-configuration-server"></a>Nasazení konfigurace serveru

Při použití nasazení místní konfigurační server [Azure Site Recovery](site-recovery-overview.md) služby pro zotavení po havárii virtuálních počítačů VMware a fyzické servery do Azure. Konfigurační server WThe koordinuje komunikaci mezi místní VMware a Azure a spravuje replikaci dat. Tento článek vás provede kroky potřebné k nasazení konfigurační server.

## <a name="prerequisites"></a>Požadavky

Doporučujeme nasadit jako vysoce dostupný virtuální počítač VMware konfigurační server. Konfigurační server můžete nastavit na fyzický počítač pro replikaci fyzický server. Minimální požadavky na hardware jsou shrnuté v následující tabulce.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Plánování kapacity

Nastavení velikosti požadavky pro konfiguraci serveru závisí na potenciální míry změny dat. Tuto tabulku použijte jako vodítko.

| **CPU** | **Paměť** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz 4 jádra) |16 GB |300 GB |500 GB nebo méně |Replikovat počítače méně než 100. |
| 12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) |18 GB |600 GB |500 GB až 1 TB |Replikovat mezi 100 150 počítačů. |
| 16 Vcpu (2 sockets * @ 2,5 GHz 8 jader) |32 GB |1 TB |1 TB 2 TB |Replikovat mezi 150 až 200 počítačů. |


Pokud replikujete virtuální počítače VMware, další informace o [informace o plánování kapacity](/site-recovery-plan-capacity-vmware.md)a spusťte [nástroj pro nasazení planner](site-recovery-deployment-planner.md) pro replikaci VMWare.



## <a name="download-the-template"></a>Stažení šablony

Site Recovery poskytuje ke stažení šablony k nastavení konfigurace serveru jako vysoce dostupný virtuální počítač VMware. 

1. V úložišti, přejděte na **Příprava infrastruktury** > **zdroj**.
2. V **připravit zdroj**, klikněte na tlačítko **+ konfigurační server**.
3. V **přidat Server**, zkontrolujte, zda **konfigurační server pro VMware** se zobrazí v **typ serveru**.
4. Stáhněte šablonu otevřete virtualizace formát OVF () pro konfigurační server.

  > [!TIP]
  Nejnovější verzi šablony konfigurace serveru si můžete stáhnout přímo z [Microsoft Download Center](https://aka.ms/asrconfigurationserver)


## <a name="import-the-template-in-vmware"></a>Import šablony VMware


1. Přihlaste se VMware vCenter server nebo vSphere ESXi hostitele – pomocí klientem VMWare vSphere.
2. Na **soubor** nabídce vyberte možnost **nasazení šablony OVF**, spusťte Průvodce šablonou nasazení OVF.  

     ![OVF šablony](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. V **zvolit zdroj**, zadejte umístění stažené OVF.
4. V **podrobnosti**, klikněte na tlačítko **Další**.
5. V **vybrat název a složku**, a **vyberte konfigurace**, přijměte výchozí nastavení.
6. V **vyberte úložiště**, nejlepší výkon, vyberte **konvenčním zřídit přes, dojde k vynulování** v **vyberte virtuální disk formát**.
4. Ve zbývající části stránky průvodce přijměte výchozí nastavení.
5. V **přejít k dokončení**:
  - Chcete-li nastavit virtuální počítač s výchozím nastavením, vyberte **zapnout po nasazení** > **Dokončit**.
  - Pokud chcete přidat další síťové rozhraní, zrušte **zapnout po nasazení**a potom vyberte **Dokončit**. Ve výchozím nastavení šablony konfigurace serveru je nasazený s jednu síťovou kartu, ale po nasazení můžete přidat další síťové adaptéry.


## <a name="add-an-additional-adapter"></a>Přidat další adaptér

Pokud chcete přidat další síťový adaptér na konfiguračním serveru, učiňte předtím, než zaregistrujete server v trezoru. Přidání další adaptéry není podporováno po registraci.

1. V inventáři vSphere klienta, klikněte pravým tlačítkem na virtuální počítač a vyberte **upravit nastavení**.
2. V **hardwaru**, klikněte na tlačítko **přidat** > **adaptér Ethernet**. Pak klikněte na tlačítko **Další**.
3. Vyberte a typ adaptéru a síť. 
4. Chcete-li připojit virtuální síťovou kartu, pokud je virtuální počítač je zapnutá, vyberte **připojení v power na**. Klikněte na tlačítko **Další** > **Dokončit**a potom klikněte na **OK**.
 

## <a name="register-the-configuration-server"></a>Zaregistrujte konfigurační server 

1. Z konzoly VMWare vSphere klienta zapněte na virtuálním počítači.
2. Virtuální počítač se spustí do prostředí instalace systému Windows Server 2016. Přijmout licenční smlouvu a zadat heslo správce.
3. Po dokončení instalace, přihlaste se k virtuálnímu počítači jako správce.
4. Při prvním přihlášení, spustí se nástroje Konfigurace obnovení lokality Azure.
5. Zadejte název, který se použije k registraci konfigurační server pomocí Site Recovery. Pak klikněte na tlačítko **Další**.
6. Tento nástroj kontroluje, zda virtuální počítač může připojit k Azure. Po připojení, klikněte na tlačítko **přihlášení**, k přihlášení do vašeho předplatného Azure. Přihlašovací údaje musí mít přístup k úložišti, ve kterém chcete zaregistrujte konfigurační server.
7. Tento nástroj provádí některé úlohy konfigurace a potom restartuje.
8. Přihlaste se na počítač znovu. Průvodce konfigurací serveru správy se spustí automaticky.

### <a name="configure-settings"></a>Konfigurace nastavení

1. V Průvodci konfigurací serveru správy > **nastavení připojení**, vyberte síťový adaptér, který bude přijímat provoz replikace. Potom klikněte na **Uložit**. Toto nastavení nelze změnit po dokončení své konfigurace.
2. V **trezor služeb zotavení vyberte**, vyberte předplatné Azure a příslušnou skupinu zdrojů a trezoru.
3. V **instalovat software jiných výrobců**, přijměte licenční agreeemtn a klikněte na **stáhněte a nainstalujte**, chcete-li nainstalovat MySQL Server.
4. Klikněte na tlačítko **nainstalovat VMware PowerLCI**. Zajistěte, aby že to provedete se napřed zavřít všechna okna prohlížeče. Pak klikněte na tlačítko **pokračovat**
5. V **ověřením konfigurace zařízení**, požadavky se ověří, než budete pokračovat.
6. V **konfigurovat vCenter Server vSphere ESXi server**, zadejte plně kvalifikovaný název domény nebo IP adresa serveru vCenter nebo hostitelů vSphere, na které virtuálních počítačích, které chcete replikovat nacházejí. Zadejte port, na kterém naslouchá server a popisný název, který se má použít pro server VMware v trezoru.
7. Zadejte přihlašovací údaje, které se použijí pro připojení k serveru VMware konfigurační server. Obnovení lokality pomocí těchto přihlašovacích údajů k automatickému zjišťování virtuální počítače VMware, které jsou k dispozici pro replikaci. Klikněte na tlačítko **přidat**a potom klikněte na **pokračovat**.
8. V **konfigurace virtuálního počítače pověření**, zadejte uživatelské jméno a heslo, které se použije k automaticky instalace služby Mobility na počítačích, pokud je zapnutá replikace. Pro počítače s Windows že účet potřebuje oprávnění místního správce na počítačích, které chcete replikovat. Pro Linux zadejte podrobnosti pro kořenový účet.
9. Klikněte na tlačítko **dokončení konfigurace** dokončit registraci. 
10. Po dokončení registrace na portálu Azure, zkontrolujte, že konfigurační server a VMware server jsou uvedené **zdroj** stránky v úložišti. Pak klikněte na tlačítko **OK** ke konfiguraci nastavení cíle.


## <a name="troubleshoot-deployment-issues"></a>Řešení problémů při nasazování

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Další postup

Přečtěte si podrobné pokyny pro nastavení zotavení po havárii [virtuální počítače VMware](tutorial-vmware-to-azure.md) a [fyzických serverů](tutorial-physical-to-azure.md) do Azure.
