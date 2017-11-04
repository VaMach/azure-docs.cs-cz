---
title: "Vytvoření přiřazení zásady k identifikaci nekompatibilní prostředky v prostředí Azure pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Pomocí prostředí PowerShell vytvořit přiřazení zásad Azure k identifikaci nekompatibilní prostředky."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 764554a6afcc7912c53fc5000a6af44abb2adc99
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Vytvoření přiřazení zásady k identifikaci nekompatibilní prostředky v prostředí Azure pomocí Azure CLI

Prvním krokem při pochopení dodržování předpisů v Azure je zároveň budete vědět, kde stát s aktuální prostředky. Tento rychlý start vás provede procesem vytvoření přiřazení zásady můžete identifikovat virtuální počítače, které nepoužívají spravované disky.

Na konci tohoto procesu se úspěšně zjistilo, jaké virtuální počítače nejsou pomocí spravovaných disků a jsou tedy *nevyhovující*.
.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="opt-in-to-azure-policy"></a>Vyjádřit výslovný souhlas Azure zásad

Azure zásad je nyní k dispozici ve verzi Public Preview a je nutné zaregistrovat k požádat o přístup.

1. Přejděte do zásad Azure v https://aka.ms/getpolicy a vyberte **zaregistrovat** v levém podokně.

   ![Vyhledejte zásad](media/assign-policy-definition/sign-up.png)

2. Vyjádřit výslovný souhlas do zásad Azure tak, že vyberete odběry ve **předplatné** seznamu chcete pracovat. Potom vyberte **zaregistrovat**.

   ![Výslovný souhlas pomocí zásad Azure](media/assign-policy-definition/preview-opt-in.png)

   Vaše žádost je automaticky schváleny pro verzi Preview. Může trvat až 30 minut pro systém ke zpracování registrace.

## <a name="create-a-policy-assignment"></a>Vytvoření přiřazení zásady

V tento rychlý start jsme vytvoří přiřazení zásady a přiřadit auditu virtuální počítače bez definice spravované disky. Definice této zásady určuje prostředky, které nejsou v souladu s podmínkami, nastavte v definici zásady.

Postupujte podle těchto kroků můžete vytvořit nové přiřazení zásad.

Zobrazení všech definic zásad a nalézt definici zásady "Auditu virtuálních počítačů bez spravovaných disků":

```azurecli
az policy definition list
```

Azure zásad se dodává s již vytvořené v definicích zásady můžete použít. Definice předdefinované zásady se zobrazí jako například:

- Vynutit značku a její hodnota
- Použít značku a její hodnota
- Vyžadovat verze serveru SQL 12.0

Potom zadejte následující informace a spusťte následující příkaz přiřadit definice zásady:

- Zobrazení **název** pro přiřazení zásad. V tomto případě použijeme *auditu virtuální počítače bez spravované disků*.
- **Zásady** – to je definice zásady, na základě vypnout, který používáte k vytvoření přiřazení. V takovém případě je definice zásady – *auditu virtuální počítače bez spravované disků*
- A **oboru** – obor Určuje, jaké prostředky nebo seskupení prostředků v získá vynucena přiřazení zásad. Může rozsahu z odběru do skupiny prostředků.

  Používat předplatné (nebo skupinu prostředků), jste již dříve zaregistrovali když jste se rozhodli do zásad Azure, v tomto příkladu používáme toto ID předplatného - **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** a název skupiny prostředků – **FabrikamOMS**. Ujistěte se, že jste do ID předplatného a název skupiny prostředků, které pracujete s změnit.

Toto je, jak by měla vypadat příkaz:

```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

Přiřazení zásady je zásada, která byla přiřazena proběhla v rámci konkrétní obor. Tento obor může také rozsahu ze skupiny pro správu do skupiny prostředků.

## <a name="identify-non-compliant-resources"></a>Určete nekompatibilní prostředky

Chcete-li zobrazit prostředky, které nesplňují předpisy v rámci této nové přiřazení:

1. Přejděte zpět na stránku zásad Azure.
2. Vyberte **dodržování předpisů** v levém podokně a Hledat **přiřazení zásady** jste vytvořili.

   ![Zásady dodržování předpisů](media/assign-policy-definition/policy-compliance.png)

   Pokud jsou všechny existující prostředky, které nejsou kompatibilní s toto nové přiřazení, se zobrazí na **nekompatibilní prostředky** kartě, jak je uvedeno výše.

## <a name="clean-up-resources"></a>Vyčištění prostředků

V této kolekci dalších příručkách stavět na tento rychlý start. Pokud budete chtít pokračovat v práci s další kurzy, neprovádí vyčištění prostředky vytvořené v tento rychlý start. Pokud neplánujete, chcete-li pokračovat, odstraňte přiřazení, které jste vytvořili spuštěním tohoto příkazu:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start jste přiřadili definici zásady k identifikaci nekompatibilní prostředky v prostředí Azure.

Další informace o přiřazení zásady, a ujistěte se, že prostředky, můžete vytvořit v **budoucí** předpisům, pokračovat v kurzu pro:

> [!div class="nextstepaction"]
> [Vytváření a Správa zásad](./create-manage-policy.md)
