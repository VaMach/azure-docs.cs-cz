---
title: "Modul služby Azure IoT Edge vytvořit pomocí jazyka C# | Microsoft Docs"
description: "V tomto kurzu umožňující prezentovat jak napsat modulu převaděč dat zakázat pomocí nejnovější balíčky Azure IoT Edge NuGet, Visual Studio Code a C#."
services: iot-hub
author: jeffreyCline
manager: timlt
keywords: Azure iot, kurz, modul, nuget, vscode, csharp, edge
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: jcline
ms.openlocfilehash: 7175ffc8de2c043593d61143b402484d33e4a8cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a>Vytvořte modul Azure IoT Edge s C & #x23;

V tomto kurzu umožňující prezentovat vytvoření modulu pro `Azure IoT Edge` pomocí `Visual Studio Code` a `C#`.

V tomto kurzu jsme provede procesem nastavení prostředí a jak napsat [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč data pomocí poslední `Azure IoT Edge NuGet` balíčky. 

>[!NOTE]
Tento kurz používá `.NET Core SDK`, který podporuje mezi různými platformami. Následující kurz je napsán pomocí `Windows 10` operačního systému. Některé příkazy v tomto kurzu se můžou lišit v závislosti na vaší `development environment`. 

## <a name="prerequisites"></a>Požadavky

V této části jsme nastavení prostředí pro `Azure IoT Edge` vývoj modulu. Platí pro obě **64bitová verze Windows** a **64-bit Linux (8 Ubuntu/Debian)** operační systémy.

Je vyžadován následující software:

- [Git klienta](https://git-scm.com/downloads)
- [Sada .NET Core SDK](https://www.microsoft.com/net/core#windowscmd)
- [Visual Studio Code](https://code.visualstudio.com/)

Není potřeba naklonujte úložiště pro tuto ukázku, ale všechny ukázkový kód popsané v tomto kurzu se nachází v úložišti následující:

- `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a>Začínáme

1. Nainstalujte `.NET Core SDK`.
2. Nainstalujte `Visual Studio Code` a `C# extension` v sadě Visual Studio Marketplace kódu.

Zobrazit [rychlý kurz video](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) o tom, jak začít používat `Visual Studio Code` a `.NET Core SDK`.

## <a name="creating-the-azure-iot-edge-converter-module"></a>Vytváření modulu převaděč Azure IoT Edge

1. Inicializace novou `.NET Core` projektu knihovny jazyka C# – třída:
    - Otevřete příkazový řádek (`Windows + R` -> `cmd` -> `enter`).
    - Přejděte do složky, kde chcete vytvořit `C#` projektu.
    - Typ **dotnet nové classlib -o IoTEdgeConverterModule -f netstandard1.3**. 
    - Tento příkaz vytvoří prázdný třídu s názvem `Class1.cs` ve vašem adresáři projektů.
2. Přejděte do složky, kde právě vytvořený projektu knihovny tříd zadáním **cd IoTEdgeConverterModule**.
3. Otevřete projekt ve `Visual Studio Code` zadáním **kód.**.
4. Po otevření projektu v `Visual Studio Code`, klikněte na **IoTEdgeConverterModule.csproj** k otevření souboru, jak je znázorněno na následujícím obrázku:

    ![Visual Studio Code okno úprav.](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. Vložit `XML` blob uvedené v následující fragment kódu mezi ukončovací `PropertyGroup` značky a zavření `Project` značky; řádek šest na předchozím obrázku a uložte soubor stisknutím `Ctrl`  +  `S`.

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. Po uložení `.csproj` souboru `Visual Studio Code` by se zobrazit výzva s `unresolved dependencies` dialogové okno, jak je vidět na následujícím obrázku: 

    ![Visual Studio Code obnovení závislosti dialogové okno](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    (a) klikněte na tlačítko `Restore` chcete obnovit všechny odkazy v projektech `.csproj` včetně souborů `PackageReferences` jsme přidali. 

    b) `Visual Studio Code` automaticky vytvoří `project.assets.json` soubor ve vašich projektů `obj` složky. Tento soubor obsahuje informace o závislostech vašeho projektu aby následné obnovení rychlejší.
 
    >[!NOTE]
    `.NET Core Tools`jsou nyní využívající MSBuild. To znamená `.csproj` místo k vytvoření souboru projektu `project.json`.

    - Pokud `Visual Studio Code` nezobrazuje výzvy k zadání, který je v pořádku, jsme můžete provést ručně. Otevřete `Visual Studio Code` integrované okno terminálu stisknutím `Ctrl`  +  `backtick` klíčů nebo používat nabídky `View`  ->  `Integrated Terminal`.
    - V `Integrated Terminal` okno – typ **dotnet obnovení**.
    
7. Přejmenujte `Class1.cs` do souboru `BleConverterModule.cs`. 

    (a) Chcete-li přejmenovat soubor nejdřív kliknutím na soubor stiskněte `F2` klíč.
    
    b) zadejte nový název **BleConverterModule**, jak je vidět na následujícím obrázku:

    ![Visual Studio Code přejmenování třídy](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. Nahraďte stávající kód v `BleConverterModule.cs` souboru zkopírováním a vložením následující fragment kódu do vaší `BleConverterModule.cs` souboru.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Globalization;
   using System.Linq;
   using System.Text;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace IoTEdgeConverterModule
   {
       public class BleConverterModule : IGatewayModule, IGatewayModuleStart
       {
           private Broker broker;
           private string configuration;
           private int messageCount;

           public void Create(Broker broker, byte[] configuration)
           {
               this.broker = broker;
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Start()
           {
           }

           public void Destroy()
           {
           }

           public void Receive(Message received_message)
           {
               string recMsg = Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               BleData receivedData = JsonConvert.DeserializeObject<BleData>(recMsg);

               float temperature = float.Parse(receivedData.Temperature, CultureInfo.InvariantCulture.NumberFormat); 
               Dictionary<string, string> receivedProperties = received_message.Properties;
            
               Dictionary<string, string> properties = new Dictionary<string, string>();
               properties.Add("source", receivedProperties["source"]);
               properties.Add("macAddress", receivedProperties["macAddress"]);
               properties.Add("temperatureAlert", temperature > 30 ? "true" : "false");
   
               String content = String.Format("{0} \"deviceId\": \"Intel NUC Gateway\", \"messageId\": {1}, \"temperature\": {2} {3}", "{", ++this.messageCount, temperature, "}");
               Message messageToPublish = new Message(content, properties);
   
               this.broker.Publish(messageToPublish);
           }
       }
   }
   ```

9. Uložte soubor stisknutím `Ctrl`  +  `S`.

10. Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče, jak je vidět na následujícím obrázku:

    ![Visual Studio Code nový soubor](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. K deserializaci `JSON` objekt, který jsme obdrželi od simulované `BLE` zařízení, zkopírujte následující kód do `Untitled-1` okno editoru kódu souboru. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace IoTEdgeConverterModule
   {
       public class BleData
       {
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

12. Uložte soubor jako `BleData.cs` stisknutím `Ctrl`  +  `Shift`  +  `S` klíče.
    - Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)` jak je vidět na následujícím obrázku:

    ![Visual Studio Code uložit jako dialogové okno](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče.

14. Zkopírujte a vložte následující fragment kódu do `Untitled-1` souboru. Tato třída je `Azure IoT Edge` modul, který používáme k vypsání data přijatá z našich `BleConverterModule`.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace PrinterModule
   {
       public class DotNetPrinterModule : IGatewayModule
       {
           private string configuration;
           public void Create(Broker broker, byte[] configuration)
           {
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Destroy()
           {
           }
   
           public void Receive(Message received_message)
           {
               string recMsg = System.Text.Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               Dictionary<string, string> receivedProperties = received_message.Properties;
               
               BleConverterData receivedData = JsonConvert.DeserializeObject<BleConverterData>(recMsg);
   
               Console.WriteLine();
               Console.WriteLine("Module           : [DotNetPrinterModule]");
               Console.WriteLine("received_message : {0}", recMsg);
   
               if(received_message.Properties["source"] == "bleTelemetry")
               {
                   Console.WriteLine("Source           : {0}", receivedProperties["source"]);
                   Console.WriteLine("MAC address      : {0}", receivedProperties["macAddress"]);
                   Console.WriteLine("Temperature Alert: {0}", receivedProperties["temperatureAlert"]);
                   Console.WriteLine("Deserialized Obj : [BleConverterData]");
                   Console.WriteLine("  DeviceId       : {0}", receivedData.DeviceId);
                   Console.WriteLine("  MessageId      : {0}", receivedData.MessageId);
                   Console.WriteLine("  Temperature    : {0}", receivedData.Temperature);
               }
   
               Console.WriteLine();
           }
       }
   }
   ```

15. Uložte soubor jako `DotNetPrinterModule.cs` stisknutím `Ctrl`  +  `Shift`  +  `S`.
    - Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)`.

16. Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče.

17. K deserializaci `JSON` objekt, který jsme obdrželi od `BleConverterModule`, kopírování a vkládání následující fragment kódu do kódu `Untitled-1` souboru. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace PrinterModule
   {
       public class BleConverterData
       {
           [JsonProperty(PropertyName = "deviceId")]
           public string DeviceId { get; set; }
   
           [JsonProperty(PropertyName = "messageId")]
           public string MessageId { get; set; }
   
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

18. Uložte soubor jako `BleConverterData.cs` stisknutím `Ctrl`  +  `Shift`  +  `S`.
    - Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)`.

19. Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče.

20. Zkopírujte a vložte následující fragment kódu do `Untitled-1` souboru.

   ```json
   {
       "loaders": [
           {
               "type": "dotnetcore",
               "name": "dotnetcore",
               "configuration": {
                   "binding.path": "dotnetcore.dll",
                   "binding.coreclrpath": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\coreclr.dll",
                   "binding.trustedplatformassemblieslocation": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\"
               }
           }
       ],
          "modules": [
           {
               "name": "simulated_device",
               "loader": {
                   "name": "native",
                   "entrypoint": {
                       "module.path": "simulated_device.dll"
                   }
               },
               "args": {
                   "macAddress": "01:02:03:03:02:01",
                   "messagePeriod": 500
               }
           },
           {
               "name": "ble_converter_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "IoTEdgeConverterModule.BleConverterModule"
                   }
               },
               "args": ""
           },
           {
               "name": "printer_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "PrinterModule.DotNetPrinterModule"
                   }
               },
               "args": ""
           }
       ],
       "links": [
           {
               "source": "simulated_device", "sink": "ble_converter_module"
           },
           {
               "source": "ble_converter_module", "sink": "printer_module"
           }
   ]
   }
   ```

21. Uložte soubor jako `gw-config.json` stisknutím `Ctrl`  +  `Shift`  +  `S`.
    - Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.

22. Povolit kopírování konfiguračního souboru do výstupního adresáře, aktualizovat `IoTEdgeConverterModule.csproj` s objektem blob následující XML:

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - Aktualizovaný `IoTEdgeConverterModule.csproj` by měl vypadat jako na následujícím obrázku:

    ![Visual Studio Code aktualizovat souboru .csproj](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče.

24. Zkopírujte a vložte následující fragment kódu do `Untitled-1` souboru.

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. Uložte soubor jako `binplace.ps1` stisknutím `Ctrl`  +  `Shift`  +  `S`.
    - Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.

26. Sestavení projektu stisknutím `Ctrl`  +  `Shift`  +  `B` klíče. Při sestavování projektu poprvé, `Visual Studio Code` vyzve vás s `No build task defined.` dialogové okno, jak je vidět na následujícím obrázku:

    ![Dialogové okno úloha sestavení Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    a) klikněte na tlačítko `Configure Build Task` tlačítko.

    b) v `Select a Task Runner` dialogové okno rozevírací nabídce. Vyberte `.NET Core` jak je vidět na následujícím obrázku: 

    ![Visual Studio Code vyberte dialogové okno úloha](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    c) kliknutím `.NET Core` vytvoří položku `tasks.json` ve vaší `.vscode` adresáře a otevře soubor v `code editor` okno. Není potřeba pro úpravu tohoto souboru, zavřete kartu.

27.  Otevřete `Visual Studio Code` integrované okno terminálu stisknutím `Ctrl`  +  `backtick` klíčů nebo používat nabídky `View`  ->  `Integrated Terminal` a typ **.\binplace.ps1** na `PowerShell` příkazového řádku. Tento příkaz zkopíruje všechny naše závislosti do výstupního adresáře.

28. Přejděte do výstupního adresáře projektů v `Integrated Terminal` okno zadáním **cd.\bin\Debug\netstandard1.3**.

29. Spustit ukázkový projekt zadáním **. \gw.exe gw-config.json** do `Integrated Terminal` okno řádku. 
    - Pokud jste postupovali podle kroků v tomto kurzu úzce, zda nyní je spuštěna `Azure IoT Edge BLE Data Converter Module` ukázkový projekt, jak je vidět na následujícím obrázku:
    
        ![Příklad Simulovaná zařízení spuštěná v kódu Visual Studio](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - Pokud chcete aplikaci ukončit, stiskněte `<Enter>` klíč.

>[!IMPORTANT]
Nedoporučuje se používat `Ctrl`  +  `C` ukončit `IoT Edge` brány aplikace (tedy **gw.exe**). Protože tato akce může způsobit neobvyklým způsobem ukončení procesu.

