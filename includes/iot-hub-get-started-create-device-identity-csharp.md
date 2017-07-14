## Vytvoření identity zařízení
<a id="create-a-device-identity" class="xliff"></a>
V této části vytvoříte konzolovou aplikaci .NET, která v registru identit ve službě IoT Hub vytvoří identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit. Další informace najdete v části Registr identit v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub.  V ID zařízení se rozlišují malá a velká písmena.

1. V sadě Visual Studio přidejte k novému řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt nazvěte **CreateDeviceIdentity** a řešení nazvěte **IoTHubGetStarted**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **CreateDeviceIdentity** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.
3. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.
   
    ![Okno Správce balíčků NuGet][11]
4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. Přidejte následující metodu do třídy **Program**:
   
        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }
   
    Tato metoda vytvoří identitu zařízení s ID **myFirstDevice**. (Pokud toto ID zařízení již v registru identit existuje, kód jednoduše načte informace o stávajícím zařízení.) Aplikace pak zobrazí primární klíč pro danou identitu. Tento klíč v aplikaci simulovaného zařízení slouží k připojení ke službě IoT Hub.
7. Nakonec do metody **Main** přidejte následující řádky:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. Spusťte aplikaci a poznamenejte si klíč zařízení.
   
    ![Klíč zařízení generovaný aplikací][12]

> [!NOTE]
> V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub. Ukládají se zde ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
