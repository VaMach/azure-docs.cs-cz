---
title: Začínáme s předkonfigurovanými řešeními | Microsoft Docs
description: V tomto kurzu se dozvíte, jak nasadit předkonfigurované řešení Azure IoT Suite.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: dobett

---
# Kurz: Začínáme s předkonfigurovanými řešeními
## Úvod
[Předkonfigurovaná řešení][lnk-preconfigured-solutions] Azure IoT Suite kombinují více služeb Azure IoT, aby mohla poskytovat komplexní řešení, implementující běžné obchodní scénáře IoT. Předkonfigurované řešení *vzdálené monitorování* se připojuje k zařízením a monitoruje je. Řešení můžete použít k analýze streamu dat ze všech zařízení a ke zlepšení obchodních výsledků díky tomu, že procesy mohou automaticky reagovat na tento stream dat.

V tomto kurzu se dozvíte, jak zřídit předkonfigurované řešení vzdáleného monitorování. Také se seznámíte se základními funkcemi předkonfigurovaného řešení vzdáleného monitorování. Mnohé z těchto funkcí jsou přístupné prostřednictvím řídicího panelu řešení, který se nasazuje spolu s předem nakonfigurovaným řešením:

![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-dashboard]

K dokončení tohoto kurzu potřebujete mít aktivní předplatné služby Azure.

> [!NOTE]
> Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].
> 
> 

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## Zobrazení řídicího panelu řešení
Přes řídicí panel řešení můžete spravovat nasazené řešení. Můžete například zobrazit telemetrická data, přidat zařízení nebo konfigurovat pravidla.

1. Až bude zřizování dokončeno a dlaždice předkonfigurovaného řešení bude hlásit **Připraveno**, klikněte na tlačítko **Spustit**. Na nové kartě se otevře portál předkonfigurovaného řešení vzdáleného monitorování.
   
   ![Spuštění předkonfigurovaného řešení][img-launch-solution]
2. Ve výchozím nastavení se na portálu řešení zobrazuje *řídicí panel řešení*. Další pohledy můžete vybrat přes nabídku vlevo.
   
   ![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-dashboard]

Řídicí panel obsahuje tyto informace:

* Mapa zobrazuje umístění každého zařízení, které je připojené k řešení. Když řešení poprvé spustíte, zahrnuje čtyři simulovaná zařízení. Simulovaná zařízení jsou implementována jako Azure WebJobs a řešení k vykreslování informací na mapě používá Bing Maps API.
* Na panelu **Historie telemetrie** se skoro v reálném čase zobrazuje telemetrie vlhkosti a teploty z vybraného zařízení a agregovaná data (tj. minimum, maximum a průměrná vlhkost).
* Panel **Historie alarmů** zobrazuje události, kdy v poslední době hodnota telemetrie překročila stanovenou mez. Kromě příkladů vytvořených předkonfigurovaným řešením můžete definovat i vlastní alarmy.

## Zobrazení seznamu zařízení
V seznamu zařízení jsou uvedena všechna zařízení, která jsou zaregistrována v řešení. Můžete sledovat a upravovat metadata v zařízení, přidávat nebo odebírat zařízení a odesílat do nich příkazy.

1. Pokud v levé nabídce kliknete na možnost **Zařízení**, zobrazí se panel *Seznam zařízení* pro toto řešení.
   
   ![Seznam zařízení na řídicím panelu][img-devicelist]
2. V seznamu zařízení jsou uvedena čtyři simulovaná zařízení, která byla vytvořena během procesu zřizování.
3. Pokud kliknete na některé zařízení v seznamu, zobrazí se podrobnosti o tomto zařízení.
   
   ![Podrobnosti o zařízení na řídicím panelu][img-devicedetails]

Panel **Informace o zařízení** se skládá ze tří částí:

* V části **Akce** jsou uvedeny akce, které můžete na zařízení provádět. Pokud zařízení vypnete, zrušíte mu tím i povolení odesílat telemetrická data nebo přijímat příkazy. Pokud zařízení vypnete, můžete je později znovu zapnout. Můžete přidat pravidlo, které bude spojeno se zařízením a které spustí alarm pokaždé, když hodnota telemetrie přesáhne určitou mez. Také můžete na zařízení poslat příkaz. Když se zařízení poprvé připojí, sdělí řešení příkazy, na které může reagovat.
* V části **Vlastnosti zařízení** je uveden seznam metadat zařízení. Některá z těchto metadat pocházejí ze samotného zařízení (například výrobce), jiná generuje řešení (například čas vytvoření). Odsud můžete upravit metadata zařízení.
* V části **Ověřovací klíče** je uveden seznam klíčů, které může zařízení použít k ověřování při komunikaci s řešením.

## Odeslání příkazu na zařízení
V podokně s informacemi o zařízení jsou uvedeny všechny příkazy, které toto zařízení podporuje, a umožňuje vám odesílat příkazy na zařízení. Jakmile zařízení poprvé spustíte, odešle do řešení informace o příkazech, které podporuje.

1. V podokně informací o vybraném zařízení klikněte na **Příkazy**.
   
   ![Příkazy zařízení na řídicím panelu][img-devicecommands]
2. Ze seznamu příkazů vyberte **PingDevice** (Otestovat zařízení příkazem ping).
3. Klikněte na **Odeslat příkaz**.
4. Stav příkazu uvidíte v historii příkazů.
   
   ![Stav příkazu na řídicím panelu][img-pingcommand]

Řešení sleduje stav každého příkazu, který odešle. Zpočátku je výsledek uveden jako **Čekající**. Když zařízení ohlásí, že příkaz provedlo, výsledek je nastaven na **Úspěch**.

## Přidání nového simulovaného zařízení
Při nasazení předkonfigurovaného řešení jsou automaticky zřízena čtyři ukázková zařízení, která se zobrazí v seznamu zařízení. Tato zařízení jsou *simulovaná zařízení*, která běží ve webové úloze Azure. Simulované zařízení umožňují snadno experimentovat s předkonfigurovaným řešením, aniž by bylo nutné nasazovat skutečná fyzická zařízení. Pokud chcete k řešení připojit skutečné zařízení, přečtěte si článek [Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm].

Následující kroky ukazují, jak do řešení přidat simulované zařízení:

1. Vraťte se zpět k seznamu zařízení.
2. Přidejte zařízení kliknutím na možnost **+ Přidat zařízení** (vlevo dole).
   
   ![Přidání zařízení do předkonfigurovaného řešení][img-adddevice]
3. Klikněte na příkaz **Přidat nové**, který se nachází na dlaždici **Simulované zařízení**.
   
   ![Nastavení podrobností o zařízení na řídicím panelu][img-addnew]
   
   Pokud se rozhodnete vytvořit**vlastní zařízení**, můžete kromě nového simulovaného zařízení vytvořit i zařízení fyzické. Další informace o připojení fyzických zařízení k řešení najdete v článku [Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování sady IoT Suite][lnk-connect-rm].
4. Vyberte možnost **Ručně definovat vlastní ID zařízení** a zadejte jedinečný název zařízení, například **mydevice_01**.
5. Klikněte na možnost **Vytvořit**.
   
   ![Uložení nového zařízení][img-definedevice]
6. V kroku 3 **Přidání simulovaného zařízení** se kliknutím na tlačítko **Hotovo** vraťte do seznamu zařízení.
7. Zařízení nyní v seznamu uvidíte jako **Spuštěné**.
   
    ![Zobrazení nového zařízení v seznamu zařízení][img-runningnew]
8. Na řídicím panelu můžete také vidět simulovanou telemetrii z nového zařízení.
   
    ![Zobrazení telemetrie z nového zařízení][img-runningnew-2]

## Úprava metadat zařízení
Když se zařízení připojí k řešení poprvé, odešle do řešení svá metadata. Při úpravě metadat zařízení prostřednictvím řídicího panelu řešení odešle nové hodnoty metadat do zařízení a uloží nové hodnoty v databázi řešení DocumentDB. Další informace najdete v článku [Registr identit zařízení a databáze DocumentDB][lnk-devicemetadata].

1. Vraťte se zpět k seznamu zařízení.
2. Na panelu **Seznam zařízení** vyberte nové zařízení a klikněte na tlačítko **Upravit**, kterým upravíte **vlastnosti zařízení**.
   
   ![Úprava metadat zařízení][img-editdevice]
3. Přejděte dolů a změňte hodnoty zeměpisné délky a šířky. Pak klikněte na možnost **Uložit změny do registru zařízení**.
   
    ![Úprava metadat zařízení][img-editdevice2]
4. Po návratu na řídicí panel se umístění zařízení na mapě změní:
   
    ![Úprava metadat zařízení][img-editdevice3]

## Přidání pravidla pro nové zařízení
Pro zařízení, které jste právě přidali, ještě nejsou stanovena žádná pravidla. V této sekci přidáte pravidlo, které spustí alarm, jakmile teplota hlášená novým zařízením přesáhne 47 stupňů. Ještě než začnete, všimněte si, že historie telemetrie nového zařízení na řídicím panelu ukazuje, že teplota měřená tímto zařízením nikdy nepřesáhne 45 stupňů.

1. Vraťte se zpět k seznamu zařízení.
2. Na panelu **Seznam zařízení** vyberte nové zařízení a potom klikněte na tlačítko **Přidat pravidlo**. Tím přidáte pravidlo pro zařízení.
3. Vytvořte pravidlo, které jako datové pole používá **teplotu**, a pokud tato veličina přesáhne 47 stupňů, použije jako výstup **AlarmTemp** (teplotní alarm):
   
    ![Přidání pravidla pro zařízení][img-adddevicerule]
4. Změny uložte kliknutím na možnost **Uložit a zobrazit pravidla**.
5. V podokně informací o novém zařízení klikněte na možnost **Příkazy**.
   
   ![Přidání pravidla pro zařízení][img-adddevicerule2]
6. V seznamu příkazů vyberte **ChangeSetPointTemp** (Změnit mezní teplotu) a příkaz **SetPointTemp** (Nastavit mezní teplotu) nastavte na 45. Pak klikněte na možnost **Odeslat příkaz**:
   
   ![Přidání pravidla pro zařízení][img-adddevicerule3]
7. Vraťte se na řídicí panel řešení. Po chvilce se v podokně **Historie alarmů** ukáže nový záznam, když teplota hlášená tímto novým zařízením přesáhne hranici 47 stupňů:
   
   ![Přidání pravidla pro zařízení][img-adddevicerule4]
8. Všechna pravidla můžete kontrolovat a upravovat na stránce **Pravidla** na řídicím panelu:
   
    ![Zobrazení pravidel zařízení][img-rules]
9. Všechny akce, které se dají provést v reakci na některé pravidlo, můžete zkontrolovat a upravit na stránce **Akce** na řídicím panelu:
   
    ![Zobrazení akcí zařízení][img-actions]

> [!NOTE]
> Můžete definovat akce, které budou pomocí služby [Logic Apps][lnk-logic-apps] odesílat zprávy e-mailem nebo jako SMS v reakci na pravidlo nebo v rámci integrace s obchodním systémem. Další informace naleznete v tématu [Připojení Logic App k předkonfigurovanému řešení vzdáleného monitorování Azure IoT Suite][lnk-logicapptutorial].
> 
> 

## Další funkce
Pomocí portálu řešení můžete hledat zařízení s konkrétními vlastnostmi, jako je například číslo modelu:

![Vyhledávání zařízení][img-search]

Zařízení můžete zakázat, zakázané zařízení lze následně odebrat:

![Zakázání a odebrání zařízení][img-disable]

## Informace pro pokročilé uživatele
Když nasadíte předkonfigurované řešení, proces nasazení vytvoří ve vybraném předplatném Azure několik prostředků. Tyto prostředky můžete zobrazit na [portálu Azure][lnk-portal]. Proces nasazení vytváří **skupinu prostředků**. Její název bude vycházet z názvu, který jste vybrali pro předkonfigurované řešení:

![Předkonfigurované řešení na portálu Azure][img-portal]

Nastavení každého prostředku se zobrazí, když jej vyberete v seznamu ve skupině prostředků.

Můžete taky zobrazit zdrojový kód pro předkonfigurované řešení. Zdrojový kód předkonfigurovaného řešení vzdáleného monitorování najdete v úložišti GitHub [azure-iot-remote-monitoring][lnk-rmgithub]:

* Složka **DeviceAdministration** (Správa zařízení) obsahuje zdrojový kód pro řídicí panel.
* Složka **Simulator** (Simulátor) obsahuje zdrojový kód pro simulované zařízení.
* Složka **EventProcessor** (Procesor událostí) obsahuje zdrojový kód pro proces back-end, který zpracovává příchozí telemetrii.

Jakmile budete hotovi, můžete předkonfigurované řešení z vašeho předplatného Azure odstranit na stránce [azureiotsuite.com][lnk-azureiotsuite]. Tento web umožňuje snadno odstranit všechny prostředky, které byly zřízeny při vytvoření předkonfigurovaného řešení.

> [!NOTE]
> Abyste zajistili, že jste odstranili opravdu všechno spojené s předkonfigurovaným řešením, odstraňte řešení na stránce [azureiotsuite.com][lnk-azureiotsuite] – neodstraňujte jednoduše jenom skupinu prostředků na portálu.
> 
> 

## Další kroky
Když jste teď nasadili fungující předkonfigurované řešení, můžete pokračovat v seznamování se sadou IoT Suite přečtením následujících článků:

* [Návod na předkonfigurované řešení vzdáleného monitorování][lnk-rm-walkthrough]
* [Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm]
* [Oprávnění na webu azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md



<!--HONumber=Sep16_HO3-->


