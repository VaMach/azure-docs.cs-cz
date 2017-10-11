---
title: "Rozhraní příkazového řádku Azure Script – vytvoření Azure databáze pro databázi MySQL | Microsoft Docs"
description: "Tento ukázkový skript rozhraní příkazového řádku vytvoří databázi Azure pro server databáze MySQL a nakonfiguruje pravidla brány firewall na úrovni serveru."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: 201db294ce362ef3e09cbe62f48bd51c8ea94dbb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Vytvoření databáze MySQL serveru a nakonfigurujte pravidlo brány firewall pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku vytvoří databázi Azure pro server databáze MySQL a nakonfiguruje pravidla brány firewall na úrovni serveru. Jakmile se skript spustí úspěšně, je přístupné všech služeb Azure a nakonfigurovanou IP adresu serveru MySQL.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
V tento ukázkový skript upravte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo.
[!code-azurecli-interactive[hlavní](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "vytvoření Azure databáze MySQL a pravidlo brány firewall na úrovni serveru.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[hlavní](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Odstraňte skupinu prostředků.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [server databáze mysql az vytvořit](/cli/azure/mysql/server#create) | Vytvoří MySQL serveru, který je hostitelem databáze. |
| [Vytvoření brány firewall serveru az mysql](/cli/azure/mysql/server/firewall-rule#create) | Vytvoří pravidlo brány firewall pro povolení přístupu k serveru a databází v něm ze zadané rozsahu IP adres. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)
