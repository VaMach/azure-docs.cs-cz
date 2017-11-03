---
title: "Rozhraní příkazového řádku Azure skriptu ukázka – přidat aplikaci ve službě Batch | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure skriptu ukázka – přidat aplikaci ve službě Batch"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: cbfe8ab565ecf7f298a9a6c0f0c8298c675f178c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a>Přidání aplikací do Azure Batch pomocí rozhraní příkazového řádku Azure

Tento skript ukazuje, jak nastavit aplikaci pro použití s fondu Azure Batch nebo úloh. Pokud chcete nastavit aplikaci, balíček vaší spustitelný soubor, společně s všechny závislosti, do souboru .zip. V tomto příkladu se nazývá souboru spustitelný soubor zip, Moje-aplikace-exe.zip'.

## <a name="prerequisites"></a>Požadavky

- Instalace rozhraní příkazového řádku Azure pomocí pokynů uvedených v [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.
- Pokud jste již nemáte, vytvořte účet Batch. V tématu [vytvořit účet Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a>Vyčištění aplikace

Po spuštění výše uvedený ukázkový skript, spusťte následující příkazy pro odebrání aplikace a všech jeho balíčků nahrané aplikace.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření aplikace a nahrajte balíček aplikace.
Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [vytvořit aplikaci služby batch az](https://docs.microsoft.com/cli/azure/batch/application#az_batch_application_create) | Vytvoří aplikace.  |
| [Sada aplikací batch az](https://docs.microsoft.com/cli/azure/batch/application#az_batch_application_set) | Aktualizace vlastností aplikace.  |
| [Vytvoření balíčku aplikace batch az](https://docs.microsoft.com/cli/azure/batch/application/package#az_batch_application_package_create) | Přidá balíček aplikace pro zadanou aplikaci.  |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu Batch rozhraní příkazového řádku najdete v [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).
