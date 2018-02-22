---
title: "Nastavení zotavení po havárii do Azure pro místní virtuální počítače VMware pomocí Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak nastavit zotavení po havárii do Azure pro místní virtuální počítače VMware pomocí služby Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 3d9248d2501c7fea0492bad2687b6bdfb0b903e8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Nastavení zotavení po havárii do Azure pro místní virtuální počítače VMware

V tomto kurzu se dozvíte, jak nastavit zotavení po havárii do Azure pro místní virtuální počítače VMware se systémem Windows. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nastavit zdroj a cíl replikace.
> * Nastavit zdrojové prostředí replikace, včetně místních komponent řešení Site Recovery, a cílové prostředí replikace.
> * Vytvoření zásady replikace
> * Povolení replikace virtuálního počítače

Toto je třetí kurz ze specializované série. Tento kurz předpokládá, že jste už dokončili úlohy z předchozích kurzů:

1. [Příprava Azure](tutorial-prepare-azure.md)
2. [Příprava VMware v místním prostředí](tutorial-prepare-on-premises-vmware.md)

Než začnete, doporučujeme [zkontrolovat architekturu](concepts-vmware-to-azure-architecture.md) určenou pro scénář zotavení po havárii.


## <a name="select-a-replication-goal"></a>Výběr cíle replikace

1. V **trezorech služby Recovery Services** klikněte na název trezoru **ContosoVMVault**.
2. V části **Začínáme** klikněte na Site Recovery. Pak klikněte na možnost **Připravit infrastrukturu**.
3. V části **Cíl ochrany** > **Kde jsou vaše počítače umístěné?** vyberte **Místní**.
4. V části **Kam chcete počítače replikovat?** vyberte **Do Azure**.
5. V části **Máte počítače ve virtuální podobě?** vyberte **Ano, s VMware vSphere Hypervisor**. Pak klikněte na **OK**.

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Pro nastavení zdrojového prostředí potřebujete jeden vysoce dostupný místní počítač, který bude hostovat místní komponenty řešení Site Recovery. Mezi tyto komponenty patří konfigurační server, procesní server a hlavní cílový server.

- Konfigurační server koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci dat.
- Procesní server funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure. Procesní server nainstaluje na všechny virtuální počítače, které chcete replikovat, službu mobility a automaticky vyhledá místní virtuální počítače VMware.
- Hlavní cílový server zpracovává replikační data během navrácení služeb z Azure po obnovení.

Když chcete nastavit konfigurační server jako vysoce dostupný virtuální počítač VMware, stáhněte připravenou šablonu OVF a importujte ji do VMware. Tím vytvoříte požadovaný virtuální počítač. Jakmile nastavíte konfigurační server, zaregistrujte ho do vybraného trezoru. Po registraci služba Site Recovery zjistí místní virtuální počítače VMware.

### <a name="download-the-vm-template"></a>Stažení šablony virtuálního počítače

1. V daném trezoru klikněte na **Připravit infrastrukturu** > **Zdroj**.
2. Na stránce **Připravit zdroj** klikněte na **+ Konfigurační server**.
3. V části **Přidat server** zkontrolujte, jestli se v části **Typ serveru** zobrazí **Konfigurační server pro VMware**.
4. Stáhněte šablonu OVF pro konfigurační server.

  > [!TIP]
  Nejnovější verzi šablony pro konfigurační server můžete stáhnout přímo ze služby [Stažení softwaru](https://aka.ms/asrconfigurationserver).

## <a name="import-the-template-in-vmware"></a>Import šablony do VMware

1. Pomocí klientské konzole VMware vSphere Client se přihlaste k VMware vCenter Serveru nebo k hostiteli vSphere ESXi.
2. V nabídce **File** (Soubor) vyberte **Deploy OVF Template** (Nasadit šablonu OVF). Tím spustíte průvodce nasazením šablony OVF.  

     ![Šablona OVF](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. V části **Select source** (Vybrat zdroj) zadejte umístění staženého souboru OVF.
4. V části **Review details** (Zkontrolovat podrobnosti) klikněte na **Další**.
5. V části **Select name and folder** (Vybrat název a složku) a **Select configuration** (Vybrat konfiguraci) odsouhlaste výchozí nastavení.
6. V nabídce **Select storage** (Vybrat úložiště) vyberte v části **Select virtual disk format** (Vybrat formát virtuálního disku) možnost **Thick Provision Eager Zeroed** (Vytvoření celého disku s vynulováním celého disku), která zajistí nejlepší výkon.
4. Na zbývajících stránkách průvodce přijměte výchozí nastavení.
5. V části **Ready to complete** (Připraveno k dokončení):
  - Pokud chcete vytvořit virtuální počítač s výchozím nastavením, vyberte **Power on after deployment** (Spustit po nasazení) > **Finish** (Dokončit).
  - Pokud chcete přidat další síťové rozhraní, zrušte zaškrtnutí políčka **Power on after deployment** (Spustit po nasazení) a pak vyberte **Finish** (Dokončit). Ve výchozím nastavení se šablona konfiguračního serveru nasadí s jedním síťovým adaptérem, ale po nasazení můžete přidat další síťové adaptéry.

  
## <a name="add-an-additional-adapter"></a>Přidání dalšího adaptéru

Pokud chcete na konfigurační server přidat další síťový adaptér, udělejte to, než zaregistrujete server do trezoru. Jakmile ho zaregistrujete, další adaptéry nebudete moct přidat.

1. V inventáři konzole vSphere Client klikněte pravým tlačítkem na virtuální počítač a vyberte **Edit Settings** (Upravit nastavení).
2. V části **Hardware** klikněte na **Add** (Přidat) > **Ethernet Adapter** (Adaptér sítě Ethernet). Pak klikněte na tlačítko **Další**.
3. Vyberte typ adaptéru a síť. 
4. Vyberte **Connect at power on** (Připojit při spuštění), aby se po spuštění virtuálního počítače připojila virtuální síťová karta. Klikněte na **Next** (Další) > **Finish** (Dokončit) a pak na **OK**.


## <a name="register-the-configuration-server"></a>Registrace konfiguračního serveru 

1. Z klientské konzole VMware vSphere Client zapněte požadovaný virtuální počítač.
2. Virtuální počítač se spustí do instalačního prostředí Windows Serveru 2016. Přijměte licenční smlouvu a zadejte heslo správce.
3. Po dokončení instalace se přihlaste k virtuálnímu počítači jako správce.
4. Při prvním přihlášení se spustí nástroj pro konfiguraci služby Azure Site Recovery.
5. Zadejte název, pod kterým konfigurační server zaregistrujete do služby Site Recovery. Pak klikněte na tlačítko **Další**.
6. Nástroj zkontroluje, jestli se virtuální počítač může připojit k Azure. Po navázání spojení klikněte na **Přihlásit se**, abyste se mohli přihlásit ke svému předplatnému Azure. Přihlašovací údaje musí zajišťovat přístup k trezoru, do kterého chcete konfigurační server zaregistrovat.
7. Nástroj provede několik konfiguračních úloh a pak restartuje počítač.
8. Znovu se přihlaste k počítači. Automaticky se spustí průvodce správou konfiguračního serveru.

### <a name="configure-settings-and-connect-to-vmware"></a>Konfigurace nastavení a připojení k VMware

1. V průvodci správou konfiguračního serveru vyberte možnost **Nastavit připojení** a vyberte síťový adaptér, který bude přijímat provoz replikace. Potom klikněte na **Uložit**. Po dokončení konfigurace není možné toto nastavení změnit.
2. V části **Vyberte trezor služby Recovery Services** vyberte své předplatné Azure a příslušnou skupinu prostředků a trezor.
3. V části **Nainstalovat software třetí strany** přijměte licenční podmínky a kliknutím na **Stáhnout a nainstalovat** nainstalujte MySQL Server.
4. Klikněte na **Nainstalovat VMware PowerCLI**. Než to uděláte, zkontrolujte, jestli jsou zavřená všechna okna prohlížeče. Pak klikněte na **Pokračovat**
5. Než budete pokračovat, v části **Ověřit konfiguraci zařízení** se ověří požadavky.
6. V části **Konfigurovat vCenter Server nebo server vSphere ESXi** zadejte plně kvalifikovaný název domény nebo IP adresu vCenter Serveru nebo hostitele vSphere, na kterém jsou umístěné virtuální počítače, které chcete replikovat. Zadejte port, přes který server naslouchá, a popisný název, který se bude v trezoru používat pro server VMware.
7. Zadejte přihlašovací údaje, které bude konfigurační server používat pro připojení k serveru VMware. Služba Site Recovery je použije k automatickému zjištění virtuálních počítačů VMware, které jsou dostupné pro replikaci. Klikněte na **Přidat** a potom na **Pokračovat**.
8. V části **Nakonfigurovat přihlašovací údaje virtuálního počítače** zadejte uživatelské jméno a heslo, které se po povolení replikace použije pro automatickou instalaci služby mobility na virtuální počítače. Když chcete replikovat počítače s Windows, je nutné, aby na nich měl tento účet oprávnění místního správce. U počítačů s Linuxem zadejte údaje superuživatele.
9. Registraci dokončete kliknutím na **Dokončit konfiguraci**. 
10. Jakmile bude registrace dokončená, zkontrolujte Azure Portal, jestli je ve vybraném trezoru na stránce **Zdroj** uvedený konfigurační server a VMware server. Pak klikněte na tlačítko **OK** a nakonfigurujte nastavení cíle.


Služba Site Recovery se připojí k serverům VMware pomocí zadaného nastavení a vyhledá virtuální počítače.

> [!NOTE]
> Může trvat 15 minut nebo déle, než se název účtu objeví na portálu. Pokud chcete provést aktualizaci okamžitě, klikněte na **Konfigurační servery** > ***název serveru*** > **Aktualizovat server**.

## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte a zkontrolujte cílové prostředky.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Zadejte, jestli váš model cílového nastavení využívá Resource Manager, nebo jde o klasický model.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![Cíl](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Vytvoření zásady replikace

1. Otevřete [Azure Portal](https://portal.azure.com) a klikněte na **Všechny prostředky**.
2. Klikněte na trezor služby Recovery Service s názvem**ContosoVMVault**.
3. Zásady replikace vytvoříte tak, že kliknete na možnost **Infrastruktura Site Recovery** > **Zásady replikace** > **+ Zásady replikace**.
4. V části **Vytvoření zásad replikace** zadejte jako název zásady **VMwareRepPolicy**.
5. V části **Prahová hodnota cíle bodu obnovení (RPO)** použijte výchozí hodnotu 60 minut. Tato hodnota určuje, jak často se tvoří body obnovení. Když průběžná replikace překročí tento limit, vygeneruje se upozornění.
6. V části **Uchování bodu obnovení** použijte výchozí hodnotu 24 hodin a nastavte délku okna uchování jednotlivých bodů obnovení. V tomto kurzu vybereme 72 hodin. Replikované virtuální počítače můžete v rámci okna uchování obnovit do libovolného časového bodu.
7. V části **Frekvence snímků konzistentní vzhledem k aplikacím** použijte jako frekvenci pro vytváření snímků konzistentních vzhledem k aplikacím výchozí hodnotu 60 minut. Kliknutím na tlačítko **OK** vytvořte zásadu.

   ![Zásada](./media/tutorial-vmware-to-azure/replication-policy.png)

Tato zásada se automaticky přidruží ke konfiguračnímu serveru. Ve výchozím nastavení se pro navrácení služeb po obnovení automaticky vytvoří zásada párování. Pokud má zásada replikace název například **rep-policy**, zásada navrácení služeb po obnovení bude označená jako **rep-policy-failback**. Tato zásada se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.

## <a name="enable-replication"></a>Povolení replikace

Jakmile bude replikace virtuálního počítače povolená, služba Site Recovery automaticky nainstaluje službu mobility. Může to trvat 15 minut nebo déle, než se změny projeví a objeví se na portálu.

Replikaci povolte následujícím způsobem:

1. Klikněte na **Replikovat aplikaci** > **Zdroj**.
2. V části **Zdroj** vyberte konfigurační server.
3. V části **Typ počítače** vyberte **Virtuální počítače**.
4. V části **vCenter/vSphere Hypervisor** vyberte vCenter Server, který spravuje hostitele vSphere, nebo vyberte samotného hostitele.
5. Vyberte procesní server (konfigurační server). Potom klikněte na **OK**.
6. V části **Cíl** vyberte předplatné a skupinu prostředků, ve které chcete vytvořit virtuální počítače, jejichž služby se převezmou při selhání. Vyberte model nasazení (klasický, nebo využívající Resource Manager), který chcete v Azure použít pro virtuální počítače, jejichž služby se převezmou při selhání.
7. Vyberte účet úložiště Azure, který chcete použít pro replikaci dat.
8. Vyberte síť Azure a podsíť, ke kterým se připojí virtuální počítače Azure, když se po převzetí služeb při selhání vytvoří.
9. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač.
10. V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.
11. V části **Vlastnosti** > **Konfigurace vlastností** vyberte účet, který použije procesový server při automatické instalaci služby mobility na virtuálním počítači.
12. V části **Nastavení replikace** > **Konfigurace nastavení replikace** zkontrolujte, jestli je vybraná správná zásada replikace.
13. Klikněte na **Povolit replikaci**.

Průběh úlohy **Povolení ochrany** můžete sledovat tady: **Nastavení** > **Úlohy** > **Úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

Pokud chcete monitorovat virtuální počítače, které jste přidali, čas posledního zjištění virtuálních počítačů můžete zkontrolovat v části **Konfigurační servery**
> **Poslední kontakt**. Pokud chcete přidat virtuální počítače bez čekání na naplánované zjišťování, zvýrazněte konfigurační server (neklikejte na něj) a klikněte na **Aktualizovat**.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Spuštění postupu zotavení po havárii](site-recovery-test-failover-to-azure.md)
