---
title: "Tom, jak přiřadit MSI přístup k prostředku Azure, pomocí rozhraní příkazového řádku Azure"
description: "Podrobné pokyny pro přiřazení MSI na jeden prostředek, přístup k jiný prostředek, pomocí rozhraní příkazového řádku Azure."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2017
ms.author: bryanla
ms.openlocfilehash: 15a7d43da7b5a700ae84a42d59a7f01f1711c5cd
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Přiřadit identita spravované služby (MSI) přístup k prostředku pomocí rozhraní příkazového řádku Azure

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Po konfiguraci prostředek služby Azure s MSI, můžete zajistit přístup MSI pro jiný prostředek, stejně jako libovolný zaregistrovaný objekt zabezpečení. Tento příklad ukazuje, jak poskytnout přístup MSI Azure virtuálního počítače k účtu úložiště Azure, pomocí rozhraní příkazového řádku Azure.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Pokud chcete spustit skript příklady rozhraní příkazového řádku, máte tři možnosti:

- Použití [prostředí cloudu Azure](../cloud-shell/overview.md) z portálu Azure (viz další část).
- Používání embedded prostředí cloudu Azure prostřednictvím "Zkuste ho" tlačítko, umístěné v pravém horním rohu každé blok kódu.
- [Nainstalujte nejnovější verzi 2.0 rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 nebo novější) Pokud byste radši chtěli použít místní konzoly rozhraní příkazového řádku. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Používat funkci RBAC přiřazení přístupu MSI pro jiný prostředek

Po povolení MSI na prostředek Azure [například virtuální počítač Azure](msi-qs-configure-cli-windows-vm.md): 

1. Pokud používáte Azure CLI v místní konzole, nejdřív přihlásit k Azure pomocí [az přihlášení](/cli/azure/#az_login). Používáte účet, který je přidružen k předplatnému Azure, pod kterou chcete nasadit virtuální počítač:

   ```azurecli-interactive
   az login
   ```

2. V tomto příkladu nabízíme virtuálního počítače Azure přístup k účtu úložiště. Nejprve používáme [seznam zdrojů az](/cli/azure/resource/#az_resource_list) získat objekt služby pro virtuální počítač s názvem "Můjvp", který byl vytvořen, když jsme povolené MSI ve virtuálním počítači:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```

3. Jakmile jsme ID objektu zabezpečení služby, používáme [vytvořit přiřazení role az](/cli/azure/role/assignment#az_role_assignment_create) Pokud chcete poskytnout virtuální počítač "Čtečky" přístup k účtu úložiště s názvem "myStorageAcct":

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Řešení potíží

Pokud soubor MSI pro prostředek nezobrazuje v seznamu dostupných identit, ověřte, aby byla správně povolená soubor MSI. V našem případě jsme se vrátit k virtuálnímu počítači Azure v [portál Azure](https://portal.azure.com) a:

- Podívejte se na stránce "Konfigurace" a zkontrolujte MSI, povoleno = "Yes".
- Podívejte se na stránce "Rozšíření" a zkontrolujte příponou MSI úspěšně nasazena.

Pokud je buď nesprávný, musíte znovu nasaďte MSI v prostředku znovu, nebo vyřešit potíže s selhání nasazení.

## <a name="related-content"></a>Související obsah

- Přehled MSI najdete v tématu [identita spravované služby přehled](msi-overview.md).
- Pokud chcete povolit MSI ve virtuálním počítači Azure, najdete v části [konfigurace Azure virtuálního počítače spravované služby Identity (MSI) pomocí rozhraní příkazového řádku Azure](msi-qs-configure-cli-windows-vm.md).

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.

