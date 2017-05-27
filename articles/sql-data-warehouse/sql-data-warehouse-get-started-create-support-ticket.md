---
title: "Jak vytvořit lístek podpory pro SQL Data Warehouse | Dokumentace Microsoftu"
description: "Jak vytvořit lístek podpory v Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 889776269aa3d722115ea0632b7c3db2775924eb
ms.openlocfilehash: cc3bc8a0463842ba11b9cdcc996319e2d811ce8a
ms.contentlocale: cs-cz
ms.lasthandoff: 01/18/2017


---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Jak vytvořit lístek podpory pro SQL Data Warehouse
Pokud máte se službou SQL Data Warehouse nějaké problémy, můžete si vytvořit lístek podpory, aby vám mohl náš technický tým pomoct.

> [!NOTE] 
> Od 20. 12. 2016 je kontrola stavu prostředků na webu Azure Portal nepřesná. Na vyřešení tohoto problému aktivně pracujeme. 


## <a name="create-a-support-ticket"></a>Vytvoření lístku podpory
1. Otevřete [Azure Portal][Azure portal].
2. Na domovské obrazovce klikněte na dlaždici **Nápovědy a podpora**.
   
    ![Nápověda a podpora](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. V okně Nápověda a podpora klikněte na **Vytvořit žádost o podporu**.
   
    ![Nová žádost o podporu](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Vyberte **Typ žádosti**.
   
    ![Typ žádosti](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Ve výchozím nastavení obsahuje každý server SQL (např. myserver.database.windows.net) **kvóty DTU** o hodnotě 45 000. Tato kvóta je jednoduše bezpečnostní omezení. Kvótu můžete zvýšit vytvořením lístku podpory a výběrem *kvóty* jako typu požadavku. Chcete-li vypočítat potřebné DTU, vynásobte celkové potřebné [DWU][DWU] číslem 7,5. Pokud například chcete hostovat dvě DW6000 na jednom serveru SQL, měli byste požadovat kvóty DTU o hodnotě 90 000.  Vaši aktuální spotřebu DTU z okna serveru SQL můžete zobrazit na portálu. Pozastavené i nepozastavené databáze se započítávají do kvóty DTU. 
   > 
   > 
5. Vyberte **předplatné**, pod které spadá databáze s problémem, který chcete nahlásit.
   
    ![Předplatné](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Jako prostředek vyberte **SQL Data Warehouse**.
   
    ![Prostředek](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Vyberte svůj [plán podpory Azure][Azure support plan].
   
   * **Podpora k fakturaci, správě kvót a předplatného** je k dispozici na všech úrovních podpory.
   * Podpora k problémům vyžadujícím **opravu** je poskytována prostřednictvím plánů podpory [Developer Support][Developer], [Standard Support][Standard], [Professional Direct][Professional Direct] nebo [Premier Support][Premier]. Problémy vyžadující opravu jsou problémy, které mají zákazníci při používání Azure a u kterých se dá předpokládat, že je způsobila společnosti Microsoft.
   * V rámci podpory na úrovni plánů [Professional Direct][Professional Direct] a [Premier Support][Premier] jsou k dispozici služby **mentorování vývojářů** a **poradenské služby**. 
     
     Pokud máte plán podpory Premier Support, můžete také problémy týkající se služby SQL Data Warehouse nahlásit na [online portálu Microsoft Premier][Microsoft Premier online portal].  Další informace o různých plánech podpory, včetně rozsahu, doby odezvy, cen atd. najdete v tématu věnovaném [plánům podpory Azure][Azure support plan].  Nejčastější dotazy týkající se podpory Azure najdete v [nejčastějších dotazech týkajících se podpory k Azure][Azure support FAQs].  
     
     ![Plán podpory](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Vyberte **typ problému** a **kategorii**. V tomto příkladu jsme vybrali typ problému „Nástroje“ a kategorii „Klientské nástroje“. 
   
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
Kromě toho se můžete spojit s komunitou SQL Data Warehouse na [Stack Overflow][Stack Overflow] nebo na [fóru pro Azure SQL Data Warehouse na webu MSDN][Azure SQL Data Warehouse MSDN forum].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/


