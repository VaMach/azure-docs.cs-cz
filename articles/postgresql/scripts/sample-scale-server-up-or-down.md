---
title: "Rozhraní příkazového řádku Azure skriptu škálování Azure databázi PostgreSQL | Microsoft Docs"
description: "Azure CLI ukázka skriptu - škálování Azure databázi PostgreSQL serveru do úroveň různých výkonu po dotazování metriky."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 11/03/2017
ms.openlocfilehash: 9fe5b56d64d325f855b6030ef3dcc0ed65ad7afc
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Sledování a škálování jediného PostgreSQL serveru pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku škáluje jedné databáze Azure pro server PostgreSQL na úroveň výkonu různých po dotazování metriky. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, v tomto článku vyžaduje, že používáte Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
Tento ukázkový skript změňte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo. Nahraďte SubscriptionID použít v příkazech monitorování az s vlastní identifikátor předplatného.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [AZ postgres server vytvořit](/cli/azure/postgres/server#create) | Vytvoří PostgreSQL serveru, který je hostitelem databáze. |
| [seznam metriky az monitorování](/cli/azure/monitor/metrics#list) | Zobrazí seznam hodnota metriky pro prostředky. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview)
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)
- Přečtěte si další informace o škálování: [úrovně služeb](../concepts-service-tiers.md) a [výpočetní jednotky a jednotky úložiště](../concepts-compute-unit-and-storage.md)
