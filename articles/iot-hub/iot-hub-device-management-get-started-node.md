<properties
    pageTitle="Začínáme se správou zařízení IoT Hub | Microsoft Azure"
    description="Úvodní kurz pro službu Azure IoT Hub pro správu zařízení pomocí jazyka C#. Implementace správy zařízení pomocí služby Azure IoT Hub a jazyka C# spolu se sadami SDK služby Microsoft Azure IoT."
    services="iot-hub"
    documentationCenter=".net"
    authors="ellenfosborne"
    manager="timlt"
    editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="elfarber"/>

# Začínáme se správou zařízení Azure IoT Hub pomocí node.js (preview)

[AZURE.INCLUDE [iot-hub-device-management-get-started-selector](../../includes/iot-hub-device-management-get-started-selector.md)]

## Úvod
Abyste mohli začít se správou zařízení Azure IoT Hub, je třeba vytvořit službu Azure IoT Hub, zřídit zařízení ve službě IoT Hub a spustit několik simulovaných zařízení. Tento kurz vás provede potřebnými kroky.

> [AZURE.NOTE]  Je třeba vytvořit novou službu IoT Hub, která aktivuje možnosti správy zařízení, a to i v případě, že již službu IoT Hub máte, protože stávající služby IoT Hubs možnosti správy zařízení neobsahují. Jakmile bude správa zařízení všeobecně dostupná, všechny stávající služby IoT Hubs se upgradují tak, aby obsahovaly možnosti správy zařízení.

## Požadavky

K dokončení postupu potřebujete mít nainstalované následující programy:

- Git
- node
- npm
- CMake (verze 2.8 nebo novější). CMake nainstalujete z adresy <https://cmake.org/download/>. Nezapomeňte zaškrtnout políčko k přidání softwaru CMake k proměnné PATH aktuálního uživatele.
- Aktivní předplatné Azure.

    Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].

## Vytvoření služby IoT Hub s aktivní správou zařízení

Je třeba vytvořit službu IoT Hub s aktivní správou zařízení, ke které se připojí simulovaná zařízení. Následující kroky vám ukážou, jak tuto úlohu dokončit pomocí portálu Azure.

1.  Přihlaste se k webu [Portál Azure].
2.  Na panelu vlevo klikněte na **Nový**, potom na **Internet věcí** a nakonec na **Azure IoT Hub**.

    ![][img-new-hub]

3.  V okně **IoT Hub** zvolte konfiguraci pro službu IoT Hub.

    ![][img-configure-hub]

  -   Do pole **Název** zadejte název služby IoT Hub. Pokud je **název** platný a dostupný, zobrazí se v poli **Název** zelená značka zaškrtnutí.
  -   Vyberte **cenovou a škálovací úroveň**. Tento kurz nevyžaduje konkrétní úroveň.
  -   V části **Skupina prostředků** vytvořte novou skupinu prostředků nebo vyberte existující. Další informace najdete v článku [Použití skupin prostředků ke správě prostředků Azure].
  -   Zaškrtněte políčko **Povolit správu zařízení**.
  -   V nabídce **Umístění** vyberte umístění pro hostování své služby IoT Hub. Správa zařízení IoT Hub je ve verzi Public Preview k dispozici pouze na východě USA, v severní Evropě a ve východní Asii. V budoucnu bude k dispozici ve všech oblastech.

  > [AZURE.NOTE]  Pokud políčko **Povolit správu zařízení** nezaškrtnete, ukázky nebudou fungovat.

4.  Po výběru možností konfigurace služby IoT Hub klikněte na **Vytvořit**. Vytvoření služby IoT Hub může službě Azure zabrat několik minut. Pokud chcete zkontrolovat stav, můžete průběh sledovat na **úvodním panelu** nebo na panelu **oznámení**.

    ![][img-monitor]

5.  Po úspěšném vytvoření služby IoT Hub otevřete její okno, poznamenejte si **Název hostitele** a potom klikněte na ikonu**Klíče**.

    ![][img-keys]

6.  Klikněte na zásady **iothubowner** a potom si z okna **iothubowner** zkopírujte a poznamenejte připojovací řetězec. Zkopírujte jej do umístění, do kterého budete mít později přístup, protože jej budete potřebovat k dokončení kurzu.

    > [AZURE.NOTE] V produkčních scénářích přihlašovací údaje **iothubowner** nepoužívejte.

    ![][img-connection]

Nyní jste vytvořili službu IoT Hub s aktivní správou zařízení. K dokončení kurzu budete potřebovat připojovací řetězec.

## Vytvoření ukázek a zřízení zařízení ve službě IoT Hub.

V této části spustíte skript, který vytvoří simulované zařízení a ukázky a zřídí sadu nových identit zařízení v registru zařízení služby IoT Hub. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru zařízení.

Pokud chcete vytvořit ukázky a zřídit zařízení ve službě IoT Hub, postupujte následovně:

1.  Otevřete terminál.

2.  Naklonujte úložiště github. **Klonujte pouze do adresáře, který neobsahuje žádné mezery.**

      ```
      git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
      ```

3.  Z kořenové složky, do které jste naklonovali úložiště **azure-iot-sdks**, přejděte do adresáře **azure-iot-sdks/node/service/samples** a spusťte skript tím, že nahradíte hodnotu zástupného znaku připojovacím řetězcem z předchozí části.

      ```
      setup.bat <IoT Hub Connection String>
      ```

Skript provede následující akce:

1.  Spustí **cmake**, který vytvoří soubory nezbytné k sestavení simulovaného zařízení. Spustitelný soubor se nachází v umístění **azure-iot-sdks/node/service/samples/cmake/iotdm\_client/samples/iotdm\_simple\_sample**. Všimněte si, že jsou zdrojové soubory ve složce **azure-iot-sdks/c/iotdm\_client/samples/iotdm\_simple\_sample**.

2.  Sestaví spustitelný soubor simulovaného zařízení **iotdm\_simple\_sample**.

3.  Spustí ``` npm install ``` a nainstaluje potřebné balíčky.

4.  Spustí ```node generate_devices.js``` ke zřízení identit zařízení ve službě IoT Hub. Zařízení jsou popsána v **sampledevices.json**. Po zřízení zařízení se přihlašovací údaje uloží do souboru **devicecreds.txt**, který se nachází v adresáři **azure-iot-sdks/node/service/samples**).

## Spuštění simulovaných zařízení

Když jsou nyní zařízení přidána do registru zařízení, můžete spustit simulovaná spravovaná zařízení. Pro každou identitu zařízení zřízenou ve službě Azure IoT Hub je třeba spustit jedno simulované zařízení.

Pomocí terminálu v adresáři **azure-iot-sdks/node/service/samples** spusťte:

  ```
  simulate.sh
  ```

Skript vypíše příkazy potřebné ke spuštění **iotdm\_simple\_sample** pro každé zařízení uvedené v souboru **devicecreds.txt**. Příkazy spouštějte jednotlivě ze samostatného okna terminálu pro každé simulované zařízení. Simulované zařízení bude spuštěné, dokud nezavřete příkazové okno.

Aplikace **iotdm\_simple\_sample** se sestavuje pomocí klientské knihovny správy zařízení služby Azure IoT Hub pro jazyk C umožňující vytvoření zařízení IoT, které lze spravovat ve službě Azure IoT Hub. Tvůrci zařízení mohou pomocí této knihovny sestavovat vlastnosti zařízení a implementovat prováděcí akce požadované úlohami zařízení. Tato knihovna je dodávána jako součást open source sad SDK služby Azure IoT Hub.

Po spuštění **simulate.sh** naleznete v okně výstupu datový proud. Výstup ukazuje příchozí a odchozí provoz a také příkazy **printf** ve funkcích zpětného volání konkrétní aplikace. To vám umožní zobrazit příchozí a odchozí provoz spolu s tím, jak ukázková aplikace zpracovává dekódované pakety. Po připojení zařízení ke službě IoT Hub služba automaticky začne sledovat prostředky v zařízení. Klientská knihovna DM služby IoT Hub poté vyvolá zpětná volání zařízení a načte z něj nejnovější hodnoty.

Níže se nachází výstup z ukázkové aplikace **iotdm\_simple\_sample**. V horní části uvidíte úspěšně **ZAREGISTROVANOU** zprávu, která zobrazuje zařízení s ID **Device11-7ce4a850** připojené ke službě IoT Hub.

> [AZURE.NOTE]  Pokud chcete méně podrobný výstup, sestavte a spusťte prodejní konfiguraci.

![][img-output]

Při dokončování dalších kroků kurzu ponechte všechna simulovaná zařízení spuštěná.

## Další kroky

Pokud chcete o funkcích správy zařízení ve službě Azure IoT Hub získat další informace, projděte následující kurzy:

- [Jak používat dvojče zařízení][lnk-tutorial-twin]

- [Jak najít dvojčata zařízení pomocí dotazů][lnk-tutorial-queries]

- [Jak používat úlohy zařízení k aktualizaci firmwaru zařízení][lnk-tutorial-jobs]

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started/image2.png
[img-monitor]: media/iot-hub-device-management-get-started/image3.png
[img-keys]: media/iot-hub-device-management-get-started/image4.png
[img-connection]: media/iot-hub-device-management-get-started/image5.png
[img-output]: media/iot-hub-device-management-get-started/image6.png

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Portál Azure]: https://portal.azure.com/
[Použití skupin prostředků ke správě prostředků Azure]: ../azure-portal/resource-group-portal.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-tutorial-jobs]: iot-hub-device-management-device-jobs.md



<!--HONumber=Jun16_HO2-->


