---
title: "Pomocí zásad Azure můžete vytvořit a spravovat zásady vynutit organizační dodržování předpisů | Microsoft Docs"
description: "Pomocí zásad Azure vynutit standardy, splňovat zákonné požadavky na dodržování předpisů a auditování, řídit náklady, zachování konzistence zabezpečení a výkonu a použít zásady široké designu enterprise."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/18/2018
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: d6a588e1d8a20ffba555461cf98009f3894ed761
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Vytvořit a spravovat zásady na vynucování dodržování shody

Vědět, jak vytvořit a spravovat zásady v Azure je důležité pro zachování kompatibilní s firemními standardy a smlouvy o úrovni služeb. V tomto kurzu dozvíte, jak provést některé běžné úlohy související s vytváření, přiřazování a správě zásad v organizaci, například pomocí zásad Azure:

> [!div class="checklist"]
> * Přiřadit zásady vynutit podmínku pro prostředky, které vytvoříte v budoucnosti
> * Vytvořit a přiřadit iniciativy definice ke sledování dodržování předpisů pro více zdrojů
> * Vyřešte nevyhovující nebo odepření prostředek
> * Implementovat novou zásadu celé organizaci

Pokud chcete přiřadit zásady k identifikaci aktuální stav dodržování předpisů stávajících prostředků, přejděte v článcích rychlý start tom, jak to udělat. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="assign-a-policy"></a>Přiřadit zásady

Prvním krokem při vynucování souladu se zásadami Azure je přiřadit definici zásady. Definuje definici zásady v jaké podmínky a zásady se nevynutí a jaká opatření se mají provést. V tomto příkladu přiřadit definici předdefinované zásady názvem *vyžadují SQL Server verze 12.0*, vynutit podmínky, že všechny databáze systému SQL Server musí být v12.0 tak, aby vyhovoval.

1. Spusťte službu zásad Azure na portálu Azure hledání a výběrem **zásad** v levém podokně.

   ![Hledání zásad](media/assign-policy-definition/search-policy.png)

2. V levém podokně stránky služby Azure Policy vyberte **Přiřazení**. Přiřazení je zásadu, která byla přiřazena proběhla v rámci konkrétní obor.
3. V horní části podokna **Přiřazení** vyberte **Přiřadit zásadu**.

   ![Přiřazení definice zásady](media/create-manage-policy/select-assign-policy.png)

4. Na stránce **Přiřadit zásadu** kliknutím na ![tlačítko Definice zásady](media/assign-policy-definition/definitions-button.png) vedle pole **Zásada** otevřete seznam dostupných definic. Můžete filtrovat definice zásady **typ** k *BuiltIn* zobrazit všechny a číst jejich popisy.

   ![Otevření dostupných definic zásad](media/create-manage-policy/open-policy-definitions.png)

5. Vyberte **vyžadují SQL Server verze 12.0**. Pokud nemůžete najít ho hned, zadejte **vyžadují SQL Server verze 12.0** do vyhledávacího pole a potom stiskněte klávesu ENTER.

   ![Vyhledejte zásadu](media/create-manage-policy/select-available-definition.png)

6. Zobrazené **název** je automaticky vyplněno, ale můžete ji změnit. V tomto příkladu použijte *vyžadují SQL Server verze 12.0*. Volitelně můžete přidat také **Popis**. Popis poskytuje podrobnosti o tom, jak toto přiřazení zásad zajistí všechny servery SQL, které jsou vytvořené v tomto prostředí jsou verze 12.0.

7. Změnou cenové úrovně na **Standard** zajistěte, že se zásada použije na stávající prostředky.

   V rámci služby Azure Policy existují dvě cenové úrovně – *Free* a *Standard*. S úrovní Free můžete vynucovat zásady pouze u budoucích prostředků, zatímco s úrovní Standard je můžete vynucovat také u stávajících prostředků, abyste lépe porozuměli vašemu stavu dodržování předpisů. Protože Azure zásady nejsou ve verzi Preview, není ještě vydání cenový model, takže nebudete dostávat faktury pro výběr *standardní*. Další informace o cenách najdete v tématu [Ceny služby Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy).

8. Vyberte **oboru** -předplatné (nebo skupinu prostředků) dříve registrován. Obor určuje, pro které prostředky nebo skupiny prostředků se toto přiřazení zásady bude vynucovat. Může sahat od předplatného až po skupinu prostředků.

   Tento příklad používá **Azure analýzy kapacity Dev** předplatné. Vaše předplatné se budou lišit.

10. Vyberte **Přiřadit**.

## <a name="implement-a-new-custom-policy"></a>Implementovat nové vlastní zásady

Teď, když jste přiřadili definici předdefinované zásady, můžete provést více zásadám Azure. Dále vytvořte nové vlastní zásady, abyste ušetřili náklady zajištěním, které virtuální počítače vytvořené v prostředí nelze v této sérii G. Tímto způsobem pokaždé, když uživatel ve vaší organizaci pokusí o vytvoření virtuálního počítače v řadě G, požadavek se odmítne.

1. Vyberte **definice** pod **vytváření** v levém podokně.

   ![Definice v části vytváření](media/create-manage-policy/definition-under-authoring.png)

2. Vyberte **+ definice zásady**.
3. Zadejte následující příkaz:

   - Název definice zásady - *vyžadují virtuálních počítačů SKU menší než G řady*
   - Popis definice zásady cílem – tuto definici zásady vynutí, že všechny virtuální počítače vytvořené v tomto rozsahu mají SKU menší než řady G na snížení nákladů.
   - Předplatné, ve kterém se nachází definice zásady. V takovém případě se definice zásady nachází v **Advisor analýzy kapacity Dev**. Seznam odběrů se budou lišit.
   - Vyberte z existujícího možnosti, nebo vytvořte novou kategorii pro tuto definici zásady.
   - Zkopírujte následující kód json a pak aktualizujte pro vaše potřeby pomocí:
      - Parametry zásad.
      - Zásady pravidla nebo podmínky, v takovém případě – velikost virtuálního počítače SKU rovna G řady
      - Účinek zásady, v takovém případě – **Odepřít**.

    Zde je, jak by měla vypadat json. Revidovaný kód vložte do portálu Azure.

    ```json
{
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/sku.name",
            "like": "Standard_G*"
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
}
    ```

    Hodnota *pole vlastnost* v zásadách pravidlo musí mít jednu z následujících akcí: název, typ, umístění, značky nebo alias. Například, `"Microsoft.Compute/VirtualMachines/Size"`.

    Chcete-li zobrazit další ukázky kódu json, přečtěte si [šablon pro Azure zásad](json-samples.md) článku.

4. Vyberte **Uložit**.

## <a name="create-a-policy-definition-with-rest-api"></a>Vytvoří definici zásady pomocí rozhraní REST API

Zásady můžete vytvořit pomocí rozhraní REST API pro definice zásady. Rozhraní REST API umožňuje vytvářet a odstraňovat definice zásady a získat informace o existující definice.
K vytvoření definice zásady, použijte následující příklad:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

```
Zahrnout obsah žádosti podobně jako v následujícím příkladu:

```
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>Vytvoří definici zásady v prostředí PowerShell

Před pokračováním v příkladu prostředí PowerShell, zkontrolujte, zda že jste nainstalovali nejnovější verzi prostředí Azure PowerShell. Ve verzi 3.6.0 byly přidány zásady parametry. Pokud máte starší verzi, příklady vrátí chybu oznamující, že parametr nebyl nalezen.

Můžete vytvořit pomocí definice zásady `New-AzureRmPolicyDefinition` rutiny.

K vytvoření definice zásad ze souboru, předejte cestu k souboru. Pro externí soubor použijte následující příklad:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Pro použití místního souboru použijte následující příklad:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

K vytvoření definice zásady s vložené pravidlo, použijte následující příklad:

```
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

Výstup je uložen v `$definition` objekt, který se používá při přiřazování zásady.
Následující příklad vytvoří definici zásady, který obsahuje parametry:

```
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters
```

## <a name="view-policy-definitions"></a>Definice zásad zobrazení

Pokud chcete zobrazit všechny definice zásady v rámci vašeho předplatného, použijte následující příkaz:

```
Get-AzureRmPolicyDefinition
```

Vrátí všechny dostupné zásady definice, včetně integrovaných zásad. Každá zásada se vrátí v následujícím formátu:

```
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Vytvoří definici zásady pomocí rozhraní příkazového řádku Azure

Můžete vytvořit definici zásady pomocí rozhraní příkazového řádku Azure pomocí příkazu definice zásady.
K vytvoření definice zásady s vložené pravidlo, použijte následující příklad:

```
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

## <a name="view-policy-definitions"></a>Definice zásad zobrazení

Pokud chcete zobrazit všechny definice zásady v rámci vašeho předplatného, použijte následující příkaz:

```
az policy definition list
```

Vrátí všechny dostupné zásady definice, včetně integrovaných zásad. Každá zásada se vrátí v následujícím formátu:

```
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Vytvořte a přiřaďte iniciativy definice

Iniciativy definicí můžete seskupit několik definice zásady pro dosažení jeden zastřešující cíle. Můžete vytvořit definici iniciativy zajistit, že prostředky v rámci oboru definice zůstat kompatibilní s definice zásady, které tvoří definici iniciativy.  Najdete v článku [přehled zásad Azure](./azure-policy-introduction.md) Další informace o iniciativy definice.

### <a name="create-an-initiative-definition"></a>Vytvoření iniciativy definice

1. Vyberte **definice** pod **vytváření** v levém podokně.

   ![Vyberte definice](media/create-manage-policy/select-definitions.png)

2. Vyberte **Initiative definice** v horní části stránky, trvá tento výběr vám **Initiative definice** formuláře.
3. Zadejte název a popis dané iniciativy.

   V tomto příkladu zajistěte, aby byly v souladu s definice zásady o získání zabezpečené prostředky. Ano, bude název iniciativa zaměřená **získat zabezpečení**, a bude popis: **vytvořil se tento initiative zpracovat všechny definice zásady, které jsou přidružené k zabezpečení prostředků**.

   ![Definice iniciativy](media/create-manage-policy/initiative-definition.png)

4. Procházet seznam **dostupné definice** a vyberte definice zásad chcete přidat do této initiative. Pro naše **získat zabezpečený** iniciativa, **přidat** následující součástí definice zásady:
   - Vyžaduje systém SQL Server verze 12.0
   - Monitorujte nechráněné webové aplikace ve službě Security Center.
   - Monitorovat projektovou síť mezi ve službě Security Center.
   - Monitorování aplikace povolených v Centru zabezpečení.
   - Monitorování bez šifrování disky virtuálních počítačů ve službě Security Center.

   ![Iniciativy definice](media/create-manage-policy/initiative-definition-2.png)

   Po výběru definice zásad ze seznamu se zobrazovat v části **zásady a parametry**, jak je vidět na předchozím obrázku.

5. Použití **definice umístění** vyberte předplatné, pro ukládání definici. Vyberte **Uložit**.

### <a name="assign-an-initiative-definition"></a>Přiřadit iniciativy definice

1. Přejděte na **definice** v části **vytváření**.
2. Vyhledejte **získat zabezpečený** iniciativy definice, které jste vytvořili.
3. Vyberte definici iniciativy a pak vyberte **přiřadit**.

   ![Přiřadit definice](media/create-manage-policy/assign-definition.png)

4. Vyplňte **přiřazení** formuláře, tak, že zadáte následující informace z příkladu. Můžete vytvořit své vlastní informace.
   - Název: získat zabezpečený přiřazení
   - Popis: Tento iniciativy přiřazení je přizpůsobit vynutit tato skupina definice zásady v **Azure Advisor kapacity Dev** předplatné.
   - Cenová úroveň: standardní
   - Obor, kde se má použít pro přiřazení: **Azure Advisor kapacity Dev**. Můžete použít vlastní předplatném nebo skupině prostředků.

5. Vyberte **Přiřadit**.

## <a name="resolve-a-non-compliant-or-denied-resource"></a>Vyřešte nevyhovující nebo odepření prostředek

V následujícím příkladu výše, po přiřazení definice zásady tak, aby vyžadovala SQL server verze 12.0 by získat odepřena vytvořen v jiné verzi systému SQL server. V této části můžete provede řešení odepření pokus o vytvoření jiné verzi systému SQL server tím, že požádá vyloučení. Vyloučení brání v podstatě vynucení zásad. Vyloučení můžete použít pro skupinu prostředků, nebo můžete zúžit vyloučení pro jednotlivé zdroje.

1. V levém podokně vyberte **Přiřazení**.
2. Procházet všechna přiřazení zásad a otevřete *vyžadují SQL Server verze 12.0* přiřazení.
3. **Vyberte** vyloučení pro prostředky ve skupině prostředků, které se pokoušíte vytvořit systému SQL server. V tomto příkladu vyloučit Microsoft.Sql/servers/databases: *azuremetrictest/testdb* a *azuremetrictest/testdb2*.

   ![Žádost o vyloučení](media/create-manage-policy/request-exclusion.png)

   Jiné způsoby může vyřešit odepření prostředek patří: oslovit kontakt přidružených k zásadě, pokud máte pro systém SQL server vytvořil, která potřebuje a přímo upravovat zásady, pokud máte přístup k silné zarovnání do bloku.

4. Klikněte na tlačítko **přiřadit**.

V této části přeložit odmítnutí pokusu o vytvoření systému SQL server s verzí 12.0, tím, že požádá vyloučení k prostředkům.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud budete chtít pokračovat v práci s další kurzy, neprovádí vyčištění prostředky vytvořené v této příručce. Pokud neplánujete, chcete-li pokračovat, použijte následující postup k odstranění některého ze přiřazení nebo definice vytvořili výše:

1. Vyberte **definice** (nebo **přiřazení** Pokud se pokoušíte odstranit přiřazení) v levém podokně.
2. Hledání pro nové initiative nebo zásady definice (nebo přiřazení), který jste právě vytvořili.
3. Vyberte tři tečky na konci definice nebo přiřazení a vyberte **odstranit definice** (nebo **odstranit přiřazení**).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste úspěšně provést následující:

> [!div class="checklist"]
> * Přiřazené zásady vynutit podmínku pro prostředky, které vytvoříte v budoucnosti
> * Vytvořit a přiřadit iniciativy definice ke sledování dodržování předpisů pro více zdrojů
> * Vyřešit nevyhovující nebo odepření prostředek
> * Implementovat novou zásadu celé organizaci

Další informace o struktury definice zásady, podívejte se na tomto článku:

> [!div class="nextstepaction"]
> [Azure definice strukturu zásad.](policy-definition.md)
