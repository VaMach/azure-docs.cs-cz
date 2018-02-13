---
title: "Nastavit zotavení po havárii do Azure pro fyzický místní servery s Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak nastavit zotavení po havárii do Azure pro místní systém Windows a Linux serverů se službou Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.openlocfilehash: 01e582cb789e402496c920e4a8fe27d5c6848531
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Nastavit zotavení po havárii do Azure pro místní fyzických serverů

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategie zotavení po havárii správy a Orchestrace replikace, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

V tomto kurzu se dozvíte, jak nastavit zotavení po havárii v místě fyzických Windows a Linux serverů do Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nastavení Azure a místní požadavky
> * Vytvoření trezoru služeb zotavení pro Site Recovery 
> * Nastavit zdroje a cíle replikace prostředí
> * Vytvoření zásady replikace
> * Povolení replikace pro server

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

- Ujistěte se, že rozumíte [scénář architektura a komponenty](concepts-physical-to-azure-architecture.md).
- Zkontrolujte [podporu požadavků](site-recovery-support-matrix-to-azure.md) pro všechny součásti.
- Ujistěte se, že servery, které chcete replikovat vyhovují [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
- Příprava Azure. Potřebujete předplatné Azure, virtuální sítě Azure a účet úložiště.
- Připravte účet automatická instalace služby Mobility na každém serveru, který chcete replikovat.



### <a name="set-up-an-azure-account"></a>Nastavit účet Azure

Získat Microsoft [účet Azure](http://azure.microsoft.com/).

- Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
- Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
- Zjistit, které [oblasti jsou podporované](https://azure.microsoft.com/pricing/details/site-recovery/) Site Recovery.

### <a name="verify-azure-account-permissions"></a>Ověřte oprávnění pro účet Azure

Ujistěte se, že váš účet Azure má oprávnění pro replikaci virtuálních počítačů do Azure.

- Zkontrolujte [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) je potřeba replikovat počítače do Azure.
- Ověřte a upravte [přístupu podle rolí](../active-directory/role-based-access-control-configure.md) oprávnění. 



### <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

Nastavení [síť Azure](../virtual-network/quick-create-portal.md).

- Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.
- Síť musí být ve stejné oblasti jako trezor služeb zotavení


## <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure

Nastavení [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Site Recovery replikuje místní počítače do úložiště Azure. Virtuální počítače Azure jsou vytvořené z úložiště po převzetí služeb při selhání.
- Účet úložiště musí být ve stejné oblasti jako trezor služeb zotavení.
- Účet úložiště může být standardní nebo [premium](../virtual-machines/windows/premium-storage.md).
- Pokud jste nastavili prémiového účtu taky musíte další standardní účet pro data protokolu.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Příprava účet pro instalaci služby Mobility

Služba Mobility musí být nainstalovaná na každém serveru, který chcete replikovat. Site Recovery tato služba nainstaluje automaticky při povolení replikace pro server. Pokud chcete automaticky nainstalovat, je nutné připravit účtu, který Site Recovery bude používat pro přístup k serveru.

- Můžete použít domény nebo místní účet
- Pro virtuální počítače Windows, pokud nepoužíváte účet domény, zakažte řízení vzdáleného přístupu uživatele v místním počítači. Chcete-li to provést, v registru podle **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD **LocalAccountTokenFilterPolicy**, s hodnotou 1.
- Chcete-li přidat položku registru zakázat s nastavením rozhraní příkazového řádku, zadejte:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Pro systémy Linux musí být účet root na zdrojovém serveru Linux.


## <a name="create-a-vault"></a>Vytvoření trezoru

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte obsah k replikaci a jeho replikace.

1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V nabídce prostředků, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
3. V **cíl ochrany**, vyberte **do Azure** > **není virtualizované/jiné**.

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavení konfigurace serveru, registrace v trezoru a vyhledání virtuálních počítačů.

1. Klikněte na tlačítko **lokality obnovení** > **Příprava infrastruktury** > **zdroj**.
2. Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.
3. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.
5. Stáhnout registrační klíč trezoru Tuto funkci potřebujete po spuštění Unified instalace. Klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/tutorial-physical-to-azure/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Zaregistrujte konfigurační server v trezoru

Než začnete, postupujte takto: 

- Ujistěte se, jestli se systémové hodiny synchronizované se na počítač serveru konfigurace, [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Měla by se shodovat. Pokud je 15 minut před nebo za instalace může selhat.
- Zkontrolujte, zda že má počítač přístup tyto adresy URL:[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

- Pravidla brány firewall založená na adresu IP by měl povolit komunikaci s Azure.
- Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).
- Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).

Spusťte instalaci Unified jako místní správce a instalace konfigurační server. Procesový server a hlavní cílový server jsou také nainstalované ve výchozím nastavení na konfiguračním serveru.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Po dokončení registrace se konfigurační server se zobrazí na **nastavení** > **servery** stránky v úložišti.


## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte a ověřte cílové prostředky.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Zadejte model nasazení cíl.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![Cíl](./media/tutorial-physical-to-azure/network-storage.png)


## <a name="create-a-replication-policy"></a>Vytvoření zásady replikace

1. Chcete-li vytvořit novou zásadu replikace, klikněte na tlačítko **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.
2. V **vytvořit zásadu replikace**, zadejte název zásady.
3. V **prahovou hodnotu RPO**, zadejte cíle (RPO) limitu bodu obnovení. Tato hodnota určuje, jak často jsou vytvořeny body obnovení data. Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.
4. V **uchování bodu obnovení**, určete, jak dlouho (v hodinách) je okno uchování pro každý bod obnovení. Replikované virtuální počítače můžete obnovit do libovolného bodu v okně. Až 24 hodin uchování je podporována pro počítače, na které se replikují do úložiště úrovně premium a 72 hodin, než standardní úložiště.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi. Klikněte na tlačítko **OK** k vytvoření zásad.

    ![Zásady replikace](./media/tutorial-physical-to-azure/replication-policy.png)


Zásady se automaticky přidruží konfigurační server. Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady. Například, pokud je zásada replikace **rep zásad** pak zásadu navrácení služeb po obnovení **rep. zásady navrácení** je vytvořena. Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.

## <a name="enable-replication"></a>Povolení replikace

Povolení replikace pro každý server.

- Site Recovery bude instalace služby Mobility, pokud je zapnutá replikace.
- Pokud povolíte replikaci pro server, může trvat 15 minut nebo déle, než změny vstoupí v platnost a zobrazovat na portálu.

1. Klikněte na tlačítko **replikujte aplikaci** > **zdroj**.
2. V **zdroj**, vyberte konfigurační server.
3. V **počítač typ**, vyberte **fyzických počítačů**.
4. Vyberte procesový server (konfigurační server). Pak klikněte na **OK**.
5. V **cíl**, vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit virtuální počítače Azure po převzetí služeb při selhání. Vyberte model nasazení, který chcete použít v Azure (klasický nebo prostředek management).
6. Vyberte účet úložiště Azure, který chcete použít pro replikaci dat. 
7. Vyberte síť Azure a podsíť, ke kterým se připojí virtuální počítače Azure, když se po převzetí služeb při selhání vytvoří.
8. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač. 
9. V **fyzické počítače**a klikněte na tlačítko **+ fyzický počítač**. Zadejte název a IP adresu. Vyberte operační systém počítače, který chcete replikovat. Trvá několik minut pro servery, aby byly zjištěny a uvedené. 
10. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte účet, který se použije pro automaticky instalaci služby Mobility na počítači procesní server.
11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, zda je vybrán správný replikace zásad. 
12. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.


Ke sledování serverů, které přidáte, můžete zkontrolovat, čas poslední zjištěné u nich **konfigurační servery** > **poslední obraťte se na**. Chcete-li přidat počítače bez čekání naplánovaný čas zjišťování, zvýrazněte konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.

## <a name="next-steps"></a>Další postup

[Spuštění postupu zotavení po havárii](tutorial-dr-drill-azure.md)
