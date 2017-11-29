---
title: "Nainstalujte rozšíření trvanlivý funkce a vzorky – Azure"
description: "Naučte se nainstalovat rozšíření trvanlivý funkce pro funkce Azure pro vývoj portálu nebo vývoj sady Visual Studio."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 03326304e438f3b716c43d268f07d8e22cd15ea3
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Nainstalujte rozšíření trvanlivý funkce a ukázky (Azure Functions)

[Trvanlivý funkce](durable-functions-overview.md) rozšíření pro Azure Functions je součástí balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Tento článek ukazuje, jak nainstalovat balíček a sady vzorků pro následující vývojových prostředí:

* Visual Studio 2017 (doporučeno) 
* portál Azure

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio teď poskytuje dosažení co nejlepších výsledků pro vývoj aplikací, které používají trvanlivý funkce.  Funkce můžete spustit místně a také mohou být publikovány do služby Azure. Můžete spustit s prázdným projektem nebo sadu ukázkových funkcích.

### <a name="prerequisites"></a>Požadavky

* Nainstalujte [nejnovější verze sady Visual Studio](https://www.visualstudio.com/downloads/) (verze 15.3 nebo novější). Zahrnout **Azure development** zatížení v možnostech instalace.

### <a name="start-with-sample-functions"></a>Začněte s ukázkových funkcích

1. Stažení [soubor .zip ukázková aplikace pro sadu Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Nepotřebujete přidat odkaz NuGet, protože ukázkový projekt již existuje.
2. Nainstalujte a spusťte [emulátoru úložiště Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) verze 5.2 nebo novější. Alternativně můžete aktualizovat *local.appsettings.json* soubor s skutečné připojovacích řetězců Azure Storage.
3. Otevřete projekt ve Visual Studio 2017. 
4. Pokyny ke spuštění ukázky začínat [funkce řetězení – Hello ukázka pořadí](durable-functions-sequence.md). Ukázku můžete spustit místně nebo publikované do Azure.

### <a name="start-with-an-empty-project"></a>Začít s prázdným projektem
 
Postupujte podle pokynů k stejné jako u počínaje ukázku, ale proveďte následující kroky místo stahování *.zip* souboru:

1. Vytvoření projektu funkce aplikace.
2. Přidejte následující odkaz balíčku NuGet pro vaše *.csproj* souboru:

   ```xml
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.DurableTask" Version="1.0.0-beta" />
   ```

## <a name="azure-portal"></a>portál Azure

Pokud dáváte přednost, můžete portál Azure pro vývoj trvanlivý funkce.

### <a name="create-an-orchestrator-function"></a>Vytvoření funkce produktu orchestrator

1. Vytvořit novou aplikaci funkce v [functions.azure.com](https://functions.azure.com/signin).
2. Konfigurace funkce aplikaci [použít verzi 2.0 runtime](functions-versions.md).
3. Vytvořte novou funkci, tak, že zvolíte "C" pro jazyk a "Vše" pro scénář. Vyberte **trvanlivý Orchestrator funkce – C#** šablony.
4. V části **není nainstalovaná rozšíření**, klikněte na tlačítko **nainstalovat** ke stažení rozšíření od NuGet.org.

### <a name="copy-sample-code-to-the-function-app"></a>Zkopírujte ukázkový kód do aplikaci funkce

1. Stažení [DFSampleApp.zip](https://github.com/Azure/azure-functions-durable-extension/raw/master/docfx/files/DFSampleApp.zip) souboru.
2. Rozbalte ukázkový soubor do `D:\home\site\wwwroot` v aplikaci funkce, pomocí modulu Kudu nebo FTP.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Spuštění funkce řetězení ukázka](durable-functions-sequence.md)
