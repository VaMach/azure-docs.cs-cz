---
title: "Jak mít verze modulu runtime Azure Functions"
description: "Azure Functions podporuje více verzí modulu runtime. Zjistěte, jak určit verzi modulu runtime Azure hostované funkce aplikace."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.author: glenga
ms.openlocfilehash: 063232e40b30d03b0ee8b087a602fed0fee3be0a
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Jak mít verze modulu runtime Azure Functions

Funkce aplikace běží na konkrétní verzi modulu runtime Azure Functions. Existují dvě hlavní verze: 1.x a 2.x. Tento článek vysvětluje, jak vybrat, které hlavní verze se má použít a postup konfigurace funkce aplikace v Azure a spustit na verzi, můžete si vybrat. Informace o tom, jak nakonfigurovat místní vývojové prostředí pro konkrétní verzi najdete v tématu [kódu a testování Azure Functions místně](functions-run-local.md).

## <a name="differences-between-runtime-1x-and-2x"></a>Rozdíly mezi runtime 1.x a 2.x

> [!IMPORTANT] 
> Modul runtime 1.x je pouze verze schválené pro použití v provozním prostředí.

| Modul runtime | Status |
|---------|---------|
|1.x|Obecně dostupná (GA)|
|2.x|Preview|

Následující části popisují rozdíly v jazycích, vazeb a podporu pro vývoj pro různé platformy.

### <a name="languages"></a>Jazyky

Následující tabulka uvádí podporované programovací jazyky v každé verzi modulu runtime.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Další informace najdete v tématu [podporované jazyky](supported-languages.md).

### <a name="bindings"></a>Vazby 

Experimentální vazby, že nejsou k dispozici v 2.x runtime 1.x podporuje. Informace o podpoře vazby a dalších funkční mezery v 2.x najdete v tématu [Runtime 2.0 známé problémy](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

Modul runtime 2.x se dá vytvořit vlastní [vazby rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). Předdefinované vazby, které používají tento model rozšiřitelnosti jsou dostupné jenom v 2.x; mezi první z nich je [Microsoft Graph vazby](functions-bindings-microsoft-graph.md).

### <a name="cross-platform-development"></a>Vývoj pro různé platformy

Modul runtime 1.x podporuje funkce vývoj pouze na portálu nebo na Windows; s 2.x jste sami vyvinuli a systémem Linux nebo systému macOS Azure Functions.

## <a name="automatic-and-manual-version-updates"></a>Verze automatickou a ruční aktualizace

Funkce umožňuje cílení na konkrétní verzi modulu runtime s použitím `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace v aplikaci funkce. Funkce aplikace se ukládají na zadaný hlavní verzi, dokud se nerozhodnete explicitně přesunout na novou verzi.

Pokud zadáte jenom hlavní verzi (~ 1"pro 1.x) nebo"beta"pro 2.x, funkce aplikace se automaticky aktualizuje na nový dílčí verze modulu runtime, jakmile budou k dispozici. Nové podverze nevyplývají nejnovější změny. Pokud zadáte dílčí verzi (například "1.0.11360"), funkce aplikace je udržováno na tuto verzi, dokud ji explicitně změnit. 

Když je veřejně dostupná nová verze, výzvu na portálu vám dává možnost Přesunout nahoru na příslušnou verzi. Po přesunutí na novou verzi, můžete vždy použít `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace přejít zpět na předchozí verzi.

Ke změně verze modulu runtime způsobí, že funkce aplikaci restartovat.

Hodnoty můžete nastavit v `FUNCTIONS_EXTENSION_VERSION` aplikace nastavení Povolit automatické aktualizace se právě "~ 1" pro modul runtime 1.x a "beta" pro 2.x.

## <a name="view-the-current-runtime-version"></a>Zobrazit aktuální verzi modulu runtime

Následujícím postupem zobrazíte verzi modulu runtime, který je aktuálně používán aplikaci funkce. 

1. V [portál Azure](https://portal.azure.com), přejděte do aplikaci funkce a v části **nakonfigurované funkce**, zvolte **funkce nastavení aplikace**. 

    ![Vyberte nastavení aplikace – funkce](./media/functions-versions/add-update-app-setting.png)

2. V **funkce nastavení aplikace** kartě, vyhledejte **verzi modulu Runtime**. Poznámka: verze konkrétní runtime a požadovaný hlavní verzi. V následujícím příkladu funkce\_rozšíření\_verze aplikace nastavení je `~1`.
 
   ![Vyberte nastavení aplikace – funkce](./media/functions-versions/function-app-view-version.png)

## <a name="target-the-version-20-runtime"></a>Cílový modul runtime verze 2.0

>[!IMPORTANT]   
> Azure Functions runtime 2.0 je ve verzi preview a aktuálně ne všechny funkce Azure Functions podporované. Další informace najdete v tématu [rozdíly mezi runtime 1.x a 2.x](#differences-between-runtime-1x-and-2x) výše v tomto článku.

Funkce aplikace můžete přesunout do verze Preview verze 2.0 modul runtime na portálu Azure. V **funkce nastavení aplikace** , zvolte **beta** pod **verzi modulu Runtime**.  

![Vyberte nastavení aplikace – funkce](./media/functions-versions/function-app-view-version.png)

Toto nastavení odpovídá nastavení `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace pro `beta`. Vyberte **~ 1** tlačítko přesunout zpět do aktuální veřejně podporovaná hlavní verze. Rozhraní příkazového řádku Azure můžete taky aktualizovat nastavení této aplikace. 

## <a name="target-a-version-using-the-portal"></a>Cílové verze pomocí portálu

Pokud budete potřebovat cílit na verzi než aktuální hlavní verze nebo verze 2.0, je nutné nastavit `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace.

1. V [portál Azure](https://portal.azure.com), přejděte do aplikace pro funkce a v části **nakonfigurované funkce**, zvolte **nastavení aplikace**.

    ![Vyberte nastavení aplikace – funkce](./media/functions-versions/add-update-app-setting1a.png)

2. V **nastavení aplikace** kartě, vyhledejte `FUNCTIONS_EXTENSION_VERSION` nastavení a změňte hodnotu na platnou verzi modulu runtime 1.x nebo `beta` pro verze 2.0. 

    ![Nastavení aplikace – funkce](./media/functions-versions/add-update-app-setting2.png)

3. Klikněte na tlačítko **Uložit** uložit aktualizaci nastavení aplikace. 

## <a name="target-a-version-using-azure-cli"></a>Cílová verze, pomocí rozhraní příkazového řádku Azure

 Můžete také nastavit `FUNCTIONS_EXTENSION_VERSION` z příkazového řádku Azure. Použití Azure CLI, aktualizovat nastavení aplikace v aplikaci funkce pomocí [az functionapp konfigurace appsettings sadu](/cli/azure/functionapp/config/appsettings#set) příkaz.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
V tomto kódu nahraďte `<function_app>` s názvem aplikace funkce. Také nahraďte `<my_resource_group>` s názvem skupiny prostředků pro funkce aplikace. Nahraďte `<version>` s platnou verzi modulu runtime 1.x nebo `beta` pro verze 2.0. 

Můžete spustit tento příkaz z [prostředí cloudu Azure](../cloud-shell/overview.md) výběrem **vyzkoušet** v předchozí ukázce kódu. Můžete také [rozhraní příkazového řádku Azure místně](/cli/azure/install-azure-cli) k provedení tohoto příkazu po provedení [az přihlášení](/cli/azure#az_login) k přihlášení.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Cílový modul runtime 2.0 ve vašem místním vývojovém prostředí](functions-run-local.md)

> [!div class="nextstepaction"]
> [Najdete v části poznámky k verzi pro verze modulu runtime](https://github.com/Azure/azure-webjobs-sdk-script/releases)