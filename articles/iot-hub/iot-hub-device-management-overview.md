<properties
 pageTitle="Přehled správy zařízení | Microsoft Azure"
 description="Přehled správy zařízení ve službě Azure IoT Hub"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/16/2016"
 ms.author="bzurcher"/>




# Přehled správy zařízení ve službě Azure IoT Hub (preview)

## Přístup ke správě zařízení ve službě Azure IoT

Správa zařízení ve službě Azure IoT Hub poskytuje funkce a model rozšiřitelnosti pro zařízení a back-endy, díky kterým lze využívat správu zařízení ve službě IoT pro široké spektrum zařízení a protokolů ve službě IoT.  Ve službě IoT lze používat zařízení od velmi omezených snímačů a jednoúčelových mikrořadičů až po výkonnější brány, které povolují další zařízení a protokoly.  Řešení služby IoT se navíc výrazně liší ve vertikálních doménách a aplikacích s jedinečnými případy použití pro operátory v jednotlivých doménách.  Řešení služby IoT mohou využívat možnosti správy zařízení ve službě IoT Hub, vzory a knihovny kódu při povolování správy zařízení pro široké spektrum zařízení a uživatelů.  

## Úvod

Klíčovou součástí vytváření úspěšného řešení služby IoT je poskytnutí strategie pro způsob, kterým operátoři provádějí průběžnou správu příslušného fondu zařízení. Operátoři služby IoT vyžadují nástroje a aplikace, které jsou jednoduché a zároveň spolehlivé a které jim umožňují zaměřit se na strategičtější aspekty přidělených úloh. Služba Azure IoT Hub vám poskytuje stavební bloky pro vytváření aplikací služby IoT, které usnadňují provádění nejdůležitějších vzorů správy zařízení.

Zařízení jsou považována za spravovaná službou IoT Hub, když spustí jednoduchou aplikaci nazývanou agent monitorování zařízení, která zajišťuje zabezpečené připojení zařízení ke cloudu. Kód agenta umožňuje operátorovi na straně aplikace vzdáleně ověřit stav aplikace a provádět operace správy, například aplikovat změny konfigurace sítě nebo nasadit aktualizace firmwaru.

## Principy správy zařízení ve službě IoT

Spolu se službou IoT přichází i jedinečná sada výzev pro správu a řešení musí odpovídat následujícím principům správy zařízení ve službě IoT:

![][img-dm_principles]

- **Škálování a automatizace**: Služba IoT vyžaduje jednoduché nástroje, které mohou automatizovat běžné úlohy a umožnit poměrně málo početnému provoznímu personálu správu miliónů zařízení. V každodenním provozu operátoři očekávají, že budou moci spravovat operace se zařízeními vzdáleně a hromadně a že budou upozorňování pouze na vznik problémů, které vyžadují jejich přímou pozornost.

- **Otevřenost a kompatibilita**: Ekosystém zařízení ve službě IoT je neobyčejně různorodý. Nástroje pro správu musí být upraveny tak, aby podporovaly velké množství tříd zařízení, platforem a protokolů. Operátoři musí být schopni poskytovat podporu pro všechna zařízení od nejvíce omezených jednoprocesových čipů po výkonné a plně funkční počítače.

- **Znalost kontextu**: Prostředí služby IoT prostředí jsou dynamická a neustále se mění. Spolehlivost služby je prvořadá. Operace správy zařízení musí zohledňovat časová období údržby podle smlouvy SLA, stavy sítě a výkonu, podmínky při používání a zeměpisnou polohu zařízení, aby bylo možné zajistit, že výpadek v důsledku údržby nebude mít vliv na klíčové obchodní operace a nebudou kvůli němu vznikat nebezpečné situace.

- **Obsluha mnoha rolí**: Základním požadavkem je podpora jedinečných pracovních postupů a procesů rolí provozu služby IoT. Personál provozu musí také pracovat v souladu s danými omezeními interních oddělení IT a předávat příslušné informace o provozu zařízení nadřízeným a pracovníkům s dalšími rolemi správy.

## Životní cyklus zařízení služby IoT 

I když se projekty služby IoT značně liší, existuje množina běžných vzorů pro správu zařízení. Ve službě Azure IoT jsou tyto vzory rozpoznány v rámci životního cyklu zařízení služby IoT, který se skládá z pěti různých fází:

![][img-device_lifecycle]

1. **Plánování**: Umožnění, aby operátoři mohli vytvořit schéma vlastností zařízení, na základě kterého mohou snadno a přesně zadávat dotazy na skupinu zařízení pro hromadné operace správy a nastavovat je jako cíl.

    *Související stavební bloky*: [Začínáme s dvojčaty zařízení][lnk-twins-getstarted], [Postup při využívání vlastností dvojčat][lnk-twin-properties]

2. **Zřízení**: Zabezpečené ověření nových zařízení ve službě IoT Hub a umožnění, aby operátoři mohli ihned zjišťovat možnosti a aktuální stav zařízení.

    *Související stavební bloky*: [Začínáme se službou IoT Hub][lnk-hub-getstarted], [Postup při využívání vlastností dvojčat][lnk-twin-properties]

3. **Konfigurace**: Provádění hromadných změn konfigurace a aktualizací firmwaru v zařízeních při zachování stavu i zabezpečení.

    *Související stavební bloky*: [Postup při využívání vlastností dvojčat][lnk-twin-properties], [Metody C2D][lnk-c2d-methods], [Plánování/vysílání úloh][lnk-jobs]

4. **Monitorování**: Monitorování celkového stavu fondu zařízení a stavu probíhajícího uvádění aktualizací, přičemž jsou operátoři upozorňování na problémy, které mohou vyžadovat jejich pozornost.

    *Související stavební bloky*: [Postup při využívání vlastností dvojčat][lnk-twin-properties]

5. **Vyřazení**: Výměna nebo zařízení nebo jejich vyloučení z provozu po selhání, po provedení cyklu upgradů nebo na konci životnosti služby.

    *Související stavební bloky*:
    
## Vzory správy zařízení ve službě IoT Hub

Služba IoT Hub umožňuje využívání následující (počátečních) vzorů správy zařízení.  Jak je uvedeno v [kurzech][lnk-get-started], můžete tyto vzory rozšířit tak, aby odpovídaly vašemu konkrétnímu scénáři, a podle těchto základních vzorů můžete navrhovat nové vzory pro další scénáře.

1. **Restartování** – Aplikace back-end informuje zařízení prostřednictvím metody D2C o zahájení restartování.  Zařízení využívá ohlášené vlastnosti dvojčete zařízení k aktualizaci stavu restartování příslušného zařízení. 

    ![][img-reboot_pattern]

2. **Obnovení výrobního nastavení** – Aplikace back-end informuje zařízení prostřednictvím metody D2C o zahájení obnovování výrobního nastavení.  Zařízení využívá ohlášené vlastnosti dvojčete zařízení k aktualizaci stavu obnovování výrobního nastavení příslušného zařízení.

    ![][img-facreset_pattern]

3. **Konfigurace** – Aplikace back-end využívá požadované vlastnosti dvojčete zařízení ke konfiguraci softwaru spuštěného v příslušném zařízení.  Zařízení využívá ohlášené vlastnosti dvojčete zařízení k aktualizaci stavu konfigurace příslušného zařízení. 

    ![][img-config_pattern]

4. **Aktualizace firmwaru** – Aplikace back-end informuje zařízení prostřednictvím metody D2C o zahájení aktualizace firmwaru.  Zařízení zahájí vícefázový proces stahování balíčku firmwaru, použití balíčku firmwaru a poté opětného připojení ke službě IoT Hub.  Během tohoto vícefázového procesu zařízení využívá ohlášené vlastnosti dvojčete zařízení k aktualizaci informací o průběhu zpracování a stavu zařízení. 

    ![][img-fwupdate_pattern]

5. **Informování o průběhu zpracování a stavu** – Aplikace back-end spouští dotazy dvojčete zařízení v sadě zařízení a předává informace o stavu a průběhu zpracování akcí spuštěných v příslušném zařízení.

    ![][img-report_progress_pattern]

## Další kroky

S použitím stavebních bloků poskytovaných službou Azure IoT Hub mohou vývojáři vytvářet aplikace služby IoT, které splňují jedinečné požadavky operátorů služby IoT v každé fázi životního cyklu zařízení.

Pokud si chcete přečíst více o funkcích správy zařízení ve službě Azure IoT Hub, podívejte se na kurz [Začínáme se správou zařízení ve službě Azure IoT Hub][lnk-get-started].

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md


<!--HONumber=Oct16_HO1-->


