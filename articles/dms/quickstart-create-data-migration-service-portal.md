---
title: "Vytvoření instance služby migrace databáze Azure pomocí portálu Azure | Microsoft Docs"
description: "Pomocí portálu Azure k vytvoření instance služby Azure databáze migrace"
services: database-migration
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/17/2017
ms.openlocfilehash: 9faac0716334d627cdde4c0ef16262670333b5d4
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Vytvoření instance služby migrace databáze Azure pomocí portálu Azure
V této úvodní použití portálu Azure k vytvoření instance služby migrace databáze Azure.  Po vytvoření služby budete moci použít jej k migraci dat z místního serveru SQL do Azure SQL database.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Otevřete svůj webový prohlížeč a přejděte na [portál Microsoft Azure Portal](https://portal.azure.com/). Zadejte přihlašovací údaje pro přihlášení k portálu. Výchozím zobrazením je váš řídicí panel služby.

## <a name="register-the-resource-provider"></a>Registrace poskytovatele prostředků
Budete muset registrovat poskytovatele prostředků Microsoft.DataMigration před vytvořením první službě migrace databáze.

1. Na portálu Azure vyberte **všechny služby**a potom vyberte **odběry**.

1. Vyberte předplatné, ve kterém chcete vytvořit instanci služby Azure databáze migrace a potom vyberte **zprostředkovatelé prostředků**.

1. Vyhledejte migrace a potom vyberte napravo od Microsoft.DataMigration **zaregistrovat**.

![Registrace poskytovatele prostředků](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-azure-database-migration-service"></a>Vytvoření služby migrace databáze Azure
1. Klikněte na tlačítko  **+**  k vytvoření nové služby.  Služba migrace databáze je stále ve verzi preview.  

1. Vyhledávání na webu marketplace pro "migrace" Vyberte "Služba migrace databáze (preview)" a pak klikněte na tlačítko **vytvořit**.

    ![Vytvoření služby migrace](media/quickstart-create-data-migration-service-portal/dms-create-service.png)

    - Vyberte **název služby** se snadno zapamatovatelný a k identifikaci vaší Instance služby migrace databáze Azure jedinečný.
    - Vyberte vaši Azure **předplatné** ve kterém chcete vytvořit službu migrace databáze.
    - Vytvořte novou **sítě** s jedinečným názvem.
    - Vyberte **umístění** který je nejblíže k zdrojového nebo cílového serveru.
    - Vyberte Basic: 1 vCore pro **cenová úroveň**.

1. Klikněte na možnost **Vytvořit**.

Po chvíli se služby Azure databáze migrace bude vytvořená a připravená k použití.  Služba migrace databáze uvidíte, jak je znázorněno na obrázku.

![Vytvoření služby migrace](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků
Můžete vyčistit prostředky, které jste vytvořili v rychlé spuštění odstraněním [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md).  Pokud chcete odstranit skupinu prostředků, přejděte do databáze služby migrace jste vytvořili, klikněte na **skupiny prostředků** název a potom vyberte **odstranit skupinu prostředků**.  Tato akce odstraní všechny prostředky ve skupině prostředků a také samotné skupině.

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace SQL serveru místně do databáze Azure SQL](tutorial-sql-server-to-azure-sql.md)