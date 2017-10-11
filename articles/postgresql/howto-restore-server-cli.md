---
title: "Postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Zjistěte, jak zálohovat a obnovit server v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 871887e67d686a965a0648d2c6f0c72b3008db05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a>Postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure

Databáze Azure pro PostgreSQL použijte k obnovení databáze serveru do předchozího stavu, která zahrnuje ze 7 na 35 dnů.

## <a name="prerequisites"></a>Požadavky
Chcete-li provést tento postup průvodce, je třeba:
- [Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Je-li nainstalovat a používat rozhraní příkazového řádku Azure místně, tento postup Průvodce vyžaduje, že používáte Azure CLI verze 2.0 nebo novější. Chcete-li ověřit verzi příkazového řádku Azure CLI zadejte `az --version`. K instalaci nebo upgradu, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="back-up-happens-automatically"></a>Zálohování se stane automaticky
Pokud používáte Azure databáze pro PostgreSQL, služba databáze automaticky provede zálohování služby každých 5 minut. 

Pro úroveň Basic zálohy jsou k dispozici po dobu 7 dnů. Pro úroveň Standard zálohování jsou k dispozici pro 35 dní. Další informace najdete v tématu [databáze Azure pro PostgreSQL cenové úrovně](concepts-service-tiers.md).

Toto automatické funkce zálohování můžete obnovit serveru a jeho databáze do stavu nebo bodu v čase.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Obnovení databáze do předchozího bodu v čase pomocí rozhraní příkazového řádku Azure
Použijte databázi Azure pro PostgreSQL a obnovte server do předchozího bodu v čase. Obnovená data se zkopíruje na nový server a existující server je ponechán beze. Například pokud tabulka omylem, bude vynechána. v poledne dnes můžete obnovit na čas těsně před polednem. Potom můžete načíst chybějící tabulku a dat z obnovené kopie serveru. 

K obnovení serveru, použijte rozhraní příkazového řádku Azure [obnovení serveru postgres az](/cli/azure/postgres/server#restore) příkaz.

### <a name="run-the-restore-command"></a>Spusťte příkaz restore

K obnovení serveru, na příkazovém řádku Azure CLI, zadejte následující příkaz:

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

`az postgres server restore` Příkaz vyžaduje následující parametry:
| Nastavení | Navrhovaná hodnota | Popis  |
| --- | --- | --- |
| skupiny prostředků |  myResourceGroup |  Skupinu prostředků, kde existuje na zdrojovém serveru.  |
| jméno | Obnovit mypgserver | Název nového serveru, který je vytvořen pomocí příkazu restore. |
| obnovení bodu v čase | 2017-04-13T13:59:00Z | Vyberte bod v čase k obnovení. Toto datum a čas musí být v rámci zdrojového serveru na zálohování dobu uchování. Použijte formát ISO8601 data a času. Například můžete použít vlastní místní časové pásmo, jako například `2017-04-13T05:59:00-08:00`. Můžete také použít formát zulština UTC, například `2017-04-13T13:59:00Z`. |
| zdrojový server | mypgserver 20170401 | Název nebo ID obnovení ze zdrojového serveru. |

Při obnovení serveru do dřívějšího bodu v čase, se vytvoří nový server. Původní server a její databáze ze zadaného bodu v čase se zkopírují na nový server.

Umístění a cenovou úroveň hodnoty pro obnovené server zůstat stejné jako původní server. 

`az postgres server restore` Příkaz je synchronní. Po obnovení serveru, můžete ho znovu opakujte postup pro jiný bod v čase. 

Po dokončení procesu obnovení, vyhledejte na nový server a ověřte, že budou data obnovena podle očekávání.

## <a name="next-steps"></a>Další kroky
[Knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
