---
title: "Použití Visual Studio Code k ladění modulu jazyka C# s hranou Azure IoT | Microsoft Docs"
description: "Ladění modulu jazyka C# s Azure IoT hranou v produktu VS Code"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1ab67cd8aaf59cde3157fcb877ce13f10cb432bb
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-debug-c-module-with-azure-iot-edge"></a>Pomocí Visual Studio Code ladění jazyka C# modul s hranou Azure IoT
Tento článek obsahuje podrobné pokyny pro používání [Visual Studio Code](https://code.visualstudio.com/) jako hlavní vývojový nástroj k ladění moduly IoT okraj.

## <a name="prerequisites"></a>Požadavky
Tento kurz předpokládá, že používáte počítač nebo virtuální počítač se systémem Windows nebo Linux jako vývojovém počítači. Zařízení IoT Edge může být jiné fyzické zařízení nebo zařízení IoT Edge můžete simulovat na vývojovém počítači.

Ujistěte se, že máte dokončené následující kurzy, než začnete v tomto návodu.
- [Použití kódu v jazyce Visual Studio k vývoji modulu jazyka C# s hranou Azure IoT](how-to-vscode-develop-csharp-module.md)

Po dokončení předchozí kurz, byste měli mít připravený, následující položky
- Místní registru Docker na vývojovém počítači spuštěna. Je navržený pro použití místního registru Docker pro prototypu a účely testování.
- `Program.cs` Soubor s nejnovější kód modulu filtru.
- Aktualizované `deployment.json` soubor pro modul senzor a filtru modulu.
- Modulu runtime Edge na vývojovém počítači spuštěna.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Sestavení modulu IoT Edge pro účely ladění
1. Spustit ladění, budete muset použít **dockerfile.debug** znovu sestavte docker image a znovu nasaďte řešení okraj. V Průzkumníku VS Code klikněte na složku Docker ho otevřete. Klikněte `linux-x64` složku, klikněte pravým tlačítkem myši **Dockerfile.debug**a klikněte na tlačítko **sestavení IoT Edge modulu Docker image**.

    ![Sestavení bitové kopie pro ladění](./media/how-to-debug-csharp-module/build-debug-image.png)

3. V **vyberte složku** okně vyhledejte nebo zadejte `./bin/Debug/netcoreapp2.0/publish`. Klikněte na tlačítko **vyberte složku EXE_DIR**.
4. Automaticky otevírané okno textového pole v horní části okna VS Code zadejte název bitové kopie. Například: `<your container registry address>/filtermodule:latest`. Když nasazujete do místního registru, měla by být `localhost:5000/filtermodule:latest`.
5. Do úložiště Docker push bitovou kopii. Použití **Edge: Push IoT Edge modulu Docker image** příkaz a zadejte adresu URL bitové kopie do místní textového pole v horní části okna VS Code. Použijte stejnou adresu URL obrázku jste použili v výše krok.
6. Můžete opakovaně použít `deployment.json` znovu nasadit. V příkazu palety, typ a vyberte **Edge: restartujte Edge** získat modul filtr s ladicí verzi.

## <a name="start-debugging-in-vs-code"></a>Spuštění ladění v produktu VS Code
1. Přejděte do okna ladění VS Code. Stiskněte klávesu **F5** a vyberte **IoT Edge(.Net Core)**

    ![Stisknutím klávesy F5](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. V `launch.json`, přejděte na **ladění IoT Edge vlastní modul (.NET Core)** části a vyplňte `<container_name>`pod `pipeArgs`. Měla by být `filtermodule` v tomto kurzu.

    ![Upravit pipeArgs](./media/how-to-debug-csharp-module/f5-debug-option.png)

3. Přejděte do souboru Program.cs. Přidejte zarážku v `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Stiskněte klávesu **F5** znovu. A vyberte možnost připojit k procesu. V tomto kurzu musí být název procesu`FilterModule.dll`

    ![Připojit proces](./media/how-to-debug-csharp-module/attach-process.png)

5. V okně VS ladit kód se zobrazí proměnné v levém panelu. 

> [!NOTE]
> Výše příklad ukazuje, jak ladění rozhraní .net jádro IoT Edge modulů na kontejnery. Je založen na verzi ladění `Dockerfile.debug`, což zahrnuje VSDBG (.NET Core příkazového řádku ladicího programu) v bitové kopii kontejneru při jeho vytváření. Doporučujeme, abyste přímo použít nebo si přizpůsobit `Dockerfile` bez VSDBG pro produkční prostředí IoT Edge moduly po dokončení ladění moduly jazyka C#.

## <a name="next-steps"></a>Další postup

V tomto kurzu vytvořili modul IoT okraj a nasazuje pro účely ladění a spuštění ladění v produktu VS Code. Můžete pokračovat na některý z následujících kurzech Další informace o další scénáře při vývoji Azure IoT Edge v produktu VS Code. 

> [!div class="nextstepaction"]
> [Vývoj a nasazení modulu jazyka C# v produktu VS Code](how-to-vscode-develop-csharp-module.md)
