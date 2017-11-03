---
title: "Azure CLI skriptu ukázkové – vytvoření účtu Batch | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření účtu Batch"
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
ms.openlocfilehash: fd2f4682a04c557b69bbfce115f41c54a96d462c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-batch-account-with-the-azure-cli"></a>Vytvoření účtu Batch pomocí rozhraní příkazového řádku Azure

Tento skript vytvoří účet Azure Batch a ukazuje, jak různé vlastnosti účtu může být dotaz a aktualizovat.

## <a name="prerequisites"></a>Požadavky

Instalace rozhraní příkazového řádku Azure pomocí pokynů uvedených v [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.

## <a name="batch-account-sample-script"></a>Ukázkový skript účtu batch

Při vytváření účtu Batch, ve výchozím nastavení jeho výpočetní uzly jsou přiřazeny interně službou Batch. Přidělené výpočetní uzly budou platit samostatné základní kvóta a účet může být ověřen prostřednictvím pověření sdílený klíč nebo Azure Active Dirctory tokenu.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a>Účet batch pomocí uživatele předplatné ukázkový skript

Můžete také zvolit Batch vytvořit jeho výpočetní uzly v rámci vlastní předplatného Azure.
Účty, které přidělit výpočetní uzly do předplatného musí být ověřeny pomocí tokenu Azure Active Directory a výpočetních uzlů přidělených se bude započítávat kvóty předplatného. Chcete-li vytvořit účet v tomto režimu, jeden musíte zadat odkaz Key Vault při vytváření účtu.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění některý z výše uvedené ukázkové skripty, pomocí následujícího příkazu odeberte skupinu zdrojů a všechny související prostředky (včetně účty Batch, účty Azure Storage a Azure trezorů klíčů).

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků účtu Batch a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_create) | Vytvoří účet Batch.  |
| [nastaven účet batch az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_set) | Aktualizace vlastnosti účtu Batch.  |
| [Zobrazit účet batch az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_show) | Načte podrobnosti zadaného účtu Batch.  |
| [seznam klíčů účtu batch az](https://docs.microsoft.com/cli/azure/batch/account/keys#az_batch_account_keys_list) | Načte přístupové klíče zadaného účtu Batch.  |
| [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Ověří proti zadaný účet Batch pro další interakce rozhraní příkazového řádku.  |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Vytvoří účet úložiště. |
| [Vytvoření az keyvault](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Vytvoří trezoru klíčů. |
| [AZ keyvault set zásad](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Aktualizujte zásady zabezpečení zadaný trezor klíčů. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu Batch rozhraní příkazového řádku najdete v [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).
