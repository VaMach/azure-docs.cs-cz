## Příklad typického výstupu

Níže je uveden příklad výstupu zapsaného do souboru protokolu v příkladu Hello World. Znaky nového řádku a tabulátoru byly přidány pro čitelnost:

```
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

## Fragmenty kódu

Tato část popisuje některé klíčové části kódu v ukázce Hello World.

### Vytvoření brány

Vývojář musí napsat *proces brány*. Tento program vytvoří vnitřní infrastrukturu (sběrnici zpráv), načte moduly a nastaví všechny součásti tak, aby správně fungovaly. Sada SDK poskytuje funkci **Gateway_Create_From_JSON**, která umožňuje spustit bránu ze souboru JSON. Pokud chcete použít funkci **Gateway_Create_From_JSON**, musíte jí předat cestu k souboru JSON s informacemi o modulech, které chcete načíst. 

Kód pro proces brány lze najít v ukázce Hello World v souboru [main.c][lnk-main-c]. Níže uvedený fragment kódu ukazuje v zájmu čitelnosti zkrácenou verzi kódu pro proces brány. Tento program vytvoří bránu a potom počká, až uživatel stiskne klávesu **ENTER**, a bránu zruší. 

```
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

Soubor nastavení JSON obsahuje seznam modulů, které se mají načíst. Každý modul musí určovat tyto údaje:

- **module_name**: jedinečný název modulu.
- **module_path**: cesta ke knihovně obsahující modul. V systému Linux se jedná o soubor .so, v systému Windows o soubor .dll.
- **args**: libovolné konfigurační informace, které modul vyžaduje.

Následující příklad ukazuje soubor nastavení JSON, který se používá ke konfiguraci ukázky Hello World v systému Linux. Jestli modul vyžaduje argumenty, závisí na návrhu modulu. V tomto příkladu protokolovací modul přebírá argument, který určuje cestu k výstupnímu souboru. Modul Hello World nepřebírá žádné argumenty:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger_hl",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "helloworld",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ]
}
```

### Publikování zpráv modulu Hello World

Kód, který používá modul Hello World k publikování zpráv, najdete v souboru ['hello_world.c'][lnk-helloworld-c]. Níže uvedený fragment kódu ukazuje upravenou verzi s dalšími komentáři a v zájmu čitelnosti byl odstraněno ošetření některých chyb:

```
int helloWorldThread(void *param)
{
    // Create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // Add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // Set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // Set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // Create a message based on the msgConfig structure
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
            // Publish the message to the bus
            (void)MessageBus_Publish(handleData->busHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### Zpracování zpráv modulu Hello World

Modul Hello World nikdy nemusí zpracovávat všechny zprávy, které publikují ostatní moduly na sběrnici zpráv. To provádí implementace zpětného volání zpráv v modulu Hello World ve funkci no-op.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### Publikování a zpracování zpráv protokolovacího modulu

Protokolovací modul přijímá zprávy ze sběrnice zpráv a zapisuje je do souboru. Nepublikuje žádné zprávy na sběrnici zpráv. Z toho důvodu protokolovací modul nikdy nevolá funkci **MessageBus_Publish**.

Funkce **Logger_Recieve** v souboru [logger.c][lnk-logger-c] je funkce zpětného volání, která je vyvolána sběrnicí zpráv při doručení zprávy protokolovacímu modulu. Níže uvedený fragment kódu ukazuje upravenou verzi s dalšími komentáři a v zájmu čitelnosti byl odstraněno ošetření některých chyb:

```
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

## Další kroky

Další informace o použití sady Gateway SDK naleznete v následujících tématech:

- [IoT Gateway SDK – odesílání zpráv zařízení do cloudu pomocí simulovaného zařízení v systému Linux][lnk-gateway-simulated].
- [Azure IoT Gateway SDK][lnk-gateway-sdk] na GitHubu.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md

<!--HONumber=Sep16_HO3-->


