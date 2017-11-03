---
title: "Jak obnovit na Server v Azure databáze pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak k obnovení serveru se ve službě Azure Database pro databázi MySQL pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 6c1c0f8a0c0e59661b70b787b551b8cfdb024cda
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Postup zálohování a obnovení serveru ve službě Azure Database pro databázi MySQL pomocí portálu Azure

## <a name="backup-happens-automatically"></a>Zálohování se automaticky stane
Při použití Azure databáze pro databázi MySQL, služba databáze automaticky provede zálohování služby každých pět minut. 

Zálohy jsou k dispozici po dobu 7 dnů, při použití úroveň Basic a 35 dní při použití úrovně Standard. Další informace najdete v tématu [Azure databáze MySQL úrovně služeb](concepts-service-tiers.md)

Pomocí této funkce automatického zálohování můžete obnovit na server a všechny jeho databáze do nového serveru starší v daném okamžiku.

## <a name="restore-in-the-azure-portal"></a>Obnovit na portálu Azure
Databáze pro databázi MySQL Azure umožňuje obnovení serveru k určitému bodu v čase a do k o novou kopii tohoto serveru. Pokud chcete obnovit svá data, můžete použít tento nový server. 

Například pokud tabulka byla vynechána neúmyslně v poledne dnes, můžete obnovit na čas těsně před poledne a načíst chybějící tabulku a dat z novou kopii tohoto serveru.

Následující postup obnovení serveru ukázka k určitému bodu v čase:

1. Přihlaste se k [portálu Azure](https://portal.azure.com/)

2. Vyhledejte vaší databázi Azure pro server databáze MySQL. V levém podokně vyberte **všechny prostředky**a potom vyberte svůj server ze seznamu.

3.  Nahoře v okně Přehled serveru, klikněte na **obnovení** na panelu nástrojů. Otevře se okno obnovení.
![Klikněte na tlačítko Obnovit](./media/howto-restore-server-portal/click-restore-button.png)

4. Vyplňte formulář obnovení potřebné informace:

- **Obnovení bodu (UTC)**: pomocí výběr data a času pro výběr, vyberte bod v čase k obnovení. Zadaný čas je ve formátu UTC, takže je pravděpodobně potřeba převést na místní čas UTC.
- **Obnovit na nový server**: Zadejte nový název serveru a obnovte existující server do.
- **Umístění**: volba oblast automaticky naplní s oblasti zdrojového serveru, a nelze ji změnit.
- **Cenová úroveň**: cenová úroveň volba automaticky naplní s stejné cenovou úroveň, jako má zdrojový server, a nelze je zde změnit. 
![Možnosti PITR obnovení](./media/howto-restore-server-portal/pitr-restore.png)

5. Klikněte na tlačítko **OK** k obnovení serveru pro zadaný bod v čase. 

6. Po dokončení obnovení vyhledejte nový server, který byl vytvořen a potom ověřte, že databáze byly obnoveny podle očekávání.

## <a name="next-steps"></a>Další kroky
- [Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md).