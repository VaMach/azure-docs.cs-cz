---
title: "Pochopení podpory Azure IoT Hub MQTT | Microsoft Docs"
description: "Příručka vývojáře – podpora pro zařízení připojující se k zařízení směřujících koncový bod služby IoT Hub pomocí protokolu MQTT. Obsahuje informace o předdefinovaných MQTT podporovat v sady SDK zařízení Azure IoT."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 309396badf3a4daa4c339a280f774bcd500ce3bb
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a>Komunikovat se službou IoT hub pomocí protokolu MQTT

Umožňuje zařízením komunikovat s koncovými body zařízení IoT Hub pomocí služby IoT Hub [protokoly MQTT v3.1.1] [ lnk-mqtt-org] protokolu na port 8883 nebo protokoly MQTT v3.1.1 přes protokol WebSocket na portu 443. IoT Hub vyžaduje veškerá komunikace zařízení být zabezpečené pomocí protokolu TLS/SSL (proto IoT Hub nepodporuje nezabezpečené připojení přes port 1883).

## <a name="connecting-to-iot-hub"></a>Připojení ke službě IoT Hub

Zařízení můžete použít protokol MQTT pro připojení do služby IoT hub, buď pomocí knihovny v [SDK služby Azure IoT] [ lnk-device-sdks] nebo přímo pomocí MQTT protokolu.

## <a name="using-the-device-sdks"></a>Pomocí sady SDK pro zařízení

[Sady SDK zařízení] [ lnk-device-sdks] podporující protokol MQTT jsou k dispozici pro Java, Node.js, C, C# a Python. Sady SDK zařízení použít standardní připojovací řetězec služby IoT Hub navázat připojení do služby IoT hub. Chcete-li používat protokol MQTT, musí být parametr protokol klienta nastavena **MQTT**. Ve výchozím nastavení, zařízení sady SDK připojení do služby IoT Hub s **CleanSession** příznak nastaven na hodnotu **0** a používat **QoS 1** k výměně zpráv službou IoT hub.

Když je zařízení připojené ke službě IoT hub, sady SDK zařízení poskytují metody, které umožní zařízení zprávy odesílat a přijímat zprávy ze služby IoT hub.

Následující tabulka obsahuje odkazy na ukázky kódu pro každý podporovaný jazyk a určuje parametr, který chcete použít k navázání připojení ke službě IoT Hub pomocí protokolu MQTT.

| Jazyk | Parametr protokolu |
| --- | --- |
| [Node.js][lnk-sample-node] |Azure-iot zařízení mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migrace aplikace na zařízení z AMQP na MQTT

Pokud používáte [sady SDK pro zařízení][lnk-device-sdks], přepínání pomocí protokolu AMQP k MQTT potřeba změnit parametr protokolu v inicializace klienta, jak bylo uvedeno dříve.

Když to uděláte, nezapomeňte zaškrtnout následující položky:

* AMQP vrátí chyby pro mnoho podmínek, zatímco MQTT ukončí připojení. V důsledku vaše zpracování logiky výjimek může vyžadovat některé změny.
* MQTT nepodporuje *odmítnout* operace při přijímání [zprávy typu cloud zařízení][lnk-messaging]. Pokud vaše aplikace back-end musí, obdrží odpověď z aplikace zařízení, zvažte použití [přímé metody][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Přímo pomocí protokolu MQTT
Pokud zařízení nelze použít sady SDK pro zařízení, můžete pořád připojit ke koncovým bodům veřejné zařízení pomocí protokolu MQTT na portu 8883. V **CONNECT** paketu zařízení by měl použijte následující hodnoty:

* Pro **ClientId** pole, použijte **deviceId**.
* Pro **uživatelské jméno** pole, použijte `{iothubhostname}/{device_id}/api-version=2016-11-14`, kde {iothubhostname} je úplná CName služby IoT hub.

    Například, pokud je název služby IoT hub **contoso.azure devices.net** a pokud je název vašeho zařízení **MyDevice01**, kompletní **uživatelské jméno** pole musí obsahovat `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.
* Pro **heslo** pole, použijte SAS token. Formát tokenu SAS je stejné jako pro protokoly HTTPS i AMQP:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    >[!NOTE]
    >SAS token hesla nejsou vyžadovány, pokud používáte ověřování pomocí certifikátu X.509. Další informace najdete v tématu [nastavit X.509 zabezpečení ve službě Azure IoT Hub][lnk-x509]

    Další informace o tom, jak generovat tokeny SAS, najdete v části zařízení [tokeny zabezpečení pomocí služby IoT Hub][lnk-sas-tokens].

    Při testování, můžete také použít [explorer zařízení] [ lnk-device-explorer] nástroj, který rychle vytvořit token SAS, který můžete zkopírovat a vložit do vlastního kódu:

  1. Přejděte na **správy** kartě v **Explorer zařízení**.
  2. Klikněte na tlačítko **tokenu SAS** (pravého horního).
  3. Na **SASTokenForm**, vyberte své zařízení do **DeviceID** rozevírací nabídku. Nastavit vaše **TTL**.
  4. Klikněte na tlačítko **generování** k vytvoření vašeho tokenu.

     Token SAS, který se vygeneruje má tuto strukturu: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

     Součást tento token, který bude použit jako **heslo** pole a připojte se pomocí MQTT: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

MQTT připojit a odpojit paketů, IoT Hub vydá událost na **monitorování Operations** kanál s další informace, které vám může pomoct vyřešit problémy s připojením.

### <a name="tlsssl-configuration"></a>Konfigurace protokolu TLS/SSL

Použít MQTT protokolu přímo, vašeho klienta *musí* připojují přes TLS/SSL. S chybami připojení se nezdaří pokusy o Přeskočit tento.

Aby bylo možné navázat připojení protokolu TLS, musíte stáhnout a odkazovat na DigiCert Baltimore kořenový certifikát. To je certifikát, který používá k zabezpečení připojení Azure a lze nalézt v [úložiště Azure-iot-sdk-c][lnk-sdk-c-certs]. Další informace o těchto certifikátech naleznete v [Digicert na webu][lnk-digicert-root-certs].

Příklad toho, jak tuto funkci implementovat pomocí verze Python [Panamerická MQTT knihovny] [ lnk-paho] v prostředí Eclipse Foundation může vypadat třeba takto.

Nejdřív nainstalujte knihovnu Panamerická z prostředí příkazového řádku:

```
>pip install paho-mqtt
```

Ve skriptu jazyka Python, implementací klienta:

```
from paho.mqtt import client as mqtt
import ssl
  
path_to_root_cer = "...local\\path\\to\\digicert.cer"
device_id = "<device id from device registry>"
sas_token = "<generated SAS token>"
subdomain = "<iothub subdomain>"

client = mqtt.Client(client_id=device_id, protocol=mqtt.MQTTv311)

client.username_pw_set(username=subdomain+".azure-devices.net/" + device_id, password=sas_token)

client.tls_set(ca_certs=path_to_root_cert, certfile=None, keyfile=None, cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1, ciphers=None)
client.tls_insecure_set(False)

client.connect(subdomain+".azure-devices.net", port=8883)
```


### <a name="sending-device-to-cloud-messages"></a>Odesílání zpráv typu zařízení cloud

Po provedení úspěšného připojení, zařízení mohou zasílat zprávy do služby IoT Hub pomocí `devices/{device_id}/messages/events/` nebo `devices/{device_id}/messages/events/{property_bag}` jako **název tématu**. `{property_bag}` Element povoluje, aby zařízení k odesílání zpráv bez dalších vlastností ve formátu kódovaná adresou url. Příklad:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> To `{property_bag}` element používá stejné kódování jako řetězce dotazů v protokolu HTTPS.
>
>

Aplikace zařízení můžete také použít `devices/{device_id}/messages/events/{property_bag}` jako **název tématu bude** k definování *bude zprávy* k přeposlání jako zprávu telemetrie.

- IoT Hub nepodporuje QoS 2 zprávy. Pokud aplikace na zařízení publikuje zprávu s **QoS 2**, IoT Hub ukončí síťové připojení.
- IoT Hub není zachována zachovat zprávy. Pokud zařízení odesílá zprávy s **zachovat** příznak nastaven na hodnotu 1, přidá IoT Hub **x-opt zachovat** vlastnosti aplikace ke zprávě. V takovém případě místo uchování zpráv zachovat, IoT Hub předává je na back-end aplikace.
- IoT Hub podporuje pouze jeden aktivní připojení MQTT na jedno zařízení. Všechny nové připojení MQTT jménem stejné ID zařízení způsobí, že IoT Hub, chcete-li vyřadit existující připojení.

Další informace najdete v tématu [– Příručka vývojáře pro zasílání zpráv][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Příjem zpráv typu cloud zařízení

Pro příjem zpráv ze služby IoT Hub, musí zařízení přihlásit se pomocí `devices/{device_id}/messages/devicebound/#` jako **tématu filtru**. Víceúrovňovou zástupného `#` ve filtru téma, které se používají pouze pro umožňuje zařízení přijímat další vlastnosti v názvu tématu. IoT Hub neumožňuje použití `#` nebo `?` zástupných znaků pro filtrování dílčí témata. Vzhledem k tomu, že Centrum IoT není zprostředkovatel zasílání zpráv protokol pub-sub obecné účely, podporuje pouze zdokumentovaných tématu názvy a téma filtry.

Zařízení neobdrží všechny zprávy ze služby IoT Hub, dokud ho se úspěšně připojila ke koncovému bodu jeho konkrétní zařízení, reprezentována `devices/{device_id}/messages/devicebound/#` tématu filtru. Po úspěšné odběru bylo úspěšně navázáno, zařízení se spustí, přijímá jen zprávy cloud zařízení, které byly odeslány do ní po čase předplatného. Pokud se zařízení připojuje s **CleanSession** příznak nastaven na hodnotu **0**, odběr je trvalé v jiných relacích. V takovém případě při příštím připojení s **CleanSession 0** zařízení obdrží nezpracovaných zprávy, které byly odeslány do ní době, kdy byl odpojen. Pokud zařízení používá **CleanSession** příznak nastaven na hodnotu **1** ale, že neobdrží všechny zprávy ze služby IoT Hub až přihlásí se k jeho zařízení koncového bodu.

IoT Hub zajišťuje zprávy s **název tématu** `devices/{device_id}/messages/devicebound/`, nebo `devices/{device_id}/messages/devicebound/{property_bag}` Pokud nejsou k dispozici žádné vlastnosti zprávy. `{property_bag}`obsahuje dvojice klíč/hodnota kódovaná adresou url vlastností zpráv. Pouze vlastnosti aplikace a vlastnosti uživatele nastavit systému (například **messageId** nebo **correlationId**) jsou zahrnuty v kontejneru objektů. Názvy vlastností systému mají předponu  **$** , vlastnosti aplikace pomocí žádná předpona. původní název vlastnosti.

Když aplikace na zařízení předplatila téma se **QoS 2**, IoT Hub uděluje maximální QoS úrovně 1 v **SUBACK** paketů. Potom IoT Hub do zařízení pomocí technologie QoS 1 dodává zprávy.

### <a name="retrieving-a-device-twins-properties"></a>Načítání vlastností dvojče zařízení

Nejprve přihlásí k odběru zařízení `$iothub/twin/res/#`, pro příjem odpovědí operaci. Pak se odešle zprávu o prázdný tématu `$iothub/twin/GET/?$rid={request id}`, s hodnotou vyplněná **ID žádosti**. Služba pak odešle zprávu odpovědi, která obsahuje data twin zařízení k tématu `$iothub/twin/res/{status}/?$rid={request id}`, používající stejný **ID žádosti** jako požadavek.

ID žádosti může být libovolná platná hodnota pro hodnotu vlastnosti zprávy dle [IoT Hub – Příručka vývojáře pro zasílání zpráv][lnk-messaging], a stav byl ověřen jako celé číslo.
Text odpovědi obsahuje sekce properties dvojče zařízení:

Text položky registru identit omezené na člen "vlastnosti", například:

        {
            "properties": {
                "desired": {
                    "telemetrySendFrequency": "5m",
                    "$version": 12
                },
                "reported": {
                    "telemetrySendFrequency": "5m",
                    "batteryLevel": 55,
                    "$version": 123
                }
            }
        }

Možné stavové kódy jsou:

|Status | Popis |
| ----- | ----------- |
| 200 | Úspěch |
| 429 | Příliš mnoho požadavků (omezení), jako za [IoT Hub, omezení šířky pásma][lnk-quotas] |
| 5** | Chyby serveru |

Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Aktualizovat vlastnosti hlášené dvojče zařízení

Následující text popisuje, jak zařízení aktualizuje vlastnosti na hlášené dvojče zařízení IoT hub:

1. Zařízení musí nejdřív přihlásit k odběru `$iothub/twin/res/#` tématu pro příjem odpovědí operaci ze služby IoT Hub.

1. Zařízení odesílá zprávu, která obsahuje aktualizace twin zařízení, která se `$iothub/twin/PATCH/properties/reported/?$rid={request id}` tématu. Tato zpráva obsahuje **ID žádosti** hodnotu.

1. Služba pak odešle zprávu odpovědi, která obsahuje novou hodnotu ETag hlášené vlastnosti kolekce k tématu `$iothub/twin/res/{status}/?$rid={request id}`. Tuto zprávu s odpovědí používá stejnou **ID žádosti** jako požadavek.

Tělo zprávy požadavku obsahuje dokument JSON, který poskytuje nové hodnoty pro hlášené vlastnosti, které (lze upravit žádná vlastnost nebo metadata).
Každý člen v dokumentu JSON aktualizací nebo přidejte odpovídající člen v dokumentu dvojče zařízení. Sadu členů do `null`, odstraní člena z nadřazeného objektu. Příklad:

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

Možné stavové kódy jsou:

|Status | Popis |
| ----- | ----------- |
| 200 | Úspěch |
| 400 | Chybný požadavek. Nesprávný formát JSON |
| 429 | Příliš mnoho požadavků (omezení), jako za [IoT Hub, omezení šířky pásma][lnk-quotas] |
| 5** | Chyby serveru |

Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>Přijímající oznámení o aktualizaci požadované vlastnosti

Když je zařízení připojené, IoT Hub odešle oznámení do tématu `$iothub/twin/PATCH/properties/desired/?$version={new version}`, které obsahují obsah aktualizace provádí back-end řešení. Příklad:

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

Jako aktualizace vlastností `null` hodnoty znamená, že se odstraňuje členem objektu JSON.


> [!IMPORTANT] 
> IoT Hub generuje oznámení o změnách jenom v případě, že zařízení je připojených. Zajistěte, aby k implementaci [toku opětovné připojení zařízení] [ lnk-devguide-twin-reconnection] zachovat požadované vlastnosti synchronizovat mezi IoT Hub a aplikace zařízení.

Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].

### <a name="respond-to-a-direct-method"></a>Reakce na přímá metoda

První, zařízení má přihlásit k odběru `$iothub/methods/POST/#`. IoT Hub odešle požadavky metod v tématu `$iothub/methods/POST/{method name}/?$rid={request id}`, buď platný kód JSON nebo prázdným textem zprávy.

Reagovat, zařízení, odešle zprávu platný kód JSON nebo prázdným textem zprávy do tématu `$iothub/methods/res/{status}/?$rid={request id}`, kde **ID žádosti** musí odpovídat jedné ve zprávě požadavku, a **stav** musí být celé číslo.

Další informace najdete v tématu [přímé Příručka pro vývojáře metoda][lnk-methods].

### <a name="additional-considerations"></a>Další aspekty

Jako poslední zabývat, pokud budete potřebovat k přizpůsobení chování MQTT protokolu na straně cloudu, přečtěte si [brány protokolu Azure IoT] [ lnk-azure-protocol-gateway] umožňující můžete nasadit vlastní vysoce výkonné Brána protokolu, který rozhraní přímo službou IoT Hub. Brány protokolu Azure IoT umožňuje přizpůsobit protokol zařízení pro přizpůsobení brownfield MQTT nasazení nebo jiné vlastní protokoly. Tento přístup, ale nevyžaduje spouštět a provozovat bránu vlastního protokolu.

## <a name="next-steps"></a>Další kroky

Další informace o protokolu MQTT, najdete v článku [MQTT dokumentace][lnk-mqtt-docs].

Další informace o plánování nasazení služby IoT Hub naleznete v tématu:

* [Azure certifikované pro katalog zařízení IoT][lnk-devices]
* [Podpora dalších protokolů][lnk-protocols]
* [Porovnání s služby Event Hubs][lnk-compare]
* [Změna velikosti, HA a zotavení po Havárii][lnk-scaling]

Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Nasazení AI do hraničních zařízení s použitím Azure IoT Edge][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-x509]: iot-hub-security-x509-get-started.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
[lnk-sdk-c-certs]: https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c
[lnk-digicert-root-certs]: https://www.digicert.com/digicert-root-certificates.htm
[lnk-paho]: https://pypi.python.org/pypi/paho-mqtt
