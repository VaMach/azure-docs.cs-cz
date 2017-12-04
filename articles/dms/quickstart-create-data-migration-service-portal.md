---
title: "Vytvoření instance služby Azure Database Migration Service pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Použití webu Azure Portal k vytvoření instance služby Azure Database Migration Service"
services: database-migration
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/28/2017
ms.openlocfilehash: 7fc4f8521afa41f21cda6576459a0794bef9ad3b
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Vytvoření instance služby Azure Database Migration Service pomocí webu Azure Portal
V tomto rychlém startu použijete Azure Portal k vytvoření instance služby Azure Database Migration Service.  Po vytvoření služby ji můžete použít k migraci dat z místního SQL Serveru do databáze SQL Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Otevřete svůj webový prohlížeč a přejděte na [portál Microsoft Azure Portal](https://portal.azure.com/). Zadejte přihlašovací údaje pro přihlášení k portálu. Výchozím zobrazením je váš řídicí panel služby.

## <a name="register-the-resource-provider"></a>Registrace poskytovatele prostředků
Než vytvoříte první instanci služby Database Migration Service, zaregistrujte poskytovatele prostředků Microsoft.DataMigration.

1. Na webu Azure Portal vyberte **Všechny služby** a pak vyberte **Předplatná**.

2. Vyberte předplatné, ve kterém chcete vytvořit instanci služby Azure Database Migration Service, a pak vyberte **Poskytovatelé prostředků**.

3. Vyhledejte „migration“ a pak napravo od Microsoft.DataMigration vyberte **Zaregistrovat**.

![Registrace poskytovatele prostředků](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-an-instance-of-the-service"></a>Vytvoření instance služby
1. Kliknutím na **+ Vytvořit prostředek** vytvořte instanci služby Azure Database Migration Service, která je aktuálně ve verzi Preview.

2. Vyhledejte na marketplace „migration“, vyberte **Azure Database Migration Service** a pak na obrazovce **Azure Database Migration Service (Preview)** klikněte na **Vytvořit**.

3. Na obrazovce **Database Migration Service**: 

    - Zvolte zapamatovatelný a jedinečný **Název služby** pro identifikaci vaší instance služby Azure Database Migration Service.
    - Vyberte **Předplatné** Azure, ve kterém chcete instanci vytvořit.
    - Vytvořte novou **Síť** s jedinečným názvem.
    - Zvolte **Umístění**, které je nejblíže vašemu zdrojovému nebo cílovému serveru.
    - Jako **cenovou úroveň** vyberte Basic: 1 virtuální jádro.

    ![Vytvoření služby Migration Service](media/quickstart-create-data-migration-service-portal/dms-create-service.png)
4. Vyberte **Vytvořit**.

Po chvíli bude vaše instance služby Azure Database Migration Service vytvořená a připravená k použití. Služba Database Migration Service bude vypadat jako na následujícím obrázku:

![Vytvořená služba Migration Service](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků
Všechny prostředky vytvořené v rámci tohoto rychlého startu můžete vyčistit odstraněním [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md).  Pokud chcete odstranit skupinu prostředků, přejděte do instance služby Azure Database Migration Service, kterou jste vytvořili. Vyberte název **skupiny prostředků** a pak vyberte **Odstranit skupinu prostředků**.  Tato akce odstraní všechny prostředky ve skupině prostředků i samotnou skupinu.

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace místního SQL Serveru do služby Azure SQL Database](tutorial-sql-server-to-azure-sql.md)