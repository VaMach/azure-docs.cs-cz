---
title: "Vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure | Dokumentace Microsoftu"
description: "Tento článek vás provede postupem vytvoření definice zásady pro identifikaci prostředků, které nedodržují předpisy."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 12/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: b28e442a075e38a4fbe7b0d9d46f2c9d23e7c6fb
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure
Prvním krokem k porozumění dodržování předpisů v Azure je vědět, jak jste na tom s vašimi stávajícími prostředky. Tento rychlý start vás provede procesem vytvoření přiřazení zásady pro identifikaci virtuálních počítačů, které nepoužívají spravované disky.

Na konci tohoto procesu úspěšně identifikujete virtuální počítače, které nepoužívají spravované disky, a proto *nedodržují předpisy*.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="create-a-policy-assignment"></a>Vytvoření přiřazení zásady

V tomto rychlém startu vytvoříme přiřazení zásady a přiřadíme definici zásady *Audit virtuálních počítačů bez spravovaných disků*.

1. V levém podokně stránky služby Azure Policy vyberte **Přiřazení**.
2. V horní části podokna **Přiřazení** vyberte **Přiřadit zásadu**.

   ![Přiřazení definice zásady](media/assign-policy-definition/select-assign-policy.png)

3. Na stránce **Přiřadit zásadu** kliknutím na ![tlačítko Definice zásady](media/assign-policy-definition/definitions-button.png) vedle pole **Zásada** otevřete seznam dostupných definic.

   ![Otevření dostupných definic zásad](media/assign-policy-definition/open-policy-definitions.png)

   Služba Azure Policy obsahuje již integrované definice zásad, které můžete použít. Zobrazí se například následující integrované definice zásad:

   - Vynucovat použití značky a její hodnoty
   - Použít značku a její hodnotu
   - Vyžadovat SQL Server verze 12.0

4. Prohledejte definice zásad a najděte definici *Audit virtuálních počítačů, které nepoužívají spravované disky*. Klikněte na tuto zásadu a pak na **Přiřadit**.

   ![Vyhledání správné definice zásady](media/assign-policy-definition/select-available-definition.png)

5. Zadejte zobrazovaný **Název** tohoto přiřazení zásady. V tomto případě použijeme *Audit virtuálních počítačů, které nepoužívají spravované disky*. Volitelně můžete přidat také **Popis**. Popis obsahuje podrobnosti o způsobu, jakým toto přiřazení zásady identifikuje všechny virtuální počítače vytvořené v tomto prostředí, které nepoužívají spravované disky.
6. Změnou cenové úrovně na **Standard** zajistěte, že se zásada použije na stávající prostředky.

   V rámci služby Azure Policy existují dvě cenové úrovně – *Free* a *Standard*. S úrovní Free můžete vynucovat zásady pouze u budoucích prostředků, zatímco s úrovní Standard je můžete vynucovat také u stávajících prostředků, abyste lépe porozuměli vašemu stavu dodržování předpisů. Vzhledem k tomu, že jsme ve verzi Limited Preview, ještě jsme nezveřejnili cenový model, takže za výběr úrovně *Standard* neobdržíte žádnou fakturu. Další informace o cenách najdete v tématu [Ceny služby Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy/).

7. Vyberte **Obor**, na který chcete zásadu použít.  Obor určuje, pro které prostředky nebo skupiny prostředků se toto přiřazení zásady bude vynucovat. Může sahat od předplatného až po skupinu prostředků.
8. Vyberte předplatné (nebo skupinu prostředků), které jste dříve zaregistrovali. V tomto příkladu používáme předplatné **Azure Analytics Capacity Dev**, ale vaše možnosti se budou lišit.

   ![Vyhledání správné definice zásady](media/assign-policy-definition/assign-policy.png)

9. Vyberte **Přiřadit**.

Nyní jste připraveni identifikovat prostředky, které nedodržují předpisy, abyste porozuměli stavu dodržování předpisů ve vašem prostředí.

## <a name="identify-non-compliant-resources"></a>Identifikace prostředků, které nedodržují předpisy

V levém podokně vyberte **Dodržování předpisů** a vyhledejte přiřazení zásady, které jste vytvořili.

![Dodržování zásad](media/assign-policy-definition/policy-compliance.png)

Pokud nějaké stávající prostředky nedodržují předpisy tohoto nového přiřazení, zobrazí se na kartě **Neodpovídající prostředky**.

Pokud se napříč stávajícími prostředky vyhodnotí nějaká podmínka a pro některé má hodnotu True, takové prostředky se označí jako nedodržující danou zásadu. Tady je tabulka znázorňující, jak různé aktuálně dostupné akce pracují s výsledkem vyhodnocení podmínky a stavem dodržování předpisů vašich prostředků.

|Prostředek  |Výsledek vyhodnocení podmínky v zásadě  |Akce v zásadě   |Stav dodržování předpisů  |
|-----------|---------|---------|---------|
|Existuje     |True     |Odepřít     |Neodpovídající |
|Existuje     |False    |Odepřít     |Odpovídající     |
|Existuje     |True     |Připojit   |Neodpovídající |
|Existuje     |False    |Připojit   |Odpovídající     |
|Existuje     |True     |Auditování    |Neodpovídající |
|Existuje     |False    |Auditování    |Neodpovídající |

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další příručky v této kolekci vycházejí z tohoto rychlého startu. Pokud chcete pokračovat v práci s dalšími kurzy, neprovádějte čištění prostředků vytvořených v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky vytvořené tímto rychlým startem na portálu Azure Portal.
1. V levém podokně vyberte **Přiřazení**.
2. Vyhledejte přiřazení, které jste právě vytvořili.

   ![Odstranění přiřazení](media/assign-policy-definition/delete-assignment.png)

3.  Vyberte **Odstranit přiřazení**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste k oboru přiřadili definici zásady za účelem zajištění, aby všechny prostředky v tomto oboru dodržovaly předpisy, a identifikace těch, které předpisy nedodržují.

Další informace o přiřazování zásad pro zajištění, aby **budoucí** vytvořené prostředky dodržovaly předpisy, najdete v tomto kurzu:

> [!div class="nextstepaction"]
> [Vytváření a správa zásad](./create-manage-policy.md)
