<properties
    pageTitle="Začínáme s předkonfigurovanými řešeními | Microsoft Azure"
    description="V tomto kurzu se dozvíte, jak nasadit předkonfigurované řešení Azure IoT Suite."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="05/25/2016"
     ms.author="dobett"/>

# Kurz: Začínáme s předkonfigurovanými řešeními

## Úvod

[Předkonfigurovaná řešení][lnk-preconfigured-solutions] Azure IoT Suite kombinují více služeb Azure IoT, aby mohla poskytovat komplexní řešení, implementující běžné obchodní scénáře IoT.

V tomto kurzu se dozvíte, jak zřídit předkonfigurované řešení *vzdáleného monitorování*. Také se seznámíte se základními funkcemi předkonfigurovaného řešení vzdáleného monitorování.

K dokončení tohoto kurzu potřebujete aktivní předplatné Azure.

> [AZURE.NOTE]  Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].

## Poskytování předkonfigurovaného řešení vzdáleného monitorování.

1.  Na webu [azureiotsuite.com][lnk-azureiotsuite] použijte své přihlašovací údaje a přihlaste se k účtu Azure. Kliknutím na **+** vytvořte nové řešení.

    > [AZURE.NOTE] Pokud máte problémy s povoleními, která jsou zapotřebí ke zřízení řešení, přečtěte si pokyny v článku [Oprávnění na webu azureiotsuite.com][lnk-permissions].

2.  Klikněte na tlačítko **Vybrat**, které se nachází na dlaždici **Vzdálené monitorování**.

3.  Zadejte **Název řešení** pro předkonfigurované řešení vzdáleného monitorování.

4.  Vyberte **Oblast** a **Předplatné**, které chcete při zřizování řešení použít.

5.  Kliknutím na tlačítko **Vytvořit řešení** zahájíte proces zřizování. To obvykle trvá několik minut.

## Počkejte, dokud proces zřizování neskončí.

1. Klikněte na dlaždici s řešením, u kterého je uveden stav **Zřizování**.
 
2. **Stavy zřizování** umožňují sledovat, jak se služby Azure nasazují na váš účet Azure.

3. Jakmile bude zřizování dokončeno, stav se změní na **Připraveno**.

4. Pokud kliknete na dlaždici, v pravém podokně se zobrazí informace o řešení.

> [AZURE.NOTE] Pokud při nasazování předkonfigurovaného řešení narazíte na problémy, podívejte se na článek [Oprávnění na webu azureiotsuite.com][lnk-permissions] a do sekce [nejčastějších dotazů][lnk-faq]. Pokud problémy přetrvávají, vytvořte na [portálu][lnk-portal] lístek služby.

Hledali jste informace, které se týkají vašeho řešení a nejsou zde uvedeny? Sdělte nám návrhy na funkce na webu [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## Zobrazení řídicího panelu řešení vzdáleného monitorování.

Přes řídicí panel řešení můžete spravovat nasazené řešení. Můžete například zobrazit telemetrická data, přidat zařízení nebo konfigurovat pravidla.

1.  Až bude zřizování dokončeno a dlaždice předkonfigurovaného řešení bude hlásit **Připraveno**, klikněte na tlačítko **Spustit**. Na nové kartě se otevře portál předkonfigurovaného řešení vzdáleného monitorování.

    ![][img-launch-solution]

2.  Ve výchozím nastavení se na portálu řešení zobrazuje *řídicí panel řešení*. Další pohledy můžete vybrat přes nabídku vlevo.

    ![][img-dashboard]

Řídicí panel obsahuje tyto informace:

- Mapa zobrazuje umístění každého zařízení, které je připojené k řešení. Když řešení poprvé spustíte, zahrnuje čtyři simulovaná zařízení. Simulovaná zařízení jsou implementována jako Azure WebJobs a řešení k vykreslování informací na mapě používá Bing Maps API.
- Na panelu **Historie telemetrie** se skoro v reálném čase zobrazuje telemetrie vlhkosti a teploty z vybraného zařízení a agregovaná data (tj. minimum, maximum a průměrná vlhkost).
- Panel **Historie alarmů** zobrazuje události, kdy v poslední době hodnota telemetrie překročila stanovenou mez. Kromě příkladů vytvořených předkonfigurovaným řešením můžete definovat i vlastní alarmy.

## Zobrazení seznamu zařízení zahrnutých v řešení

V seznamu zařízení jsou uvedena všechna zařízení, která jsou zaregistrována v řešení. Můžete sledovat a upravovat metadata v zařízení, přidávat nebo odebírat zařízení a odesílat do nich příkazy.

1.  Pokud v levé nabídce kliknete na možnost **Zařízení**, zobrazí se panel *Seznam zařízení* pro toto řešení.

    ![][img-devicelist]

2.  V seznamu zařízení jsou uvedena čtyři simulovaná zařízení, která byla vytvořena během procesu zřizování.

3.  Pokud kliknete na některé zařízení v seznamu, zobrazí se bližší informace o tomto zařízení.

    ![][img-devicedetails]

Panel **Informace o zařízení** se skládá ze tří částí:

- V části **Akce** jsou uvedeny akce, které můžete na zařízení provádět. Pokud zařízení vypnete, zrušíte mu tím i povolení odesílat telemetrická data nebo přijímat příkazy. Pokud zařízení vypnete, můžete je později znovu zapnout. Můžete přidat pravidlo, které bude spojeno se zařízením a které spustí alarm pokaždé, když hodnota telemetrie přesáhne určitou mez. Také můžete na zařízení poslat příkaz. Když se zařízení poprvé připojí, sdělí řešení příkazy, na které může reagovat.
- V části **Vlastnosti zařízení** je uveden seznam metadat zařízení. Některá z těchto metadat pocházejí ze samotného zařízení (například výrobce), jiná generuje řešení (například čas vytvoření). Odsud můžete upravit metadata zařízení.
- V části **Ověřovací klíče** je uveden seznam klíčů, které může zařízení použít k ověřování při komunikaci s řešením.

## Odeslání příkazu na zařízení

V podokně s informacemi o zařízení jsou uvedeny všechny příkazy, které toto zařízení podporuje, a umožňuje vám odesílat příkazy na zařízení. Jakmile zařízení poprvé spustíte, odešle do řešení informace o příkazech, které podporuje.

1.  V podokně informací o vybraném zařízení klikněte na **Příkazy**.

    ![][img-devicecommands]

2.  Ze seznamu příkazů vyberte **PingDevice** (Otestovat zařízení příkazem ping).

3.  Klikněte na **Odeslat příkaz**.

4.  Stav příkazu uvidíte v historii příkazů.

    ![][img-pingcommand]

Řešení sleduje stav každého příkazu, který odešle. Zpočátku je výsledek uveden jako **Čekající**. Když zařízení ohlásí, že příkaz provedlo, výsledek je nastaven na **Úspěch**.

## Přidání nového simulovaného zařízení

1.  Vraťte se zpět k seznamu zařízení.

2.  V levém dolním rohu klikněte na příkaz **+ Přidat zařízení** a přidejte tak nové zařízení.

    ![][img-adddevice]

3.  Klikněte na příkaz **Přidat nové**, který se  nachází na dlaždici **Simulované zařízení**.

    ![][img-addnew]
    
    Pokud se rozhodnete vytvořit**vlastní zařízení**, můžete kromě nového simulovaného zařízení vytvořit i zařízení fyzické. Další informace najdete v tématu [Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování sady IoT Suite][lnk-connecting-devices].

4.  Vyberte možnost **Ručně definovat vlastní ID zařízení** a zadejte jedinečný název zařízení, například **mydevice_01**.

5.  Klikněte na možnost **Vytvořit**.

    ![][img-definedevice]

6. V kroku 3 **Přidání simulovaného zařízení** se kliknutím na tlačítko **Hotovo** vraťte do seznamu zařízení.

7. Zařízení nyní v seznamu uvidíte jako **Spuštěné**.

    ![][img-runningnew]

8. Na řídicím panelu můžete také vidět simulovanou telemetrii z nového zařízení.

    ![][img-runningnew-2]

## Úprava metadat zařízení

1.  Vraťte se zpět k seznamu zařízení.

2.  Na panelu **Seznam zařízení** vyberte nové zařízení a klikněte na tlačítko **Upravit**, kterým upravíte **vlastnosti zařízení**.

    ![][img-editdevice]

3. Přejděte dolů a změňte hodnoty zeměpisné délky a šířky. Pak klikněte na možnost **Uložit změny do registru zařízení**.

    ![][img-editdevice2]

4. Po návratu na řídicí panel se umístění zařízení na mapě změní:

    ![][img-editdevice3]

## Přidání pravidla pro nové zařízení

Pro zařízení, které jste právě přidali, ještě nejsou stanovena žádná pravidla. V této sekci přidáte pravidlo, které spustí alarm, jakmile teplota hlášená novým zařízením přesáhne hodnotu 47 stupňů. Ještě než začnete, všimněte si, že historie telemetrie nového zařízení na řídicím panelu ukazuje, že teplota měřená tímto zařízením nikdy nepřesáhne 45 stupňů.

1.  Vraťte se zpět k seznamu zařízení.

2.  Na panelu **Seznam zařízení** vyberte nové zařízení a potom klikněte na tlačítko **Přidat pravidlo**. Tím k novému zařízení přidáte pravidlo.

3. Vytvořte pravidlo, které jako datové pole používá **teplotu**, a pokud tato veličina přesáhne 47 stupňů, použije jako výstup **AlarmTemp** (teplotní alarm):

    ![][img-adddevicerule]

4. Změny uložte kliknutím na možnost **Uložit a zobrazit pravidla**.

5.  V podokně informací o novém zařízení klikněte na možnost **Příkazy**.

    ![][img-adddevicerule2]

6.  V seznamu příkazů vyberte **ChangeSetPointTemp** (Změnit mezní teplotu) a příkaz **SetPointTemp** (Nastavit mezní teplotu) nastavte na 45. Pak klikněte na možnost **Odeslat příkaz**:

    ![][img-adddevicerule3]

7.  Vraťte se na řídicí panel řešení. Po chvilce se v podokně **historie alarmů** ukáže nový záznam, když teplota hlášená tímto novým zařízením přesáhne hranici 47 stupňů:

    ![][img-adddevicerule4]

8. Všechna pravidla můžete kontrolovat a upravovat na stránce **Pravidla** na řídicím panelu:

    ![][img-rules]

9. Všechny akce, které se dají provést v reakci na některé pravidlo, můžete zkontrolovat a upravit na stránce **Akce** na řídicím panelu:

    ![][img-actions]

> [AZURE.NOTE] Můžete definovat akce, které budou pomocí služby [Logic Apps][lnk-logic-apps] odesílat zprávy e-mailem nebo jako SMS v reakci na pravidlo nebo v rámci integrace s obchodním systémem.

## Informace pro pokročilé uživatele

Když nasadíte předkonfigurované řešení, proces nasazení vytvoří ve vybraném předplatném Azure několik prostředků. Tyto prostředky můžete zobrazit na [portálu Azure][lnk-portal]. Proces nasazování vytvoří **skupinu prostředků**. Její název bude vycházet z názvu, který jste vybrali pro předkonfigurované řešení:

![][img-portal]

Nastavení každého prostředku se zobrazí, když jej vyberete v seznamu ve skupině prostředků. Výše uvedený snímek obrazovky ukazuje nastavení pro centrum IoT, použité v předkonfigurovaném nastavení.

Můžete taky zobrazit zdrojový kód pro předkonfigurované řešení. Zdrojový kód předkonfigurovaného řešení vzdáleného monitorování najdete v [azure-iot-remote-monitoring][lnk-rmgithub]:

- Složka **DeviceAdministration** (Správa zařízení) obsahuje zdrojový kód pro řídicí panel.
- Složka **Simulator** (Simulátor) obsahuje zdrojový kód pro simulované zařízení.
- Složka **EventProcessor** (Procesor událostí) obsahuje zdrojový kód pro proces back-end, který zpracovává příchozí telemetrii.

Až budete hotovi, můžete na stránkách [azureiotsuite.com][lnk-azureiotsuite] odstranit předkonfigurované řešení ze svého předplatného Azure a smazat tak všechny prostředky, které jste při vytváření předkonfigurovaného řešení zřídili.

> [AZURE.NOTE] Abyste zajistili, že jste odstranili opravdu všechno, co je spojené s předkonfigurovaným řešením, neodstraňujte pouze skupinu prostředků na portálu, ale úplně je odstraňte z [azureiotsuite.com][lnk-azureiotsuite].

## Další kroky

Když už jste vytvořili pracovní předkonfigurované řešení, můžete pokračovat na následující kurzy:

-   [Pokyny k přizpůsobení předkonfigurovaného řešení][lnk-customize]
-   [Přehled předkonfigurovaného řešení prediktivní údržby][lnk-predictive]

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

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-predictive]: iot-suite-predictive-overview.md
[lnk-connecting-devices]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-faq]: iot-suite-faq.md



<!--HONumber=Jun16_HO2-->


