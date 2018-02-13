---
title: "Rychlý úvod: Škálovat výpočetní v Azure SQL Data Warehouse - portálu Azure | Microsoft Docs"
description: "Azure úkoly portálu pro správu výpočetního výkonu. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení výpočetní prostředky, abyste ušetřili náklady."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: cbe2f2d17f309e01e831aa9ee31e01e044896d10
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Rychlý úvod: Škálovat výpočetní v Azure SQL Data Warehouse na portálu Azure

Škálování výpočetní kapacity v Azure SQL Data Warehouse na portálu Azure. Škálování výpočetní pro lepší výkon, nebo určený počet číslic zpět výpočetní abyste ušetřili náklady. 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Než začnete

Je možné škálovat datový sklad, který už máte, nebo můžete použít [rychlý start: Vytvořte a připojte - portál](create-data-warehouse-portal.md) k vytvoření datového skladu s názvem **mySampleDataWarehouse**.  Tento rychlý start škáluje **mySampleDataWarehouse**.

## <a name="scale-compute"></a>Škálování výpočetní kapacity

V SQL Data Warehouse můžete zvýšit nebo snížit výpočetní prostředky úpravou jednotky datového skladu. [Vytvořit a připojit - portál](create-data-warehouse-portal.md) vytvořit **mySampleDataWarehouse** a inicializovat se 400 Dwu. Následující kroky upravit Dwu pro **mySampleDataWarehouse**.

Chcete-li změnit jednotky datového skladu:

1. Klikněte na tlačítko **databází SQL** v levé straně na portálu Azure.
2. Vyberte **mySampleDataWarehouse** z **databází SQL** stránky. Otevře se datového skladu.
3. Klikněte na tlačítko **škálování**.

    ![Klikněte na škálování](media/quickstart-scale-compute-portal/click-scale.png)

2. V panelu škálování nastavte posuvník doleva nebo doprava, chcete-li změnit nastavení DWU.

    ![Posuvník](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Klikněte na **Uložit**. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** potvrďte nebo **žádné** zrušit.

    ![Kliknutí na Uložit](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Další postup
Jste se naučili nyní škálování výpočetní pro datový sklad. Další informace o službě Azure SQL Data Warehouse najdete v kurzu načítání dat.

> [!div class="nextstepaction"]
>[Načtení dat do datového skladu SQL](load-data-from-azure-blob-storage-using-polybase.md)
