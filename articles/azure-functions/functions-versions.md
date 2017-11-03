---
title: "Jak mít verze modulu runtime Azure Functions | Microsoft Docs"
description: "Funkce podporuje více verzí modulu runtime. Zjistěte, jak určit verzi modulu runtime Azure hostované funkce aplikace."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: glenga
ms.openlocfilehash: 26d4276a0a550d78a9c7657c464bd3320c956fb0
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Jak mít verze modulu runtime Azure Functions

Modul runtime Azure Functions implementuje bez serveru provádění kódu v Azure. Tento modul runtime zjistíte v různých prostředích s jinými než hostované v Azure. [Nástroje základní funkce Azure](functions-run-local.md) implementuje tento modul runtime na vašem vývojovém počítači. [Azure Functions Runtime](functions-runtime-overview.md) vám umožní používat funkce v místním prostředí. 

Funkce podporuje více hlavní verze modulu runtime. Hlavní verze aktualizace můžete zavést zásadní změny. Toto téma popisuje, jak můžete cílit na vaše aplikace funkce runtime konkrétní verzi, když jsou hostované v Azure. 

Funkce umožňuje cílení na konkrétní hlavní verzi modulu runtime s použitím `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace v aplikaci funkce. To platí pro obě veřejné a zobrazit jejich náhled verze. Funkce aplikace se ukládají na zadaný hlavní runtime verze, dokud se nerozhodnete explicitně přesunout na novou verzi. Které funkce aplikace se aktualizuje na nový dílčí verze modulu runtime, jakmile budou k dispozici. Vedlejší verze aktualizace není zavést zásadní změny.  

Až bude nová hlavní verze veřejně dostupné, budete mít možnost Přesunout nahoru na příslušnou verzi při zobrazení aplikace funkce na portálu. Po přesunutí na novou verzi, můžete vždy použít `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace a vrátit k předchozí verzi modulu runtime.

Každé změně verzi modulu runtime způsobí, že funkce aplikaci restartovat. Poznámky k verzi pro všechny verze modulu runtime (hlavní i dílčí) jsou publikovány v [úložiště GitHub](https://github.com/Azure/azure-webjobs-sdk-script/releases).   
## <a name="view-the-current-runtime-version"></a>Zobrazit aktuální verzi modulu runtime

Pokud chcete zobrazit konkrétní runtime verzi aktuálně používá funkce aplikace použijte následující postup. 

1. V [portál Azure](https://portal.azure.com), přejděte do aplikace pro funkce a v části **nakonfigurované funkce**, zvolte **funkce nastavení aplikace**. 

    ![Vyberte nastavení aplikace – funkce](./media/functions-versions/add-update-app-setting.png)

2. V **funkce nastavení aplikace** kartě, vyhledejte **verzi modulu Runtime**. Poznámka: verze konkrétní runtime a požadovaný hlavní verzi. V následujícím příkladu hlavní verze je nastaveno na `~1.0`.
 
   ![Vyberte nastavení aplikace – funkce](./media/functions-versions/function-app-view-version.png)

## <a name="target-the-functions-version-20-runtime"></a>Cílový modul runtime verze 2.0 funkce

>[!IMPORTANT]   
> Azure Functions runtime 2.0 je ve verzi preview a aktuálně ne všechny funkce Azure Functions podporované. Další informace najdete v tématu [Azure Functions za běhu 2.0 známé problémy](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues)  

<!-- Add a table comparing the 1.x and 2.x runtime features-->

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

Funkce aplikace můžete přesunout do verze Preview verze 2.0 modul runtime na portálu Azure. V **funkce nastavení aplikace** , zvolte **beta** pod **verzi modulu Runtime**.  

   ![Vyberte nastavení aplikace – funkce](./media/functions-versions/function-app-view-version.png)

Toto nastavení odpovídá nastavení `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace pro `beta`. Vyberte **~ 1** tlačítko přesunout zpět do aktuální veřejně podporovaná hlavní verze. Rozhraní příkazového řádku Azure můžete taky aktualizovat nastavení této aplikace. 

## <a name="target-a-specific-runtime-version-from-the-portal"></a>Cíl konkrétní runtime verze z portálu.

Když potřebujete cíle hlavní verzi než aktuální hlavní verze nebo verze 2.0, je nutné nastavit `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace.

1. V [portál Azure](https://portal.azure.com), přejděte do aplikace pro funkce a v části **nakonfigurované funkce**, zvolte **nastavení aplikace**.

    ![Vyberte nastavení aplikace – funkce](./media/functions-versions/add-update-app-setting1a.png)

2. V **nastavení aplikace** kartě, vyhledejte `FUNCTIONS_EXTENSION_VERSION` nastavení a změňte hodnotu na platný hlavní verzi modulu runtime 1.x nebo `beta` pro verze 2.0. 

    ![Nastavení aplikace – funkce](./media/functions-versions/add-update-app-setting2.png)

3. Klikněte na tlačítko **Uložit** uložit aktualizaci nastavení aplikace. 

## <a name="target-a-specific-version-using-azure-cli"></a>Cíl na konkrétní verzi pomocí rozhraní příkazového řádku Azure

 Můžete také nastavit `FUNCTIONS_EXTENSION_VERSION` z příkazového řádku Azure. Použití Azure CLI, aktualizovat nastavení aplikace v aplikaci funkce pomocí [az functionapp konfigurace appsettings sadu](/cli/azure/functionapp/config/appsettings#set) příkaz.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
V tomto kódu nahraďte `<function_app>` s názvem aplikace funkce. Také nahraďte `<my_resource_group>` s názvem skupiny prostředků pro funkce aplikace. Nahraďte `<version>` s platnou hlavní verzi modulu runtime 1.x nebo `beta` pro verze 2.0. 

Můžete spustit tento příkaz z [prostředí cloudu Azure](../cloud-shell/overview.md) výběrem **vyzkoušet** v předchozí ukázce kódu. Můžete také [rozhraní příkazového řádku Azure místně](/cli/azure/install-azure-cli) k provedení tohoto příkazu po provedení [az přihlášení](/cli/azure#az_login) k přihlášení.
