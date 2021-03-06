---
title: "Jak obnovit na Server v Azure databáze pro databázi MySQL"
description: "Tento článek popisuje, jak k obnovení serveru se ve službě Azure Database pro databázi MySQL pomocí portálu Azure."
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 5bef3f11d0b546fbd6b1161b20d7dfb81e975f99
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a>Postup zálohování a obnovení serveru ve službě Azure Database pro databázi MySQL pomocí portálu Azure

## <a name="backup-happens-automatically"></a>Zálohování se automaticky stane
Azure databáze MySQL servery jsou pravidelně zálohovány k povolení funkce obnovení. Pomocí této funkce můžete obnovit na server a všechny jeho databáze do starší bodu v čase, na nový server.

## <a name="prerequisites"></a>Požadavky
Chcete-li provést tento postup průvodce, je třeba:
- [Azure databáze MySQL server a databáze](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Konfigurace zálohování sady

Zkontrolujte výběr mezi konfigurace vašeho serveru pro místně redundantní zálohy nebo geograficky redundantní zálohy při vytváření serveru v **cenová úroveň** okno.

> [!NOTE]
> Po vytvoření serveru druh redundance, kterou má, nelze přepnout místně redundantní geograficky redundantní vs.
>

Při vytváření serveru přes portál Azure, **cenová úroveň** je okno, kde můžete vybrat buď **místně redundantní** nebo **geograficky redundantní** zálohy pro váš server. Toto okno slouží také k výběru **dobu uchování zálohování** – jak dlouho (ve dnech) chcete uložených pro záloh serveru.

   ![Cenová úroveň - vyberte zálohování redundance](./media/howto-restore-server-portal/pricing-tier.png)

Další informace o nastavení tyto hodnoty během vytváření najdete v tématu [databáze Azure pro rychlý start serveru MySQL](quickstart-create-mysql-server-database-using-azure-portal.md).

Doba uchovávání záloh může být změněn na serveru pomocí následujících kroků:
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. Vyberte svou databázi Azure pro server databáze MySQL. Tato akce otevře **přehled** stránky.
3. Vyberte **cenová úroveň** v nabídce v části **nastavení**. Pomocí posuvníku můžete změnit **dobu uchování zálohování** na vaši volbu mezi 7 až 35 dnů.
Na tomto snímku obrazovky má bylo zvýšeno na 34 dnů.
![Doba uchovávání záloh vyšší](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Klikněte na tlačítko **OK** pro potvrzení změny.

Doba uchovávání záloh řídí jak daleko zpět v době obnovení bodu v čase se dá načíst, protože je založena na zálohování, které jsou k dispozici. Obnovení bodu v čase je popsána dále v následující části. 

## <a name="point-in-time-restore-in-the-azure-portal"></a>Obnovení bodu v čase na portálu Azure
Databáze pro databázi MySQL Azure umožňuje obnovení serveru zpět bodu v čase a do k o novou kopii tohoto serveru. Můžete použít tento nový server Pokud chcete obnovit svá data, nebo mají vaší klientské aplikace, přejděte na tento nový server.

Například pokud tabulka byla omylem vyřadit v poledne v současné době můžete obnovit na čas těsně před poledne a načíst chybějící tabulku a data z této novou kopii tohoto serveru. Obnovení bodu v čase není na serveru úrovni, na úrovni databáze.

Následující kroky obnovit ukázkový server bodu v čase:
1. Na portálu Azure vyberte svou databázi Azure pro server databáze MySQL. 

2. Na panelu nástrojů serveru **přehled** vyberte **obnovení**.

   ![Azure databáze pro obnovení databáze MySQL – přehled – tlačítko](./media/howto-restore-server-portal/2-server.png)

3. Vyplňte formulář obnovení potřebné informace:

   ![Azure databáze pro databázi MySQL - informace o obnovení ](./media/howto-restore-server-portal/3-restore.png)
  - **Bod obnovení**: vyberte v daném okamžiku chcete obnovit.
  - **Cílový server**: Zadejte název pro nový server.
  - **Umístění**: nelze vybrat oblast. Ve výchozím nastavení je stejný jako zdrojový server.
  - **Cenová úroveň**: při provádění obnovení bodu v čase nelze změnit tyto parametry. Je stejná jako u zdrojového serveru. 

4. Klikněte na tlačítko **OK** k obnovení serveru k obnovení v daném okamžiku. 

5. Po dokončení obnovení vyhledejte nový server, který je vytvořen k ověření, že data byla obnovena podle očekávání.

>[!Note]
>Poznámka: na nový server vytvořené v okamžiku obnovení má stejné přihlašovací jméno správce serveru a zvolili hesla, která byla platná pro existující server v bodu v čase. Heslo můžete změnit na nový server **přehled** stránky.

## <a name="next-steps"></a>Další postup
- Další informace o službě service [zálohování](concepts-backup.md).
- Další informace o [kontinuity podnikových procesů](concepts-business-continuity.md) možnosti.
