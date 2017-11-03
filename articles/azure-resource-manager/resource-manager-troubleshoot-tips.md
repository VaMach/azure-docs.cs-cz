---
title: "Pochopení chyby nasazení Azure | Microsoft Docs"
description: "Popisuje, jak Další informace o nasazení Azure chyby."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení do azure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: b67bb30fa259fa08e37e11afec724c8b8c3eb633
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="understand-azure-deployment-errors"></a>Pochopení chyby nasazení Azure
Toto téma popisuje chyby nasazení a jak můžete zjistit další informace o chybě. Řešení pro běžné chyby nasazení najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).

## <a name="two-types-of-errors"></a>Dva typy chyb
Existují dva typy chyb, které dostanete:

* Chyby ověření
* Chyby nasazení

Následující obrázek znázorňuje protokol aktivit pro předplatné. Reprezentuje dvě nasazení. V jedné nasazení, šablony se nezdařilo ověření (**ověřením**) a neproběhla. V druhé nasazení, šablony ověření proběhlo úspěšně, ale při vytváření prostředků (**zápisu nasazení**). 

![Zobrazí kód chyby](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

Chyby ověření jsou vyvolány scénáře, které lze určit před nasazením. Obsahují chyby syntaxe v šabloně nebo pokusu o nasazení prostředky, které překročí vaší kvóty předplatného. Chyby nasazení jsou vyvolány podmínky, které se vyskytují během procesu nasazení. Patří mezi ně pokusu o přístup k prostředku, který je nasazován paralelně.

Oba typy chyb vrátí kód chyby, který můžete použít k nasazení řešení. Oba typy chyb se zobrazí v [protokol aktivit](resource-group-audit.md). Ale chyb při ověřování se nezobrazí v historii nasazení protože nasazení nikdy spuštěn.

## <a name="determine-error-code"></a>Určete kód chyby

Informace o chybě najdete prohlížením chybovou zprávu a kód chyby. [Odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md) článku jsou uvedené řešení v kódu chyby. Toto téma ukazuje, jak pomocí portálu Azure ke zjištění kód chyby.

### <a name="validation-errors"></a>Chyby ověření

Pokud nasazujete prostřednictvím portálu, zobrazí chybu ověření po odeslání svoje hodnoty.

![zobrazit chyba portálu ověření](./media/resource-manager-troubleshoot-tips/validation-error.png)

Vyberte další podrobnosti najdete v zprávě. Na následujícím obrázku vidíte **InvalidTemplateDeployment** chyba a zprávu, která určuje zásadu blokován nasazení.

![Zobrazit podrobnosti o ověření](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a>Chyby nasazení

Při operaci ověřením úspěšně projde, ale během nasazení se nezdaří, zobrazí se tato chyba v oznámení. Vyberte oznámení.

![Chyba oznámení](./media/resource-manager-troubleshoot-tips/notification.png)

Zobrazí podrobnosti o tomto nasazení. Vyberte možnost Najít další informace o této chybě.

![nasazení se nezdařilo.](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

Zobrazí chybovou zprávu a kódy chyb. Všimněte si, že jsou dva kódy chyb. První kód chyby (**DeploymentFailed**) je obecná chyba, která neobsahuje podrobnosti, budete muset vyřešit chyby. Druhý kód chyby (**StorageAccountNotFound**) poskytuje informace potřebné. 

![Podrobnosti o chybě](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a>Povolit protokolování ladění
Někdy potřebovat další informace o požadavku a odpovědi, které chcete zjistit, kde došlo k chybě. Pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure, můžete požádat, že další informace o protokolu během nasazení.

- PowerShell

   V prostředí PowerShell nastavená **DeploymentDebugLogLevel** parametr ke všem, ResponseContent nebo RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Zkontrolujte požadavek obsahu pomocí následující rutiny:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Nebo obsahu se odpovědi:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Tyto informace můžete určit, zda probíhá nesprávně nastavení na hodnotu v šabloně.

- Azure CLI

   Zkontrolujte operace nasazení pomocí následujícího příkazu:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Vnořené šablony

   Do protokolu informace o ladění vnořené šablony, pomocí **debugSetting** element.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a>Vytvořit šablonu řešení potíží
V některých případech je nejjednodušší způsob, jak vyřešit šablony otestovat části. Můžete vytvořit zjednodušená šablonu, která umožňuje zaměřit se na část, která budete mít dojem, je příčinou chyby. Předpokládejme například, že jste obdrželi chybu při odkazování na prostředek. Místo práci s celou šablony, vytvořte šablonu, která vrátí část, která může být příčinou problému. Může pomoct určit, zda jsou předávání v správné parametry, pomocí šablony funkce správně, a získávání prostředek očekáváte.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Nebo Předpokládejme, že dochází k chybám nasazení, které budete mít dojem, že se vztahují k nesprávné sady závislosti. Testování šablony porušením do zjednodušené šablony. Nejprve vytvořte šablonu, která nasadí pouze jeden prostředek (např. SQL Server). Pokud jste si jistí, že máte správně definované prostředku, přidejte na prostředek, který na něm závisí (např. SQL Database). Až budete mít tyto dva prostředky správně definované, přidejte další závislé prostředky (jako jsou zásady auditu). Mezi každou testovací nasazení Odstraňte skupinu prostředků. Ověřte, že je adekvátní testování závislosti. 

## <a name="check-deployment-sequence"></a>Zkontrolujte pořadí nasazení

Mnoho chyb nasazení dojít, když se prostředky nasadí neočekávané pořadí. Tyto chyby jsou vyvolány při závislosti nejsou správně nastavené. Pokud chybí potřebné závislosti, jeden prostředek se pokusí použít hodnotu pro jiný prostředek, ale druhá ještě neexistuje. Zobrazí chyba oznamující, že prostředek nebyl nalezen. Setkat tohoto typu chyby občas může lišit v době nasazení pro každý zdroj. Například první pokus o nasazení vašich prostředků úspěšné, protože požadovaný prostředek náhodně dokončí v čase. Však druhý pokus selže, protože požadovaný prostředek se nedokončila v čase. 

Ale budete chtít vyhnout nastavení závislosti, které nejsou potřebné. Až budete mít nepotřebné závislosti, můžete prodloužit doba trvání nasazení tím, že prostředky, které nejsou na sobě navzájem závislé z nasazuje paralelně. Kromě toho můžete vytvořit cyklické závislosti, které blokují nasazení. [Odkaz](resource-group-template-functions-resource.md#reference) funkce vytvoří implicitní závislostí v odkazovaném prostředku, když tento prostředek je nasazen do stejné šablony. Proto můžete mít další závislosti, než je určeno závislosti **dependsOn** vlastnost. [ResourceId](resource-group-template-functions-resource.md#resourceid) funkce vytvořit implicitní závislostí nebo ověřit, zda prostředek existuje.

Pokud narazíte na potíže závislostí, budete muset získat přehled o pořadí nasazení prostředků. Chcete-li zobrazit pořadí operace nasazení:

1. Vyberte historii nasazení skupiny prostředků.

   ![Vyberte historii nasazení](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. Vyberte nasazení z historie a vyberte **události**.

   ![Vybrat nasazení události](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. Zkontrolujte posloupnost událostí pro každý zdroj. Věnujte pozornost stav jednotlivých operací. Například následující obrázek ukazuje tři účty úložiště, které nasadí paralelně. Všimněte si, že tři účty úložiště jsou spuštěné ve stejnou dobu.

   ![Paralelní nasazení](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   Další obrázek ukazuje tři účty úložiště, které nejsou nasazené paralelně. Druhý účtu úložiště závisí na první účet úložiště, a třetí účet úložiště na druhý účtu úložiště. Proto první účet úložiště spuštěna, přijat a dokončit před zahájením na další.

   ![sekvenční nasazení](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

I pro složitější scénáře můžete použít stejný postup na zjišťování, když je nasazení spuštěno a dokončeno pro každý zdroj. Projděte nasazení událostí zobrazíte, pokud je pořadí je jiné než byste očekávali. Pokud ano, přehodnocovat závislosti pro tento prostředek.

Správce prostředků identifikuje cyklické závislosti během ověřování šablony. Vrátí, zda existuje chybovou zprávu, která konkrétně stavy cyklickou závislost. K vyřešení cyklická závislost:

1. V šabloně najdete prostředek určený v cyklická závislost. 
2. Pro tento prostředek, zkontrolujte **dependsOn** vlastnost a jakékoli použití **odkaz** funkce na prostředcích, které závisí na. 
3. Zkontrolujte na prostředcích, které jsou závislé na tyto prostředky. Postupujte podle závislosti, dokud si všimnete na prostředek, který závisí na původní prostředků.
5. Pro prostředků zahrnutých v cyklická závislost, pečlivě zkontrolujte všechna jeho použití **dependsOn** vlastnost k identifikaci všechny závislosti, které nejsou potřebné. Odeberte tyto závislosti. Pokud si nejste jistí, že je potřeba závislost, zkuste ji odebrat. 
6. Znovu nasaďte šablonu.

Odebírání hodnot **dependsOn** vlastnost mohou způsobit chyby při nasazování šablony. Pokud dojde k chybě, přidejte závislosti zpět do šablony. 

Pokud tento přístup nevyřeší cyklická závislost, zvažte přesunutí součást logiky nasazení do podřízené prostředky (například rozšíření nebo nastavení konfigurace). Nakonfigurujte tyto podřízené prostředky k nasazení po prostředků zahrnutých v cyklická závislost. Předpokládejme například, kterou nasazujete dva virtuální počítače, ale je nutné nastavit vlastnosti u každé z nich odkazovat na druhý. Je můžete nasadit v následujícím pořadí:

1. vm1
2. virtuálního počítače 2
3. Rozšíření na vm1 závisí na vm1 a virtuálního počítače 2. Rozšíření nastaví hodnoty vm1, který získá z virtuálního počítače 2.
4. Rozšíření na virtuálního počítače 2 závisí na vm1 a virtuálního počítače 2. Rozšíření virtuálního počítače 2, který získá ze vm1 nastaví hodnoty.

Ve stejný přístup funguje pro aplikace služby App Service. Zvažte přesunutí hodnoty konfigurace do podřízených prostředkem prostředek aplikace. Můžete nasadit dva webové aplikace v následujícím pořadí:

1. webapp1
2. webapp2
3. konfigurace pro webapp1 závisí na webapp1 a webapp2. Obsahuje nastavení aplikace s hodnotami z webapp2.
4. konfigurace pro webapp2 závisí na webapp1 a webapp2. Obsahuje nastavení aplikace s hodnotami z webapp1.


## <a name="next-steps"></a>Další kroky
* Řešení pro běžné chyby nasazení najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* Další informace o auditování akce najdete v tématu [auditovat operace s Resource Managerem](resource-group-audit.md).
* Další informace o akce k určení chyby během nasazení najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).
