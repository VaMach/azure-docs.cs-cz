---
title: "Nastavit zotavení po havárii do Azure pro místní virtuální počítače VMware s Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak nastavit zotavení po havárii do Azure pro virtuální počítače VMware místní službou Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 1c9bfe567b1e0872abc7aba054127735d5f61754
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Nastavení pro virtuální počítače VMware místní zotavení po havárii do Azure

V tomto kurzu se dozvíte, jak nastavit zotavení po havárii do Azure VMware místní virtuální počítač se službou Windows. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření trezoru služeb zotavení pro Site Recovery
> * Nastavit zdroje a cíle replikace prostředí
> * Vytvoření zásady replikace
> * Povolení replikace pro virtuální počítač

Toto je třetí kurz v řadě. V tomto kurzu předpokládá, že jste už dokončili úkoly v předchozí kurzy:

1. [Příprava Azure](tutorial-prepare-azure.md)
2. [Příprava VMware v místním prostředí](tutorial-prepare-on-premises-vmware.md)

Než začnete, je vhodné [zkontrolujte architekturu](concepts-vmware-to-azure-architecture.md) pro scénáře zotavení po havárii.

## <a name="configure-vmware-account-permissions"></a>Konfigurace oprávnění pro uživatelský účet VMware

1. Umožňuje vytvořte roli na úrovni vCenter. Zadejte název role **Azure_Site_Recovery**.
2. Přiřaďte následující oprávnění k **Azure_Site_Recovery** role.

   **Úkol** | **Role nebo oprávnění** | **Podrobnosti**
   --- | --- | ---
   **Zjišťování virtuálních počítačů** | Objekt datového centra –> Propagate pro podřízený objekt role = jen pro čtení | Uživatel alespoň jen pro čtení.<br/><br/> Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).
   **Úplná replikace, převzetí služeb při selhání a navrácení služeb po obnovení** |  Objekt datového centra –> Propagate pro podřízený objekt role = Azure_Site_Recovery<br/><br/> Úložiště dat -> přidělte místo, procházet úložiště dat, operace se soubory nízké úrovně, odstraňte soubor, aktualizovat soubory virtuálního počítače<br/><br/> Síť -> přiřazení sítě<br/><br/> Zdroj -> Přiřazení virtuálního počítače do fondu zdrojů, migrovat napájený vypnout virtuální počítač, migrace napájený na virtuálním počítači<br/><br/> Úlohy -> Vytvořit úlohu, úloha aktualizace<br/><br/> Virtuální počítač -> Konfigurace<br/><br/> Virtuální počítač -> interakcí -> odpovědí otázku, připojení zařízení, nakonfigurovat média CD, nakonfigurovat disketová média, vypnout, zapnutí, instalaci nástroje VMware<br/><br/> Virtuální počítač -> inventáře -> vytvořit, registraci, zrušení registrace<br/><br/> Virtuální počítač -> zřizování -> Povolit stahování virtuálního počítače, povolí nahrát soubory virtuálního počítače<br/><br/> Virtuální počítač -> snímky -> odebrat snímky | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).

3. Vytvořte uživatele na hostiteli systému vCenter server nebo vSphere. Přiřazení role uživatele.

## <a name="specify-what-you-want-to-replicate"></a>Zadejte, co chcete replikovat

Služba Mobility musí být nainstalovaná na jednotlivé virtuální počítače, které chcete replikovat. Site Recovery tato služba nainstaluje automaticky při povolení replikace pro virtuální počítač. Pro automatické instalaci je nutné připravit účet, který Site Recovery bude používat pro přístup k virtuálnímu počítači.

Můžete vytvořit, domény nebo místní účet. Pro virtuální počítače s Linuxem musí být účet root na zdrojovém serveru Linux. Pro virtuální počítače Windows, pokud nepoužíváte účet domény, zakažte řízení vzdáleného přístupu uživatele v místním počítači:

  - V registery v části **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD **LocalAccountTokenFilterPolicy** a nastavte hodnotu na 1.

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavení zdrojového prostředí se skládá z stahovat instalaci Unified služby Site Recovery, nastavení konfigurace serveru a její registrací v trezoru a zjišťování virtuálních počítačů.

Konfigurační server je jediný místní virtuální počítač VMware pro hostování všech součástí Site Recovery. Tento virtuální počítač se spustí konfigurační server, procesový server a hlavní cílový server.

- Konfigurační server koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci dat.
- Procesový server funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure. Procesový server také nainstaluje služba Mobility na virtuálních počítačích, které chcete replikovat, a provádí automatického zjišťování virtuálních počítačů na místní servery VMware.
- Hlavní cílový server zpracovává replikační data během navrácení služeb po obnovení z Azure.

Konfigurace serveru virtuálního počítače by měla být vysoce dostupný virtuální počítač VMware, který splňuje následující požadavky:

| **Požadavek** | **Podrobnosti** |
|-----------------|-------------|
| Počet jader procesoru| 8 |
| Paměť RAM | 12 GB |
| Počet disků | 3 - OS disk, disk mezipaměti server proces, jednotka pro uchování (pro navrácení služeb po obnovení) |
| Volné místo na disku (mezipaměť procesového serveru) | 600 GB |
| Volné místo na disku (disk pro uchování) | 600 GB |
| Verze operačního systému | Windows Server 2012 R2 |
| Národní prostředí operačního systému | Angličtina (en-us) |
| Verze VMware vSphere PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") |
| Role Windows Serveru | Tyto role nepovolíte: Active Directory Domain Services, Internetová informační služba. technologie Hyper-V |
| Typ síťový adaptér | VMXNET3 |
| Typ IP adresy | Statická |
| Porty | 443 (orchestrace řídicího kanálu)<br/>9443 (přenos dat)|

Na konfiguračním serveru virtuálních počítačů Ujistěte se, jestli se systémové hodiny synchronizované se čas serveru.
Čas musí být synchronizovány s během 15 minut. Pokud je časový rozdíl je větší než 15 minut, instalace se nezdaří.

Zkontrolujte, zda že server konfigurace virtuálních počítačů přístup tyto adresy URL:

- *. accesscontrol.windows.net. Používá se k řízení přístupu a správě identit.
- *. backup.windowsazure.com. Používá se ke koordinaci a přenosu dat replikace.
- *. blob.core.windows.net. Používá se pro přístup k účtu úložiště, který ukládá replikovaná data.
- *. hypervrecoverymanager.windowsazure.com. Používá se pro koordinaci a operace správy replikací.
- Time.nist.gov a time.windows.com. Používá se ke kontrole synchronizace mezi systémovým a globálním časem.

Adresy URL pro cloud Azure Government:

- *.ugv.hypervrecoverymanager.windowsazure.us
- *.ugv.backup.windowsazure.us
- *.ugi.hypervrecoverymanager.windowsazure.us
- *.ugi.backup.windowsazure.us

Všechna pravidla brány firewall založená na adresu IP by měl umožňují komunikaci s [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a porty 443 (HTTPS) a 9443 (replikaci dat). Ujistěte se, zda povolit rozsahy IP adres pro oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).

### <a name="download-the-site-recovery-unified-setup"></a>Stažení webu obnovení sjednocený instalační program

1. Otevřete [portál Azure](https://portal.azure.com) a klikněte na **všechny prostředky**.
2. Klikněte na trezoru služby zotavení s názvem **ContosoVMVault**.
3. Klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
4. Vyberte **místní** pro kde vaše počítače nacházejí, **do Azure** pro které chcete replikovat počítače, a **Ano, s hypervisoru VMware vSphere**. Potom klikněte na **OK**.
5. V podokně připravit zdroj, klikněte na tlačítko **+ konfigurační server**.
6. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
7. Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.
8. Stáhnout registrační klíč trezoru Tuto funkci potřebujete po spuštění Unified instalace. Klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/tutorial-vmware-to-azure/source-settings.png)

### <a name="set-up-the-configuration-server"></a>Nastavení konfigurace serveru

1. Spusťte instalační soubor sjednocené instalace.
2. V **než začnete**, vyberte **nainstalujte konfigurační server a procesový server** klikněte **Další**.

3. V **licence k softwaru třetích stran**, klikněte na tlačítko **souhlasím** a stáhnout a nainstalovat MySQL, potom klikněte na **Další**.

4. Na stránce **Registrace** vyberte registrační klíč, který jste si stáhli z trezoru.

5. Na stránce **Nastavení internetu** určete, jak se zprostředkovatel, který běží na konfiguračním serveru, připojí k Azure Site Recovery přes internet.

   - Pokud se chcete připojit s proxy serverem, který je aktuálně nastavený na počítač, vyberte **připojit k Azure Site Recovery pomocí proxy serveru**.
   - Pokud chcete, aby zprostředkovatel připojil přímo, vyberte **připojit se přímo k Azure Site Recovery bez serveru proxy**.
   - Pokud stávající proxy server vyžaduje ověřování, nebo pokud chcete používat vlastní proxy server pro připojení poskytovatele, vyberte **Connect s vlastním nastavením proxy**a zadejte adresu, port a přihlašovací údaje.

   ![Brána firewall](./media/tutorial-vmware-to-azure/combined-wiz4.png)

6. Na stránce **Kontrola předpokladů** instalační program provede kontrolu a ověří, že lze spustit instalaci. Pokud se zobrazí varování u položky **Kontrola synchronizace globálního času**, ověřte, že čas na systémových hodinách (nastavení **Datum a čas**) je stejný jako časové pásmo.

   ![Požadavky](./media/tutorial-vmware-to-azure/combined-wiz5.png)

7. Na stránce **Konfigurace MySQL** vytvořte přihlašovací údaje pro přihlašování k nainstalované instanci serveru MySQL.

8. V **prostředí podrobnosti**, vyberte **Ano** k ochraně virtuálních počítačů VMware. Instalační program zkontroluje, zda je nainstalován PowerCLI 6.0.

9. Na stránce **Umístění instalace** vyberte, kam chcete nainstalovat binární soubory a ukládat mezipaměť. Vybraná jednotka musí mít minimálně 5 GB dostupného místa na disku, ale pro mezipaměť doporučujeme jednotku alespoň s 600 GB volného místa.

10. Na stránce **Výběr sítě** zadejte naslouchací proces (síťový adaptér a port SSL), na kterém konfigurační server odesílá a přijímá data replikace. Výchozím portem pro odesílání a příjem přenosů replikace je port 9443, ale toto číslo portu můžete změnit podle potřeb vašeho prostředí. Můžeme také otevřít port 443, který se používá k orchestraci operací replikace. Nepoužívejte port 443 pro odesílání nebo přijímání provoz replikace.

11. Na stránce **Souhrn** zkontrolujte informace a klikněte na **Nainstalovat**. Instalační program nainstaluje konfigurační server a zaregistruje ho služba Azure Site Recovery.

    ![Souhrn](./media/tutorial-vmware-to-azure/combined-wiz10.png)

    Po dokončení instalace se vygeneruje heslo. Budete ho potřebovat k povolení replikace, proto si ho zkopírujte a uložte na bezpečném místě. Server se zobrazí na **nastavení** > **servery** podokně v trezoru.

### <a name="configure-automatic-discovery"></a>Konfigurace automatického zjišťování

Pokud chcete zjistit virtuální počítače, musí připojit k místní servery VMware konfigurační server. Pro účely tohoto kurzu přidejte vCenter server nebo hostitelů vSphere, pomocí účtu, který má oprávnění správce na serveru.

1. Na konfiguračním serveru, spusťte **CSPSConfigtool.exe**. Je k dispozici jako zástupce na ploše a umístěný ve složce *umístění instalace*\home\svsystems\bin.

2. Klikněte na **Správa účtu** > **Přidat účet**.

   ![Přidat účet](./media/tutorial-vmware-to-azure/credentials1.png)

3. V části **Podrobnosti účtu** přidejte účet, který se použije pro automatické zjišťování.

   ![Podrobnosti](./media/tutorial-vmware-to-azure/credentials2.png)

Přidání serveru:

1. Otevřete [portál Azure](https://portal.azure.com) a klikněte na **všechny prostředky**.
2. Klikněte na trezoru služby zotavení s názvem **ContosoVMVault**.
3. Klikněte na tlačítko **lokality obnovení** > **Příprava infrastruktury** > **zdroje**
4. Vyberte **+ vCenter** pro připojení k systému vCenter server nebo vSphere hostitele ESXi.
5. V **přidat vCenter**, zadejte popisný název serveru. Zadejte IP adresu nebo plně kvalifikovaný název domény.
6. Ponechte port nastaven na hodnotu 443, pokud vaše servery VMware naslouchat požadavkům na jiném portu.
7. Vyberte účet, který chcete použít pro připojení k serveru. Klikněte na **OK**.

Site Recovery se připojuje k VMware servery pomocí zadané nastavení a vyhledá virtuální počítače.

> [!NOTE]
> Může trvat 15 minut nebo déle, než se název účtu objeví na portálu. Chcete-li provést aktualizaci okamžitě, klikněte na **Konfigurační servery** > ***název serveru*** > **Aktualizovat server**.

## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte a ověřte cílové prostředky.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![cíl](./media/tutorial-vmware-to-azure/storage-network.png)

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

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Spuštění postupu zotavení po havárii](site-recovery-test-failover-to-azure.md)
