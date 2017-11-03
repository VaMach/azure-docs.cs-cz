---
title: "Azure ukázka skriptu rozhraní příkazového řádku – spuštění úlohy pomocí služby Batch | Microsoft Docs"
description: "Azure ukázka skriptu rozhraní příkazového řádku – spuštění úlohy pomocí služby Batch"
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
ms.openlocfilehash: 73d93622d418359be421e043d0af4e4befc6f4b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>Spuštění úlohy v Azure Batch pomocí rozhraní příkazového řádku Azure

Tento skript vytvoří dávkovou úlohu a přidá řadu úkolů do úlohy. Také ukazuje, jak sledovat úlohy a její úkoly. Nakonec ukazuje, jak zadat dotaz na službu Batch efektivně pro informace o úlohách úlohy.

## <a name="prerequisites"></a>Požadavky

- Instalace rozhraní příkazového řádku Azure pomocí pokynů uvedených v [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.
- Pokud jste již nemáte, vytvořte účet Batch. V tématu [vytvořit účet Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.
- Konfigurace aplikace pro spuštění ze spouštěcí úkol, pokud jste tak ještě neučinili. V tématu [přidání aplikací do Azure Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) ukázkový skript, který se vytvoří aplikace a odesílá balíček aplikace do Azure.
- Nakonfigurujte fond, na kterém má být úloha spuštěna. V tématu [fondech Správa Azure Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) ukázkový skript, který vytvoří fond s konfigurace cloudové služby nebo konfigurace virtuálního počítače.

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a>Vyčištění úlohy

Po spuštění výše uvedený ukázkový skript, spusťte následující příkaz pro odebrání úlohy a všechny její úkoly. Všimněte si, že bude třeba fondu odstranit samostatně. V tématu [fondech Správa Azure Batch pomocí rozhraní příkazového řádku Azure](./batch-cli-sample-manage-pool.md) Další informace o vytváření a odstraňování fondy.

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření úlohy Batch a úloh. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Ověřování na základě účtu Batch.  |
| [vytvoření úlohy batch az](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_create) | Vytvoří dávkovou úlohu.  |
| [sady úloh az batch](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_set) | Aktualizace vlastnosti dávkovou úlohu.  |
| [Zobrazit úlohy batch az](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_show) | Načte podrobnosti o zadaný dávkovou úlohu.  |
| [vytvoření úlohy batch az](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_create) | Přidá úlohu do zadané úlohy Batch.  |
| [Zobrazit úlohy batch az](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_show) | Načte podrobnosti úlohy z zadaný dávkové úlohy.  |
| [Seznam úkolů az batch](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_list) | Obsahuje úkoly spojené s zadanou úlohu.  |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu Batch rozhraní příkazového řádku najdete v [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).
