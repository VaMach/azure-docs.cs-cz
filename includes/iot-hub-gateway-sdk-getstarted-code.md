## <a name="typical-output"></a>Příklad typického výstupu

Dál je uvedený příklad výstupu zapsaného do souboru protokolu v ukázce Hello World. Výstup je z důvodů lepší čitelnosti formátovaný:

```json
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Fragmenty kódu

Tato část popisuje některé klíčové části kódu v ukázce hello\_world.

### <a name="gateway-creation"></a>Vytvoření brány

Vývojář musí napsat *proces brány*. Tento program vytvoří vnitřní infrastrukturu (zprostředkovatele), načte moduly a nastaví všechny součásti tak, aby správně fungovaly. SDK poskytuje funkci **Gateway\_Create\_From\_JSON**, která umožňuje spustit bránu ze souboru JSON. Pokud chcete použít funkci **Gateway\_Create\_From\_JSON**, musíte jí předat cestu k souboru JSON s informacemi o modulech, které chcete načíst.

Kód pro proces brány najdete v ukázce Hello World v souboru [main.c][lnk-main-c]. Následující fragment kódu ukazuje v zájmu čitelnosti zkrácenou verzi kódu pro proces brány. Tento ukázkový program vytvoří bránu a potom počká, až uživatel stiskne klávesu **ENTER**, a bránu zruší.

```c
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

Soubor nastavení JSON obsahuje seznam modulů, které se mají načíst, a propojení mezi těmito moduly. Každý modul musí určovat tyto údaje:

* **name**: jedinečný název modulu.
* **loader:** zavaděč, který umí načíst požadovaný modul. Zavaděče jsou rozšiřovacím bodem pro načítání různých typů modulů. Poskytujeme zavaděče pro použití s moduly napsanými v nativním C, Node.js, Javě a .NET. Ukázka Hello World používá pouze nativní zavaděč, protože všechny moduly v této ukázce jsou dynamické knihovny napsané v jazyce C. Další informace o používání modulů napsaných v jiných jazycích najdete v ukázkách [Node.js](https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/java_sample) a [.NET](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/dotnet_binding_sample).
    * **name**: název zavaděče sloužícího k načtení modulu.
    * **entrypoint**: cesta ke knihovně obsahující modul. V Linuxu je touto knihovnou soubor .so, v systému Windows soubor .dll. Vstupní bod se odvíjí od typu použitého zavaděče. Vstupním bodem zavaděče Node.js je soubor .js. Vstupním bodem zavaděče Java je cesta ke třídě a název třídy. Vstupním bodem zavaděče .NET je název sestavení a název třídy.

* **args**: libovolné konfigurační informace, které modul vyžaduje.

Následující kód ukazuje kód JSON, který deklaruje všechny moduly pro ukázku Hello World na Linuxu. Jestli modul vyžaduje nějaké argumenty, závisí na návrhu modulu. V tomto příkladu protokolovací modul přebírá argument, který určuje cestu k výstupnímu souboru, a modul hello\_world nemá žádné argumenty.

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

* **source**: název modulu z části `modules` nebo „\*“
* **sink**: název modulu z části `modules`

Každé propojení definuje trasu a směr zpráv. Zprávy z modulu `source` se doručí do modulu `sink`. Hodnota `source` může být nastavená na „\*“, což značí, že modul `sink` přijímá zprávy ze všech modulů.

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

```c
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

### <a name="helloworld-module-message-processing"></a>Zpracování zpráv modulu hello\_world

Modul hello\_world nikdy nemusí zpracovávat zprávy, které do zprostředkovatele publikují ostatní moduly. Proto implementace zpětného volání zpráv v modulu hello\_world je funkcí no-op.

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Publikování a zpracování zpráv protokolovacího modulu

Protokolovací modul přijímá zprávy od zprostředkovatele a zapisuje je do souboru. Nikdy publikuje žádné zprávy. Z toho důvodu protokolovací modul nikdy nevolá funkci **Broker_Publish**.

Funkce **Logger_Recieve** v souboru [logger.c][lnk-logger-c] je funkce zpětného volání, kterou zprostředkovatel vyvolá při doručení zprávy protokolovacímu modulu. Následující fragment kódu ukazuje upravenou verzi kódu s přidanými komentáři. V zájmu čitelnosti je odstraněné ošetření některých chyb:

```c
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

Další informace o použití sady IoT Gateway SDK najdete v těchto článcích:

* [IoT Gateway SDK – odesílání zpráv typu zařízení-cloud pomocí simulovaného zařízení v systému Linux][lnk-gateway-simulated].
* [Azure IoT Gateway SDK][lnk-gateway-sdk] na GitHubu.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md