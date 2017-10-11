---
title: "Replikace virtuálních počítačů Hyper-V do Azure na portálu classic | Microsoft Docs"
description: "Tento článek popisuje, jak replikovat virtuální počítače Hyper-V do Azure, když počítače nejsou spravované v cloudech VMM."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 3f4c4483-e3dd-495a-bd02-c16e9e28c88d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/21/2017
ms.author: raynew
ms.openlocfilehash: 438f32ee3605e2dd0c46de7993a359cc269262fe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-without-vmm-with-azure-site-recovery"></a>Replikaci mezi místními technologie Hyper-V virtuální počítače a Azure (bez VMM) s Azure Site Recovery
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Portál Classic](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

Tento článek popisuje, jak replikovat místní technologie Hyper-V virtuální počítače Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure. V tomto scénáři nejsou spravované servery Hyper-V v cloudech VMM.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery na portálu Azure

Azure má dva různé [modely nasazení](../resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi: Azure Resource Manager a Classic. Obsahuje také dva portály: portál Azure Classic a web Azure Portal.

Tento článek popisuje postup nasazení na portálu classic. Portál Classic může sloužit k udržování existujících trezorů. Pomocí portálu Classic nelze vytvářet nové trezory.

## <a name="site-recovery-in-your-business"></a>Site Recovery v podnikání

Organizace potřebují strategii BCDR, která určuje, jak aplikace a data zůstanou spuštěné a dostupné během plánovaných a neplánovaných výpadků a jak co nejdříve obnovit normální provozní podmínky. Tady je výčet, co může Site Recovery poskytnout:

* Ochrana mimo pracoviště pro firemní aplikace běžící na virtuálních počítačích Hyper-V
* Jedno umístění pro nastavování, správu a sledování replikace, převzetí služeb při selhání a zotavení
* Snadné převzetí služeb při selhání do Azure a navrácení služeb po obnovení z Azure na hostitelské servery technologie Hyper-V ve vaší místní lokalitě
* Plány obnovy, které obsahují několik virtuálních počítačů, aby u vrstvených úloh aplikací docházelo ke společnému převzetí služeb při selhání

## <a name="azure-prerequisites"></a>Požadavky Azure
* Potřebujete účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
* K ukládání replikovaných dat potřebujete účet úložiště Azure. Účet musí povolenou geografickou replikací. Měl by být ve stejné oblasti jako trezor Azure Site Recovery a být přidružený ke stejnému předplatnému. [Další informace o Azure storage](../storage/common/storage-introduction.md). Všimněte si, že nepodporujeme přesunutí účty úložiště vytvořené pomocí [nový portál Azure](../storage/common/storage-create-storage-account.md) mezi skupinami prostředků.
* Budete potřebovat virtuální síť Azure, aby virtuální počítače Azure se připojí k síti, při selhání z primární lokality.

## <a name="hyper-v-prerequisites"></a>Požadavky technologie Hyper-V
* Ve zdrojové lokalitě místní budete potřebovat jeden nebo více serverů se systémem **Windows Server 2012 R2** s nainstalovanou rolí technologie Hyper-V nebo **Microsoft Hyper-V Server 2012 R2**. Tento server proveďte následující kroky:
* Obsahovat jeden nebo více virtuálních počítačů.
* Být připojen k Internetu, buď přímo nebo prostřednictvím proxy serveru.
* Používat opravy popsané v článku KB [2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977").

## <a name="virtual-machine-prerequisites"></a>Požadavky virtuálního počítače
Virtuální počítače, které chcete chránit musí být v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="provider-and-agent-prerequisites"></a>Zprostředkovatel a agent požadavky
Jako součást nasazení Azure Site Recovery nainstalujete zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure na každém serveru technologie Hyper-V. Poznámky:

* Doporučujeme vždy spustit nejnovější verze zprostředkovatele a agenta. Tyto jsou k dispozici na portálu Site Recovery.
* Všechny servery Hyper-V v trezoru musí mít stejné verze zprostředkovatel a agent.
* Zprostředkovatel, který běží na serveru se připojí k Site Recovery přes internet. Musíte bez serveru proxy, s nastavením proxy serveru, který je aktuálně konfigurována na serveru Hyper-V, nebo s nastavením vlastní proxy server, které můžete konfigurovat během instalace zprostředkovatele. Budete potřebovat a ujistěte se, zda proxy serveru, který chcete použít k přístup tyto adresy URL pro připojení k Azure:

  * *.accesscontrol.windows.net
  * *.backup.windowsazure.com
  * *.hypervrecoverymanager.windowsazure.com
  * *.store.core.windows.net      
  * *.blob.core.windows.net
  - https://www.msftncsi.com/ncsi.txt
  - time.windows.com
  - time.nist.gov
* Kromě toho povolit IP adresy popsané v [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653) a protokol HTTPS (443). Budete muset povolit rozsahy IP oblasti Azure, který chcete použít a západní USA.

Tento obrázek zobrazuje různých komunikačních kanálů a porty používané službou Site Recovery pro orchestration a replikace

![B2A topologie](./media/site-recovery-hyper-v-site-to-azure-classic/b2a-topology.png)

## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru
1. Přihlaste se k [portálu pro správu](https://portal.azure.com).
2. Rozbalte položku **datové služby** > **služeb zotavení** a klikněte na tlačítko **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. Jako **Název** zadejte popisný název pro identifikaci trezoru.
5. V rozevírací nabídce **Oblast** vyberte zeměpisnou oblast trezoru. Informace o tom, které oblasti jsou podporované, najdete v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) v části s geografickou dostupností.
6. Klikněte na **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-hyper-v-site-to-azure-classic/vault.png)

Podle informací na stavovém řádku si ověřte, že se trezor úspěšně vytvořil. Trezor bude uvedený jako **aktivní** na hlavní stránce Služeb zotavení.

## <a name="step-2-create-a-hyper-v-site"></a>Krok 2: Vytvoření web Hyper-V
1. Na stránce služeb zotavení klikněte na trezor otevřete stránku rychlý Start. Stránku Rychlý Start je možné kdykoli otevřít pomocí ikony.

    ![Rychlý start](./media/site-recovery-hyper-v-site-to-azure-classic/quick-start-icon.png)
2. V rozevíracím seznamu vyberte **mezi místními servery technologie Hyper-V a Azure**.

    ![Scénář web Hyper-V](./media/site-recovery-hyper-v-site-to-azure-classic/select-scenario.png)
3. V **vytvořit web Hyper-V** klikněte na tlačítko **web vytvořit Hyper-V**. Zadejte název lokality a uložte.

    ![Web Hyper-V](./media/site-recovery-hyper-v-site-to-azure-classic/create-site.png)

## <a name="step-3-install-the-provider-and-agent"></a>Krok 3: Instalace zprostředkovatel a agent
Nainstalujte zprostředkovatel a agent na každém serveru technologie Hyper-V, s virtuálními počítači, který chcete chránit.

Pokud instalujete na clusteru s podporou technologie Hyper-V, provede kroky 5-11 na každém uzlu v clusteru převzetí služeb při selhání. Po všechny uzly jsou zaregistrované a povolit ochranu, budou virtuální počítače chráněné i v případě, že migraci mezi uzly v clusteru.

1. V **Příprava technologie Hyper-V serverů**, klikněte na tlačítko **stáhněte si registrační klíč** souboru.
2. Na **stáhnout registrační klíč** klikněte na **Stáhnout** vedle webu. Stáhněte si klíč do bezpečného umístění, které můžete snadno přístupné serverem Hyper-V. Klíč je platný po dobu 5 dní, po jeho vygenerování.

    ![Registrační klíč](./media/site-recovery-hyper-v-site-to-azure-classic/download-key.png)
3. Klikněte na tlačítko **stáhnout poskytovatele** získat nejnovější verzi.
4. Spusťte soubor na každém serveru technologie Hyper-V, který chcete zaregistrovat v trezoru. Soubor nainstaluje dvě součásti:
   * **Zprostředkovatele Azure Site Recovery**– zpracovává komunikace a orchestraci mezi serverem Hyper-V a na portálu Azure Site Recovery.
   * **Azure agenta služeb zotavení**– zpracovává přenos dat mezi virtuálními počítači běžícími na zdrojovém technologie Hyper-V a úložiště Azure.
5. V nastavení služby **Microsoft Update** můžete vyjádřit výslovný souhlas se stahováním a instalací aktualizací. Toto nastavení povoleno, zprostředkovatel a Agent aktualizace se nainstalují souladu s vašimi zásadami Microsoft Update.

    ![Aktualizace Microsoft Update](./media/site-recovery-hyper-v-site-to-azure-classic/provider1.png)
6. V **instalace** zadejte, kam chcete nainstalovat zprostředkovatel a Agent na serveru Hyper-V.

    ![Umístění instalace](./media/site-recovery-hyper-v-site-to-azure-classic/provider2.png)
7. Po dokončení instalace pokračujte v instalaci, zaregistrujte server v trezoru.
8. Na **nastavení trezoru** klikněte na tlačítko **Procházet** a vyberte soubor klíče. Zadejte předplatné Azure Site Recovery, název trezoru a Web Hyper-V, do které patří server technologie Hyper-V.

    ![Registrace serveru](./media/site-recovery-hyper-v-site-to-azure-classic/provider8.PNG)
9. Na **připojení k Internetu** stránku, je určit, jak se zprostředkovatel připojuje k Azure Site Recovery. Pokud chcete použít výchozí nastavení připojení k internetu nakonfigurované na serveru, vyberte **Použít výchozí nastavení proxy serveru systému**. Pokud nezadáte hodnotu použije se výchozí nastavení.

   ![Nastavení internetu](./media/site-recovery-hyper-v-site-to-azure-classic/provider7.PNG)
10. Spustí se registrace k registraci serveru v trezoru.

    ![Registrace serveru](./media/site-recovery-hyper-v-site-to-azure-classic/provider15.PNG)
11. Po dokončení registrace metadata z technologie Hyper-V server načte Azure Site Recovery a server se zobrazí na **lokalit Hyper-V** na kartě **servery** stránky v úložišti.

### <a name="install-the-provider-from-the-command-line"></a>Nainstalujte zprostředkovatele z příkazového řádku
Jako alternativu můžete nainstalovat zprostředkovatele Azure Site Recovery z příkazového řádku. Tato metoda by měla použít, pokud chcete nainstalovat zprostředkovatele na počítači se systémem Windows Server Core 2012 R2. Spusťte z příkazového řádku následujícím způsobem:

1. Stáhněte si instalační soubor zprostředkovatele a registrační klíč do složky. Například C:\ASR.
2. Spusťte příkazový řádek jako správce a zadejte:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Potom zprostředkovatele nainstalujte spuštěním:

        C:\ASR> setupdr.exe /i
4. Spusťte následující příkaz k dokončení registrace:

        CD C:\Program Files\Microsoft Azure Site Recovery Provider
        C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Kde parametry patří:

* **/ Credentials**: Zadejte umístění registrační klíč, který jste stáhli.
* **/ FriendlyName**: Zadejte název pro identifikaci hostitelský server Hyper-V. Tento název se zobrazí na portálu
* **/ EncryptionEnabled**: volitelné. Zadejte, jestli chcete šifrovat repliky virtuálního počítače v Azure (šifrování neaktivních dat).
* **/ proxyaddress**; **/proxyport**; **/proxyusername**; **/proxypassword**: volitelné. Zadejte parametry serveru proxy, pokud chcete používat vlastní proxy server nebo váš stávající proxy server vyžaduje ověření.

## <a name="step-4-create-an-azure-storage-account"></a>Krok 4: Vytvoření účtu úložiště Azure
* V **Příprava prostředků** vyberte **vytvořit účet úložiště** vytvořit účet úložiště Azure, pokud nemáte. Účet musí mít povolenou geografickou replikací. Musí být ve stejné oblasti jako trezor Azure Site Recovery a být přidružený ke stejnému předplatnému.

    ![Vytvořit účet úložiště](./media/site-recovery-hyper-v-site-to-azure-classic/create-resources.png)

> [!NOTE]
> 1. Nepodporujeme přesun účty úložiště vytvořené pomocí [nový portál Azure](../storage/common/storage-create-storage-account.md) mezi skupinami prostředků.
> 2. [Migrace účtů úložiště](../azure-resource-manager/resource-group-move-resources.md) napříč skupinami prostředků v rámci stejného předplatného nebo napříč předplatnými se pro účty úložiště použité k nasazení Site Recovery nepodporuje.
>

## <a name="step-5-create-and-configure-protection-groups"></a>Krok 5: Vytvoření a konfigurace skupin ochrany
Skupiny ochrany jsou logickými seskupeními virtuálních počítačů, které chcete chránit pomocí stejné nastavení ochrany. Použití nastavení ochrany pro skupinu ochrany a tato nastavení se použijí na všechny virtuální počítače, které přidáte do skupiny.

1. V **vytvořit a nakonfigurovat skupiny ochrany** klikněte na tlačítko **vytvořte skupinu ochrany**. Pokud všechny požadované součásti nejsou v místní zpráva se objeví, a můžete kliknout na **zobrazit podrobnosti** Další informace.
2. V **skupin ochrany** přidejte skupinu ochrany. Zadejte název, technologie Hyper-V zdrojové lokality, cíl **Azure**, vaše jméno předplatné Azure Site Recovery a účtu úložiště Azure.

    ![Skupiny ochrany](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group.png)
3. V **nastavení replikace** nastavit **frekvence kopírování** k určení, jak často se mají synchronizovat rozdílová data mezi zdrojem a cílem. Můžete nastavit na 30 sekund, 5 minut nebo 15 minut.
4. V **zachovat body obnovení** zadejte počet hodin historie obnovení by měly být uložené.
5. V **frekvence snímků konzistentní vzhledem k aplikacím** můžete určit, zda vytváření snímků, které pomůže zajistit, že aplikace budou v konzistentním stavu při pořízení snímku služby Stínová kopie svazku (VSS). Ve výchozím nastavení se tyto nezavedou. Ujistěte se, že tato hodnota nastavena na hodnotu menší než počet dalších bodů obnovení, které nakonfigurujete. Je podporováno pouze pokud virtuální počítač běží operační systém Windows.
6. V **čas spuštění počáteční replikace** zadejte, kdy se mají odesílat počáteční replikace virtuálních počítačů ve skupině ochrany do Azure.

    ![Skupiny ochrany](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group2.png)

## <a name="step-6-enable-virtual-machine-protection"></a>Krok 6: Povolení ochrany virtuálního počítače
Přidáte virtuální počítače do skupiny ochrany k povolení ochrany pro ně.

> [!NOTE]
> Ochrana virtuálních počítačů s Linuxem a statickou IP adresou není podporována.
>
>

1. Na **počítače** karta pro skupinu ochrany, klikněte na tlačítko ** Přidat virtuální počítače do skupiny ochrany a povolit ochranu **.
2. Na **povolit ochranu virtuálního počítače** vyberte virtuální počítače, které chcete chránit.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-hyper-v-site-to-azure-classic/add-vm.png)

    Spustí úlohy povolení ochrany. Můžete sledovat průběh **úlohy** kartě. Po spuštění úlohy dokončit ochranu virtuálního počítače je připraven k převzetí služeb při selhání.
3. Po ochrany je nastavení můžete provádět následující akce:

   * Zobrazení virtuálních počítačů v **chráněné položky** > **skupin ochrany** > *protectiongroup_name* > **virtuální počítače** můžete přejít k podrobnostem a podrobnosti počítače v **vlastnosti** kartě...
   * Konfigurovat vlastnosti převzetí služeb při selhání pro virtuální počítače v **chráněné položky** > **skupin ochrany** > *protectiongroup_name* > **virtuální počítače** *virtual_machine_name* > **konfigurace**. Můžete nakonfigurovat:

     * **Název**: název virtuálního počítače v Azure.
     * **Velikost**: cílovou velikost virtuálního počítače, který převezme.

       ![Konfigurovat vlastnosti virtuálního počítače](./media/site-recovery-hyper-v-site-to-azure-classic/vm-properties.png)
   * Nakonfigurujte další virtuální počítač v *chráněné položky** > **skupin ochrany** > *protectiongroup_name* > **virtuální počítače** *virtual_machine_name* > **konfigurace**, včetně:

     * **Síťové adaptéry**: počet síťových adaptérů je závisí na velikosti, kterou zadáte pro cílový virtuální počítač. Zkontrolujte [specifikace velikosti virtuálního počítače](../virtual-machines/linux/sizes.md) pro počet síťových adaptérů nepodporuje velikost virtuálního počítače.

       Když změníte velikost virtuálního počítače a uložíte nastavení, počet síťových adaptérů se změní při příštím otevření stránky **Konfigurace**. Počet síťových adaptérů cílových virtuálních počítačů je minimální počet síťových adaptérů na zdrojovém virtuálním počítači a maximální počet síťových adaptérů nepodporuje velikost virtuálního počítače vybrali. Jsou vysvětleny níže:

       * Pokud je počet síťových adaptérů na zdrojovém počítači menší nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, pak bude mít cíl stejný počet adaptérů jako zdroj.
       * Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.
       * Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.

     * **Síť Azure**: Zadejte síť, ke kterému virtuální počítač by měl převzetí služeb při selhání. Pokud má virtuální počítač více síťových adaptérů, které by všechny adaptéry připojené ke stejné síti Azure.
     * **Podsíť** pro každý síťový adaptér na virtuálním počítači, vyberte podsíť v síti Azure, ke kterému je počítač připojen po převzetí služeb při selhání.
     * **Cílová IP adresa**: Pokud síťový adaptér zdrojového virtuálního počítače je nakonfigurován pro použití statické IP adres, potom můžete zadat IP adresu pro cílový virtuální počítač na zkontrolujte, zda má počítač stejnou IP adresu po převzetí služeb při selhání.  Pokud nezadáte IP adresu budou všechny dostupnou adresu přiřadit v době převzetí služeb při selhání. Pokud zadáte adresu, která se používá, převzetí služeb při selhání se nezdaří.

     > [!NOTE]
     > [Migrace sítí](../azure-resource-manager/resource-group-move-resources.md) napříč skupinami prostředků v rámci stejného předplatného nebo napříč předplatnými se pro sítě použité k nasazení Site Recovery nepodporuje.
     >

     ![Konfigurovat vlastnosti virtuálního počítače](./media/site-recovery-hyper-v-site-to-azure-classic/multiple-nic.png)




## <a name="step-7-create-a-recovery-plan"></a>Krok 7: Vytvoření plánu obnovení
Chcete-li otestovat nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo plán obnovení, který obsahuje jeden nebo více virtuálních počítačů. [Další informace](site-recovery-create-recovery-plans.md) o vytvoření plánu obnovení.

## <a name="step-8-test-the-deployment"></a>Krok 8: Testování nasazení
Testovací předání služeb při selhání do Azure je možné spustit dvěma způsoby.

* **Testovací převzetí služeb při selhání bez sítě Azure:** Tento typ testovacího převzetí služeb při selhání ověří, že virtuální počítač se zobrazí správně v Azure. Virtuální počítač nebude po převzetí služeb při selhání připojen k žádné síti Azure.
* **Testovací převzetí služeb při selhání se sítí Azure**– tento typ převzetí služeb při selhání kontroly očekávaným dodává zobrazí celé prostředí replikace a že převzetí služeb virtuálních počítačů při selhání připojí k zadané cílové síti Azure. Všimněte si, že pro testovací převzetí služeb při selhání podsíť testovacího virtuálního počítače budou být započítáno na základě podsítě virtuálního počítače repliky. Tím se replikace liší od standardní replikace, při které je podsíť virtuálního počítače repliky založena na podsíti zdrojového virtuálního počítače.

Pokud chcete spustit testovací převzetí služeb bez zadání síť Azure, nemusíte připravovat nic dalšího.

Pokud chcete spustit testovací převzetí služeb při selhání s cílovou sítí Azure, budete muset vytvořit novou síť Azure, která bude izolovaná od produkční sítě Azure (což je výchozí chování při vytváření nových sítí v Azure). Čtení [spustit testovací převzetí služeb](site-recovery-failover.md) další podrobnosti.

Replikace a sítě nasazení plně otestovat, budete muset nastavení infrastruktury, aby replikovaného virtuálního počítače fungovat jako očekává. Jeden způsob k provedení nastavit virtuální počítač jako řadič domény s DNS a replikaci do Azure pomocí Site Recovery k jeho vytvoření v testovací síti spuštěním testovací převzetí služeb.  [Další informace](site-recovery-active-directory.md#test-failover-considerations) aspekty testovací převzetí služeb při selhání pro Active Directory.

Spusťte testovací převzetí služeb takto:

> [!NOTE]
> Aby byl při předání služeb při selhání do Azure zajištěn nejvyšší možný výkon, zajistěte, abyste měli na chráněném počítači nainstalovaného agenta Azure. Tato funkce umožňuje rychlejší spouštění a také pomáhá s diagnostikou v případě problémů. Agenta pro Linux najdete [tady](https://github.com/Azure/WALinuxAgent) a agenta pro Windows [tady](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

1. Na kartě **Plány obnovení** vyberte plán a klikněte na **Testovací převzetí služeb při selhání**.
2. Na stránce s **potvrzením testovacího převzetí služeb při selhání** vyberte možnost **Žádné** nebo konkrétní síť Azure.  Všimněte si, že pokud vyberete **žádné** testu převzetí služeb zkontroluje, že virtuální počítač replikoval správně do Azure, ale není Zkontrolujte konfiguraci sítě replikace.

    ![Testovací převzetí služeb při selhání](./media/site-recovery-hyper-v-site-to-azure-classic/test-nonetwork.png)
3. Na kartě **Úloha** můžete sledovat průběh převzetí služeb při selhání. Měli byste také mít možnost zobrazit si testovací repliku virtuálního počítače na portálu Azure. Pokud máte nastaven přístup k virtuálním počítačům ze své místní sítě, můžete iniciovat připojení k virtuálnímu počítači přes Vzdálenou plochu.
4. Když převzetí služeb při selhání dosáhne fáze **Dokončit testování**, dokončete kliknutím na **Dokončit test** testovací převzetí služeb při selhání. Přechodem k podrobnostem na kartě **Úloha** můžete sledovat průběh a stav převzetí služeb při selhání a provádět všechny potřebné akce.
5. Po převzetí služeb při selhání budete moct zobrazit testovací repliku virtuálního počítače na portálu Azure. Pokud máte nastaven přístup k virtuálním počítačům ze své místní sítě, můžete iniciovat připojení k virtuálnímu počítači přes Vzdálenou plochu.

   1. Ověřte, že se virtuální počítače úspěšně spustí.
   2. Pokud se chcete připojit k virtuálnímu počítači v Azure pomocí Vzdálené plochy po převzetí služeb po selhání, pak před spuštěním testu převzetí služeb povolte na virtuálním počítači připojení ke Vzdálené ploše. Musíte taky přidat koncový bod RDP na virtuální počítač. Můžete využít [služby Azure automation runbook](site-recovery-runbook-automation.md) to provést.
   3. Pokud po převzetí služeb při selhání použijete k připojení k virtuálnímu počítači v Azure pomocí vzdálené plochy veřejnou IP adresu, nesmíte mít žádné zásady domény, které by vám bránily v připojení k virtuálnímu počítači pomocí veřejné adresy.
6. Po dokončení testování udělejte toto:

   * Klikněte na **Testovací převzetí služeb při selhání se dokončilo**. Vyčistěte testovací prostředí. Testovací virtuální počítač se automaticky vypne a odstraní.
   * Klikněte na **Poznámky** a zaznamenejte a uložte jakékoli připomínky související s testovacím převzetím služeb při selhání.
7. Když převzetí služeb při selhání dosáhne **dokončit testování** fáze dokončit ověření následujícím způsobem:
   1. Zobrazte si virtuální počítač repliky na portálu Azure. Ověřte, že se virtuální počítač úspěšně spustí.
   2. Pokud máte nastaven přístup k virtuálním počítačům ze své místní sítě, můžete iniciovat připojení k virtuálnímu počítači přes Vzdálenou plochu.
   3. Kliknutím na **Dokončit test** testovací převzetí služeb při selhání dokončete.
   4. Klikněte na **Poznámky** a zaznamenejte a uložte jakékoli připomínky související s testovacím převzetím služeb při selhání.
   5. Klikněte na **Testovací převzetí služeb při selhání se dokončilo**. Vyčistěte testovací prostředí. Testovací virtuální počítač se automaticky vypne a odstraní.

## <a name="next-steps"></a>Další kroky
Po nastavení a zprovoznění nasazení si můžete přečíst [další informace](site-recovery-failover.md) o převzetí služeb při selhání.
