---
title: "Rychlý start Azure – Zálohování virtuálního počítače pomocí Azure CLI | Dokumentace Microsoftu"
description: "Zjistěte, jak zálohovat virtuální počítače pomocí Azure CLI."
services: virtual-machines-linux, azure-backup
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: virtual-machines-linux, azure-backup
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/18/2017
ms.author: iainfou
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: b01916516d15ffee46f135e175ee4136f79acbe5
ms.contentlocale: cs-cz
ms.lasthandoff: 09/25/2017

---

# <a name="back-up-a-virtual-machine-in-azure-with-the-cli"></a>Zálohování virtuálního počítače v Azure pomocí rozhraní příkazového řádku
Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Svá data můžete chránit prováděním záloh v pravidelných intervalech. Azure Backup vytváří body obnovení, které je možné uchovávat v geograficky redundantních trezorech obnovení. Tento článek podrobně popisuje, jak zálohovat virtuální počítač v Azure pomocí Azure CLI. K provedení těchto kroků můžete také využít [Azure PowerShell](quick-backup-vm-powershell.md) nebo [Azure Portal](quick-backup-vm-portal.md).

V tomto rychlém startu se povolí zálohování na existujícím virtuálním počítači Azure. Pokud potřebujete vytvořit virtuální počítač, můžete [vytvořit virtuální počítač pomocí Azure CLI](../virtual-machines/linux/quick-create-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít Azure CLI verze 2.0.18 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli). 


## <a name="register-the-azure-backup-resource-provider"></a>Registrace poskytovatele prostředků Azure Backup
Při prvním použití služby Azure Backup musíte ve svém předplatném zaregistrovat poskytovatele služby Azure Recovery Services pomocí příkazu [az provider register](/cli/azure/provider?view=azure-cli-latest#az_provider_register).

```azurecli-interactive
az provider register --namespace Microsoft.RecoveryServices
```


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru služby Recovery Services
Trezor služby Recovery Services je logický kontejner, který uchovává zálohovaná data pro každý chráněný prostředek, například virtuální počítače Azure. Úloha zálohování pro chráněný prostředek při spuštění vytvoří uvnitř trezoru služby Recovery Services bod obnovení. Pomocí některého z těchto bodů obnovení pak můžete obnovit data k danému bodu v čase.

Vytvořte trezor služby Recovery Services pomocí příkazu **az backup vault create**. Zadejte stejnou skupinu prostředků a umístění, jako má virtuální počítač, který chcete chránit. Pokud jste k vytvoření virtuálního počítače použili [rychlý úvod k virtuálním počítačům](../virtual-machines/linux/quick-create-cli.md), skupina prostředků má název *myResourceGroup*, virtuální počítač má název *myVM* a prostředky jsou v umístění *eastus*.

```azurecli-interactive 
az backup vault create --resource-group myResourceGroup \
    --name myRecoveryServicesVault \
    --location eastus
```

Ve výchozím nastavení je trezor nastavený pro geograficky redundantní úložiště. Tato úroveň redundance úložiště vaše data chrání ještě více tím, že zajišťuje replikaci zálohovaných dat do sekundární oblasti Azure, která je od primární oblasti vzdálená stovky kilometrů.


## <a name="enable-backup-for-an-azure-vm"></a>Povolení zálohování pro virtuální počítač Azure
K definování, kdy se spouští úloha zálohování a jak dlouho se uchovávají body obnovení, vytváříte a používáte zásady. Výchozí zásada ochrany spouští úlohu zálohování každý den a uchovává body obnovení po dobu 30 dnů. Tyto výchozí hodnoty zásady můžete použít k rychlému zajištění ochrany vašeho virtuálního počítače. Pokud chcete pro virtuální počítač povolit ochranu zálohováním, použijte příkaz **az backup protection enable-for-vm**. Zadejte skupinu prostředků a virtuální počítač, který chcete chránit, a pak zásadu, která se má použít:

```azurecli-interactive 
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```


## <a name="start-a-backup-job"></a>Spuštění úlohy zálohování
Pokud chcete spustit zálohování ihned a nečekat na spuštění úlohy výchozí zásadou v naplánovaném čase, použijte příkaz **az backup protection backup-now**. Tato první úloha zálohování vytvoří úplný bod obnovení. Každá úloha zálohování po tomto prvotním zálohování vytváří přírůstkové body obnovení. Přírůstkové body obnovení jsou efektivní z hlediska úložiště a času, protože přenášejí pouze změny provedené od posledního zálohování.

Pro zálohování virtuálního počítače se používají následující parametry:

- `--container-name` je název vašeho virtuálního počítače.
- `--item-name` je název vašeho virtuálního počítače.
- Hodnota `--retain-until` by měla být nastavená na poslední datum požadované dostupnosti bodu obnovení ve formátu času UTC (**dd-mm-yyyy**).

Následující příklad zálohuje virtuální počítač *myVM* a nastaví vypršení platnosti bodu obnovení na 18. října 2017:

```azurecli-interactive 
az backup protection backup-now \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --retain-until 18-10-2017
```

Vzhledem k tomu, že první úloha zálohování vytváří úplný bod obnovení, může tento proces trvat až 20 minut.


## <a name="monitor-the-backup-job"></a>Monitorování úlohy zálohování
Pokud chcete monitorovat stav úloh zálohování, použijte příkaz **az backup job list**:

```azurecli-interactive 
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

Výstup bude vypadat podobně jako v následujícím příkladu, který ukazuje, že je úloha zálohování ve stavu *InProgress* (Probíhající):

```
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
a0a8e5e6  Backup           InProgress  myvm         2017-09-19T03:09:21  0:00:48.718366
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Když se *Status* (Stav) úlohy zálohovaní změní na *Completed* (Dokončeno), váš virtuální počítač je chráněný službou Recovery Services a má uložený úplný bod obnovení.


## <a name="clean-up-deployment"></a>Vyčištění nasazení
Pokud už ochrana virtuálního počítače není potřeba, můžete ji vypnout, odebrat body obnovení a trezor služby Recovery Services a pak odstranit skupinu prostředků a související prostředky virtuálního počítače. Pokud jste použili existující virtuální počítač, můžete vynechat poslední příkaz [az group delete](/cli/azure/group?view=azure-cli-latest#az_group_delete), aby se zachovala skupina prostředků a virtuální počítač.

Pokud se chystáte pokračovat ke kurzu služby Backup, který vysvětluje postup obnovení dat virtuálního počítače, přeskočte kroky v této části a přejděte na [Další kroky](#next-steps). 

```azurecli-interactive 
az backup protection disable \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --delete-backup-data true
az backup vault delete \
    --resource-group myResourceGroup \
    --name myRecoveryServicesVault \
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Další kroky
V tomto rychlém startu jste vytvořili trezor služby Recovery Services, povolili ochranu virtuálního počítače a vytvořili prvotní bod obnovení. Pokud se chcete o službách Azure Backup a Recovery Services dozvědět více, pokračujte ke kurzům.

> [!div class="nextstepaction"]
> [Zálohování několika virtuálních počítačů Azure](./tutorial-backup-vm-at-scale.md)

