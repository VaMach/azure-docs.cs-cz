---
title: "Funkce Azure s Azure IoT hranou ladění pomocí Visual Studio Code | Microsoft Docs"
description: "Ladění jazyka C# funkce Azure s Azure IoT Edge v produktu VS Code"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: db86a08a19e97f8f415849aa060fe87d77cccf68
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Pomocí Visual Studio Code funkce Azure s Azure IoT hranou ladění

Tento článek obsahuje podrobné pokyny pro používání [Visual Studio Code](https://code.visualstudio.com/) jako hlavní vývojový nástroj k ladění funkcí Azure IoT hranu.

## <a name="prerequisites"></a>Požadavky
Tento kurz předpokládá, že používáte počítač nebo virtuální počítač se systémem Windows nebo Linux jako vývojovém počítači. Zařízení IoT Edge může být jiné fyzické zařízení nebo zařízení IoT Edge můžete simulovat na vývojovém počítači.

Ujistěte se, že máte dokončené následující kurzy, než začnete v tomto návodu.
- [Pomocí kódu v jazyce Visual Studio pro vývoj a nasazení Azure Functions okraj Azure IoT](how-to-vscode-develop-azure-function.md)

Po dokončení předchozí kurz, byste měli mít připravený, následující položky
- Místní registru Docker na vývojovém počítači spuštěna. Je navržený pro použití místního registru Docker pro prototypu a účely testování.
- `run.csx` Soubor s nejnovější kód funkce filtru.
- Aktualizované `deployment.json` soubor pro modul senzor a modul funkce filtru.
- Modulu runtime Edge na vývojovém počítači spuštěna.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Sestavení modulu IoT Edge pro účely ladění
1. Spustit ladění, budete muset použít **dockerfile.debug** znovu sestavte docker image a znovu nasaďte řešení okraj. V Průzkumníku VS Code klikněte na složku Docker ho otevřete. Klikněte `linux-x64` složku, klikněte pravým tlačítkem myši **Dockerfile.debug**a klikněte na tlačítko **sestavení IoT Edge modulu Docker image**.

    ![Sestavení bitové kopie pro ladění](./media/how-to-debug-csharp-function/build-debug-image.png)

2. V **vyberte složku** okno, přejděte na **FilterFunction** projektu a klikněte na tlačítko **vyberte složku jako EXE_DIR**.
3. Automaticky otevírané okno textového pole v horní části okna VS Code zadejte název bitové kopie. Například: `<your container registry address>/filterfunction:latest`. Když nasazujete do místního registru, měla by být `localhost:5000/filterfunction:latest`.

    ![Push bitové kopie](./media/how-to-debug-csharp-function/push-image.png)

4. Do úložiště Docker push bitovou kopii. Použití **Edge: Push IoT Edge modulu Docker image** příkaz a zadejte adresu URL bitové kopie do místní textového pole v horní části okna VS Code. Použijte stejnou adresu URL obrázku jste použili v výše krok.
5. Můžete opakovaně použít `deployment.json` znovu nasadit. V příkazu palety, typ a vyberte **Edge: restartujte Edge** získat funkce filtru s ladicí verzi.

## <a name="start-debugging-in-vs-code"></a>Spuštění ladění v produktu VS Code
1. Přejděte do okna ladění VS Code. Stiskněte klávesu **F5** a vyberte **IoT Edge(.Net Core)**

    ![Stisknutím klávesy F5](./media/how-to-debug-csharp-function/f5-debug-option.png)

2. V `launch.json`, přejděte na **ladění funkce Edge IoT (.NET Core)** části a vyplňte `<container_name>`pod `pipeArgs`. Měla by být `filterfunction` v tomto kurzu.

    ![Update launch.json](./media/how-to-debug-csharp-function/update-launch-json.png)

3. Přejděte do run.csx. Přidáte zarážky ve funkci.
4. Přejděte do okna ladění (Ctrl + Shift + D), zvolte **ladění funkce Edge IoT (.NET Core)** z rozevíracího seznamu. 

    ![Vyberte režim ladění](./media/how-to-debug-csharp-function/choose-debug-mode.png)

5. Klikněte na tlačítko Spustit ladění nebo klikněte na tlačítko **F5**a vyberte možnost připojit k procesu.

    ![Připojit proces – funkce](./media/how-to-debug-csharp-function/attach-function-process.png)

6. V okně VS ladit kód se zobrazí proměnné v levém panelu. 

> [!NOTE]
> Výše příklad ukazuje, jak ladění rozhraní .net Core IoT Edge funkce kontejnerům. Je založen na verzi ladění `Dockerfile.debug`, což zahrnuje VSDBG (.NET Core příkazového řádku ladicího programu) v bitové kopii kontejneru při jeho vytváření. Doporučujeme, abyste přímo použít nebo si přizpůsobit `Dockerfile` bez VSDBG pro produkční prostředí IoT Edge funkci po dokončení ladění funkce jazyka C#.

## <a name="next-steps"></a>Další postup

V tomto kurzu vytvořit funkci Azure a nasadit IoT okraj pro účely ladění a spuštění ladění v produktu VS Code. Můžete pokračovat na některý z následujících kurzech Další informace o další scénáře při vývoji Azure IoT Edge v produktu VS Code. 

> [!div class="nextstepaction"]
> [Vývoj a nasazení modulu jazyka C# v produktu VS Code](how-to-vscode-develop-csharp-module.md)

