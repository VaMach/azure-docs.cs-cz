---
title: "Rychlý úvod: Pozastavení a obnovení výpočetní v Azure SQL Data Warehouse - portálu Azure | Microsoft Docs"
description: "Azure portálu úlohy, které pozastavit výpočetní pro Azure SQL Data Warehouse abyste ušetřili náklady. Když budete chtít použít datový sklad, obnovit výpočty."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/23/2018
ms.author: barbkess
ms.openlocfilehash: f8b4e29595c6a71696cf2c4939ad4c6c096a28b5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Rychlý úvod: Pozastavení a obnovení výpočetní pro Azure SQL Data Warehouse na portálu Azure
Pozastavit výpočetní pro Azure SQL Data Warehouse abyste ušetřili náklady. Když budete chtít použít datový sklad, obnovit výpočty.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Než začnete

Použití [vytvořit a připojit - portál](create-data-warehouse-portal.md) k vytvoření datového skladu názvem **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>Pozastavit výpočetní
Abyste ušetřili náklady, můžete pozastavit a obnovit výpočetní prostředky na vyžádání. Například pokud nebudete používat databázi v noci a o víkendech, můžete pozastavit tyto v době a obnovit během dne. Zatímco databáze byla pozastavena, se nezapočítávají za výpočetní prostředky. Můžete však bude účtovat poplatek za úložiště. 

Postupujte podle těchto kroků se pozastavit SQL data warehouse.

1. Klikněte na tlačítko **databází SQL** v levé straně na portálu Azure.
2. Vyberte **mySampleDataWarehouse** z **databází SQL** stránky. Otevře se datového skladu. 
3. Na **mySampleDataWarehouse** stránky, Všimněte si **stav** je **Online**.

    ![Výpočetní online](media/pause-and-resume-compute-portal/compute-online.png)

4. Chcete-li pozastavit datového skladu, klikněte na tlačítko **pozastavit** tlačítko. 
5. Zobrazí se dotaz potvrzení dotazem, pokud chcete pokračovat. Klikněte na **Ano**.
6. Chvíli počkejte a pak Všimněte si **stav** je **pozastaveno**.

    ![Pozastavování](media/pause-and-resume-compute-portal/pausing.png)

7. Po dokončení operace pozastavení stav je **pozastaveno** a je tlačítko možnost **spustit**.
8. Výpočetní prostředky pro datový sklad jsou nyní v režimu offline. Dokud obnovit službu se nezapočítávají pro výpočet.

    ![Výpočetní do offline režimu](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Obnovit výpočetní
Následujícím postupem obnovit SQL data warehouse.

1. Klikněte na tlačítko **databází SQL** v levé straně na portálu Azure.
2. Vyberte **mySampleDataWarehouse** z **databází SQL** stránky. Otevře se datového skladu. 
3. Na **mySampleDataWarehouse** stránky, Všimněte si, **stav** je **pozastaveno**.

    ![Výpočetní do offline režimu](media/pause-and-resume-compute-portal/compute-offline.png)

4. Chcete-li obnovit datového skladu, klikněte na tlačítko **spustit**. 
5. Zobrazí se dotaz potvrzení dotazem, pokud chcete spustit. Klikněte na **Ano**.
6. Upozornění **stav** je **obnovování**.

    ![Obnovení](media/pause-and-resume-compute-portal/resuming.png)

7. Když datový sklad je zpět do režimu online, je ve stavu **Online** a je tlačítko možnost **pozastavení**.
8. Výpočetní prostředky pro datový sklad jsou teď online a můžete použít službu. Poplatky za výpočetní mít byl obnoven.

    ![Výpočetní online](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Vám budou účtovány jednotky datového skladu a data uložená v datovém skladu. Výpočetní prostředky a prostředky úložiště se účtují odděleně. 

- Pokud chcete zachovat data v úložišti, pozastavte výpočetní.
- Pokud chcete zamezit budoucím poplatkům, můžete datový sklad odstranit. 

Pomocí tohoto postupu podle potřeby vyčistěte prostředky.

1. Přihlaste se k [portál Azure](https://portal.azure.com)a klikněte na váš datový sklad.

    ![Vyčištění prostředků](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Pokud chcete pozastavit výpočetní prostředky, klikněte na tlačítko **Pozastavit**. Když je datový sklad pozastavený, zobrazí se tlačítko **Spustit**.  Pokud chcete obnovit výpočetní prostředky, klikněte na **Spustit**.

2. Pokud chcete odebrat datový sklad, aby se vám neúčtovaly výpočetní prostředky ani prostředky úložiště, klikněte na **Odstranit**.

3. Chcete-li odebrat serveru SQL, který jste vytvořili, klikněte na tlačítko **mynewserver 20171113.database.windows.net**a potom klikněte na **odstranit**.  S tímto odstraněním buďte opatrní, protože odstraněním serveru se odstraní také všechny databáze k tomuto serveru přiřazené.

4. Pokud chcete odebrat skupinu prostředků, klikněte na **myResourceGroup** a pak klikněte na **Odstranit skupinu prostředků**.


## <a name="next-steps"></a>Další postup
Nyní máte pozastavený a obnovený výpočetní pro datový sklad. Další informace o službě Azure SQL Data Warehouse najdete v kurzu načítání dat.

> [!div class="nextstepaction"]
>[Načtení dat do datového skladu SQL](load-data-from-azure-blob-storage-using-polybase.md)
