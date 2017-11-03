---
title: "Vytvoření databáze MySQL v Azure a připojení k ní"
description: "Další informace o použití portálu Azure k vytvoření databáze MySQL a potom k němu připojit z webové aplikace PHP v Azure."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: c072cb3a7d376d1e3c2b9f741f5410106e701256
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Vytvoření databáze MySQL v Azure a připojení k ní
V tomto kurzu se dozvíte, jak k vytvoření databáze MySQL v [portál Azure](https://portal.azure.com) (zprostředkovatel je [ClearDB](http://www.cleardb.com/)) a jak se připojit k němu z webové aplikace PHP spuštěné v [Azure App Service](app-service/app-service-web-overview.md).

> [!NOTE]
> Můžete také vytvořit databázi MySQL jako součást <a href="https://portal.azure.com/#create/WordPress.WordPress" target="_blank">šablony aplikace Marketplace</a>.
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Vytvoření databáze MySQL na portálu Azure
K vytvoření databáze MySQL na portálu Azure, postupujte takto:

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. V levé nabídce klikněte na tlačítko **nový** > **Data + úložiště** > **databáze MySQL**.

    ![Vytvoření databáze MySQL v Azure – spuštění](./media/store-php-create-mysql-database/create-db-1-start.png)
3. V nová databáze MySQL [okno](azure-portal-overview.md), nakonfigurujte novou databázi MySQL takto (*okno*: stránky portálu, které se otevře vodorovně):

   * **Název databáze**: Zadejte jedinečné osobní jméno
   * **Předplatné**: Zvolte předplatné použít
   * **Databáze typu**: vyberte **sdílené** pro nízkonákladové nebo volné vrstvy, nebo **Dedicated** získat vyhrazených prostředcích.
   * **Skupina prostředků**: přidejte databáze MySQL na stávající [skupiny prostředků](azure-resource-manager/resource-group-overview.md) nebo ji umístit do nové. Prostředky ve stejné skupině se dají snadno spravovat společně.
   * **Umístění**: Vyberte umístění blízko vás. Při přidávání do existující skupiny prostředků, že je uzamčené umístění skupiny prostředků.
   * **Cenová úroveň**: klikněte na tlačítko **cenová úroveň**, zvolte cenovou možnost (**Mercury** vrstvy je zdarma) a potom klikněte na **vyberte**.
   * **Právní podmínky**: klikněte na tlačítko **právní podmínky**, přečtěte si podrobnosti o nákupu a klikněte na tlačítko **nákupu**.
   * **Připnout na řídicí panel**: vyberte, pokud chcete získat přístup přímo z řídicího panelu. To je obzvláště užitečné, pokud se nevyznáte v portálu navigační ještě.

     Na následujícím snímku obrazovky je právě příklad konfiguraci databáze MySQL.  
     ![Vytvoření databáze MySQL v Azure – konfigurace](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Po dokončení konfigurace, klikněte na tlačítko **vytvořit**.

    ![Vytvoření databáze MySQL v Azure – vytvoření](./media/store-php-create-mysql-database/create-db-3-create.png)

    Zobrazí se automaticky otevírané okno notification informací, víte, že se zahájilo nasazení.

    ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Zobrazí se další automaticky otevírané okno po nasazení proběhla úspěšně. Na portálu také se automaticky otevře okna databáze MySQL.

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a>Připojení k vaší databázi MySQL
Pokud chcete zobrazit informace o připojení pro novou databázi MySQL, stačí kliknout na **vlastnosti** v okně vaší webové aplikace.

![Vytvoření databáze MySQL v Azure – okno databáze MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Teď můžete použít informace o tomto připojení v jakékoli webové aplikaci. Ukázka, která ukazuje, jak používat informace o připojení z jednoduchou aplikaci PHP je k dispozici [zde](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).
