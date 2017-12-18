---
title: "Použití Azure CLI k vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure | Dokumentace Microsoftu"
description: "Použijte Azure CLI k vytvoření přiřazení zásady Azure Policy pro identifikaci prostředků, které nedodržují předpisy."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 12/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 88ceb47d46b66e716c6c263098d5b9458e4aff22
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure pomocí Azure CLI

Prvním krokem k porozumění dodržování předpisů v Azure je vědět, jak jste na tom s vašimi stávajícími prostředky. Tento rychlý start vás provede procesem vytvoření přiřazení zásady pro identifikaci virtuálních počítačů, které nepoužívají spravované disky.

Na konci tohoto procesu úspěšně identifikujete virtuální počítače, které nepoužívají spravované disky, a proto *nedodržují předpisy*.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-policy-assignment"></a>Vytvoření přiřazení zásady

V tomto rychlém startu vytvoříme přiřazení zásady a přiřadíme definici Audit virtuálních počítačů bez spravovaných disků. Tato definice zásady identifikuje prostředky, které nesplňují podmínky nastavené v definici zásady.

Podle těchto pokynů vytvořte nové přiřazení zásady.

Zobrazte všechny definice zásad a vyhledejte definici zásady Audit virtuálních počítačů bez spravovaných disků:

```azurecli
az policy definition list
```

Služba Azure Policy obsahuje již integrované definice zásad, které můžete použít. Zobrazí se například následující integrované definice zásad:

- Vynucovat použití značky a její hodnoty
- Použít značku a její hodnotu
- Vyžadovat SQL Server verze 12.0

Dále zadejte následující informace a spuštěním následujícího příkazu přiřaďte definici zásady:

- **Name** – Zobrazovaný název přiřazení zásady. V tomto případě použijeme *Audit virtuálních počítačů bez spravovaných disků*.
- **Policy** – Toto je definice zásady založená na té, kterou používáte k vytvoření tohoto přiřazení. V tomto případě je to definice zásady *Audit virtuálních počítačů bez spravovaných disků*.
- **Scope** – Obor určuje, pro které prostředky nebo skupiny prostředků se toto přiřazení zásady bude vynucovat. Může sahat od předplatného až po skupinu prostředků.

  Použijte předplatné (nebo skupinu prostředků), které jste dříve zaregistrovali. V tomto příkladu používáme ID předplatného **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** a název skupiny prostředků **FabrikamOMS**. Nezapomeňte tyto hodnoty změnit na ID předplatného a název skupiny prostředků, se kterými pracujete.

Příkaz by měl vypadat nějak takto:

```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

Přiřazení zásady je zásada, která byla přiřazena, aby proběhla v rámci zadaného oboru. Tento obor může také sahat od skupiny pro správu až po skupinu prostředků.

## <a name="identify-non-compliant-resources"></a>Identifikace prostředků, které nedodržují předpisy

Zobrazení prostředků, které nedodržují předpisy v rámci tohoto nového přiřazení:

1. Přejděte zpět na stránku služby Azure Policy.
2. V levém podokně vyberte **Dodržování předpisů** a vyhledejte **Přiřazení zásady**, které jste vytvořili.

   ![Dodržování zásad](media/assign-policy-definition/policy-compliance.png)

   Pokud nějaké stávající prostředky nedodržují předpisy tohoto nového přiřazení, zobrazí se na kartě **Neodpovídající prostředky**, jak je znázorněno výše.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další příručky v této kolekci vycházejí z tohoto rychlého startu. Pokud chcete pokračovat v práci s dalšími kurzy, neprovádějte čištění prostředků vytvořených v rámci tohoto rychlého startu. Pokud pokračovat nechcete, spuštěním následujícího příkazu odstraňte přiřazení, která jste vytvořili:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste přiřadili definici zásady pro identifikaci prostředků, které nedodržují předpisy, ve vašem prostředí Azure.

Další informace o přiřazování zásad pro zajištění, aby prostředky vytvořené **v budoucnu** dodržovaly předpisy, najdete v tomto kurzu:

> [!div class="nextstepaction"]
> [Vytváření a správa zásad](./create-manage-policy.md)
