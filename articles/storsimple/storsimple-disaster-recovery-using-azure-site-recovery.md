---
title: "Automatizovat StorSimple sdílená_složka zotavení po Havárii s Azure Site Recovery | Microsoft Docs"
description: "Popisuje kroky a osvědčené postupy pro vytváření řešení zotavení po havárii pro sdílené složky hostované na úložiště Microsoft Azure StorSimple."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 23049a2c-055e-4d0e-b8f5-af2a87ecf53f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/09/2017
ms.author: vidarmsft
ms.openlocfilehash: b4d575587eec1bcf43c33c7faeb8360ec67b5214
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatizované zotavení po havárii řešení pomocí Azure Site Recovery pro sdílené složky hostované na StorSimple
## <a name="overview"></a>Přehled
Microsoft Azure StorSimple je řešení hybridní cloudové úložiště, které řeší složitosti nestrukturovaných dat běžně spojovaných se sdílenými složkami. StorSimple používá úložiště v cloudu jako rozšíření v případě místních řešení a automaticky úrovně dat mezi místní úložiště a úložiště v cloudu. Integrovat ochranu dat, místní a cloudových snímků, eliminuje potřebu rozrůstající infrastruktury úložiště.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) je služba na základě Azure, která poskytuje možnosti zotavení po havárii tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů. Azure Site Recovery podporuje různé technologie replikace konzistentně replikovat, ochranu a selhání plynule přepne virtuální počítače a aplikací do soukromého a veřejného nebo hostované cloudy.

Pomocí Azure Site Recovery, replikaci virtuálního počítače a možností snímku StorSimple cloudu, můžete chránit prostředí serveru celý soubor. V případě přerušení můžete jedním kliknutím a převeďte online sdílené složky v Azure za několik minut.

Tento dokument podrobně vysvětluje, jak můžete vytvořit řešení zotavení po havárii pro vaše sdílené složky hostované na StorSimple úložiště a provést plánované, neplánované a testovací převzetí služeb při selhání pomocí plán obnovení jedním kliknutím. V podstatě ukazuje, jak je možné upravit plán obnovení v trezoru Azure Site Recovery pro povolení StorSimple převzetí služeb při selhání při havárii scénáře. Kromě toho popisuje podporované konfigurace a požadavky. Tento dokument předpokládá, že jste obeznámení se základy Azure Site Recovery a StorSimple architektury.

## <a name="supported-azure-site-recovery-deployment-options"></a>Podporované možnosti nasazení Azure Site Recovery
Zákazníci může nasazovat souborové servery, jako fyzických serverech nebo virtuálních počítačů (VM) systémem Hyper-V nebo VMware a poté vytvořit sdílené složky ze svazků carved mimo úložišti StorSimple. Nasazení fyzických i virtuálních buď do sekundární lokality nebo do Azure můžete chránit pomocí Azure Site Recovery. Tento dokument popisuje podrobnosti řešení zotavení po Havárii s Azure jako lokalitě pro obnovení pro souborový server, který virtuální počítač hostovaný na Hyper-V a sdílené složky na úložišti StorSimple. Podobně se dá implementovat scénáře, ve kterých je souborový server virtuálního počítače na virtuální počítače VMware nebo fyzický počítač.

## <a name="prerequisites"></a>Požadavky
Implementace řešení obnovení po havárii jedním kliknutím, které využívá Azure Site Recovery pro sdílené složky hostované na úložišti StorSimple má následující požadavky:

* Místní server Windows Server 2012 R2 souboru, který virtuální počítač hostovaný na Hyper-V nebo VMware nebo fyzický počítač
* StorSimple úložiště místním zařízení zaregistrované pomocí nástroje Azure StorSimple manager
* Zařízení StorSimple cloudu vytvořit na portálu Azure StorSimple manager (to je možné mít v vypnutí dolů stavu)
* Sdílené složky hostované na svazcích, které jsou nakonfigurované v zařízení StorSimple
* [Trezor služeb Azure Site Recovery](../site-recovery/site-recovery-vmm-to-vmm.md) vytvořené v odběru Microsoft Azure

Kromě toho, pokud je váš web obnovení Azure, spusťte [nástroj hodnocení připravenosti virtuálního počítače Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) na virtuálních počítačích zajistit, aby byly kompatibilní s virtuálními počítači Azure a Azure Site Recovery services.

Aby se zabránilo latence problémy (které může mít za následek vyšší poplatky za), ujistěte se, vytvoříte vaše zařízení StorSimple cloudu, účet automation a úložiště účtů v ve stejné oblasti.

## <a name="enable-dr-for-storsimple-file-shares"></a>Povolit zotavení po Havárii pro StorSimple sdílené složky
Jednotlivé komponenty místního prostředí je potřeba chránit umožnit dokončení replikace a obnovení. Tato část popisuje, jak:

* Nastavení replikace služby Active Directory a DNS (volitelné)
* Povolení ochrany souborového serveru virtuálního počítače pomocí Azure Site Recovery
* Povolit ochranu svazky zařízení StorSimple
* Konfigurace sítě

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Nastavení replikace služby Active Directory a DNS (volitelné)
Pokud chcete chránit počítače využívající služby Active Directory a DNS, aby byly k dispozici na webu zotavení po Havárii, budete muset explicitně je chránit (tak, aby souborových serverů jsou dostupné po převzetí služeb při selhání s ověřováním). Existují dvě doporučené možnosti podle složitosti zákazníka v místním prostředí.

#### <a name="option-1"></a>možnost 1
Pokud zákazník má malý počet aplikací, jeden řadič domény pro celou místní lokality a bude selhání celý web, pak doporučujeme používat replikace Azure Site Recovery replikovat počítače řadiče domény na sekundární lokality (Toto je použít pro site-to-site a site Azure).

#### <a name="option-2"></a>Možnost 2
Pokud zákazník má velký počet aplikací, běží doménové struktury služby Active Directory a současně se selhání několik aplikací, pak doporučujeme nastavení dalšího řadiče domény v lokalitě zotavení po Havárii (sekundární lokality nebo v Azure).

Naleznete [řešení automatizované zotavení po Havárii pro Active Directory a DNS pomocí Azure Site Recovery](../site-recovery/site-recovery-active-directory.md) pokyny při provádění řadiče domény k dispozici na webu zotavení po Havárii. Pro zbývající část tohoto dokumentu budeme předpokládat, že řadič domény je k dispozici na webu zotavení po Havárii.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Povolení ochrany souborového serveru virtuálního počítače pomocí Azure Site Recovery
Tento krok vyžaduje, abyste připravili prostředí serveru na místní soubor, vytvořit a příprava trezoru služby Azure Site Recovery a povolte ochranu souborů virtuálního počítače.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Příprava prostředí serveru soubor na místě
1. Nastavte **řízení uživatelských účtů** k **Nikdy neupozorňovat**. To je potřeba, aby skripty pro automatizaci Azure můžete použít pro připojení cíle iSCSI po selhání převezme Azure Site Recovery.

   1. Stiskněte klávesu Windows + Q a vyhledejte **nástroje Řízení uživatelských účtů**.
   2. Vyberte **řízení uživatelských účtů změnu nastavení**.
   3. Přetáhněte na panelu dolů směrem k **Nikdy neupozorňovat**.
   4. Klikněte na tlačítko **OK** a pak vyberte **Ano** po zobrazení výzvy.

      ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png)
2. Nainstalujte agenta virtuálního počítače na všechny virtuální počítače souborového serveru. To je potřeba, aby skripty pro automatizaci Azure můžete spustit na neúspěšný přes virtuální počítače.

   1. [Stáhněte si agenta](http://aka.ms/vmagentwin) k `C:\\Users\\<username>\\Downloads`.
   2. Otevřete prostředí Windows PowerShell v režimu správce (Spustit jako správce) a potom zadejte následující příkaz a přejděte k umístění stahování:

      `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

      > [!NOTE]
      > Název souboru může změnit v závislosti na verzi.
      >
      >
3. Klikněte na **Další**.
4. Přijměte **podmínky smlouvy** a pak klikněte na **Další**.
5. Klikněte na **Dokončit**.
6. Vytvoření sdílené složky pomocí svazky carved mimo úložišti StorSimple. Další informace najdete v tématu [pomocí služby StorSimple Manager můžete spravovat svazky](storsimple-manage-volumes.md).

   1. V místní virtuální počítače, stiskněte klávesu Windows + Q a vyhledejte **iSCSI**.
   2. Vyberte **iniciátor iSCSI**.
   3. Vyberte **konfigurace** kartě a zkopírujte název iniciátoru.
   4. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
   5. Vyberte **StorSimple** a pak vyberte službu StorSimple Manager, který obsahuje fyzického zařízení.
   6. Vytvořte svazek kontejnery a pak vytvořte svazky. (Tyto svazky jsou pro sdílených složek souborů na souborovém serveru virtuálních počítačů). Zkopírujte název iniciátoru a poskytněte vhodný název pro záznamy řízení přístupu, při vytváření svazků.
   7. Vyberte **konfigurace** kartě a Poznámka dolů na IP adresu zařízení.
   8. Na místní virtuální počítače, přejděte na **iniciátor iSCSI** znovu a zadejte IP adresu v části rychlý připojit. Klikněte na tlačítko **rychlé připojení** (zařízení by měl být připojili).
   9. Otevřete Azure portálu a vyberte možnost **svazky a zařízení** kartě. Klikněte na tlačítko **automatické konfigurace**. By se zobrazit svazku, který jste právě vytvořili.
   10. Na portálu, vyberte **zařízení** a pak vyberte **vytvoření nového virtuálního zařízení.** (Tato virtuální zařízení bude použit, pokud dojde k selhání). Toto nové virtuální zařízení je možné mít ve stavu offline, aby se zabránilo poplatkům. Uvedení virtuálního zařízení do režimu offline, přejděte na **virtuální počítače** části na portálu a ho vypnout.
   11. Přejděte zpět na virtuálních počítačích na místě a spusťte správu disků (stiskněte klávesu Windows + X a vyberte **Správa disků**).
   12. Si všimnete některé další disky (v závislosti na počtu svazků, které jste vytvořili). Klikněte pravým tlačítkem na první z nich, vyberte **inicializovat Disk**a vyberte **OK**. Klikněte pravým tlačítkem myši **Unallocated** vyberte **nový jednoduchý svazek**, ho přiřadit písmeno jednotky a průvodce dokončete.
   13. Opakujte krok l pro všechny disky. Nyní uvidíte všechny disky na **tento počítač** v Průzkumníku Windows.
   14. Pomocí role Souborová služba a služba úložiště pro vytvoření sdílených složek na těchto svazků.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>K vytvoření a přípravě trezoru Azure Site Recovery
Odkazovat [dokumentace Azure Site Recovery](../site-recovery/site-recovery-hyper-v-site-to-azure.md) začít pracovat s Azure Site Recovery, než začnete chránit souborový server virtuálního počítače.

#### <a name="to-enable-protection"></a>K povolení ochrany
1. Odpojte cíle iSCSI z místní virtuální počítače, které chcete chránit pomocí Azure Site Recovery:

   1. Stiskněte klávesu Windows + Q a vyhledejte **iSCSI**.
   2. Vyberte **nastavit iniciátor iSCSI**.
   3. Odpojte zařízení StorSimple, který jste dříve připojen. Alternativně můžete vypnout souborový server pro několik minut při povolování ochrany.

   > [!NOTE]
   > To způsobí, že sdílené složky je dočasně nedostupná.
   >
   >
2. [Povolení ochrany virtuálního počítače](../site-recovery/site-recovery-hyper-v-site-to-azure.md) souborového serveru virtuálního počítače z portálu Azure Site Recovery.
3. Po zahájení počáteční synchronizace se můžete připojit cíl znovu. Přejděte k iniciátoru iSCSI, vyberte zařízení StorSimple a klikněte na tlačítko **Connect**.
4. Po dokončení synchronizace a stav virtuálního počítače je **chráněné**vyberte virtuální počítač, vyberte **konfigurace** kartě a odpovídajícím způsobem aktualizace sítě virtuálního počítače (jedná se o síť, neúspěšný přes Virtuální počítače budou součástí). Pokud síť není zobrazena, znamená to, že synchronizace stále probíhají.

### <a name="enable-protection-of-storsimple-volumes"></a>Povolit ochranu svazky zařízení StorSimple
Pokud nevyberete **povolit výchozí zálohování tohoto svazku** možnost pro svazky zařízení StorSimple, přejděte na **zásady zálohování** ve Správci zařízení StorSimple služby a vytvořit vhodné zásady zálohování pro všechny svazky. Doporučujeme nastavit frekvenci záloh na cíl bodu obnovení (RPO), které chcete zobrazit pro aplikaci.

### <a name="configure-the-network"></a>Konfigurace sítě
Pro souborový server virtuálního počítače Konfigurace nastavení sítě v Azure Site Recovery, tak, aby všechny sítě virtuálních počítačů připojených k správné síťové zotavení po Havárii po převzetí služeb při selhání.

Můžete vybrat virtuální počítač v **replikované položky** kartu a nakonfigurujte nastavení sítě, jak je znázorněno na následujícím obrázku.

![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení
Můžete vytvořit plán obnovení v nástroji automatické obnovení systému pro automatizaci procesu převzetí služeb při selhání ze sdílených složek. Pokud dojde k narušení služeb, můžete zahrnout sdílené složky za několik minut, s právě jedním kliknutím. Chcete-li tato automatizace, budete potřebovat účet Azure automation.

#### <a name="to-create-an-automation-account"></a>Chcete-li vytvořit účet Automation.
1. Přejděte na portál Azure &gt; **automatizace** části.
2. Klikněte na tlačítko **+ přidat** tlačítko, otevře se okno pod okno.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)

   * Název – zadejte nový účet automation.
   * Předplatné – zvolte předplatné
   * Prostředek skupiny – vytvořit novou nebo vyberte existující skupinu prostředků
   * Umístění – vyberte umístění, uložte jej v stejnou geograficky nebo oblast, ve kterém byly vytvořeny cloudu zařízení StorSimple a účty úložiště.
   * Vytvoření účtu spustit v Azure jako - vyberte **Ano** možnost.

3. Přejděte na účet Automation, klikněte na tlačítko **Runbooky** &gt; **procházet galerii** importovat všechny požadované sady runbook do účtu automation.
4. Přidejte následující sady runbook tak, že najdete **zotavení po havárii** značky v galerii:

   * Vyčištění svazky zařízení StorSimple po převzetí služeb při selhání testu (TFO)
   * Kontejnery svazků zařízení StorSimple převzetí služeb při selhání
   * Po převzetí služeb při selhání připojte svazky na zařízení StorSimple
   * Odinstalovat rozšíření vlastních skriptů ve virtuálním počítači Azure
   * Spustit virtuální zařízení StorSimple

     ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)

5. Publikovat všechny skripty výběrem sady runbook v účtu automation a klikněte na tlačítko **upravit** &gt; **publikovat** a potom **Ano** zobrazení zprávy pro ověření. Po provedení tohoto kroku **Runbooky** se zobrazí karta následujícím způsobem:

    ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)

6. V rámci účtu automation, vyberte **prostředky** kartě &gt; klikněte na tlačítko **proměnné** &gt; **přidat proměnnou** a přidejte následující proměnné. Můžete se k šifrování tyto prostředky. Tyto proměnné jsou specifické pro plán obnovení. Pokud vaše obnovení plánu (které vytvoříte v dalším kroku) TestPlan je název a potom proměnných by měl být TestPlan StorSimRegKey, TestPlan AzureSubscriptionName a tak dále.

   * *RecoveryPlanName***- StorSimRegKey**: registrační klíč pro službu StorSimple Manager.
   * *RecoveryPlanName***- AzureSubscriptionName**: Název předplatného Azure.
   * *RecoveryPlanName***- ResourceName**: název StorSimple prostředku, který má zařízení StorSimple.
   * *RecoveryPlanName***- DeviceName**: zařízení, která má k převzetí služeb při selhání.
   * *RecoveryPlanName***- VolumeContainers**: řetězec kontejnery svazků oddělených čárkami k dispozici na zařízení, které potřebují k více než, například při selhání volcon1, volcon2, volcon3.
   * *RecoveryPlanName***- TargetDeviceName**: zařízení StorSimple cloudu, na kterém jsou kontejnery pro převzetí služeb při selhání.
   * *RecoveryPlanName***- TargetDeviceDnsName**: název služby cílové zařízení (lze najít v **virtuální počítač** části: název služby je stejný jako název DNS).
   * *RecoveryPlanName***- StorageAccountName**: název účtu úložiště, ve kterém se uloží skript (což je spouštět na neúspěšný přes virtuálních počítačů). To může být jakékoli úložiště účet, který má nějaké místo pro uložení skript dočasně.
   * *RecoveryPlanName***- StorageAccountKey**: přístupový klíč pro výše uvedený účet úložiště.
   * *RecoveryPlanName***- ScriptContainer**: název kontejneru, ve kterém se uloží skript v cloudu. Pokud kontejner neexistuje, bude vytvořen.
   * *RecoveryPlanName***- VMGUIDS**: při ochraně virtuálního počítače, Azure Site Recovery přiřadí každé virtuálních počítačů jedinečný kód, který poskytuje podrobnosti o chybných nad virtuálních počítačů. Chcete-li získat VMGUID, vyberte **služeb zotavení** a klikněte na **chráněné položky** &gt; **skupin ochrany** &gt;  **Počítače** &gt; **vlastnosti**. Pokud máte víc virtuálních počítačů, přidejte identifikátory GUID jako řetězec oddělených čárkami.
   * *RecoveryPlanName***- AutomationAccountName** – název účtu automation, ve které jste přidali runbooky a prostředky.

  Například, pokud název plánu obnovení je fileServerpredayRP pak vaše **pověření** & **proměnné** karet se zobrazí následující po přidání všechny prostředky.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

7. Přejděte na **služeb zotavení** a vyberte trezor Azure Site Recovery, který jste vytvořili dříve.
8. Vyberte **plány obnovení (služba Site Recovery)** možnost z **spravovat** skupiny a vytvoření nového plánu obnovení následujícím způsobem:

   a.  Klikněte na tlačítko **+ plán obnovení** tlačítko, otevře se okno pod okno.

      ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)

   b.  Zadejte název plánu obnovení, vyberte zdroje, cíl a nasazení modelu hodnoty.

   c.  Vyberte virtuální počítače ze skupiny ochrany, kterou chcete zahrnout do plánu obnovení a klikněte na tlačítko **OK** tlačítko.

   d.  Vyberte plán obnovení, který jste předtím vytvořili, klikněte na **přizpůsobit** tlačítko otevřete zobrazení pro vlastní plán obnovení.

   e.  Klikněte pravým tlačítkem na **všechny skupiny vypnutí** a klikněte na tlačítko **akce bez přidání**.

   f.  Otevře okno akce vložit, zadejte název, vyberte **primární straně** v kde možnost spustit, vyberte účet Automation (ve kterém jste přidali sady runbook) a pak vyberte možnost **převzetí služeb při selhání StorSimple svazku kontejnery**  sady runbook.

   g.  Klikněte pravým tlačítkem na **1. skupina: spuštění** a klikněte na tlačítko **přidat chráněné položky** možnost, pak vyberte virtuální počítače, které mají být chráněné v plánu obnovení a klikněte na **Ok** tlačítko. Volitelné, pokud je již vybrána virtuálních počítačů.

   h.  Klikněte pravým tlačítkem na **1. skupina: spuštění** a klikněte na tlačítko **Post akce** možnost pak přidejte následující skripty:

   * Runbook Start-StorSimple-virtuální-zařízení
   * Selhání runbook přes StorSimple svazku kontejnery
   * Připojení svazky po převzetí runbook
   * Odinstalujte--rozšíření vlastních skriptů runbook

   i.  Přidání ručně prováděné akce po výše 4 skripty ve stejné **1. skupina: po kroky** části. Tato akce je bod, ve kterém můžete ověřit, zda vše funguje správně. Tato akce je nutné přidat jenom jako součást testovací převzetí služeb při selhání (takže vyberte pouze **testovací převzetí služeb při selhání** políčko).

   j.  Po ruční akci, přidejte **čištění** skriptu pomocí stejného postupu, který jste použili pro jiné sady runbook. **Uložit** plánu obnovení.

    > [!NOTE]
    > Při spouštění testovací převzetí služeb, by všechno, co je v kroku manuální akce ověřit, protože svazky zařízení StorSimple, které měl byla klonovat na cílovém zařízení se odstraní jako součást vymazání po dokončení ruční akce.
    >

    ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Provedení testu převzetí služeb
Odkazovat [Active Directory DR řešení](../site-recovery/site-recovery-active-directory.md) doprovodná Příručka pro důležité informace o konkrétní do služby Active Directory během testu převzetí služeb. Místní instalace nebude vůbec narušen, když dojde k testu převzetí služeb. Svazky zařízení StorSimple, které byly připojené k virtuálnímu počítači místní jsou klonovat do cloudu zařízení StorSimple v Azure. Virtuální počítač pro účely testování je zapínají v Azure a klonovaný svazky jsou připojené k virtuálnímu počítači.

#### <a name="to-perform-the-test-failover"></a>K provedení testu převzetí služeb
1. Na portálu Azure vyberte váš trezor site recovery.
2. Klikněte na tlačítko pro souborový server virtuálního počítače vytvořit plán obnovení.
3. Klikněte na tlačítko **testovací převzetí služeb při selhání**.
4. Vyberte virtuální síť Azure, ke které se virtuální počítače Azure připojí po převzetí služeb při selhání.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
5. Kliknutím na **OK** zahajte převzetí služeb při selhání. Průběh můžete sledovat kliknutím na virtuálním počítači otevřete jeho vlastnosti, nebo na **testovací převzetí služeb při selhání úlohy** v název trezoru &gt; **úlohy** &gt; **úlohy Site Recovery**.
6. Po dokončení převzetí byste také měli vidět Azure repliky na počítač se zobrazovat na portálu Azure &gt; **virtuální počítače**. Vaše ověření můžete provést.
7. Jakmile ověřovací hotovi, klikněte na tlačítko **dokončení ověření**. To bude čištění svazky zařízení StorSimple a vypnutí cloudu zařízení StorSimple.
8. Jakmile jste hotovi, klikněte na možnost **vyčistit testovací převzetí služeb při selhání** v plánu obnovení. V poznámkách k zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb. Tato akce odstraní virtuální počítač, který byly vytvořeny během testovacího převzetí služeb při selhání.

## <a name="perform-a-planned-failover"></a>Proveďte plánované převzetí služeb při selhání
   Při plánovaném převzetí služeb při souboru na místní server, který je korektně vypnout virtuální počítač a cloudem pořízení snímku zálohy svazků v zařízení StorSimple. Svazky zařízení StorSimple se převzetí služeb při selhání pro virtuální zařízení, je zapínají repliku virtuálního počítače v Azure a svazky, které jsou připojené k virtuálnímu počítači.

#### <a name="to-perform-a-planned-failover"></a>K provedení plánovaného převzetí služeb při selhání
1. Na portálu Azure vyberte **služeb zotavení** trezoru &gt; **plány obnovení (služba Site recovery)** &gt; **recoveryplan_name** vytvořené pro Souborový server virtuálního počítače.
2. V okně plán obnovení, klikněte na **Další** &gt; **plánované převzetí služeb při selhání**.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
3. Na **potvrďte plánované převzetí služeb při selhání** okně zvolte zdrojové a cílové umístění a vyberte Cílová síť a klikněte na ikonu zaškrtnutí ✓ zahájíte proces převzetí služeb při selhání.
4. Po vytvoření repliky virtuálních počítačů jsou ve stavu čekání na potvrzení. Klikněte na tlačítko **potvrzení** potvrzení převzetí služeb při selhání.
5. Po dokončení replikace virtuální počítače se spustí do sekundárního umístění.

## <a name="perform-a-failover"></a>Provedení převzetí služeb při selhání
Během neplánované převzetí služeb při selhání svazky zařízení StorSimple se převzetí služeb při selhání k virtuálnímu zařízení, bude se zapínají repliku virtuálního počítače na platformě Azure a svazky jsou připojené k virtuálnímu počítači.

#### <a name="to-perform-a-failover"></a>K provedení převzetí služeb při selhání
1. Na portálu Azure vyberte **služeb zotavení** trezoru &gt; **plány obnovení (služba Site recovery)** &gt; **recoveryplan_name** vytvořené pro Souborový server virtuálního počítače.
2. V okně plán obnovení, klikněte na **Další** &gt; **převzetí služeb při selhání**.
3. Na **potvrzení převzetí služeb při selhání** okně zvolte zdrojové a cílové umístění.
4. Vyberte **vypnout virtuální počítače a synchronizovat nejnovější data** k určení, že Site Recovery opakovat vypnutí chráněného virtuálního počítače a data synchronizovat tak, aby nejnovější verzi dat převezme služby při selhání.
5. Po převzetí služeb při selhání virtuální počítače jsou ve stavu čekání na potvrzení. Klikněte na tlačítko **potvrzení** potvrzení převzetí služeb při selhání.


## <a name="perform-a-failback"></a>Provést navrácení služeb po obnovení
Během navrácení služeb po obnovení kontejnery svazků zařízení StorSimple se po selhání zpět do fyzického zařízení po zálohování se provede.

#### <a name="to-perform-a-failback"></a>K provedení navrácení služeb po obnovení
1. Na portálu Azure vyberte **služeb zotavení** trezoru &gt; **plány obnovení (služba Site Recovery)** &gt; **recoveryplan_name** vytvořené pro Souborový server virtuálního počítače.
2. V okně plán obnovení, klikněte na **Další** &gt; **plánované převzetí služeb při selhání**.
3. Vyberte zdrojové a cílové umístění, vyberte odpovídající synchronizace dat a možností vytvoření virtuálního počítače.
4. Klikněte na tlačítko **OK** tlačítko Spustit proces navrácení služeb po obnovení.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>Osvědčené postupy
### <a name="capacity-planning-and-readiness-assessment"></a>Kapacity assessment plánování a připravenosti
#### <a name="hyper-v-site"></a>Web Hyper-V
Použití [nástroje Plánovač kapacity uživatele](http://www.microsoft.com/download/details.aspx?id=39057) návrhu serveru, úložiště a síťové infrastruktury pro vaše prostředí repliky technologie Hyper-V.

#### <a name="azure"></a>Azure
Můžete spustit [nástroj hodnocení připravenosti virtuálního počítače Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) na virtuálních počítačích zajistit, aby byly kompatibilní s virtuálními počítači Azure a služeb Azure Site Recovery. Nástroj hodnocení připravenosti zkontroluje konfigurací virtuálních počítačů a upozorní, když konfigurace nejsou kompatibilní s Azure. Například vydá varování, pokud je jednotka C: větší než 127 GB.

Plánování kapacity se skládá z nejméně dva důležité procesy:

* Mapování místní virtuální počítače Hyper-V na velikosti virtuálního počítače Azure (například A6, A7, A8 a A9).
* Určení požadované šířky pásma Internetu.

## <a name="limitations"></a>Omezení
* V současné době mohou být přebrány pouze 1 zařízení StorSimple (na jednom zařízení StorSimple cloudu). Tento scénář souborový server, který zahrnuje několik zařízení StorSimple se ještě nepodporuje.
* Pokud dojde k chybě při povolování ochrany pro virtuální počítač, ujistěte se, že jste odpojeni cíle iSCSI.
* Všechny kontejnery svazků, které byly seskupeny dohromady z důvodu zásady zálohování pokrývání uzlů napříč kontejnery svazků převezme služby při selhání společně.
* Všechny svazky v kontejnery svazků, kterou jste vybrali převezme služby při selhání.
* Svazky, které přidat až více než 64 TB nelze po selhání, protože maximální kapacita o jeden Cloud zařízení StorSimple je 64 TB.
* Pokud selže plánované/neplánované převzetí služeb při selhání a virtuální počítače jsou vytvořeny v Azure, pak neprovádí vyčištění virtuálních počítačů. Místo toho proveďte navrácení služeb po obnovení. Pokud odstraníte virtuální počítače pak místní virtuální počítače nelze zapnout znovu.
* Po selhání Pokud si nejste moci zobrazit svazky, přejděte na virtuální počítače, spusťte správu disků, zkontrolujte znovu disky a potom je uvést online.
* V některých případech může být jiná než písmena místní písmena jednotek v lokalitě zotavení po Havárii. Pokud k tomu dojde, musíte ručně odstranit problém po dokončení převzetí služeb při selhání.
* Pro Azure pověření, která je zadána v účtu služby automation jako prostředek je třeba zakázat službu Multi-Factor authentication. Pokud toto ověřování není zakázán, skripty nebudou povolena, aby se spouštěly automaticky a plánu obnovení se nezdaří.
* Časový limit úlohy převzetí služeb při selhání: StorSimple skript bude vypršení časového limitu, pokud převzetí služeb při selhání kontejnery svazků trvá déle než limit Azure Site Recovery za skriptu (aktuálně 120 minut).
* Časový limit úlohy zálohování: StorSimple skriptu časového limitu, pokud zálohování svazků trvá déle než limit Azure Site Recovery za skriptu (aktuálně 120 minut).

  > [!IMPORTANT]
  > Zálohování spustit ručně z portálu Azure a poté znovu spusťte plánu obnovení.

* Časový limit úlohy klonovat: StorSimple skriptu časového limitu, pokud klonování svazků trvá déle než limit Azure Site Recovery za skriptu (aktuálně 120 minut).
* Čas synchronizace Chyba: StorSimple skriptů chyby se informacemi o tom, že zálohy byly úspěšné i v případě, že se záloha nachází úspěšné na portálu. Možnou příčinou to může být, že zařízení StorSimple čas může být mimo rozsah synchronizace s aktuální čas v časovém pásmu.

  > [!IMPORTANT]
  > Synchronizace času zařízení s aktuální čas v časovém pásmu.

* Zařízení převzetí služeb při selhání Chyba: skript StorSimple může selhat, pokud dojde k selhání zařízení po spuštění plánu obnovení.

  > [!IMPORTANT]
  > Po dokončení převzetí služeb při selhání zařízení, spusťte znovu plánu obnovení.


## <a name="summary"></a>Souhrn
Pomocí Azure Site Recovery, můžete vytvořit plán obnovení po havárii dokončení automatizované pro souborový server virtuálního počítače s sdílené složky hostované na úložišti StorSimple. Můžete spustit převzetí služeb při selhání během několika sekund z libovolného místa v případě přerušení a získání aplikace fungovaly za pár minut.
