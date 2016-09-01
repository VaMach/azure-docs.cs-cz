<properties
 pageTitle="Přehled správy zařízení | Microsoft Azure"
 description="Přehled správy zařízení ve službě Azure IoT Hub: dvojčata zařízení, dotazy na zařízení, úlohy zařízení"
 services="iot-hub"
 documentationCenter=""
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="juanpere"/>

# Přehled správy zařízení ve službě Azure IoT Hub (preview)

Správa zařízení ve službě Azure IoT Hub umožňuje na standardech založenou správu zařízení IoT, která zahrnuje vzdálenou správu, konfiguraci a aktualizaci zařízení.

Ve službě Azure IoT existují tři hlavní koncepty správy zařízení:

1.  **Dvojče zařízení:** Reprezentace fyzického zařízení ve službě IoT Hub.

2.  **Dotazy na zařízení**: Umožňuje vyhledat dvojčata zařízení a generovat souhrnnou znalost více dvojčat zařízení. Můžete například spustit dotaz pro vyhledání všech dvojčat zařízení s verzí firmwaru 1.0.

3.  **Úlohy zařízení**: Akce, kterou chcete provést na jednom nebo více fyzických zařízeních, například aktualizace firmwaru, restartování nebo obnovení továrních nastavení.

## Dvojče zařízení

Dvojče je reprezentace fyzického zařízení ve službě Azure IoT. K reprezentaci dvojčat zařízení se používá objekt **Microsoft.Azure.Devices.Device**.

![][img-twin]

Dvojče zařízení má následující komponenty:

1.  **Pole zařízení:** Pole zařízení jsou předdefinované vlastnosti používané v zasílání zpráv a správě zařízení ve službě IoT Hub. Pomáhají službě IoT Hub identifikovat a připojit se k fyzickým zařízením. Pole zařízení nejsou synchronizována do zařízení a jsou uložena výhradně ve dvojčatech zařízení. Pole zařízení obsahují ID zařízení a ověřovací údaje.

2.  **Vlastnosti zařízení:** Vlastnosti zařízení jsou předdefinovaný slovník vlastností, které fyzické zařízení popisují. Fyzické zařízení je nadřazené každé vlastnosti zařízení a je autoritativním úložištěm každé odpovídající hodnoty. Konečná konzistentní reprezentace těchto vlastností je uložena ve dvojčeti zařízení v cloudu. Koherence a aktuálnost podléhá nastavení synchronizace, jak je popsáno v článku [Kurz: jak používat dvojče zařízení][lnk-tutorial-twin]. Mezi vlastnosti zařízení patří například verze firmwaru, stav baterie a název výrobce.

3.  **Vlastnosti služby:** Vlastnosti služby jsou páry **&lt;klíč, hodnota&gt;**, které vývojář přidá do slovníku vlastností služby. Tyto vlastnosti rozšiřují datový model pro dvojče zařízení a umožní vám lépe jej charakterizovat. Vlastnosti služby nejsou synchronizovány do zařízení a jsou uloženy výhradně ve dvojčeti zařízení v cloudu. Příkladem vlastnosti služby je **&lt;NextServiceDate, 11/12/2017&gt;**, kterou lze použít k vyhledání zařízení podle data příštího servisu.

4.  **Značky:** Značky jsou podmnožinou vlastností služby, které jsou do větší míry libovolné řetězce než slovníkové vlastnosti. Lze je použít k popisu dvojčat zařízení nebo uspořádání zařízení do skupin. Značky se nesynchronizují do zařízení a jsou uloženy výhradně ve dvojčeti zařízení. Pokud například dvojče zařízení představuje fyzický nákladní vůz, můžete přidat značky pro každý typ nákladu ve voze – **jablka**, **melouny** a **brambory**.

## Dotazy zařízení

V předchozí části jste se dozvěděli o různých součástech dvojčete zařízení. Teď si vysvětlíme, jak najít dvojčata zařízení v registru zařízení služby IoT Hub podle vlastností zařízení, vlastností služby nebo značek. Dotaz je možné použít například v situaci, kdy hledáte zařízení, která je nutné aktualizovat. Můžete zadat dotaz na všechna zařízení se zadanou verzí firmwaru a použít výsledek jako vstup pro konkrétní akci (označované ve službě IoT Hub jako úloha zařízení, což je vysvětleno v následující části).

Dotazovat můžete podle značek a vlastnosti:

-   Pokud chcete zadat dotaz pomocí značek, předejte pole řetězců a dotaz vrátí sadu zařízení, která mají ve značkách všechny tyto řetězce.

-   Pokud chcete zadat dotaz na dvojčata zařízení pomocí vlastností služby nebo vlastností zařízení, použijte dotazovací výraz JSON. Následující příklad ukazuje, jak můžete zadat dotaz na všechna zařízení s vlastností zařízení s klíčem **FirmwareVersion** a hodnotou **1.0**. Zde je vidět, že **typ** vlastnosti je **zařízení**, což znamená, že dotazujeme na základě vlastností zařízení, nikoli vlastností služby:

  ```
  {                           
      "filter": {                  
        "property": {                
          "name": "FirmwareVersion",   
          "type": "device"             
        },                           
        "value": "1.0",              
        "comparisonOperator": "eq",  
        "type": "comparison"         
      },                           
      "project": null,             
      "aggregate": null,           
      "sort": null                 
  }
  ```

## Úlohy zařízení

Dalším konceptem ve správě zařízení jsou úlohy zařízení, které umožňují koordinaci vícekrokových operací na více zařízeních.

Správa zařízení ve službě Azure IoT Hub teď poskytuje šest typů úloh zařízení (další úlohy přidáme podle potřeb zákazníků):

- **Aktualizace firmwaru**: Aktualizuje firmware (nebo image operačního systému) na fyzickém zařízení.
- **Restart**: Restartuje fyzické zařízení.
- **Obnovení nastavení z výroby**: Obnoví firmware (nebo image operačního systému) fyzického zařízení na záložní image z výroby uloženou v zařízení.
- **Aktualizace konfigurace**: Nakonfiguruje agenta klienta IoT Hub spuštěného na fyzickém zařízení.
- **Načíst vlastnost zařízení**: Získá nejnovější hodnotu vlastnosti zařízení na fyzickém zařízení.
- **Zapsat vlastnost zařízení**: Změní vlastnost zařízení na fyzickém zařízení.

Podrobnosti o použití těchto úloh najdete v tématu [Dokumentace API pro jazyk C\# a node.js][lnk apidocs].

Úloha může provádět akci na více zařízeních. Při spuštění úlohy je vytvořena přidružená podřízená úloha pro každé z těchto zařízení. Podřízená úloha běží na jednom zařízení. Každá podřízená úloha má ukazatel na nadřazenou úlohu. Nadřazená úloha slouží pouze jako kontejner pro podřízené úlohy, neimplementuje žádnou logiku k rozlišení mezi typy zařízení (např. aktualizace Intel Edison versus aktualizace Raspberry Pi). Následující diagram znázorňuje vztah mezi nadřazenou úlohou, jejími podřízenými úlohami a přidruženými fyzickými zařízeními.

![][img-jobs]

Dotazem na historii úloh můžete zjistit stav úloh, které jste spustili. Příklady dotazů naleznete v [naší knihovně dotazů][lnk-query-samples].

## Implementace zařízení

Po probrání konceptů na straně služby teď přejdeme k vytvoření spravovaného fyzického zařízení. Klientská knihovna správy zařízení ve službě IoT Hub Azure umožňuje spravovat zařízení IoT pomocí služby Azure IoT Hub. Správa zahrnuje takové akce, jako je restartování, obnovení výrobních nastavení nebo aktualizace firmwaru.  V současné době poskytujeme knihovnu jazyka C nezávislou na platformě, ale brzy doplníme podporu pro další jazyky.  

Klientská knihovna správy zařízení zajišťuje v rámci správě zařízení dvě hlavní věci:

- Synchronizace vlastností fyzického zařízení s odpovídajícím dvojčetem zařízení ve službě IoT Hub
- Zařazování úloh zařízení odeslaných službou IoT Hub do zařízení

Další informace o těchto funkcích a implementaci na fyzickém zařízení najdete v článku [Představení klientské knihovny správu zařízení ve službě Azure IoT Hub pro jazyk C][lnk-library-c].

## Další kroky

K implementaci klientských aplikací v celé řadě hardwarových platforem a operačních systémů zařízení můžete použít sady SDK zařízení IoT. Sady SDK zařízení IoT zahrnují knihovny, které usnadňují odesílání telemetrických dat do služby IoT Hub a příjem příkazů typu cloud-zařízení. Při používání sad SDK si ke komunikaci se službou IoT Hub můžete vybrat z řady síťových protokolů. Další informace naleznete v tématu [informace sadách SDK pro zařízení][lnk-device-sdks].

Pokud si chcete přečíst více o funkcích správy zařízení ve službě Azure IoT Hub, podívejte se na kurz [Začínáme se správou zařízení ve službě Azure IoT Hub][lnk-get-started].

<!-- Images and links -->
[img-twin]: media/iot-hub-device-management-overview/image1.png
[img-jobs]: media/iot-hub-device-management-overview/image2.png
[img-client]: media/iot-hub-device-management-overview/image3.png

[lnk-lwm2m]: http://technical.openmobilealliance.org/Technical/technical-information/release-program/current-releases/oma-lightweightm2m-v1-0
[lnk-library-c]: iot-hub-device-management-library.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk apidocs]: http://azure.github.io/azure-iot-sdks/
[lnk-query-samples]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/doc/get_started/dm_queries/query-samples.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks



<!--HONumber=Aug16_HO4-->


