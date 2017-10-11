---
title: "Spravovat servery Azure pomocí modelu nasazení classic a trezory Azure Backup | Microsoft Docs"
description: "Pomocí tohoto kurzu se dozvíte, jak ke správě trezoru zálohování Azure a serverů."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: markgal;
ms.openlocfilehash: 91451b2cdc42ed05ef7c1ba9c66ad5b4b45dd788
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Správa serverů a trezorů Azure Backup s využitím klasického modelu nasazení
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Classic](backup-azure-manage-windows-server-classic.md)
>
>

V tomto článku najdete přehled úlohy zálohování správy, který je k dispozici prostřednictvím portálu Azure classic a Microsoft Azure Backup agent.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.

> [!IMPORTANT]
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>

## <a name="management-portal-tasks"></a>Úlohy správy portálu
1. Přihlaste se k [portálu pro správu](https://manage.windowsazure.com).
2. Klikněte na tlačítko **služeb zotavení**, pak klikněte na název úložiště záloh, chcete-li zobrazit stránku rychlý Start.

    ![Služby zotavení](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Výběrem možnosti v horní části stránky rychlý Start, uvidíte dostupné úlohy správy.

![Spravovat karty](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Řídicí panel
Vyberte **řídicí panel** zobrazíte přehled využití pro server. **Přehled využití** zahrnuje:

* Počet serverů, Windows zaregistrovat do cloudu
* Počet virtuálních počítačů Azure chráněna v cloudu
* Celkový úložný využívat v Azure
* Stav posledních úloh

V dolní části řídicího panelu můžete provádět následující úlohy:

* **Správa certifikátů** – Pokud certifikát byl použit k registraci serveru a pak aktualizuje certifikát. Pokud používáte přihlašovací údaje trezoru, nepoužívejte **Správa certifikátu**.
* **Odstranit** -odstraní aktuální úložiště záloh. Pokud se už používá úložiště záloh, chcete-li odstranit pro uvolnění prostoru úložiště. **Odstranit** je aktivní jenom po všechny registrované servery byly odstraněny z trezoru.

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Registrovaných položek
Vyberte **registrované položky** k zobrazení názvů serverů, které jsou registrovány k tomuto úložišti.

![Registrovaných položek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

**Typ** filtrovat výchozí hodnoty pro virtuální počítač Azure. Pokud chcete zobrazit názvy serverů, které jsou registrovány k tomuto trezoru, vyberte **systému Windows server** z rozevírací nabídky.

Odsud můžete provádět následující úlohy:

* **Povolit opakovanou registraci** – Pokud je vybraná tato možnost pro server, můžete použít **Průvodce registrací** v Microsoft Azure Backup agent místní registrace serveru u služby backup trezoru ještě jednou. Možná budete muset znovu registrovat z důvodu chyby v certifikátu, nebo pokud server museli znovu sestavit.
* **Odstranit** -odstraní server z trezoru záloh. Všechna uložená data přidružené k serveru budou okamžitě odstraněna.

    ![Úlohy registrovaných položek](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Chráněné položky
Vyberte **chráněné položky** Chcete-li zobrazit položky, které byly zálohovány ze serverů.

![Chráněné položky](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurace
Z **konfigurace** kartě můžete vybrat možnost redundance odpovídající úložiště. Nyní můžete vybrat možnost redundance úložiště je správné po vytvoření trezoru a předtím, než všechny počítače, které jsou registrovány k němu.

> [!WARNING]
> Jakmile položku byl registrovaný k úložišti, možnost redundance úložiště je uzamčen a nemůže být upraven.
>
>

![Konfigurace](./media/backup-azure-manage-windows-server-classic/configure.png)

Najdete v tomto článku Další informace [redundance úložiště](../storage/common/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Úlohy agenta Microsoft Azure Backup
### <a name="console"></a>Konzola
Otevřete **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).

![Agenta zálohování](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Z **akce** k dispozici na pravé straně konzole agenta zálohování můžete provádět následující úlohy správy:

* Registrace serveru
* Plán zálohování
* Zálohovat nyní
* Změnit vlastnosti

![Konzole akce agenta.](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> K **obnovit Data**, najdete v části [obnovit soubory do systému Windows server nebo klientský počítač systému Windows](backup-azure-restore-windows-server.md).
>
>

### <a name="modify-an-existing-backup"></a>Upravit existující zálohy
1. V agentovi nástroje Microsoft Azure Backup klikněte na tlačítko **plánem zálohování**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. V **Průvodce plánem zálohování** ponechte **provádět změny položky zálohování nebo časy** možnost a klikněte na tlačítko **Další**.

    ![Upravit naplánovaného zálohování](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. Pokud chcete přidat nebo změnit položky, na **výběr položek k zálohování** obrazovce klikněte na tlačítko **přidat položky**.

    Můžete také nastavit **nastavení vyloučení** z této stránky v průvodci. Pokud chcete vyloučit soubory nebo typy souborů, přečtěte si postup pro přidání [nastavení vyloučení](#exclusion-settings).
4. Vyberte soubory a složky, které chcete zálohovat a klikněte na tlačítko **nevadí**.

    ![Přidání položek](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. Zadejte **plán zálohování** a klikněte na tlačítko **Další**.

    Můžete naplánovat denní (probíhající maximálně 3krát denně) nebo týdenní zálohování.

    ![Zadejte plán zálohování](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Zadat plán zálohování je podrobně popsány v tomto [článku](backup-azure-backup-cloud-as-tape.md).
   >
   >
6. Vyberte **zásady uchovávání informací** pro záložní kopii a klikněte na tlačítko **Další**.

    ![Vyberte vaše zásady uchovávání informací](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. Na **potvrzení** obrazovky Zkontrolujte zadané informace a klikněte na tlačítko **Dokončit**.
8. Až průvodce dokončí vytváření **plán zálohování**, klikněte na tlačítko **Zavřít**.

    Po úpravě ochrany, můžete ověřit, že zálohy spouštějí správně přechodem na **úlohy** kartě a potvrzující, že změny se projeví v úloh zálohování.

### <a name="enable-network-throttling"></a>Povolit omezení šířky pásma sítě
Agent Azure Backup poskytuje omezování karta, která umožňuje řídit použití šířky pásma sítě při přenosu dat. Tento ovládací prvek může být užitečné, pokud potřebujete zálohovat data v pracovní době, ale nechcete, aby proces zálohování narušoval ostatní internetový provoz. Omezování dat přenos platí pro zálohování a obnovení aktivity.  

Povolit omezení:

1. V **agenta zálohování**, klikněte na tlačítko **změnit vlastnosti**.
2. Vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.

    ![Omezení šířky pásma sítě](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. Jakmile jste povolili omezování, zadejte povolenou šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.

    Hodnoty šířky pásma začít až 512 kilobitů za sekundu (Kbps) a můžete přejít až 1023 MB za sekundu (Mbps). Můžete také určit spuštění a dokončení pro **pracovní hodiny**, a které dny v týdnu, jsou považovány za pracovních dnů. Mimo pracovní dobu v určené době se považuje za mimo pracovní dobu.
4. Klikněte na **OK**.

## <a name="exclusion-settings"></a>Nastavení vyloučení
1. Otevřete **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).

    ![Otevřete agenta zálohování](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. V agentovi nástroje Microsoft Azure Backup klikněte na tlačítko **plánem zálohování**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. V ponechejte Průvodce plánem zálohování **provádět změny položky zálohování nebo časy** možnost a klikněte na tlačítko **Další**.

    ![Změna plánu](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. Klikněte na tlačítko **nastavení vyloučení**.

    ![Vyberte položky, které chcete vyloučit](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. Klikněte na tlačítko **Přidat vyloučení**.

    ![Přidat vyloučení](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. Vyberte umístění a potom klikněte na **OK**.

    ![Vyberte umístění pro vyloučení](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. Přidat příponu souboru v **typ souboru** pole.

    ![Vyloučit podle typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Přidání rozšíření MP3

    ![Příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Chcete-li přidat jiné rozšíření, klikněte na tlačítko **Přidat vyloučení** a zadejte jinou příponu typu souboru (Přidání rozšíření .jpeg).

    ![Další příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. Pokud jste přidali všechna rozšíření, klikněte na tlačítko **OK**.
9. Pomocí Průvodce plánem zálohování nadále pokračovat kliknutím **Další** dokud **potvrzovací stránku**, pak klikněte na tlačítko **Dokončit**.

    ![Vyloučení potvrzení](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Další kroky
* [Obnovení systému Windows Server nebo klienta Windows z Azure](backup-azure-restore-windows-server.md)
* Další informace o zálohování Azure, najdete v části [Přehled zálohování Azure](backup-introduction-to-azure-backup.md)
* Přejděte [fórum Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933)
