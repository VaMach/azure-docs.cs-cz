---
title: "Nastavit zdroje a cíle pro replikaci technologie Hyper-V do Azure (s nástrojem System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky nastavit zdroj a cíl nastavení pro replikaci virtuálních počítačů technologie Hyper-V v cloudech VMM do úložiště Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: c72f839d0a1288dccb7deb3e44fc2b20d64818f0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-with-vmm-replication-to-azure"></a>Krok 8: Nastavte zdroj a cíl pro replikaci technologie Hyper-V (s nástrojem VMM) do Azure.

Po [vytvoření trezoru](vmm-to-azure-walkthrough-create-vault.md) a určení, co chcete replikovat, použijte tento článek ke konfiguraci nastavení zdrojové a cílové při replikaci místní virtuální počítače Hyper-V v cloudech System Center Virtual Machine Manager (VMM) do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

S1. Klikněte na **Připravit infrastrukturu** > **Zdroj**.

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. V okně **Připravit zdroj** klikněte na **+ VMM** a přidejte server VMM.

    ![Nastavení zdroje](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. V okně **Přidat server** zkontrolujte, že se v části **Typ serveru** zobrazuje **server System Center VMM** a že server VMM splňuje [obecné požadavky a požadavky na adresu URL](#prerequisites).
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery.
5. Stáhněte si registrační klíč. Budete ho potřebovat, když spustíte instalaci. Klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-the-provider-on-the-vmm-server"></a>Instalace zprostředkovatele na server VMM

1. Na serveru VMM spusťte instalační soubor zprostředkovatele.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V okně **Instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**.

    ![Umístění instalace](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. Po dokončení instalace klikněte na **Zaregistrovat**, aby se server VMM zaregistroval v trezoru.
5. Na stránce **Nastavení trezoru** klikněte na **Procházet** a vyberte soubor klíče trezoru. Určuje předplatné Azure Site Recovery a název trezoru.

    ![Registrace serveru](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. V části **Připojení k Internetu** určete, jak se zprostředkovatel, který běží na serveru VMM, připojí k Site Recovery přes internet.

   * Pokud chcete, aby se zprostředkovatel připojil přímo, vyberte **Připojit k Azure Site Recovery přímo bez proxy serveru**.
   * Pokud váš existující proxy server vyžaduje ověření nebo chcete použít vlastní proxy server, vyberte **Připojit k Azure Site Recovery pomocí proxy serveru**.
   * Pokud budete používat vlastní proxy server, musíte zadat adresu, port a přihlašovací údaje.
   * Pokud používáte proxy server, měli byste už mít povolené adresy URL popisované v [požadavcích](#on-premises-prerequisites).
   * Pokud používáte vlastní proxy server, vytvoří se automaticky účet VMM RunAs (DRAProxyAccount) pomocí zadaných přihlašovacích údajů proxy serveru. Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. Nastavení účtu VMM RunAs můžete upravovat v konzole VMM. V části **Nastavení** rozbalte položku **Zabezpečení** > **Účty Spustit jako** a potom změňte heslo pro DRAProxyAccount. Budete muset službu VMM restartovat, aby se toto nastavení projevilo.

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. Přijměte nebo změňte umístění certifikátu SSL, který je automaticky generován pro šifrování dat. Tento certifikát se používá, pokud povolíte šifrování dat pro cloud chráněný platformou Azure na portálu Azure Site Recovery. Uchovávejte tento certifikát v bezpečí. Při spuštění předání služeb při selhání do Azure ho budete potřebovat k dešifrování, pokud je povolené šifrování dat.
8. Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM.
9. Pokud chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru, zaškrtněte políčko **Synchronizovat metadata cloudu**. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete provádět synchronizaci se všemi cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole VMM. Kliknutím na **Zaregistrovat** proces dokončete.

    ![Registrace serveru](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. Spustí se registrace. Po dokončení registrace se server zobrazí v části **Infrastruktura Site Recovery** > **Servery VMM**.


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Instalace agenta Služeb zotavení Azure na hostitele Hyper-V

1. Po nastavení zprostředkovatele si musíte stáhnout instalační soubor pro agenta Recovery Services. Spusťte instalační program na každém serveru Hyper-V v cloudu VMM.

    ![Lokality Hyper-V](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. Na stránce **Kontrola předpokladů** klikněte na **Další**. Automaticky se nainstalují všechny chybějící požadované součásti.

    ![Požadavky agenta Služeb zotavení](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. V části **Nastavení instalace** přijměte nebo změňte umístění instalace a mezipaměti. Mezipaměť můžete nakonfigurovat na jednotce, na které je minimálně 5 GB dostupného úložiště. Pro mezipaměť ale doporučujeme disk minimálně s 600 GB volného místa. Pak klikněte na **Nainstalovat**.
4. Po dokončení instalace klikněte na **Zavřít**.

    ![Registrace agenta MARS](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Agenta Služeb zotavení Microsoft Azure můžete nainstalovat z příkazového řádku pomocí následujícího příkazu:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Nastavení internetového přístupu proxy serveru k Site Recovery z hostitelů Hyper-V

Agent Služeb zotavení, který běží na hostitelích Hyper-V, potřebuje internetový přístup k Azure pro replikaci virtuálního počítače. Pokud získáváte přístup k internetu prostřednictvím serveru proxy, nastavte ho následujícím způsobem:

1. Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostiteli Hyper-V. Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Konfigurace proxy** zadejte informace o proxy serveru.

    ![Registrace agenta MARS](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. Zkontrolujte, že má agent přístup k adresám URL popsaným v části s [požadavky](#on-premises-prerequisites).

## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí
Zadejte účet úložiště Azure, který se má používat pro replikaci, a síť Azure, ke které se virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné a skupinu prostředků, ve kterých chcete vytvořit virtuální počítače, pro které bylo provedeno převzetí služeb při selhání. Vyberte model nasazení (Classic nebo Resource Manager), který chcete v Azure použít pro virtuální počítače, pro které bylo provedeno převzetí služeb při selhání.

    ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. Pokud jste ještě nevytvořili účet úložiště a chcete ho vytvořit pomocí Resource Manageru, klikněte na **+ Účet úložiště** a můžete to provést přímo tady.  V okně **Vytvořit účet úložiště** zadejte název účtu, typ, předplatné a umístění. Účet by měl být ve stejném umístění jako trezor Služeb zotavení.

   ![Úložiště](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * Pokud chcete vytvořit účet úložiště pomocí klasického modelu, můžete to udělat na webu Azure Portal. [Další informace](../storage/common/storage-create-storage-account.md)
   * Pokud pro replikovaná data používáte účet úložiště Storage úrovně Premium, nastavte si ještě účet úložiště úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat.
5. Pokud jste ještě nevytvořili síť Azure a chcete ji vytvořit pomocí modelu Resource Manageru, klikněte na **+Síť** a můžete to provést přímo tady. V okně **Vytvořit virtuální síť** zadejte název sítě, rozsah adres, podrobnosti o podsíti, předplatné a umístění. Síť by měla být ve stejném umístění jako trezor Služeb zotavení.

   ![Síť](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   Pokud chcete vytvořit síť pomocí klasického modelu, udělejte to na portálu Azure Portal. [Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).





## <a name="next-steps"></a>Další kroky

Přejděte na [krok 9: nakonfigurování mapování sítě](vmm-to-azure-walkthrough-network-mapping.md)
