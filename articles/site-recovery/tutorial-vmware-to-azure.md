---
title: "Nastavit zotavení po havárii do Azure pro místní virtuální počítače VMware s Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak nastavit zotavení po havárii do Azure pro virtuální počítače VMware místní službou Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8acc8deff8b635c97e8722d65a728aebf0e49bb3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Nastavení pro virtuální počítače VMware místní zotavení po havárii do Azure

Tento kurz ukazuje, jak nastavit zotavení po havárii do Azure pro místní virtuální počítače VMware s Windows. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Zadejte replikace zdroje a cíle.
> * Nastavení prostředí zdroje replikace, včetně místní součásti Site Recovery a cílovém prostředí replikace.
> * Vytvoření zásady replikace
> * Povolení replikace pro virtuální počítač

Toto je třetí kurz v řadě. V tomto kurzu předpokládá, že jste už dokončili úkoly v předchozí kurzy:

1. [Příprava Azure](tutorial-prepare-azure.md)
2. [Příprava VMware v místním prostředí](tutorial-prepare-on-premises-vmware.md)

Než začnete, je vhodné [zkontrolujte architekturu](concepts-vmware-to-azure-architecture.md) pro scénáře zotavení po havárii.


## <a name="select-a-replication-goal"></a>Vyberte cíle replikace

1. V **trezory služeb zotavení**, klikněte na název trezoru **ContosoVMVault**.
2. V **Začínáme**, klikněte na možnost obnovení lokality. Pak klikněte na tlačítko **Příprava infrastruktury**.
3. V **cíl ochrany** > **kam se vaše počítače umístěné**, vyberte **místní**.
4. V ** kde chcete replikovat počítače, vyberte **do Azure**.
5. V **jsou vaše počítače virtualizovaných**, vyberte **Ano, s hypervisoru VMware vSphere**. Pak klikněte na **OK**.

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavení prostředí zdroje, musíte jedinou vysokou dostupností, na místním počítači do součásti Site Recovery místního hostitele. Součásti zahrnují konfigurační server, procesový server a hlavní cílový server.

- Konfigurační server koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci dat.
- Procesový server funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure. Procesový server také nainstaluje služba Mobility na virtuálních počítačích, které chcete replikovat, a provádí automatického zjišťování virtuálních počítačů VMware na místě.
- Hlavní cílový server zpracovává replikační data během navrácení služeb po obnovení z Azure.

Nastavení konfigurace serveru jako vysoce dostupný virtuální počítač VMware, stáhněte si šablonu připravené OVF a importovat šablony do VMware vytvořte virtuální počítač. Po nastavení konfigurace serveru, registrace v trezoru. Po registraci Site Recovery zjišťuje místní virtuální počítače VMware.

### <a name="download-the-vm-template"></a>Stáhněte si šablonu virtuálního počítače

1. V úložišti, přejděte na **Příprava infrastruktury** > **zdroj**.
2. V **připravit zdroj**, klikněte na tlačítko **+ konfigurační server**.
3. V **přidat Server**, zkontrolujte, zda **konfigurační server pro VMware** se zobrazí v **typ serveru**.
4. Stáhněte šablonu otevřete virtualizace formát OVF () pro konfigurační server.

  > [!TIP]
  Nejnovější verzi šablony konfigurace serveru si můžete stáhnout přímo z [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

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

### <a name="configure-settings-and-connect-to-vmware"></a>Konfigurace nastavení a připojte se k VMware

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


Site Recovery se připojuje k VMware servery pomocí zadané nastavení a vyhledá virtuální počítače.

> [!NOTE]
> Může trvat 15 minut nebo déle, než se název účtu objeví na portálu. Chcete-li provést aktualizaci okamžitě, klikněte na **Konfigurační servery** > ***název serveru*** > **Aktualizovat server**.

## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte a ověřte cílové prostředky.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![Cíl](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Vytvoření zásady replikace

1. Otevřete [portál Azure](https://portal.azure.com) a klikněte na **všechny prostředky**.
2. Klikněte na trezoru služby zotavení s názvem **ContosoVMVault**.
3. Chcete-li vytvořit zásadu replikace, klikněte na tlačítko **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.
4. V **vytvořit zásadu replikace**, zadejte název zásady **VMwareRepPolicy**.
5. V **prahovou hodnotu RPO**, použít výchozí hodnotu 60 minut. Tato hodnota určuje, jak často jsou vytvořeny body obnovení. Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.
6. V **uchování bodu obnovení**, použijte výchozí 24 hodin pro jak dlouho je okno uchování pro každý bod obnovení. V tomto kurzu jsme vyberte 72 hodin. Replikované virtuální počítače můžete obnovit do libovolného bodu v okně.
7. V **frekvence snímkování konzistentní aplikace vzhledem**, použít výchozí hodnotu 60 minut frekvence vytvořený snímky konzistentní s aplikacemi. Klikněte na tlačítko **OK** k vytvoření zásad.

   ![Zásada](./media/tutorial-vmware-to-azure/replication-policy.png)

Zásady se automaticky přidruží konfigurační server. Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady. Například, pokud je zásada replikace **rep zásad** bude navrácení služeb po obnovení zásad **rep. zásady navrácení**. Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.

## <a name="enable-replication"></a>Povolení replikace

Obnovení lokality nainstaluje službu Mobility, když je povolená replikace pro virtuální počítač. Může trvat 15 minut nebo déle než se změny ovlivňuje a zobrazovat na portálu.

Následujícím způsobem povolte replikaci:

1. Klikněte na tlačítko **replikujte aplikaci** > **zdroj**.
2. V **zdroj**, vyberte konfigurační server.
3. V **počítač typ**, vyberte **virtuální počítače**.
4. V **vCenter vSphere Hypervisor**, vyberte server vCenter, který spravuje hostitelů vSphere nebo vyberte hostitele.
5. Vyberte procesový server (konfigurační server). Klikněte na tlačítko IThen **OK**.
6. V **cíl**, vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit neúspěšný přes virtuální počítače. Vyberte model nasazení, kterou chcete použít v Azure (klasický nebo prostředek management), pro selhání virtuálních počítačů.
7. Vyberte účet úložiště Azure, který chcete použít pro replikaci dat.
8. Vyberte síť Azure a podsíť, ke kterým se připojí virtuální počítače Azure, když se po převzetí služeb při selhání vytvoří.
9. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač.
10. V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.
11. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte účet, který se použije pro automaticky instalaci služby Mobility na počítači procesní server.
12. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, zda je vybrán správný replikace zásad.
13. Klikněte na tlačítko **povolit replikaci**.

Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

K monitorování virtuálních počítačů, které přidáte, můžete zkontrolovat poslední zjištěných pro virtuální počítače v **konfigurační servery**
> **Obraťte se na poslední**. Chcete-li přidat virtuální počítače bez čekání na naplánovaného zjišťování, zvýrazněte konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Spuštění postupu zotavení po havárii](site-recovery-test-failover-to-azure.md)
