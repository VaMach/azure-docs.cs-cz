---
title: "Postup zálohování a obnovení serveru se ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Informace o zálohování a obnovení serveru se ve službě Azure Database pro databázi MySQL pomocí rozhraní příkazového řádku Azure."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 10/30/2017
ms.openlocfilehash: 6a63cccee5be89a1da8f9656c6e7d4c7422702ca
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-by-using-the-azure-cli"></a>Postup zálohování a obnovení serveru se ve službě Azure Database pro databázi MySQL pomocí rozhraní příkazového řádku Azure

Azure Database pro databázi MySQL použijte k obnovení databáze serveru do předchozího stavu, která zahrnuje ze 7 na 35 dnů.

## <a name="prerequisites"></a>Požadavky
Chcete-li provést tento postup průvodce, je třeba:
- [Azure databáze MySQL server a databáze](quickstart-create-mysql-server-database-using-azure-portal.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Je-li nainstalovat a používat rozhraní příkazového řádku Azure místně, tento postup Průvodce vyžaduje, že používáte Azure CLI verze 2.0 nebo novější. Chcete-li ověřit verzi příkazového řádku Azure CLI zadejte `az --version`. K instalaci nebo upgradu, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="backup-happens-automatically"></a>Zálohování se automaticky stane
Pokud používáte Azure Database pro databázi MySQL, služba databáze automaticky provede zálohování služby každých 5 minut. 

Pro úroveň Basic zálohy jsou k dispozici po dobu 7 dnů. Pro úroveň Standard zálohování jsou k dispozici pro 35 dní. Další informace najdete v tématu [Azure Database pro databázi MySQL cenové úrovně](concepts-service-tiers.md).

Toto automatické funkce zálohování můžete obnovit serveru a jeho databáze do stavu nebo bodu v čase.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Obnovení databáze do předchozího bodu v čase pomocí rozhraní příkazového řádku Azure
Použití Azure databáze pro databázi MySQL a obnovte server do předchozího bodu v čase. Obnovená data se zkopíruje na nový server a existující server je ponechán beze. Například pokud tabulka omylem, bude vynechána. v poledne dnes můžete obnovit na čas těsně před polednem. Potom můžete načíst chybějící tabulku a dat z obnovené kopie serveru. 

K obnovení serveru, použijte rozhraní příkazového řádku Azure [obnovení serveru mysql az](/cli/azure/mysql/server#restore) příkaz.

### <a name="run-the-restore-command"></a>Spusťte příkaz restore

K obnovení serveru, na příkazovém řádku Azure CLI, zadejte následující příkaz:

```azurecli-interactive
az mysql server restore --resource-group myResourceGroup --name myserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server myserver4demo
```

`az mysql server restore` Příkaz vyžaduje následující parametry:
| Nastavení | Navrhovaná hodnota | Popis  |
| --- | --- | --- |
| skupiny prostředků | myResourceGroup |  Skupinu prostředků, kde existuje na zdrojovém serveru.  |
| jméno | Obnovit myserver | Název nového serveru, který je vytvořen pomocí příkazu restore. |
| obnovení bodu v čase | 2017-04-13T13:59:00Z | Vyberte bod v čase k obnovení. Toto datum a čas musí být v období uchovávání záloh zdrojového serveru. Použijte formát ISO8601 data a času. Například můžete použít vlastní místní časové pásmo, jako například `2017-04-13T05:59:00-08:00`. Můžete také použít formát zulština UTC, například `2017-04-13T13:59:00Z`. |
| zdrojový server | myserver4demo | Název nebo ID obnovení ze zdrojového serveru. |

Při obnovení serveru do dřívějšího bodu v čase, se vytvoří nový server. Původní server a její databáze ze zadaného bodu v čase se zkopírují na nový server.

Umístění a cenovou úroveň hodnoty pro obnovené server zůstat stejné jako původní server. 

`az mysql server restore` Příkaz je synchronní. Po obnovení serveru, můžete ho znovu opakujte postup pro jiný bod v čase. 

Po dokončení procesu obnovení, vyhledejte na nový server a ověřte, že budou data obnovena podle očekávání.

## <a name="next-steps"></a>Další kroky
[Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
