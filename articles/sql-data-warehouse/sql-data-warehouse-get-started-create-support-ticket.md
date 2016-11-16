---
title: "Jak vytvořit lístek podpory pro SQL Data Warehouse | Dokumentace Microsoftu"
description: "Jak vytvořit lístek podpory v Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ede330870f451bd1e0ed6805fa937e74438fd644


---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Jak vytvořit lístek podpory pro SQL Data Warehouse
Pokud byste měli s SQL Data Warehouse nějaké problémy, můžete si vytvořit lístek podpory, aby vám mohl náš technický tým pomoct.

## <a name="create-a-support-ticket"></a>Vytvoření lístku podpory
1. Otevřete [portál Azure Portal][portál Azure Portal].
2. Na domovské obrazovce klikněte na dlaždici **Nápovědy a podpora**.
   
    ![Nápověda a podpora](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. V okně Nápověda a podpora klikněte na **Vytvořit žádost o podporu**.
   
    ![Nová žádost o podporu](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Vyberte **Typ žádosti**.
   
    ![Typ žádosti](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Ve výchozím nastavení obsahuje každý server SQL (např. myserver.database.windows.net) **kvóty DTU** o hodnotě 45 000. Tato kvóta je jednoduše bezpečnostní omezení. Kvótu můžete zvýšit vytvořením lístku podpory a výběrem *kvóty* jako typu požadavku. Pokud chcete vypočítat potřebnou DTU, vynásobte celkovou potřebnou [DWU][DWU] číslem 7,5. Pokud například chcete hostovat dvě DW6000 na jednom serveru SQL, měli byste požadovat kvóty DTU o hodnotě 90 000.  Vaši aktuální spotřebu DTU z okna serveru SQL můžete zobrazit na portálu. Pozastavené i nepozastavené databáze se započítávají do kvóty DTU. 
   > 
   > 
5. Vyberte **předplatné**, pod které spadá databáze s problémem, který chcete nahlásit.
   
    ![Předplatné](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Jako prostředek vyberte **SQL Data Warehouse**.
   
    ![Prostředek](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Vyberte svůj [plán podpory Azure][plán podpory Azure].
   
   * **Podpora k fakturaci, správě kvót a předplatného** je k dispozici na všech úrovních podpory.
   * **Podpora k problémům vyžadujícím opravu** je poskytována prostřednictvím podpory [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] nebo [Premier][Premier]. Problémy vyžadující opravu jsou problémy, které mají zákazníci při používání Azure a u kterých se dá předpokládat, že je způsobila společnosti Microsoft.
   * V rámci podpory na úrovních [Professional Direct][Professional Direct] a [Premier][Premier] jsou k dispozici služby **mentorování vývojářů** a **poradenské služby**. 
     
     Pokud máte plán podpory Premier Support, můžete také problémy týkající se SQL Data Warehouse nahlásit na [online portálu Microsoft Premier][online portálu Microsoft Premier].  Další informace o různých plánech podpory, včetně rozsahu, doby odezvy, cen atd. najdete v tématu věnovaném [plánům podpory Azure][plán podpory Azure].  Nejčastější dotazy týkající se podpory Azure najdete v [nejčastějších dotazech týkajících se podpory Azure][nejčastějších dotazech týkajících se podpory Azure].  
     
     ![Plán podpory](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Vyberte **typ problému** a **kategorii**.
   
    ![Kategorie typu problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. Popište problém a zvolte úroveň dopadu na chod firmy.
   
    ![Popis problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. Předvyplní se vaše **kontaktní údaje** pro tento lístek podpory. V případě potřeby je aktualizujte.
    
    ![Kontaktní údaje](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. Kliknutím na **Vytvořit** odešlete žádost o podporu.

## <a name="monitor-a-support-ticket"></a>Monitorování lístku podpory
Po odeslání žádosti o podporu vás bude kontaktovat tým podpory Azure. Pokud chcete zkontrolovat stav a podrobnosti žádosti, klikněte na řídicím panelu na **Spravovat žádosti o podporu**.

![Zkontrolování stavu](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Další prostředky
Kromě toho se můžete spojit s komunitou SQL Data Warehouse na [Stack Overflow][Stack Overflow] nebo na [fóru pro Azure SQL Data Warehouse na MSDN][fóru pro Azure SQL Data Warehouse na MSDN].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[portál Azure Portal]: https://portal.azure.com/
[plán podpory Azure]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[nejčastějších dotazech týkajících se podpory Azure]: https://azure.microsoft.com/support/faq/
[online portálu Microsoft Premier]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[fóru pro Azure SQL Data Warehouse na MSDN]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/




<!--HONumber=Nov16_HO2-->


