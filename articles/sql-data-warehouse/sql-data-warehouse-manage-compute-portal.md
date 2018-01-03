---
title: "Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure) | Microsoft Docs"
description: "Azure úkoly portálu pro správu výpočetního výkonu. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení výpočetní prostředky, abyste ušetřili náklady."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 63888d5dd103b585cf18e4787d3e779810163e3d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure)
> [!div class="op_single_selector"]
> * [Přehled](sql-data-warehouse-manage-compute-overview.md)
> * [Azure Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a>Škálování výpočetní výkon
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Chcete-li změnit výpočetní prostředky:

1. Otevřete [portál Azure][Azure portal], otevřete databázi a klikněte na tlačítko **škálování**.

    ![Klikněte na škálování][1]
2. V okně škálování nastavte posuvník doleva nebo doprava, chcete-li změnit nastavení DWU.

    ![Posuvník][2]
3. Klikněte na **Uložit**. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** potvrďte nebo **žádné** zrušit.

    ![Kliknutí na Uložit][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pozastavit výpočetní
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Chcete-li pozastavit databázi:

1. Otevřete [portál Azure] [ Azure portal] a otevřete vaší databáze. Všimněte si, že je stav **Online**.

    ![Online stav][6]
2. Chcete-li pozastavit paměťovou a výpočetní prostředky, klikněte na tlačítko **pozastavení**, a pak se zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** potvrďte nebo **žádné** zrušit.

    ![Potvrďte pozastavení][7]
3. Během spouštění databáze SQL Data Warehouse, je stav **pozastaveno**.
4. Pokud je stav **pozastaveno**, se provádí operace pozastavení a jsou už se vám účtována Dwu.

    ![Pozastavení stav][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Obnovit výpočetní
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Chcete-li obnovit databázi:

1. Otevřete [portál Azure] [ Azure portal] a otevřete vaší databáze. Všimněte si, že je stav **pozastaveno**.

    ![Pozastavení databáze][4]
2. Obnovit databázi, klikněte na tlačítko **spustit**, a pak se zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** potvrďte nebo **žádné** zrušit.

    ![Potvrďte obnovení][5]
3. Během spouštění databáze SQL Data Warehouse, je stav "Obnovování".
4. Pokud je stav **online**, databáze je připravena.

    ![Online stav][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [přehled správy][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
