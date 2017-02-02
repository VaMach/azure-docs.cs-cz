---
title: "Začínáme se zálohováním a obnovením databází SQL Azure pro ochranu a obnovení dat | Dokumentace Microsoftu"
description: "Tento kurz vás seznámí s obnovením k určitému bodu v čase z automatizovaných záloh, ukládáním automatizovaných záloh v trezoru služby Azure Recovery Services a obnovením z trezoru služby Azure Recovery Services."
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: a9adc82faab977ad5da1c9dc281d45db3101d8c8
ms.openlocfilehash: 3fb97f80d5012e993f92eb592d877faabc94c43e


---
# <a name="get-started-with-backup-and-restore-for-data-protection-and-recovery"></a>Začínáme se zálohováním a obnovením pro ochranu a obnovení dat
V tomto úvodním kurzu se naučíte, jak pomocí portálu Azure Portal provádět tyto akce:

- Zobrazení existujících záloh databáze
- Obnovení databáze k dřívějšímu bodu v čase
- Konfigurace dlouhodobého uchovávání souboru zálohy databáze v trezoru služby Azure Recovery Services
- Obnovení databáze z trezoru služby Azure Recovery Services

**Časový odhad**: Dokončení tohoto kurzu trvá přibližně 30 minut (za předpokladu, že už máte splněné požadavky).

> [!TIP]
> Stejné úlohy můžete v úvodním kurzu provádět i pomocí [PowerShellu](sql-database-get-started-backup-recovery-powershell.md).
>

## <a name="prerequisites"></a>Požadavky

* Potřebujete mít účet Azure. Můžete si [zdarma otevřít účet Azure](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele sady Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Musíte být schopni připojit se k webu Azure Portal pomocí účtu, který je členem role přispěvatele nebo vlastníka předplatného. Další informace o řízení přístupu na základě role (RBAC) najdete v tématu [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md).

* Dokončili jste kurz [Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database s využitím webu Azure Portal a aplikace SQL Server Management Studio](sql-database-get-started.md) nebo ekvivalentní [verzi tohoto kurzu pro prostředí PowerShell](sql-database-get-started-powershell.md). Pokud ne, tak tento požadovaný kurz dokončete nebo spusťte skript prostředí PowerShell na konci [verze tohoto kurzu pro prostředí PowerShell](sql-database-get-started-powershell.md) a teprve potom pokračujte.


> [!NOTE]
> Tento kurz vám pomůže seznámit se s obsahem těchto výukových okruhů: [Zálohování SQL Database](sql-database-automated-backups.md), [Dlouhodobé uchovávání záloh](sql-database-long-term-retention.md) a [Obnovení databáze Azure SQL pomocí automatizovaných záloh](sql-database-recovery-using-backups.md).
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Přihlášení k webu Azure Portal pomocí účtu Azure
Pomocí [stávajícího předplatného](https://account.windowsazure.com/Home/Index) se k webu Azure Portal připojíte následujícím postupem.

1. Otevřete prohlížeč, kterému dáváte přednost, a připojte se k [portálu Azure](https://portal.azure.com/).
2. Přihlaste se k webu [Portál Azure](https://portal.azure.com/).
3. Na **přihlašovací stránce** zadejte přihlašovací údaje ke svému předplatnému.
   
   ![Přihlášení](./media/sql-database-get-started/login.png)

<a name="create-logical-server-bk"></a>

## <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database"></a>Zobrazení nejstaršího bodu obnovení ze záloh databáze generovaných službou

V této části kurzu zobrazíte informace o nejstarším bodu obnovení z [automatizovaných záloh vaší databáze vygenerovaných službou](sql-database-automated-backups.md). 

1. Otevřete okno **Databáze SQL** pro vaši databázi **sqldbtutorialdb**.

   ![Nové okno ukázkové databáze](./media/sql-database-get-started/new-sample-db-blade.png)

2. Na panelu nástrojů klikněte na **Obnovit**.

   ![panel nástrojů – obnovit](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

3. V okně Obnovit zkontrolujte nejstarší bod obnovení.

   ![nejstarší bod obnovení](./media/sql-database-get-started-backup-recovery/oldest-restore-point.png)

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Obnovení databáze k dřívějšímu bodu v čase

V této části kurzu obnovíte databázi k určitému bodu v čase do nové databáze.

1. V okně **Obnovit** databáze zkontrolujte výchozí název nové databáze, do které se má vaše databáze obnovit k dřívějšímu bodu v čase (výchozí název tvoří název existující databáze s připojeným časovým razítkem). Tento název se změní, aby odpovídal času, který zadáte v několika dalších krocích.

   ![název obnovené databáze](./media/sql-database-get-started-backup-recovery/restored-database-name.png)

2. Klikněte na ikonu **kalendáře** ve vstupním poli **Bod obnovení (UTC)**.

   ![bod obnovení](./media/sql-database-get-started-backup-recovery/restore-point.png)

2. V kalendáři vyberte datum v rámci doby uchovávání.

   ![datum bodu obnovení](./media/sql-database-get-started-backup-recovery/restore-point-date.png)

3. Do vstupního pole **Bod obnovení (UTC)** zadejte pro vybrané datum čas, ke kterému chcete obnovit data v databázi z automatizovaných záloh databáze.

   ![čas bodu obnovení](./media/sql-database-get-started-backup-recovery/restore-point-time.png)

   >[!NOTE]
   >Všimněte si, že došlo ke změně názvu databáze, aby odpovídal datu a času, které jste vybrali. Všimněte si také, že nemůžete změnit server, na který provádíte obnovení k určitému bodu v čase. Pokud chcete provést obnovení na jiný server, použijte [Geografické obnovení](sql-database-disaster-recovery.md#recover-using-geo-restore). A konečně si všimněte, že můžete provést obnovení do [elastického fondu](sql-database-elastic-jobs-overview.md) nebo do jiné cenové úrovně. 
   >

4. Kliknutím na **OK** obnovte svou databázi k dřívějšímu bodu v čase do nové databáze.

5. Pokud chcete zobrazit stav úlohy obnovení, na panelu nástrojů klikněte na ikonu oznámení.

   ![průběh úlohy obnovení](./media/sql-database-get-started-backup-recovery/restore-job-progress.png)

6. Po dokončení úlohy obnovení otevřete okno **Databáze SQL** a zobrazte nově obnovenou databázi.

   ![obnovená databáze](./media/sql-database-get-started-backup-recovery/restored-database.png)

> [!NOTE]
> Odtud se můžete pomocí aplikace SQL Server Management Studio připojit k obnovené databázi a provádět požadované úlohy, jako je například [extrakce části dat z obnovené databáze a zkopírování do existující databáze nebo odstranění existující databáze a přejmenování obnovené databáze na název existující databáze](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Konfigurace dlouhodobého uchovávání automatizovaných záloh v trezoru služby Azure Recovery Services 

V této části kurzu [nakonfigurujete trezor služby Azure Recovery Services pro uchovávání automatizovaných záloh](sql-database-long-term-retention.md) po dobu delší, než je doba uchovávání vaší úrovně služby. 


> [!TIP]
> Chcete-li odstranit zálohy, přečtěte si téma [Odstranění dlouhodobého uchovávání záloh](sql-database-long-term-retention-delete.md).
>

1. Otevřete okno **SQL Server** pro váš server **sqldbtutorialserver**.

   ![Okno SQL Server](./media/sql-database-get-started/sql-server-blade.png)

2. Klikněte na **Dlouhodobé uchovávání záloh**.

   ![odkaz na dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. V okně **sqldbtutorial – Dlouhodobé uchovávání záloh** si přečtěte a přijměte podmínky verze Preview (pokud jste to ještě neudělali nebo pokud je tato funkce stále ve verzi Preview).

   ![přijetí podmínek verze preview](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Pokud chcete nakonfigurovat dlouhodobé uchovávání záloh pro databázi sqldbtutorialdb, vyberte databázi v mřížce a potom na panelu nástrojů klikněte na **Konfigurovat**.

   ![výběr databáze pro dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. V okně **Konfigurovat** klikněte v části **Trezor služby Recovery Services** na **Konfigurace požadovaných nastavení**.

   ![odkaz na konfiguraci trezoru](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. V okně **Trezor služby Recovery Services** vyberte existující trezor, pokud nějaký existuje. Pokud se pro vaše předplatné nenašel žádný trezor služby Recovery Services, kliknutím proces ukončete a vytvořte trezor služby Recovery Services.

   ![odkaz na vytvoření nového trezoru](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. V okně **Trezory služby Recovery Services** klikněte na **Přidat**.

   ![odkaz na přidání nového trezoru](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. V okně **Trezor služby Recovery Services** zadejte platný název nového trezoru služby Recovery Services.

   ![název nového trezoru](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Vyberte svoje předplatné a skupinu prostředků a potom vyberte umístění trezoru. Až budete hotovi, klikněte na **Vytvořit**.

   ![vytvoření nového trezoru](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > Trezor musí být umístěn ve stejné oblasti Azure jako logický server SQL Azure a musí používat stejnou skupinu prostředků jako logický server.
   >

10. Po vytvoření nového trezoru proveďte nezbytné kroky a vraťte se do okna **Trezor služby Recovery Services**.

11. V okně **Trezor služby Recovery Services** klikněte na trezor a potom na **Vybrat**.

   ![výběr existujícího trezoru](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. V okně **Konfigurovat** zadejte platný název nové zásady uchovávání informací, podle potřeby upravte výchozí zásadu uchovávání informací, a potom klikněte na **OK**.

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. V okně **sqldbtutorial – Dlouhodobé uchovávání záloh** klikněte na **Uložit** a potom kliknutím na **OK** použijte zásadu dlouhodobého uchovávání záloh na všechny vybrané databáze.

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Kliknutím na **Uložit** povolte nakonfigurované dlouhodobé uchovávání záloh v trezoru služby Azure Recovery Services pomocí této nové zásady.

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

15. Po povolení dlouhodobého uchovávání záloh otevřete okno **sqldbtutorialvault** (přejděte do části **Všechny prostředky** a vyberte trezor ze seznamu prostředků pro vaše předplatné).

   ![zobrazení trezoru služby recovery services](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault.png)

> [!IMPORTANT]
> Po nakonfigurování se zálohy v trezoru objeví během příštích sedm dnů. Nepokračujte v tomto kurzu, dokud se zálohy neobjeví v trezoru.
>

## <a name="view-backups-in-long-term-retention"></a>Zobrazení záloh v rámci dlouhodobého uchovávání

V této části kurzu zobrazíte informace o zálohách databáze v rámci [dlouhodobého uchovávání záloh](sql-database-long-term-retention.md). 

1. Pokud chcete zobrazit velikost úložiště, které využívají vaše zálohy databáze v trezoru, otevřete okno **sqldbtutorialvault** (přejděte do části **Všechny prostředky** a vyberte trezor ze seznamu prostředků pro vaše předplatné).

   ![zobrazení trezoru služby recovery services se zálohami](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Otevřete okno **Databáze SQL** pro vaši databázi **sqldbtutorialdb**.

   ![Nové okno ukázkové databáze](./media/sql-database-get-started/new-sample-db-blade.png)

3. Na panelu nástrojů klikněte na **Obnovit**.

   ![panel nástrojů – obnovit](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. V okně Obnovit klikněte na **Dlouhodobé**.

5. V části Zálohy v trezoru Azure klikněte na **Zvolit zálohu** a zobrazte dostupné zálohy databáze v rámci dlouhodobého uchovávání záloh.

   ![zálohy v trezoru](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

## <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Obnovení databáze ze zálohy v rámci dlouhodobého uchovávání záloh

V této části kurzu obnovíte databázi do nové databáze ze zálohy v trezoru služby Azure Recovery Services.

1. V okně **Zálohy v trezoru Azure** klikněte na zálohu, kterou chcete obnovit, a potom klikněte na **Vybrat**.

   ![výběr zálohy v trezoru](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. Do textového pole **Název databáze** zadejte název obnovené databáze.

   ![nový název databáze](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Kliknutím na **OK** obnovte databázi ze zálohy v trezoru do nové databáze.

4. Pokud chcete zobrazit stav úlohy obnovení, na panelu nástrojů klikněte na ikonu oznámení.

   ![průběh úlohy obnovení z trezoru](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Po dokončení úlohy obnovení otevřete okno **Databáze SQL** a zobrazte nově obnovenou databázi.

   ![obnovená databáze z trezoru](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Odtud se můžete pomocí aplikace SQL Server Management Studio připojit k obnovené databázi a provádět požadované úlohy, jako je například [extrakce části dat z obnovené databáze a zkopírování do existující databáze nebo odstranění existující databáze a přejmenování obnovené databáze na název existující databáze](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="next-steps"></a>Další kroky

- Další informace o automatických zálohách generovaných službou najdete u popisu [automatických záloh](sql-database-automated-backups.md).
- Další informace o dlouhodobém uchovávání záloh najdete v části [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md)
- Další informace o obnovování ze záloh najdete v části [obnovení ze zálohy](sql-database-recovery-using-backups.md)



<!--HONumber=Jan17_HO2-->


