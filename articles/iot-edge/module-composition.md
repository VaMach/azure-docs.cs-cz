---
title: "Složení modulu Azure IoT Edge | Microsoft Docs"
description: "Zjistěte, co na moduly Azure IoT okraj a jak může být znovu"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: f3bc2f14b182e502c651ff44ef49b88cd34e1f50
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="understand-how-iot-edge-modules-can-be-used-configured-and-reused---preview"></a>Pochopení IoT Edge moduly použití, nakonfigurovaná a znovu použít – náhled

Azure IoT okraj můžete vytvořit více modulů IoT Edge před jejich nasazením na IoT hraniční zařízení. Tento článek vysvětluje koncepty nejdůležitější kolem skládání více modulů IoT Edge před nasazením. 

## <a name="the-deployment-manifest"></a>Manifest nasazení
*– Manifest nasazení* je dokument JSON, který popisuje:

* Které moduly IoT Edge mají být nasazeny, spolu s jejich vytvoření a možnosti správy;
* Konfigurace Edge rozbočovače, které popisují, jak by měla zprávy toku mezi moduly a mezi moduly a IoT Hub;
* Hodnoty, Volitelně můžete nastavit v požadované vlastnosti dvojčata modulu, ke konfiguraci jednotlivých modulu aplikací.

V kurzů k Azure IoT Edge sestavení manifest nasazení tak, že přejdete v průvodci na portálu Azure IoT okraj. Můžete také použít nasazení manifestu programově pomocí REST nebo sady SDK služby IoT Hub. Odkazovat na [nasazení a monitorování] [ lnk-deploy] Další informace o nasazení IoT okraj.

Na vysoké úrovni nakonfiguruje manifest nasazení požadované vlastnosti modulů IoT Edge na IoT hraniční zařízení nasazené. Dva z těchto modulů nejsou vždy: agent okraj a okraj rozbočovače.

Manifest dodržuje tuto strukturu:

    {
        "moduleContent": {
            "$edgeAgent": {
                "properties.desired": {
                    // desired properties of the Edge agent
                }
            },
            "$edgeHub": {
                "properties.desired": {
                    // desired properties of the Edge hub
                }
            },
            "{module1}": {  // optional
                "properties.desired": {
                    // desired properties of module with id {module1}
                }
            },
            "{module2}": {  // optional
                ...
            },
            ...
        }
    }

Na konci této části je uveden příklad manifest nasazení.

> [!IMPORTANT]
> Všechny IoT hraniční zařízení musí být nakonfigurované manifest nasazení. Nově instalovaný modul runtime IoT okraj sestavy kód chyby, dokud nebude nakonfigurována s platný manifest. 

### <a name="specify-the-modules"></a>Zadejte moduly
Požadované vlastnosti modulu twin agenta Edge obsahují: požadované moduly, konfiguraci a správě možnosti, spolu s parametry konfigurace pro agenta okraj.

Tato část manifest na vysoké úrovni, obsahuje odkazy na modulu Image a možnosti správy pro IoT Edge moduly runtime (agent okraj a okraj centra) a moduly zadaného uživatelem.

Odkazovat [potřeby vlastnosti agenta Edge] [ lnk-edgeagent-desired] podrobný popis tohoto oddílu manifest.

> [!NOTE]
> Manifest nasazení obsahující pouze IoT Edge runtime (agenta a rozbočovače) je neplatný.


### <a name="specify-the-routes"></a>Zadejte trasy
Hraniční rozbočovače poskytuje způsob, jak deklarativně směrování zpráv mezi moduly a mezi moduly a IoT Hub.

Trasy mají následující syntaxi:

        FROM <source>
        [WHERE <condition>]
        INTO <sink>

*Zdroj* může být jakákoli z následujících akcí:

| Zdroj | Popis |
| ------ | ----------- |
| `/*` | Všechny zprávy typu zařízení cloud ze všech zařízení nebo modulu |
| `/messages/*` | Jakékoli zařízení cloud zprávy odeslané zařízením nebo modul prostřednictvím některé nebo žádný výstup |
| `/messages/modules/*` | Všechny zprávy ve zařízení cloud modulem prostřednictvím některé nebo žádný výstup. |
| `/messages/modules/{moduleId}/*` | Jakékoli zprávy typu zařízení cloud poslal {moduleId} se žádný výstup |
| `/messages/modules/{moduleId}/outputs/*` | Jakékoli zprávy typu zařízení cloud poslal {moduleId} některé výstup |
| `/messages/modules/{moduleId}/outputs/{output}` | Žádné zařízení cloud zpráva byla odeslána pomocí {moduleId} {výstupní} |

Podmínka může být jakékoli podmínky nepodporuje [IoT Hub dotazovací jazyk] [ lnk-iothub-query] pro pravidla směrování IoT Hub.

Jímky může být jedna z následujících akcí:

| Jímka | Popis |
| ---- | ----------- |
| `$upstream` | Odeslání zprávy do služby IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Odeslat zprávu jako vstup `{input}` modulu`{moduleId}` |

Je důležité si uvědomit, že Edge rozbočovače poskytuje jednou na nejmenší záruky, to znamená, že zprávy se uloží místně v případě trasu nelze doručení zprávy do jeho podřízený, např rozbočovače Edge se nemůže připojit ke službě IoT Hub nebo není připojený modul cíl.

Hraniční rozbočovače ukládá zprávy až za dobu určenou v `storeAndForwardConfiguration.timeToLiveSecs` vlastnost Edge rozbočovače požadovaných vlastností.

### <a name="specifying-the-desired-properties-of-the-module-twin"></a>Určení požadované vlastnosti modulu twin

Manifest nasazení můžete zadat požadované vlastnosti modulu twin jednotlivých modulů uživatele zadané v části agent okraj.

Pokud jsou požadované vlastnosti zadané v manifestu nasazení, jejich přepsat všechny požadované vlastnosti právě twin modulu.

Pokud nezadáte požadované vlastnosti modul twin v manifestu nasazení, IoT Hub nezmění twin modulu jakýmkoli způsobem a nebudete moci nastavit požadované vlastnosti prostřednictvím kódu programu.

### <a name="deployment-manifest-example"></a>Příklad nasazení manifestu

Tento příklad nasazení manifestu dokumentu JSON.

    {
    "moduleContent": {
        "$edgeAgent": {
            "properties.desired": {
                "schemaVersion": "1.0",
                "runtime": {
                    "type": "docker",
                    "settings": {
                        "minDockerVersion": "v1.25",
                        "loggingOptions": ""
                    }
                },
                "systemModules": {
                    "edgeAgent": {
                        "type": "docker",
                        "settings": {
                        "image": "microsoft/azureiotedge-agent:1.0-preview",
                        "createOptions": ""
                        }
                    },
                    "edgeHub": {
                        "type": "docker",
                        "status": "running",
                        "restartPolicy": "always",
                        "settings": {
                        "image": "microsoft/azureiotedge-hub:1.0-preview",
                        "createOptions": ""
                        }
                    }
                },
                "modules": {
                    "tempSensor": {
                        "version": "1.0",
                        "type": "docker",
                        "status": "running",
                        "restartPolicy": "always",
                        "settings": {
                        "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
                        "createOptions": "{}"
                        }
                    },
                    "filtermodule": {
                        "version": "1.0",
                        "type": "docker",
                        "status": "running",
                        "restartPolicy": "always",
                        "settings": {
                        "image": "myacr.azurecr.io/filtermodule:latest",
                        "createOptions": "{}"
                        }
                    }
                }
            }
        },
        "$edgeHub": {
            "properties.desired": {
                "schemaVersion": "1.0",
                "routes": {
                    "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
                    "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
                },
                "storeAndForwardConfiguration": {
                    "timeToLiveSecs": 10
                }
            }
        }
    }
    }

## <a name="reference-edge-agent-module-twin"></a>Referenční dokumentace: Edge agenta modulu twin

Je volána twin modulu pro agenta hraniční `$edgeAgent` a koordinuje komunikaci mezi hraniční agenta spuštěného na zařízení a služby IoT Hub.
Požadované vlastnosti se nastavují při použití manifest nasazení na určité zařízení v rámci jednoho zařízení nebo v odpovídajícím měřítku nasazení. V tématu [nasazení a monitorování] [ lnk-deploy] Další informace o tom, jak nasadit modulů na IoT hraniční zařízení.

### <a name="edge-agent-twin-desired-properties"></a>Vlastnosti twin požadovaného agenta Edge

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| schemaVersion | Musí být "1.0" | Ano |
| Runtime.Type | Musí být "docker" | Ano |
| runtime.settings.minDockerVersion | Nastavte na tento – manifest nasazení požadavek na minimální verzi Docker | Ano |
| runtime.settings.loggingOptions | Stringified JSON obsahující možnosti protokolování pro kontejner agenta okraj. [Možnosti protokolování docker][lnk-docker-logging-options] | Ne |
| systemModules.edgeAgent.type | Musí být "docker" | Ano |
| systemModules.edgeAgent.settings.image | Identifikátor URI bitovou kopii agenta okraj. Edge agent v současné době není možné aktualizovat sám sebe. | Ano |
| systemModules.edgeAgent.settings.createOptions | Stringified JSON obsahující možnosti pro vytvoření kontejneru agenta okraj. [Možnosti vytvoření docker][lnk-docker-create-options] | Ne |
| systemModules.edgeAgent.configuration.id | ID nasazení, které tento modul pro nasazení. | Centrum IoT hub je nastavena v případě použije tento manifestu pomocí nasazení. Není součástí manifest nasazení. |
| systemModules.edgeHub.type | Musí být "docker" | Ano |
| systemModules.edgeHub.status | Musí být "spuštění" | Ano |
| systemModules.edgeHub.restartPolicy | Musí být "vždy" | Ano |
| systemModules.edgeHub.settings.image | Identifikátor URI bitovou kopii Edge rozbočovače. | Ano |
| systemModules.edgeHub.settings.createOptions | Stringified JSON obsahující možnosti pro vytvoření kontejneru Edge rozbočovače. [Možnosti vytvoření docker][lnk-docker-create-options] | Ne |
| systemModules.edgeHub.configuration.id | ID nasazení, které tento modul pro nasazení. | Centrum IoT hub je nastavena v případě použije tento manifestu pomocí nasazení. Není součástí manifest nasazení. |
| moduly. {moduleId} .version | Uživatelem definované řetězec představující verze tohoto modulu. | Ano |
| moduly. .Type – {moduleId} | Musí být "docker" | Ano |
| moduly. {moduleId} .restartPolicy | {"nikdy" \| "na-se nezdařilo" \| "na-není v pořádku" \| "vždy"} | Ano |
| moduly. {moduleId}.settings.image | Identifikátor URI pro image modulu. | Ano |
| moduly. {moduleId}.settings.createOptions | Stringified JSON obsahující možnosti pro vytvoření kontejneru modulu. [Možnosti vytvoření docker][lnk-docker-create-options] | Ne |
| moduly. {moduleId}.configuration.id | ID nasazení, které tento modul pro nasazení. | Centrum IoT hub je nastavena v případě použije tento manifestu pomocí nasazení. Není součástí manifest nasazení. |

### <a name="edge-agent-twin-reported-properties"></a>Hraniční agenta twin hlášené vlastnosti

Edge agent oznámil, že vlastnosti zahrnují tři hlavní údaje:

1. Stav aplikace vidět poslední požadované vlastnosti;
2. Stav modulů, které jsou aktuálně spuštěné na zařízení, vykazované Edge agenta; a
3. Kopie požadované vlastnosti, které jsou aktuálně spuštěné na zařízení.

Tento poslední část informace je užitečné v případě, že nejsou úspěšně použít nejnovější požadované vlastností modulem runtime a zařízení stále běží předchozí manifest nasazení.

> [!NOTE]
> Hlášené vlastnosti agenta hraniční jsou užitečné, protože může být dotazován s [IoT Hub dotazovací jazyk] [ lnk-iothub-query] prozkoumat stav nasazení ve velkém měřítku. Odkazovat na [nasazení] [ lnk-deploy] Další informace o tom, jak tuto funkci používat.

V následující tabulce nezahrnuje informace, které budou zkopírována z požadované vlastnosti.

| Vlastnost | Popis |
| -------- | ----------- |
| lastDesiredVersion | Tato int odkazuje na poslední verzi požadované vlastnosti zpracovat agentem okraj. |
| lastDesiredStatus.code | Toto je kód stavu odkazující na poslední požadované vlastnosti kontaktu s agentem okraj. Povolené hodnoty: `200` úspěch, `400` neplatná konfigurace `412` neplatné schéma verze `417` požadované vlastnosti jsou prázdná, `500` se nezdařilo |
| lastDesiredStatus.description | Textový popis stavu |
| DeviceHealth | `healthy`Pokud je stav modulu runtime všechny moduly, buď `running` nebo `stopped`, `unhealthy` jinak |
| configurationHealth. {deploymentId} .health | `healthy`Pokud stav modulu runtime všechny moduly nastavit nasazení {deploymentId} je buď `running` nebo `stopped`, `unhealthy` jinak |
| runtime.platform.OS | Vytváření sestav operačního systému spuštěné na zařízení |
| Runtime.Platform.Architecture | Vytváření sestav architekturu procesoru na zařízení |
| systemModules.edgeAgent.runtimeStatus | Reportovaný stav agenta Edge: {"spuštění" \| "není v pořádku"} |
| systemModules.edgeAgent.statusDescription | Textový popis reportovaný stav agenta okraj. |
| systemModules.edgeHub.runtimeStatus | Aktuální stav rozbočovače Edge: {"spuštění" \| "stopped" \| "se nezdařilo" \| "omezení rychlosti" \| "není v pořádku"} |
| systemModules.edgeHub.statusDescription | Textový popis aktuální stav Edge rozbočovače, pokud není v pořádku. |
| systemModules.edgeHub.exitCode | Pokud byl ukončen, ukončovací kód hlášené kontejneru Edge rozbočovače |
| systemModules.edgeHub.startTimeUtc | Čas posledního spuštění Edge rozbočovače |
| systemModules.edgeHub.lastExitTimeUtc | Čas, kdy Edge rozbočovače poslední byl ukončen |
| systemModules.edgeHub.lastRestartTimeUtc | Čas poslední restartováním Edge rozbočovače |
| systemModules.edgeHub.restartCount | Počet pokusů, které byl tento modul restartován v rámci zásad restartování. |
| moduly. {moduleId} .runtimeStatus | Aktuální stav modulu: {"spuštění" \| "stopped" \| "se nezdařilo" \| "omezení rychlosti" \| "není v pořádku"} |
| moduly. {moduleId} .statusDescription | Textový popis aktuální stav modulu, pokud není v pořádku. |
| moduly. {moduleId} .exitCode | Pokud byl ukončen, ukončovací kód hlášené kontejner modulu |
| moduly. {moduleId} .startTimeUtc | Čas posledního spuštění modulu |
| moduly. {moduleId} .lastExitTimeUtc | Čas, kdy modul poslední byl ukončen |
| moduly. {moduleId} .lastRestartTimeUtc | Čas, kdy modul posledního restartování |
| moduly. {moduleId} .restartCount | Počet pokusů, které byl tento modul restartován v rámci zásad restartování. |

## <a name="reference-edge-hub-module-twin"></a>Referenční dokumentace: Edge rozbočovače modul twin

Je volána twin modulu pro rozbočovač na hraniční `$edgeHub` a koordinuje komunikaci mezi hraniční rozbočovače spuštěné v zařízení a služby IoT Hub.
Požadované vlastnosti se nastavují při použití manifest nasazení na určité zařízení v rámci jednoho zařízení nebo v odpovídajícím měřítku nasazení. V tématu [nasazení] [ lnk-deploy] Další informace o tom, jak nasadit modulů na IoT hraniční zařízení.

### <a name="edge-hub-twin-desired-properties"></a>Hraniční rozbočovače twin požadovaných vlastností

| Vlastnost | Popis | Požadované v manifestu nasazení |
| -------- | ----------- | -------- |
| schemaVersion | Musí být "1.0" | Ano |
| trasy. {routeName} | Řetězec představující trasu rozbočovače okraj. | `routes` Prvek může být existuje, ale je prázdný. |
| storeAndForwardConfiguration.timeToLiveSecs | Dobu v sekundách, které udržuje Edge rozbočovače zprávy v případě odpojené směrování koncových bodů, například odpojen od služby IoT Hub nebo místní modulu | Ano |

### <a name="edge-hub-twin-reported-properties"></a>Hraniční rozbočovače twin hlášené vlastnosti

| Vlastnost | Popis |
| -------- | ----------- |
| lastDesiredVersion | Tato int odkazuje na poslední verzi požadované vlastnosti zpracovává Edge rozbočovače. |
| lastDesiredStatus.code | Toto je kód stavu odkazující na poslední požadované vlastnosti pohledu Edge rozbočovače. Povolené hodnoty: `200` úspěch, `400` neplatná konfigurace `500` se nezdařilo |
| lastDesiredStatus.description | Textový popis stavu |
| Klienti. {identity zařízení nebo modul} .status | Stav tohoto zařízení nebo modul. Možné hodnoty {"připojené" \| "odpojen"}. Pouze modul identit může být v odpojeném stavu. Podřízené zařízení připojující se k rozbočovači hraniční se zobrazí, jenom když připojení. |
| Klienti. {identity zařízení nebo modul} .lastConnectTime | Poslední čas modulu připojení na zařízení |
| Klienti. {identity zařízení nebo modul} .lastDisconnectTime | Čas poslední zařízení nebo modul odpojení |

## <a name="next-steps"></a>Další postup

Nyní když znáte použití modulů IoT Edge, [pochopení požadavků a nástrojů pro vývoj modulů IoT Edge][lnk-module-dev].

[lnk-deploy]: module-deployment-monitoring.md
[lnk-edgeagent-desired]: #edge-agent-twin-desired-properties
[lnk-edgeagent-reported]: #edge-agent-twin-reported-properties
[lnk=edgehub-desired]: #edge-hub-twin-desired-properties
[lnk-edgehub-reported]: #edge-hub-twin-reported-properties
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md
