<properties
    pageTitle="Začínáme se sadou SDK IoT Hub Gateway | Microsoft Azure"
    description="Návod pro sadu SDK Azure IoT Hub Gateway při použití systému Linux ukazuje klíčové koncepty, které byste měli při použití sady SDK Azure IoT Hub Gateway chápat."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>



# Sada IoT Gateway (beta verze) – začátky použití v systému Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## Postup pro sestavení ukázky

Než začnete, musíte [připravit vývojové prostředí][lnk-setupdevbox] pro práci se sadou SDK v systému Linux.

1. Otevřete prostředí.
2. Přejděte do kořenové složky místní kopie úložiště **azure-iot-gateway-sdk**.
3. Spusťte skript **tools/build.sh**. Tento skript vytvoří v kořenové složce místní kopie úložiště **azure-iot-gateway-sdk** pomocí nástroje **cmake** složku s názvem **build** a vygeneruje soubor pravidel. Skript potom sestaví řešení a spustí testy.

> [AZURE.NOTE]  Pokaždé když spustíte skript **build.sh**, odstraní a potom znovu vytvoří složku **build** v kořenové složce místní kopie úložiště **azure-iot-gateway-sdk**.

## Spuštění ukázky

1. Skript **build.sh** generuje výstup ve složce **build** v místní kopii úložiště **azure-iot-gateway-sdk**. To zahrnuje dva moduly používané v tomto příkladu.

    Skript umístí modul **liblogger_hl.so** do složky **build/modules/logger/** a modul **libhello_world_hl.so** do složky **build/modules/hello_world/**. Tyto cesty použijte jako hodnotu **module path**, jak vidíte v následujícím souboru nastavení JSON.

2. Soubor **hello_world_lin.json** ve složce **samples/hello_world/src** je příklad souboru nastavení JSON pro Linux, který můžete použít ke spuštění ukázky. Následující příklad nastavení JSON vychází z předpokladu, že budete ukázku spouštět z kořenové složky místní kopie úložiště **azure-iot-gateway-sdk**.

3. U modulu **logger_hl** nastavte v části **args** hodnotu **filename** na název a cestu souboru, který bude obsahovat data protokolu.

    Toto je příklad souboru nastavení JSON pro Linux, který bude zapisovat do souboru **log.txt** ve složce, ve které spustíte ukázku.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Ve svém prostředí přejděte do složky **azure-iot-gateway-sdk**.
4. Spusťte následující příkaz:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Sep16_HO3-->


