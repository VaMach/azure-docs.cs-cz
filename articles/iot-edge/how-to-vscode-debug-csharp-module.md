---
title: "Použití Visual Studio Code k ladění modulu jazyka C# s hranou Azure IoT | Microsoft Docs"
description: "Ladění modulu jazyka C# s Azure IoT hrany v Visual Studio Code."
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 46d9ca0bf6c9ddf95c147fc2eb62d275c973845e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Použití Visual Studio Code k ladění modulu jazyka C# s hranou Azure IoT
Tento článek obsahuje podrobné pokyny pro používání [Visual Studio Code](https://code.visualstudio.com/) jako hlavní vývojový nástroj k ladění moduly Azure IoT okraj.

## <a name="prerequisites"></a>Požadavky
Tento kurz předpokládá, že používáte počítač nebo virtuální počítač se systémem Windows nebo Linux jako vývojovém počítači. Může být zařízení IoT Edge jiného fyzického zařízení, nebo můžete simulovat zařízení IoT Edge na vývojovém počítači.

Než začnete v tomto návodu, kurzu následující:
- [Použití kódu v jazyce Visual Studio k vývoji modulu jazyka C# s hranou Azure IoT](how-to-vscode-develop-csharp-module.md)

Po dokončení předchozí kurzu byste měli mít připravené následující položky:
- Místní registru Docker na vývojovém počítači spuštěna. Toto je při vytváření prototypu a pro účely testování.
- `Program.cs` Soubor s nejnovější modul kód filtru.
- Aktualizované `deployment.json` soubor pro moduly senzor a filtr.
- Modulu runtime IoT okraj, který běží na počítači pro vývoj.

## <a name="build-your-iot-edge-module-for-debugging"></a>Sestavení pro ladění modulu IoT Edge
1. Spusťte ladění pomocí **dockerfile.debug** znovu sestavte Docker image a znovu nasaďte řešení IoT okraj. V kódu Průzkumníka Visual Studio vyberte složku Docker ho otevřete. Vyberte **linux x64** složku, klikněte pravým tlačítkem na **Dockerfile.debug**a vyberte **sestavení IoT Edge modulu Docker image**.

    ![Snímek obrazovky Průzkumník kódu VS](./media/how-to-debug-csharp-module/build-debug-image.png)

3. V **vyberte složku** okně vyhledejte nebo zadejte **./bin/Debug/netcoreapp2.0/publish**. Potom vyberte **vyberte složku jako EXE_DIR**.
4. Automaticky otevírané okno textového pole v horní části okna VS Code zadejte název bitové kopie. Například: `<your container registry address>/filtermodule:latest`. Když nasazujete do místního registru, měla by být: `localhost:5000/filtermodule:latest`.
5. Do úložiště Docker push bitovou kopii. Použití **Edge: Push IoT Edge modulu Docker image** příkaz a zadejte adresu URL bitové kopie do místní textového pole v horní části okna VS Code. Použijte stejnou adresu URL obrázku jste použili v předchozím kroku.
6. Můžete opakovaně použít `deployment.json` znovu nasadit. Příkaz palety, typ a vyberte **Edge: restartujte Edge** získat modul filtr s ladicí verzi.

## <a name="start-debugging-in-vs-code"></a>Spuštění ladění v produktu VS Code
1. Přejdete do okna VS Code ladění. Stiskněte klávesu **F5**a vyberte **IoT Edge(.NET Core)**.

    ![Snímek obrazovky VS Code ladění okna](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. V `launch.json`, vyhledejte **ladění IoT Edge vlastní modul (.NET Core)** části. V části **pipeArgs**, zadejte `<container_name>`. Měla by být `filtermodule` v tomto kurzu.

    ![Snímek obrazovky VS Code ladění okna](./media/how-to-debug-csharp-module/f5-debug-option.png)

3. Přejděte do **Program.cs**. Přidejte zarážku v `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Stiskněte klávesu **F5** znovu a vyberte možnost připojit k procesu. V tomto kurzu, musí být název procesu `FilterModule.dll`.

    ![Snímek obrazovky VS Code ladění okna](./media/how-to-debug-csharp-module/attach-process.png)

5. V okně ladění VS Code uvidíte proměnné v levém panelu. 

> [!NOTE]
> Předchozí příklad ukazuje, jak k ladění modulů .NET Core IoT Edge na kontejnery. Je založen na verzi ladění `Dockerfile.debug`, což zahrnuje VSDBG (.NET Core příkazového řádku ladicího programu) v bitové kopii kontejneru při jeho vytváření. Po dokončení ladění moduly jazyka C#, doporučujeme přímo použít nebo si přizpůsobit `Dockerfile` bez VSDBG pro produkční prostředí IoT Edge moduly.

## <a name="next-steps"></a>Další postup

V tomto kurzu jste vytvořili modul IoT okraj a nasazení pro ladění. Můžete spustit ladění v produktu VS Code. Další informace o další scénáře při vývoji Azure IoT Edge v produktu VS Code najdete v tématu: 

> [!div class="nextstepaction"]
> [Vývoj a nasazení modulu jazyka C# v produktu VS Code](how-to-vscode-develop-csharp-module.md)
