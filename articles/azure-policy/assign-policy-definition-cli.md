---
title: "Vytvoření přiřazení zásady k identifikaci nekompatibilní prostředky v prostředí Azure pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Pomocí prostředí PowerShell vytvořit přiřazení zásad Azure k identifikaci nekompatibilní prostředky."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 10/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 92b532691986e72eca68d9bc3033e20ff8ffef3b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Vytvoření přiřazení zásady k identifikaci nekompatibilní prostředky v prostředí Azure pomocí Azure CLI

Prvním krokem při pochopení dodržování předpisů v Azure je zároveň budete vědět, kde stát s vaší aktuální prostředky. Tento rychlý start vás provede procesem vytváření zásad přiřazení můžete identifikovat nekompatibilní prostředky se definice zásady – *vyžadují SQL Server verze 12.0*. Na konci tohoto procesu se úspěšně určili jste servery jsou různé verze v podstatě nevyhovující.

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tato příručka údaje, pomocí rozhraní příkazového řádku Azure k vytvoření přiřazení zásady k identifikaci nekompatibilní prostředky ve vašem prostředí Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).
 
## <a name="opt-in-to-azure-policy"></a>Vyjádřit výslovný souhlas Azure zásad

Azure zásad je teď dostupná ve verzi Preview omezené, je nutné zaregistrovat k požádat o přístup.

1. Přejděte do zásad Azure v https://aka.ms/getpolicy a vyberte **zaregistrovat** v levém podokně.

   ![Vyhledejte zásad](media/assign-policy-definition/sign-up.png)

2. Vyjádřit výslovný souhlas do zásad Azure tak, že vyberete odběry ve **předplatné** seznamu chcete pracovat. Potom vyberte **zaregistrovat**.

   ![Výslovný souhlas pomocí zásad Azure](media/assign-policy-definition/preview-opt-in.png)

   Může trvat několik dní, abychom mohli přijmout vaši žádost o registraci, na základě poptávky. Jakmile vaši žádost o získá přijatá, budete upozorněni e-mailem, můžete začít používat službu.

## <a name="create-a-policy-assignment"></a>Vytvoření přiřazení zásady

V tento rychlý start jsme vytvoří přiřazení zásady a přiřadit definici vyžadují 12.0 verze SQL serveru. Definice této zásady určuje prostředky, které nejsou v souladu s podmínkami, nastavte v definici zásady.

Postupujte podle těchto kroků můžete vytvořit nové přiřazení zásad.

Zobrazení všech definic zásad a najít definice zásady "Požadovat, SQL Server verze 12.0":

```azurecli
az policy definition list
```

Azure zásad se dodává s již vytvořené v definicích zásady můžete použít. Definice předdefinované zásady se zobrazí jako například:

- Vynutit značku a její hodnota
- Použít značku a její hodnota
- Vyžadovat verze serveru SQL 12.0

Potom zadejte následující informace a spusťte následující příkaz přiřadit definice zásady:

- Zobrazení **název** pro přiřazení zásad. V tomto případě použijeme *vyžadují SQL Server verze 12.0 přiřazení*.
- **Zásady** – to je definice zásady, na základě vypnout, který používáte k vytvoření přiřazení. V takovém případě je definice zásady – *vyžadují SQL Server verze 12.0*
- A **oboru** – obor Určuje, jaké prostředky nebo seskupení prostředků v získá vynucena přiřazení zásad. Může rozsahu z odběru do skupiny prostředků.

  Používat předplatné (nebo skupinu prostředků), jste již dříve zaregistrovali když jste se rozhodli do zásad Azure, v tomto příkladu používáme toto ID předplatného - **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** a název skupiny prostředků – **FabrikamOMS**. Ujistěte se, že jste do ID předplatného a název skupiny prostředků, které pracujete s změnit. 

Toto je, jak by měla vypadat příkaz:

```azurecli
az policy assignment create --name Require SQL Server version 12.0 Assignment --policy Require SQL Server version 12.0 --scope /subscriptions/ 
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
az policy assignment delete –name Require SQL Server version 12.0 Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start jste přiřadili definici zásady k identifikaci nekompatibilní prostředky v prostředí Azure.

Další informace o přiřazení zásady, a ujistěte se, že prostředky, můžete vytvořit v **budoucí** předpisům, pokračovat v kurzu pro:

> [!div class="nextstepaction"]
> [Vytváření a Správa zásad](./create-manage-policy.md)

