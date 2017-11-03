---
title: "Ukázka skriptu rozhraní příkazového řádku Azure - Správa fondů ve službě Batch | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure - Správa fondů ve službě Batch"
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
ms.openlocfilehash: ae7eab97c1da1113b0248b74a9dd67de8ce49e36
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a>Správa fondech Azure Batch pomocí rozhraní příkazového řádku Azure

Tyto skriptu ukazuje některé z nástrojů dostupných v rozhraní příkazového řádku Azure k vytváření a správě fondů výpočetních uzlů ve službě Azure Batch.

> [!NOTE]
> Příkazy v této ukázce vytvořit virtuální počítače Azure. Spuštěných virtuálních počítačů bude nabíhat poplatky ke svému účtu. Chcete-li minimalizovat tyto poplatky, odstraňte virtuální počítače po dokončení spuštění ukázky. V tématu [vyčištění fondy](#clean-up-pools).

Fondy batch můžete nakonfigurovat ve dvou způsobů, buď konfigurací cloudové služby (jenom Windows) nebo konfigurace virtuálního počítače (Windows a Linux). Následující ukázkové skripty ukazují, jak vytvořit fondy s obě konfigurace.

## <a name="prerequisites"></a>Požadavky

- Instalace rozhraní příkazového řádku Azure pomocí pokynů uvedených v [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.
- Pokud jste již nemáte, vytvořte účet Batch. V tématu [vytvořit účet Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.
- Konfigurace aplikace pro spuštění ze spouštěcí úkol, pokud jste tak ještě neučinili. V tématu [přidání aplikací do Azure Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) ukázkový skript, který se vytvoří aplikace a odesílá balíček aplikace do Azure.

## <a name="pool-with-cloud-service-configuration-sample-script"></a>Fond se cloudové služby konfigurace ukázkový skript

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a>Fond se skript ukázka konfigurace virtuálního počítače

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a>Vyčištění fondy

Po spuštění výše uvedený ukázkový skript, spusťte následující příkaz k odstranění fondů.
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření a manipulace s fondy Batch.
Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Ověřování na základě účtu Batch.  |
| [souhrnný seznam aplikací batch az](https://docs.microsoft.com/cli/azure/batch/application/summary#az_batch_application_summary_list) | Zobrazí seznam dostupných aplikací na účtu Batch.  |
| [Vytvoření fondu služby batch az](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Vytvoření fondu virtuálních počítačů.  |
| [Sada fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | Aktualizujte vlastnosti fondu.  |
| [seznam uzlu. agent SKU fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#az_batch_pool_node_agent_skus_list) | Agent k dispozici uzel seznamu SKU a informace o obrázku.  |
| [Změna velikosti fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_resize) | Resize – počet běžících virtuálních počítačů zadaného fondu.  |
| [Zobrazit fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Zobrazí vlastnosti fondu.  |
| [Odstranění fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_delete) | Odstraní zadaný fond.  |
| [Povolit automatické škálování fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Povolit automatické škálování na fond a použít vzorec.  |
| [zakázat automatické škálování fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Zakážete automatické škálování ve fondu.  |
| [seznam uzlů az batch](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_list) | Zobrazí seznam všech výpočetním uzlu ve fondu zadaný.  |
| [restartování uzlu az batch](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_reboot) | Restartování zadaný výpočetním uzlu.  |
| [Odstranění uzlu az batch](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_delete) | Odstraňte uvedené uzly ze zadaného fondu.  |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu Batch rozhraní příkazového řádku najdete v [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).

