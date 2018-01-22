---
title: "Zřízení prostředí Azure SSIS Integration Runtime pomocí portálu | Dokumentace Microsoftu"
description: "Tento článek vysvětluje, jak vytvořit prostředí Azure-SSIS Integration Runtime pomocí webu Azure Portal."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: spelluru
ms.openlocfilehash: b6a795f8a26340f24f9e09aea371ba90afe50101
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="provision-an-azure-ssis-integration-runtime-by-using-the-data-factory-ui"></a>Zřízení prostředí Azure SSIS Integration Runtime pomocí uživatelského rozhraní služby Data Factory
Tento kurz obsahuje postup pro zřízení prostředí Azure-SSIS Integration Runtime (IR) ve službě Azure Data Factory pomocí webu Azure Portal. Následně můžete pomocí SQL Server Data Tools (SSDT) nebo aplikace SQL Server Management Studio (SSMS) do tohoto modulu runtime v Azure nasadit balíčky SSIS (SQL Server Integration Services). Koncepční informace o Azure-SSIS IR najdete v [přehledu prostředí Azure-SSIS Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime).

V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření a spuštění prostředí Azure-SSIS Integration Runtime

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Požadavky
- **Předplatné Azure**. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete. 
- **Server služby Azure SQL Database**. Pokud ještě nemáte databázový server, vytvořte si ho na webu Azure Portal před tím, než začnete. Azure Data Factory na tomto databázovém serveru vytvoří databázi katalogu služby SSIS (SSISDB). Doporučujeme vytvořit databázový server ve stejné oblasti Azure jako prostředí Integration Runtime. Tato konfigurace umožňuje prostředí Integration Runtime zapisovat protokoly spuštění do databáze SSISDB bez přecházení mezi oblastmi Azure. 
    - Ujistěte se, že nastavení **Povolit přístup ke službám Azure** je pro databázový server zapnuté. Další informace najdete v tématu [Zabezpečení databáze SQL Azure](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Pokud chcete toto nastavení povolit pomocí PowerShellu, přečtěte si téma věnované rutině [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
    - Přidejte IP adresu klientského počítače nebo rozsah IP adres, který obsahuje IP adresu klientského počítače, do seznamu IP adres v nastavení brány firewall pro databázový server. Další informace najdete v tématu [Pravidla brány firewall na úrovni serveru a databáze služby Azure SQL Database](../sql-database/sql-database-firewall-configure.md). 
 
## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).    
2. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Na stránce **Nová datová továrna** jako **název** zadejte **MyAzureSsisDataFactory**. 
      
     ![Stránka Nová datová továrna](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba, změňte název datové továrny (například na vaše_jméno_MyAzureSsisDataFactory) a zkuste to znovu. Pravidla pojmenování artefaktů služby Data Factory najdete v článku [Data Factory – pravidla pojmenování](naming-rules.md).
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
4. Pro **Skupinu prostředků** proveďte jeden z následujících kroků:
     
      - Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků. 
      - Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.   
         
      Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
4. Jako **verzi** vyberte **V2 (Preview)**.
5. Vyberte **umístění** pro objekt pro vytváření dat. V seznamu se zobrazí pouze podporovaná umístění pro vytváření datových továren.
6. Zaškrtněte **Připnout na řídicí panel**.     
7. Klikněte na možnost **Vytvořit**.
8. Na řídicím panelu vidíte následující dlaždice se statusem: **Nasazování datové továrny**. 

    ![nasazování dlaždice datové továrny](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Po vytvoření se zobrazí stránka **Datová továrna**, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Kliknutím na dlaždici **Vytvořit a monitorovat** otevřete na samostatné kartě uživatelské rozhraní služby Data Factory. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Zřízení prostředí Azure SSIS Integration Runtime

1. Na stránce Začínáme klikněte na dlaždici **Konfigurace prostředí SSIS Integration Runtime**. 

   ![Dlaždice Konfigurace prostředí SSIS Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Na stránce **Obecná nastavení** v okně **Instalace prostředí Integration Runtime** proveďte následující kroky: 

   ![Obecná nastavení](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Zadejte **název** prostředí Integration Runtime.
    2. Zadejte **umístění** prostředí Integration Runtime. Zobrazí se pouze podporovaná umístění.
    3. Vyberte **velikost uzlu**, která se má pro modul runtime služby SSIS nakonfigurovat.
    4. Zadejte **počet uzlů** v clusteru.
    5. Klikněte na **Další**. 
1. V **Nastavení SQL** proveďte následující kroky: 

    ![Nastavení SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Zadejte **předplatné** Azure, které obsahuje server SQL Azure. 
    2. Jako **Koncový bod databázového serveru katalogu** vyberte váš server SQL Azure.
    3. Zadejte uživatelské jméno **správce**.
    4. Zadejte **heslo** pro tohoto správce.  
    5. Vyberte **úroveň služby** pro databázi SSISDB. Výchozí hodnota je Basic.
    6. Klikněte na **Další**. 
1.  Na stránce **Upřesnit nastavení** vyberte hodnotu pro **Maximální počet paralelních zpracování na uzel**.   

    ![Upřesnit nastavení](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Tento krok je **volitelný**. Pokud máte klasickou virtuální síť, ke které chcete prostředí Integration Runtime připojit, vyberte možnost **Vybrat virtuální síť, ke které se prostředí Azure SSIS Integration Runtime připojí, a povolit službám Azure konfigurovat oprávnění a nastavení virtuální sítě** a pak proveďte následující kroky: 

    ![Upřesnit nastavení s virtuální sítí](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Zadejte **předplatné**, které obsahuje klasickou virtuální síť. 
    2. Vyberte **Virtuální síť**. <br/>
    4. Vyberte **Podsíť**.<br/> 
1. Kliknutím na **Dokončit** zahajte vytváření prostředí Azure SSIS Integration Runtime. 

    > [!IMPORTANT]
    > - Tento proces trvá přibližně 20 minut.
    > - Služba Data Factory se připojí k vaší službě Azure SQL Database a připraví databázi katalogu služby SSIS (SSISDB). Skript také nakonfiguruje oprávnění a nastavení vaší virtuální sítě, pokud je zadáte, a připojí k této virtuální síti novou instanci prostředí Azure SSIS Integration Runtime.
7. V okně **Připojení** v případě potřeby přepněte na **Prostředí Integration Runtime**. Kliknutím na **Aktualizovat** aktualizujte stav. 

    ![Stav vytváření](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Pomocí odkazů ve sloupci **Akce** můžete prostředí Integration Runtime monitorovat, zastavit nebo spustit, upravit nebo odstranit. Pomocí posledního odkazu zobrazíte kód JSON pro prostředí Integration Runtime. Tlačítka pro úpravu a odstranění jsou aktivní, pouze když je prostředí IR zastavené. 

    ![Azure SSIS IR – akce](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Klikněte na odkaz **Monitorovat** ve sloupci **Akce**.  

    ![Azure SSIS IR – podrobnosti](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Pokud k prostředí Azure SSIS IR byly přidružené nějaké **chyby**, na této stránce se zobrazí počet chyb a odkaz na zobrazení podrobností o chybách. Například pokud už na databázovém serveru existuje katalog služby SSIS, zobrazí se chyba značící existenci databáze SSISDB.  
11. Kliknutím na **Prostředí Integration Runtime** v horní části přejděte zpět na předchozí stránku, kde se zobrazí všechna prostředí Integration Runtime přidružená k dané datové továrně.  

## <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Prostředí Azure SSIS Integration Runtime na portálu

1. V uživatelském prostředí služby Azure Data Factory přepněte na kartu **Upravit**, klikněte na **Připojení** a pak přepněte zpět na kartu **Prostředí Integration Runtime**, kde se zobrazí existující prostředí Integration Runtime ve vaší datové továrně. 
    ![Zobrazení existujících IR](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Kliknutím na **Nový** vytvořte nové prostředí Azure-SSIS IR. 

    ![Prostředí Integration Runtime prostřednictvím nabídky](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Pokud chcete vytvořit prostředí Azure-SSIS Integration Runtime, klikněte na **Nový**, jak je znázorněno na obrázku. 
3. V okně Instalace prostředí Integration Runtime vyberte **Migrovat metodou „lift and shift“ existující balíčky služby SSIS ke spuštění v Azure** a pak klikněte na **Další**.

    ![Zadání typu prostředí Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Zbývající kroky k nastavení prostředí Azure-SSIS IR najdete v části [Zřízení prostředí Azure SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

    
## <a name="deploy-ssis-packages"></a>Nasazení balíčků SSIS
Teď použijte SQL Server Data Tools (SSDT) nebo aplikaci SQL Server Management Studio (SSMS) k nasazení vašich balíčků SSIS do Azure. Připojte se k serveru SQL Azure, který hostuje katalog služby SSIS (SSISDB). Název serveru SQL Azure je ve formátu `<servername>.database.windows.net` (pro službu Azure SQL Database). 

Projděte si následující články v dokumentaci ke službě SSIS: 

- [Nasazení, spuštění a monitorování balíčku SSIS v Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Připojení ke katalogu SSIS v Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Plánování spouštění balíčku v Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Připojení k místním zdrojům dat s využitím ověřování systému Windows](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili: 

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření a spuštění prostředí Azure-SSIS Integration Runtime

Pokud se chcete dozvědět víc o kopírování dat z místního prostředí do cloudu, přejděte k následujícímu kurzu: 

> [!div class="nextstepaction"]
>[Kopírování dat v cloudu](tutorial-copy-data-portal.md)
