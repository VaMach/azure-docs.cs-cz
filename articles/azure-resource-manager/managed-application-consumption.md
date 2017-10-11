---
title: "Využívat Azure spravované aplikace | Microsoft Docs"
description: "Popisuje, jak zákazník vytvoří Azure spravované aplikace z publikovaných souborů."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: ed8fbaf2a4546c8e31eeced11cd0b5627fd62c0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="consume-an-internal-managed-application"></a>Využívat interní spravované aplikace

Můžete využívat Azure [spravované aplikace](managed-application-overview.md) , jsou určené pro členy vaší organizace. Spravované aplikace můžete například vybrat od vašeho IT oddělení, které zajišťují dodržování standardů organizace. Tyto spravované aplikace jsou k dispozici prostřednictvím katalogu služeb, není v Azure Marketplace.

Než budete pokračovat v tomto článku, musíte mít k dispozici v katalogu služeb spravované aplikace pro vaše předplatné. Pokud někdo ve vaší organizaci dosud vytvořena spravované aplikace, najdete v části [publikování spravované aplikace pro interní používání](managed-application-publishing.md).

V současné době můžete rozhraní příkazového řádku Azure nebo portálu Azure využívat spravované aplikace.

## <a name="create-the-managed-application-by-using-the-portal"></a>Vytvoření spravované aplikace prostřednictvím portálu

Pokud chcete nasadit spravované aplikace prostřednictvím portálu, postupujte takto:

1. Přejděte na portálu Azure. Vyhledejte **katalogu služeb spravované aplikace**.

   ![Katalog služeb spravované aplikace](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. Vyberte spravované aplikaci, kterou chcete vytvořit ze seznamu dostupných řešení. Vyberte **Vytvořit**.

   ![Výběr spravované aplikace](./media/managed-application-consumption/select-offer.png)

1. Zadejte hodnoty pro parametry, které jsou požadovány k přidělení prostředků. Vyberte **– Západ střední USA** pro umístění. Vyberte **OK**.

   ![Parametry spravované aplikace](./media/managed-application-consumption/input-parameters.png)

1. Šablona ověří hodnoty, které jste zadali. V případě úspěšného ověření vyberte **OK** ke spuštění nasazení.

   ![Ověřování spravovaných aplikací](./media/managed-application-consumption/validation.png)

Po dokončení nasazení s příslušnými prostředky v šabloně definovánu připravené ve skupině spravovaných prostředků, které jste zadali.

## <a name="create-the-managed-application-by-using-azure-cli"></a>Vytvoření spravované aplikace pomocí rozhraní příkazového řádku Azure

Existují dva způsoby vytvoření spravované aplikace pomocí rozhraní příkazového řádku Azure:

* Použijte příkaz pro vytvoření spravovaných aplikací.
* Použijte příkaz regulární šablonu nasazení.

### <a name="use-the-template-deployment-command"></a>Použijte příkaz nasazení šablony

Nasaďte soubor applianceMainTemplate.json vytvořený dodavatele.

Pak vytvořte dvě skupiny prostředků. První skupina prostředků je, kde se má vytvořit prostředek spravované aplikace: Microsoft.Solutions/appliances. Druhý skupina prostředků obsahuje všechny prostředky, které jsou definované v mainTemplate.json. Tato skupina prostředků je spravovaný ISV.

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Použití `westcentralus` jako umístění skupiny prostředků.
>

Pokud chcete nasadit applianceMainTemplate.json v mainResourceGroup, použijte následující příkaz:

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

Po spuštění předchozí šablony, budete vyzváni k zadání hodnot parametrů, které jsou definované v šabloně. Kromě parametrů, které jsou potřebné k zajištění prostředků v šabloně je třeba dvě hodnoty klíčů parametr:

- **managedResourceGroupId**: ID skupiny prostředků obsahující prostředky, které jsou definované v applianceMainTemplate.json. ID je ve formátu `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. V předchozím příkladu je ID `managedResourceGroup`.
- **applianceDefinitionId**: ID definice prostředku spravované aplikace. Tato hodnota je poskytovaný ISV.

> [!NOTE]
> Vydavatel musí udělit přístup ke skupině prostředků, který obsahuje definici spravované aplikace. Definice prostředků se vytvoří ve vašem předplatném vydavatele. Přístup pro čtení k tomuto prostředku musí tedy uživatele, skupiny uživatelů nebo aplikací v klientovi zákazníka.

Po dokončení nasazení úspěšně, uvidíte, že spravované aplikace se vytvoří v mainResourceGroup. StorageAccount prostředek je vytvořen v managedResourceGroup.

### <a name="use-the-create-command"></a>Použijte příkaz create

Můžete použít `az managedapp create` příkaz pro vytvoření spravované aplikace z definice spravované aplikace.

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* **Id definice zařízení**: ID prostředku definici spravované aplikaci vytvořili v předchozím kroku. Pokud chcete získat toto ID, spusťte následující příkaz:

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  Tento příkaz vrátí definice spravovaných aplikací. Je třeba hodnotu vlastnost ID.

* **spravované id rg**: název skupiny prostředků obsahující prostředky, které jsou definované v applianceMainTemplate.json. Tato skupina prostředků je skupina spravovaných prostředků. Je spravovaný vydavatele. Pokud neexistuje, vytvoří se pro vás.
* **Skupina prostředků**: skupině prostředků, kde se má vytvořit prostředek spravované aplikace. Microsoft.Solutions/appliance prostředků je umístěn v této skupině prostředků.
* **Parametry**: parametry, které jsou potřebné pro prostředky, které jsou definované v applianceMainTemplate.json.

## <a name="known-issues"></a>Známé problémy

Tato verze preview zahrnuje následující problémy:

* Při vytváření spravované aplikace se zobrazí 500 vnitřní chybu serveru. Pokud narazíte na potíže, je pravděpodobně přerušované. Opakujte operaci.
* Novou skupinu prostředků je potřeba pro skupinu spravovaných prostředků. Pokud použijete existující skupinu prostředků, nasazení se nezdaří.
* Skupinu prostředků, která obsahuje prostředek Microsoft.Solutions/appliances musí být vytvořené v **westcentralus** umístění.

## <a name="next-steps"></a>Další kroky

* Úvod do spravovaných aplikací, najdete v části [spravované aplikace přehled](managed-application-overview.md).
* Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).
* Informace o publikování spravované aplikace do Azure Marketplace najdete v tématu [Azure spravované aplikace na webu Marketplace](managed-application-author-marketplace.md).
* Informace o využívání spravované aplikace z webu Marketplace najdete v tématu [využívat Azure spravované aplikace na webu Marketplace](managed-application-consume-marketplace.md).
