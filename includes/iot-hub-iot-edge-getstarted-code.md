## <a name="typical-output"></a>Příklad typického výstupu

Následující příklad ukazuje výstup do souboru protokolu zapíše Hello, World vzorek. Výstup je z důvodů lepší čitelnosti formátovaný:

```json
[{
    "time": "Mon Apr 11 13:42:50 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:42:50 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:42:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:43:00 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:45:00 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Fragmenty kódu

Tato část popisuje některé klíčové části kódu v ukázce hello\_world.

### <a name="iot-edge-gateway-creation"></a>Vytvoření brány IoT Edge

Chcete-li vytvořit bránu, implementujte *procesu gateway*. Tento program vytvoří interní infrastruktury (zprostředkovatel), načte moduly IoT okraj a nakonfiguruje proces brány. IoT Edge poskytuje funkci **Gateway\_Create\_From\_JSON**, která umožňuje spustit bránu ze souboru JSON. Použít **brány\_vytvořit\_z\_JSON** fungovat, předejte ji do souboru JSON, který určuje moduly IoT Edge se načíst cestu.

Můžete najít kód pro proces brány v *Hello, World* ukázku v [main.c] [ lnk-main-c] souboru. Následující fragment kódu ukazuje v zájmu čitelnosti zkrácenou verzi kódu pro proces brány. Tento ukázkový program vytvoří bránu a potom počká, až uživatel stiskne klávesu **ENTER**, a bránu zruší.

```C
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

Nastavení souboru JSON obsahuje seznam modulů IoT Edge načíst a odkazů mezi moduly. Každý modul IoT Edge musíte zadat a:

* **name**: jedinečný název modulu.
* **loader:** zavaděč, který umí načíst požadovaný modul. Zavaděče jsou rozšiřovacím bodem pro načítání různých typů modulů. Okraj IoT poskytuje zavaděče pro použití s moduly, které jsou napsané v nativní C, Node.js, Java a rozhraní .NET. Hello World vzorek pouze používá nativní zavaděč C, protože všechny moduly, které jsou v této ukázce jsou dynamické knihovny, které jsou napsané v C. Další informace o tom, jak používat moduly IoT Edge napsané v různých jazycích, najdete v článku [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), nebo [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) ukázky.
    * **název**: název zavaděč používá k načtení modulu.
    * **entrypoint**: cesta ke knihovně obsahující modul. V systému Linux Tato knihovna je soubor .so, v systému Windows tato knihovna je soubor s příponou DLL. Vstupní bod se odvíjí od typu použitého zavaděče. Vstupní bod zavaděč Node.js je soubor .js. Vstupní bod Java zavaděč je cestu pro třídy a název třídy. Vstupní bod zavaděč .NET je název sestavení a název třídy.

* **args**: libovolné konfigurační informace, které modul vyžaduje.

Následující kód ukazuje JSON používá k deklaraci levého okraje IoT moduly pro ukázku Hello World v systému Linux. Jestli modul vyžaduje nějaké argumenty, závisí na návrhu modulu. V tomto příkladu protokolovací modul přebírá argument, který určuje cestu k výstupnímu souboru, a modul hello\_world nemá žádné argumenty.

```json
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

Soubor JSON obsahuje také propojení mezi moduly, která se předávají do zprostředkovatele. Propojení má dvě vlastnosti:

* **Zdroj**: název modulu z `modules` části nebo `\*`.
* **sink**: název modulu z části `modules`

Každé propojení definuje trasu a směr zpráv. Zprávy z **zdroj** modulu budou doručeny do **podřízený** modulu. Můžete nastavit **zdroj** modulu `\*`, což znamená, že **podřízený** modul přijímá zprávy od libovolný modul.

Následující kód ukazuje kód JSON, který konfiguruje propojení mezi moduly použitými v ukázce hello\_world na Linuxu. Všechny zprávy vytvořené modulem `hello_world` využívá modul `logger`.

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a>Publikování zpráv modulu hello\_world

Kód, který používá modul hello\_world k publikování zpráv, najdete v souboru [hello_world.c][lnk-helloworld-c]. Následující fragment kódu ukazuje upravenou verzi kódu s přidanými komentáři a v zájmu čitelnosti je odstraněné ošetření některých chyb:

```C
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

Hello\_world modul nikdy zpracovává zprávy, které z ostatních modulů IoT Edge publikování pro zprostředkovatele. Proto implementace zpětného volání zpráv v modulu hello\_world je funkcí no-op.

```C
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-processing"></a>Zpracování zpráv modulu protokolovacího nástroje

Protokolovací modul přijímá zprávy od zprostředkovatele a zapisuje je do souboru. Nikdy publikuje žádné zprávy. Z toho důvodu protokolovací modul nikdy nevolá funkci **Broker_Publish**.

**Logger_Receive** v fungovat [logger.c] [ lnk-logger-c] soubor je zpětné volání zprostředkovatele vyvolá pro doručení zprávy do modulu protokolovacího nástroje. Následující fragment kódu ukazuje upravenou verzi kódu s přidanými komentáři. V zájmu čitelnosti je odstraněné ošetření některých chyb:

```C
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Další kroky

V tomto článku jste spustili jednoduché IoT hraniční bránu, která zapisuje zprávy do souboru protokolu. Pokud chcete spustit ukázku, která odesílá zprávy do služby IoT Hub, najdete v části:

- [Okraj IoT – odesílání zpráv typu zařízení cloud s simulované zařízení pomocí Linux][lnk-gateway-simulated-linux] 
- [Okraj IoT – odesílání zpráv typu zařízení cloud s simulované zařízení pomocí Windows][lnk-gateway-simulated-windows].


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md