---
title: "Vytvářet a spravovat virtuální počítače v DevTest Labs pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se používat Azure DevTest Labs k vytváření a správě virtuálních počítačů pomocí Azure CLI 2.0"
services: devtest-lab,virtual-machines
documentationcenter: na
author: lisawong19
manager: douge
editor: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: liwong
ms.openlocfilehash: a3af12ba0598d60b55b3714ae1690fe3e5b54a42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Vytvářet a spravovat virtuální počítače s DevTest Labs pomocí rozhraní příkazového řádku Azure
Tento úvodní vám pomohou vytvoření, spuštění, připojením, aktualizaci a mazání vývojovém počítači ve svém testovacím prostředí. 

Než začnete:

* Pokud nebyl vytvořen testovacím prostředí, pokyny naleznete [zde](devtest-lab-create-lab.md).

* [Instalace rozhraní příkazového řádku 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Chcete-li spustit, spusťte az přihlášení k vytvoření připojení pomocí Azure. 

## <a name="create-and-verify-the-virtual-machine"></a>Vytvoření a ověření, jestli virtuální počítač 
Vytvoření virtuálního počítače z image pořízenou prostřednictvím marketplace s ssh ověřování.
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> Umístit **skupinu prostředků pro testovací prostředí** názvem v parametru – skupinu prostředků.
>

Pokud chcete vytvořit virtuální počítač pomocí vzorce, použijte--vzorce parametru v [az testovacího prostředí virtuálního počítače vytvořit](https://docs.microsoft.com/cli/azure/lab/vm#az_lab_vm_create).


Ověřte, že virtuální počítač je k dispozici.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-to-the-virtual-machine"></a>Spuštění a připojení k virtuálnímu počítači
Spuštění virtuálního počítače.
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> Umístit **skupinu prostředků pro testovací prostředí** názvem v parametru – skupinu prostředků.
>

Připojení k virtuálnímu počítači: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) nebo [vzdálené plochy](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>Aktualizovat virtuální počítač
Artefakty se vztahují na virtuální počítač.
```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

Zobrazí seznam artefaktů, které jsou k dispozici v testovacím prostředí.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a>Zastavení a odstranění virtuálního počítače    
Zastavení virtuálního počítače.
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Odstraňte virtuální počítač.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]