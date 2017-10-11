---
title: "Jak obnovit na Server v Azure databáze pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak k obnovení serveru se v databázi Azure pro PostgreSQL pomocí portálu Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: 3fbdb7741481bd3620466c3489d3609f9ea6961f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL pomocí portálu Azure

## <a name="backup-happens-automatically"></a>Zálohování se automaticky stane
Při použití Azure databáze PostgreSQL, služba databáze automaticky provede zálohování služby každých 5 minut. 

Zálohy jsou k dispozici po dobu 7 dnů, při použití úroveň Basic a 35 dní při použití úrovně Standard. Další informace najdete v tématu [databáze Azure pro PostgreSQL úrovně služeb](concepts-service-tiers.md)

Pomocí této funkce automatického zálohování můžete obnovit na server a všechny jeho databáze do nového serveru starší v daném okamžiku.

## <a name="restore-in-the-azure-portal"></a>Obnovit na portálu Azure
Azure databázi PostgreSQL umožňuje obnovit server zpět k určitému bodu v čase a do k o novou kopii tohoto serveru. Pokud chcete obnovit svá data, můžete použít tento nový server. 

Například pokud tabulka byla omylem vyřadit v poledne v současné době můžete obnovit na čas těsně před poledne a načíst chybějící tabulku a data z této novou kopii tohoto serveru.

Následující kroky obnovit ukázkový server k určitému bodu v čase:
1. Přihlaste se k [portálu Azure](https://portal.azure.com/)
2. Vyhledejte vaší databázi Azure pro PostgreSQL server. Na portálu Azure klikněte na tlačítko **všechny prostředky** z levé nabídce a zadejte název, například **mypgserver 20170401**, k vyhledání existujícího serveru. Klikněte na název serveru uvedený ve výsledcích hledání. Otevře se stránka **Přehled** vašeho serveru a poskytne vám možnosti další konfigurace.

   ![Portál Azure – vyhledávání umístit si server](media/postgresql-howto-restore-server-portal/1-locate.png)

3. Nahoře v okně Přehled serveru, klikněte na **obnovení** na panelu nástrojů. Otevře se okno obnovení.

   ![Azure databáze pro obnovení PostgreSQL – přehled – tlačítko](./media/postgresql-howto-restore-server-portal/2_server.png)

4. Vyplňte formulář obnovení potřebné informace:

   ![Azure databázi PostgreSQL - informace o obnovení ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - **Bod obnovení**: Vyberte bodu v čase, k níž dojde před server byl změněn
  - **Cílový server**: Zadejte nový název serveru, kterou chcete obnovit
  - **Umístění**: nelze vyberte oblast, ve výchozím nastavení je stejné jako na zdrojovém serveru
  - **Cenová úroveň**: tuto hodnotu nelze změnit, při obnovení serveru. Je stejný jako zdrojový server. 

5. Klikněte na tlačítko **OK** k obnovení serveru pro obnovení do bodu v čase. 

6. Po dokončení obnovení vyhledejte nový server, který je vytvořen k ověření, že data byla obnovena podle očekávání.

## <a name="next-steps"></a>Další kroky
- [Knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
