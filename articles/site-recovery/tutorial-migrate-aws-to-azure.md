---
title: "Migrovat virtuální počítače z AWS do Azure s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak migrovat virtuální počítače běží v Amazon Web Services (AWS) do Azure, pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 814d8ee4952dd08707849eadc1e4e97ab6087da0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Migrace Amazon Web Services (AWS) virtuálních počítačů do Azure

V tomto kurzu se naučíte, jak migrovat Amazon Web Services (AWS) virtuálních počítačů (VM) k virtuálním počítačům Azure pomocí Site Recovery. Při migraci EC2 instance do Azure, virtuální počítače jsou zpracovány jako v případě, že jsou fyzické, místních počítačů. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Příprava prostředků Azure
> * Instance AWS EC2 Příprava na migraci
> * Nasazení konfigurace serveru
> * Povolení replikace pro virtuální počítače
> * Test převzetí služeb při selhání, abyste měli jistotu, všechno funguje
> * Spuštění jednorázové převzetí služeb při selhání do Azure

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) před tím, než začnete.


## <a name="prepare-azure-resources"></a>Příprava prostředků Azure 

Je potřeba mít připravený v Azure pro migrované instance EC2 používat několik prostředků. Jedná se o účet úložiště, úložiště a virtuální sítě.

### <a name="create-a-storage-account"></a>vytvořit účet úložiště

Bitové kopie replikovaných počítačů jsou uložené v úložišti Azure. Virtuální počítače Azure se vytvářejí z úložiště, když jste převzetí služeb při selhání místním nasazením a Azure.

1. V [portál Azure](https://portal.azure.com) nabídky, klikněte na tlačítko **nový** -> **úložiště** -> **účet úložiště**.
2. Zadejte název účtu úložiště. Tyto kurzy používáme název **awsmigrated2017**. Název musí být jedinečné v rámci Azure a musí být v rozmezí 3 až 24 znaků, jenom čísla a malá písmena.
3. Ponechejte výchozí hodnoty pro **modelu nasazení**, **účet druhu**, **výkonu**, a **zabezpečení přenosu požadované**.
5. Vyberte výchozí **RA-GRS** pro **replikace**.
6. Vyberte odběr, který chcete použít pro tento kurz.
7. Pro **skupiny prostředků**, vyberte **vytvořit nový**. V tomto příkladu používáme **migrationRG** jako název.
8. Vyberte **západní Evropa** jako umístění. 
9. Vytvořte účet úložiště kliknutím na **Vytvořit**.

### <a name="create-a-vault"></a>Vytvoření trezoru

1. V [portál Azure](https://portal.azure.com), v levém navigačním panelu, klikněte na tlačítko **další služby** , vyhledejte a vyberte **trezory služeb zotavení**.
2. Na stránce trezory služeb zotavení klikněte na tlačítko **+ přidat** v levé horní části stránky.
3. Pro **název**, typ *myVault*. 
4. Pro **předplatné**, vyberte odpovídající předplatné.
4. Pro **skupiny prostředků**, vyberte **použít existující** a vyberte *migrationRG*. 
5. V **umístění**, vyberte *západní Evropa*. 
5. Rychle se dostat k nový trezor z řídicího panelu, vyberte **připnout na řídicí panel**.
7. Až budete hotovi, klikněte na tlačítko **vytvořit**.

Nový trezor se zobrazí na **řídicí panel** > **všechny prostředky**a v hlavním **trezory služeb zotavení** stránky.

### <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

Virtuální počítače Azure vytvořené po migraci (převzetí služeb při selhání), budou se připojené k této síti.

1. V [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **sítě** >
    **virtuální síť**
3. Pro **název**, typ *myMigrationNetwork*.
4. Ponechte výchozí hodnotu pro **adresní prostor**.
5. Pro **předplatné**, vyberte odpovídající předplatné.
6. Pro **skupiny prostředků**, vyberte **použít existující** a zvolte *migrationRG* z rozevíracího seznamu.
7. Pro **umístění**, vyberte **západní Evropa**. 
8. Ponechte výchozí nastavení pro **podsíť**, i **název** a **rozsah IP adres**.
9. Nechte **koncové body služby** zakázána.
10. Až budete hotovi, klikněte na tlačítko **vytvořit**.


## <a name="prepare-the-ec2-instances"></a>Příprava EC2 instancí

Budete potřebovat jeden nebo více virtuálních počítačů, které chcete migrovat. Tyto instance EC2 by měl běžet 64bitovou verzi Windows Server 2008 R2 SP1 nebo novější, Windows Server 2012, Windows Server 2012 R2 nebo Red Hat Enterprise Linux 6.7 (pouze HVM virtualizované instance). Server musí mít jenom Citrix PV nebo AWS PV ovladače. Instance systémem RedHat PV ovladače nejsou podporovány.

Služba Mobility musí být nainstalovaná na jednotlivé virtuální počítače, které chcete replikovat. Site Recovery tato služba nainstaluje automaticky při povolení replikace pro virtuální počítač. Pro automatické instalaci je nutné připravit účet na EC2 instancí, které Site Recovery bude používat pro přístup k virtuálnímu počítači.

Můžete vytvořit, domény nebo místní účet. Pro virtuální počítače s Linuxem musí být účet root na zdrojovém serveru Linux. Pro virtuální počítače Windows, pokud nepoužíváte účet domény, zakažte řízení vzdáleného přístupu uživatele v místním počítači:

  - V registru v části **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD **LocalAccountTokenFilterPolicy** a nastavte hodnotu na 1.
    
Musíte také samostatné instanci EC2, který můžete použít jako konfigurační server Site Recovery. Tato instance musí používat Windows Server 2012 R2. 
    

## <a name="prepare-the-infrastructure"></a>Příprava infrastruktury 

Na stránce portálu pro svůj trezor vyberte **Site Recovery** z **Začínáme** části a pak klikněte na **Příprava infrastruktury**.

### <a name="1-protection-goal"></a>1 cíl ochrany

Vyberte tyto hodnoty na **cíl ochrany** stránky:
   
|    |  | 
|---------|-----------|
| Kde se nachází vaše počítače? | **Místní**|
| Kam chcete počítače replikovat? |**Do Azure**|
| Virtualizované vaše počítače? | **Není virtualizované / jiné**|

Až budete hotovi, klikněte na tlačítko **OK** přesunout k další části.

### <a name="2-source-prepare"></a>Příprava zdrojového 2 

Na **připravit zdroj** klikněte na tlačítko **+ konfigurační Server**. 

1. Pomocí instance EC2 systémem Windows Server 2012 R2 můžete vytvořit konfigurační server a zaregistrovat ji pomocí trezoru obnovení.

2. Proxy server nakonfigurovat na EC2 instance virtuálních počítačů používáte jako konfigurační server tak, aby měl přístup [adresy URL služby](site-recovery-support-matrix-to-azure.md).

3. Stažení [Unified instalace serveru Microsoft Azure Site Recovery](http://aka.ms/unifiedinstaller_wus) program. Můžete ho stáhnout do místního počítače a poté zkopírujte přes virtuální počítač používáte jako konfigurační server.

4. Klikněte na **Stáhnout** tlačítko Stáhnout registrační klíč trezoru. Zkopírujte stažený soubor přes virtuální počítač používáte jako konfigurační server.

5. Ve virtuálním počítači, klikněte pravým tlačítkem na jste stáhli pro instalační program **Unified instalace serveru Microsoft Azure Site Recovery** a vyberte **spustit jako správce**. 

    1. V **než začnete**, vyberte **nainstalujte konfigurační server a procesový server** a pak klikněte na **Další**.
    2. V **licence k softwaru třetích stran**, vyberte **třetích stran licenční smlouvy souhlasím.** a pak klikněte na **Další**.
    3. V **registrace**, klikněte na tlačítko Procházet a přejděte do, kam umístit soubor registračního klíče trezoru a pak klikněte na **Další**.
    4. V **nastavení Internetu**, vyberte **připojit k Azure Site Recovery bez serveru proxy.** a pak klikněte na **Další**.
    5. V **Kontrola předpokladů** , jeho spuštění stránky kontroluje několik položek. Po dokončení klikněte na tlačítko **Další**. 
    6. V **MySQL konfigurace**, zadejte požadované hesla a pak klikněte na tlačítko **Další**.
    7. V **prostředí podrobnosti**, vyberte **ne**, nemusíte chránit počítače VMware a pak klikněte na tlačítko **Další**.
    8. V **umístění instalovat**, klikněte na tlačítko **Další** přijměte výchozí nastavení.
    9. V **výběr sítě**, klikněte na tlačítko **Další** přijměte výchozí nastavení.
    10. V **Souhrn** klikněte na tlačítko **nainstalovat**.
    11. **Průběh instalace** vám zobrazí informace o tom, kde jsou během procesu instalace. Po dokončení klikněte na tlačítko **Dokončit**. Získat automaticky otevíraného okna o nutnosti možné restartovat počítač, klikněte na tlačítko **OK**. Můžete také získat automaticky otevíraného okna o heslo konfiguračního serveru připojení, zkopírujte přístupové heslo do schránky a uložit ho někde bezpečné.
    
6. Ve virtuálním počítači, spusťte **cspsconfigtool.exe** vytvořit jeden nebo více účtů pro správu na konfiguračním serveru. Ujistěte se, že účty pro správu mají oprávnění správce na EC2 instancí, které chcete migrovat. 

Po dokončení nastavení konfigurace serveru, přejděte zpět na portál a vyberte server, který jste právě vytvořili pro **konfigurační Server** a klikněte na tlačítko *OK** přejděte krok 3 připravit cílový.

### <a name="3-target-prepare"></a>Příprava 3 cíl 

V této části zadejte informace o prostředcích, které jste vytvořili, když se [prostředky Azure Příprava](#prepare-azure-resources) výše v tomto kurzu.

1. V **předplatné**, vyberte předplatné Azure, který jste použili pro [Příprava Azure](tutorial-prepare-azure.md) kurzu.
2. Jako model nasazení vyberte **Resource Manager**.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure. Tady by měly být prostředky, které jste vytvořili, když se [prostředky Azure Příprava](#prepare-azure-resources) výše v tomto kurzu
4. Až to budete mít, klikněte na **OK**.


### <a name="4-replication-settings-prepare"></a>Nastavení replikace 4 Příprava 

Je třeba vytvořit zásadu replikace před povolením replikace

1. Klikněte na tlačítko **+ replikovat a přidružit**.
2. V **název**, typ **myReplicationPolicy**.
3. Ponechejte zbývající výchozí nastavení a klikněte na tlačítko **OK** k vytvoření zásad. Nová zásada se automaticky přidruží konfigurační server. 

### <a name="5-deployment-planning-select"></a>5 nasazení plánování vyberte 

V **dokončili jste plánování nasazení?**, vyberte **udělám to později** z rozevíracího seznamu a pak klikněte na **OK**.

Až všechny skončíte s všechny 5 části **připravit infrastrukturu**, klikněte na tlačítko **OK**.


## <a name="enable-replication"></a>Povolení replikace

Povolení replikace pro každý virtuální počítač, který chcete migrovat. Pokud je zapnutá replikace, Site Recovery automaticky nainstaluje službu Mobility. 

1. Otevřete web [Azure Portal](htts://portal.azure.com).
1. Na stránce pro svůj trezor pod **Začínáme**, klikněte na tlačítko **Site Recovery**.
2. V části **pro místní počítače a virtuální počítače Azure**, klikněte na tlačítko **krok 1:Replicate aplikace**. Stránky průvodce se následující informace a klikněte na tlačítko Dokončit **OK** na každé stránce po dokončení:
    - 1 zdroj konfigurace:
      
    |  |  |
    |-----|-----|
    | Zdroj: | **Místní**|
    | Umístění zdroje:| Název instance serveru EC2 konfigurace.|
    |Typ počítače: | **Fyzické počítače**|
    | Procesový server: | V rozevíracím seznamu vyberte konfigurační server.|
    
    - Nakonfigurujte cíl 2
        
    |  |  |
    |-----|-----|
    | Cíl: | Ponechte výchozí nastavení.|
    | Předplatné: | Vyberte odběr, který používáte.|
    | POST-převzetí služeb při selhání skupiny prostředků:| Použít skupinu prostředků, kterou jste vytvořili v [prostředky Azure Příprava](#prepare-azure-resources) části.|
    | Model nasazení POST-převzetí služeb při selhání: | Zvolte **Resource Manager**|
    | Účet úložiště: | Zvolte účet úložiště, kterou jste vytvořili v [prostředky Azure Příprava](#prepare-azure-resources) části.|
    | Síť Azure: | Zvolte **nakonfigurovat pro vybrané počítače**|
    | POST-převzetí služeb při selhání sítě Azure: | Vyberte síť, kterou jste vytvořili v [prostředky Azure Příprava](#prepare-azure-resources) části.|
    | Podsítě: | Vyberte **výchozí** z rozevíracího seznamu.|
    
    - Vyberte 3 fyzické počítače
        
        Klikněte na tlačítko **+ fyzický počítač** a pak zadejte **název**, **IP adresu** a **typ operačního systému** EC2 instance, která chcete migrovat a pak klikněte na tlačítko **OK**.
        
    - 4 vlastnosti konfigurovat vlastnosti
        
        Vyberte účet, který jste vytvořili na konfiguračním serveru z rozevíracího seznamu a klikněte na tlačítko **OK**.
        
    - Nastavení replikace 5 nakonfigurovat nastavení replikace
    
        Zkontrolujte, zda je vybraný v rozevíracím seznamu zásady replikace **myReplicationPolicy** a pak klikněte na **OK**.
        
3. Po dokončení průvodce klikněte na tlačítko **povolit replikaci**.
        

Můžete sledovat průběh **povolení ochrany** úlohy v **monitorování a sestavy** > **úlohy** > **úlohy Site Recovery** . Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.        
        
Když povolíte replikaci pro virtuální počítač, ho trvat 15 minut nebo déle změny mohly ovlivňuje a zobrazovat na portálu.

## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

Když spustíte testovací převzetí služeb, dojde k následujícímu:

1. Kontrola předpokladů postupné a zkontrolujte, zda jsou všechny podmínky pro převzetí služeb při selhání na místě.
2. Převzetí služeb při selhání zpracuje data, takže aby bylo možné vytvořit virtuální počítač Azure. Pokud vyberte nejnovější bod obnovení, bod obnovení vytvořen z dat.
3. Virtuální počítač Azure je vytvořený pomocí dat, která zpracuje v předchozím kroku.

Na portálu spusťte testovací převzetí služeb takto:

1. Na stránce pro svůj trezor, přejděte na **chráněné položky** > **replikované položky**> klikněte na virtuální počítač > **+ testovací převzetí služeb při selhání**.

2. Vyberte bod obnovení pro převzetí:
    - **Nejnovější zpracované**: selhání virtuálního počítače na nejnovější bod obnovení, který byl zpracován pomocí Site Recovery. Zobrazí se časové razítko. Pomocí této možnosti je žádné čas strávený zpracováváním dat, takže nabízí nízkou RTO (plánovanou dobu obnovení).
    - **Nejnovější aplikace konzistentní**: tuto možnost převezme všech virtuálních počítačů na nejnovější bod obnovení konzistentních s aplikací. Zobrazí se časové razítko.
    - **Vlastní**: Vyberte libovolného bodu obnovení.
3. V **testovací převzetí služeb při selhání**, vyberte cíl Azure sítě, které virtuální počítače Azure připojí po převzetí služeb při selhání. Měl by být jste vytvořili v síti [prostředky Azure Příprava](#prepare-azure-resources) části.
4. Kliknutím na **OK** zahajte převzetí služeb při selhání. Průběh můžete sledovat kliknutím na virtuálním počítači otevřete jeho vlastnosti. Nebo můžete kliknout **testovací převzetí služeb při selhání** úlohy na stránce pro svůj trezor v **monitorování a sestavy** > **úlohy** >
   **lokality Úlohy obnovení**.
5. Po dokončení převzetí repliku virtuálního počítače Azure se zobrazí na portálu Azure > **virtuální počítače**. Zkontrolujte, zda je virtuální počítač odpovídající velikost, byl připojený k síti správné, a zda je spuštěna.
6. Teď by měla být možné se připojit k replikovaný virtuální počítač v Azure.
7. Chcete-li odstranit virtuální počítače Azure vytvořené během testu převzetí služeb, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** v plánu obnovení. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb.

V některých scénářích převzetí služeb při selhání vyžaduje další zpracování, který trvá přibližně osm až deset minut na dokončení. 


## <a name="migrate-to-azure"></a>Migrace do Azure

Spusťte skutečné převzetí služeb při selhání pro EC2 instance, které chcete migrovat na virtuálních počítačích Azure. 

1. V **chráněné položky** > **replikované položky** klikněte na tlačítko instance AWS > **převzetí služeb při selhání**.
2. V **převzetí služeb při selhání** vyberte **bod obnovení** k převzetí služeb při selhání. Vyberte nejnovější bod obnovení.
3. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání** Pokud chcete, aby Site Recovery se pokusit o proveďte vypnutí zdrojových virtuálních počítačích než převzetí služeb při selhání. Převzetí služeb při selhání pokračovat i v případě, že vypnutí selže. Můžete sledovat průběh převzetí služeb při selhání **úlohy** stránky.
4. Zkontrolujte, zda virtuální počítač se zobrazí v **replikované položky**. 
5. Klikněte pravým tlačítkem na každý virtuální počítač > **dokončení migrace**. To dokončení procesu migrace, zastaví replikaci pro virtuální počítač AWS a zastaví fakturace Site Recovery pro virtuální počítač.

    ![Dokončení migrace](./media/tutorial-migrate-aws-to-azure/complete-migration.png)

> [!WARNING]
> **Nemáte zrušení převzetí služeb při selhání v průběhu**: před zahájením převzetí služeb při selhání replikace virtuálního počítače je zastavena. Pokud zrušíte převzetí služeb při selhání v průběhu, zastaví převzetí služeb při selhání, ale nebude znovu replikaci virtuálního počítače.  


    

## <a name="next-steps"></a>Další kroky

V tomto tématu když jste se naučili jak migrovat instance AWS EC2 virtuálních počítačích Azure. Další informace o virtuálních počítačích Azure, pokračujte kurzy pro virtuální počítače Windows.

> [!div class="nextstepaction"]
> [Kurzy pro virtuální počítače Azure s Windows](../virtual-machines/windows/tutorial-manage-vm.md)
