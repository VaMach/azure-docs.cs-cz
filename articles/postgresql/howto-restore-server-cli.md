---
title: "Postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Informace o zálohování a obnovení serveru v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: 0cfce63b1523f939dc2d706dba771e56ce9ccd6c
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a>Postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure

Databáze Azure pro PostgreSQL použijte k obnovení databáze serveru do předchozího stavu, která zahrnuje ze 7 na 35 dnů.

## <a name="prerequisites"></a>Požadavky
Chcete-li provést tento postup průvodce, je třeba:
- [Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Je-li nainstalovat a používat rozhraní příkazového řádku Azure místně, tento postup Průvodce vyžaduje, že používáte Azure CLI verze 2.0 nebo novější. Chcete-li ověřit verzi příkazového řádku Azure CLI zadejte `az --version`. K instalaci nebo upgradu, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="backup-happens-automatically"></a>Zálohování se automaticky stane
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
| obnovení bodu v čase | 2017-04-13T13:59:00Z | Vyberte bod v čase k obnovení. Toto datum a čas musí být v období uchovávání záloh zdrojového serveru. Použijte formát ISO8601 data a času. Například můžete použít vlastní místní časové pásmo, jako například `2017-04-13T05:59:00-08:00`. Můžete také použít formát zulština UTC, například `2017-04-13T13:59:00Z`. |
| zdrojový server | mypgserver 20170401 | Název nebo ID obnovení ze zdrojového serveru. |

Při obnovení serveru do dřívějšího bodu v čase, se vytvoří nový server. Původní server a její databáze ze zadaného bodu v čase se zkopírují na nový server.

Umístění a cenovou úroveň hodnoty pro obnovené server zůstat stejné jako původní server. 

`az postgres server restore` Příkaz je synchronní. Po obnovení serveru, můžete ho znovu opakujte postup pro jiný bod v čase. 

Po dokončení procesu obnovení, vyhledejte na nový server a ověřte, že budou data obnovena podle očekávání.

## <a name="next-steps"></a>Další kroky
[Knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
