---
title: "Portál Azure pro zásady prostředků | Microsoft Docs"
description: "Popisuje, jak pomocí portálu Azure vytvořit a spravovat zásady Resource Manager. Zásady můžete použít na předplatné nebo prostředek skupiny."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: 868b2cc1559053057d17b34c03e2e31347f399bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-portal-to-assign-and-manage-resource-policies"></a>Pomocí portálu Azure přiřadit a spravovat zásady prostředků
Portál Azure umožňuje přiřadit zásady prostředků skupiny prostředků a předplatná. Uživatelské rozhraní usnadňuje vyberte zásadu, kterou chcete přiřadit a zadejte hodnoty parametrů pro tuto zásadu přizpůsobit nastavení zásad. 

Přiřazení zásad prostřednictvím portálu, definice zásady již musí existovat v rámci vašeho předplatného. Vaše předplatné má několik předdefinovaných zásad definice, které jsou připravené k přiřazení skupiny prostředků nebo předplatných. Zobrazí tyto předdefinované zásady a všechny vlastní zásady, které jste definovali při použití portálu k přiřazení zásad. Úvod do zásad a jak definovat vlastní zásady, najdete v části [přehled zásad prostředků](resource-manager-policy.md).

Zásady jsou zdědí všechny podřízené prostředky. Ano Pokud je zásada pro skupinu prostředků, se vztahuje na všechny prostředky v příslušné skupině prostředků. V tomto článku termín **oboru** odkazuje na skupinu prostředků nebo předplatného, které je přiřazen zásady. 

Při vytváření nebo aktualizaci prostředků (PUT a oprava operations), se vyhodnocují zásady.

## <a name="assign-a-policy"></a>Přiřadit zásady

1. Přiřazení zásad na skupinu prostředků nebo předplatného, vyberte tuto skupinu prostředků nebo předplatného. V nastavení, vyberte **zásady**.

   ![Vyberte zásady](./media/resource-manager-policy-portal/select-policies.png)

2. Chcete-li vytvořit přiřazení zásady pro tento rozsah, vyberte **přidat přiřazení**.

   ![Přidat přiřazení](./media/resource-manager-policy-portal/add-assignment.png)

3. Vyberte zásadu, kterou chcete přiřadit. I když nepřidali jste žádné definice zásady pro vaše předplatné, zobrazí předdefinované zásady, které jsou k dispozici pro přiřazení. Tyto integrované zásady zahrnují mnoho běžných scénářů.

   ![Vyberte definice](./media/resource-manager-policy-portal/select-definition.png)

4. Po výběru zásady, najdete v části Popis zásady a parametry pro tuto zásadu. Například na následujícím obrázku **povolené umístění** parametr, který je vyžadován pro zásadu, která k dispozici umístění.

   ![Zobrazit parametry](./media/resource-manager-policy-portal/show-parameters.png)

5. V uživatelském rozhraní vyberte hodnoty pro parametry zásad (jako je například umístění, které lze použít pro nasazení) zadejte.

   ![Vyberte hodnoty parametru](./media/resource-manager-policy-portal/select-parameters.png)

6. Zadejte hodnoty pro ostatní parametry. Obor je automaticky přiřadit na základě v okně, které jste vybrali při spuštění přiřazení zásady. Až to budete mít, vyberte **OK**.

   ![Definujte parametry](./media/resource-manager-policy-portal/define-parameters.png)

  Přiřadili jste zásadu pro zadaný obor.

## <a name="view-policy-assignments"></a>Zobrazení přiřazení zásad.

Po přiřazení zásady, najdete v seznamu zásad pro tento obor. **Podrobnosti** karta zobrazuje souhrnné údaje o přiřazení zásad.

![Zobrazit podrobnosti](./media/resource-manager-policy-portal/show-details.png)

**Pravidlo přiřazení** kartě se zobrazují ve formátu JSON pro definici zásady.

![Zobrazit pravidlo přiřazení](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a>Změnit existující přiřazení zásady

Chcete-li změnit zásadu, vyberte **upravit přiřazení** nebo **odstranit**

![upravit nebo odstranit přiřazení](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a>Přiřazení vlastních zásad

Pokud jste definovali vlastní zásady v rámci vašeho předplatného, tyto zásady jsou k dispozici pro přiřazení prostřednictvím portálu. Tyto zásady jsou uvedena **[vlastní]**

![vlastní zásady](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a>Další kroky
* Další informace o syntaxi JSON pro definování zásad najdete v tématu [přehled zásad prostředků](resource-manager-policy.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).
* Schéma zásad je publikována v [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

