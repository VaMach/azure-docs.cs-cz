## <a name="prepare-your-raspberry-pi"></a>Příprava vašeho Malinová platformy

### <a name="install-raspbian"></a>Nainstalujte Raspbian

Pokud používáte vaše platformy malin poprvé, musíte nainstalovat operační systém Raspbian pomocí NOOBS na kartu SD. součástí sady. [Malin pí softwaru průvodce] [ lnk-install-raspbian] popisuje postup instalace operačního systému na vaše malin pí. Tento kurz předpokládá, že jste nainstalovali Raspbian operačního systému na vaše malin pí.

> [!NOTE]
> Používání SD karet součástí [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] již má nainstalované NOOBS. Můžete spustit pí malin z této karty a zvolit instalaci operačního systému Raspbian.

### <a name="set-up-the-hardware"></a>Nastavení hardwaru

Tento kurz používá BME280 senzoru součástí [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] ke generování telemetrická data. Používá DIODU označíte, když pí malin zpracovává volání metody na řídicím panelu řešení.

Součásti na panelu chléb jsou:

- Red DIODU
- 220 Ohm odpor (červená, red menších.)
- Senzor BME280

Následující diagram ukazuje, jak připojit hardwaru:

![Nastavení hardwaru pro malin platformy][img-connection-diagram]

Následující tabulka shrnuje připojení z platformy malin součástí na breadboard:

| Raspberry Pi            | Breadboard             |Barva         |
| ----------------------- | ---------------------- | ------------- |
| ZEM (Pin 14)            | Indikátor - sunout pin (18A)      | Fialová          |
| GPCLK0 (Pin 7)          | Odpor (25A)         | Orange          |
| SPI_CE0 (Pin 24)        | CS (39A)               | Modrá          |
| SPI_SCLK (Pin 23)       | SCK (36A)              | Žlutá        |
| SPI_MISO (Pin 21)       | SDO (37A)              | Bílá         |
| SPI_MOSI (Pin 19)       | SDI (38A)              | Zelená         |
| ZEM (Pin 6)             | ZEM (35A)              | Černá         |
| 3.3 V (Pin 1)           | 3Vo (34A)              | Červená           |

Chcete-li dokončit nastavení hardwaru, je potřeba:

- Připojte vaše platformy malin k napájení součástí sady.
- Vaše platformy malin připojte k síti pomocí kabelu Ethernet, který je součástí vaší sady. Alternativně můžete nastavit [bezdrátové připojení] [ lnk-pi-wireless] vaše malin pí.

Teď jste dokončili nastavení hardwaru vaší malin pí.

### <a name="sign-in-and-access-the-terminal"></a>Přihlaste se a přístup k terminálu

Máte dvě možnosti pro přístup k Terminálové prostředí na vaše malin platformy:

- Pokud máte klávesnici a monitorování, které jsou připojené k vaší malin platformy, můžete použít Raspbian grafického uživatelského rozhraní pro přístup k okno terminálu.

- Přístup na příkazovém řádku vaší malin pí pomocí protokolu SSH ze stolního počítače.

#### <a name="use-a-terminal-window-in-the-gui"></a>Použijte okno terminálu v grafickém uživatelském rozhraní

Výchozí pověření pro Raspbian jsou uživatelské jméno **pí** a heslo **malin**. Na hlavním panelu v grafickém uživatelském rozhraní, můžete spustit **Terminálové** nástroj pomocí ikonu, která vypadá jako monitorování.

#### <a name="sign-in-with-ssh"></a>Přihlaste se pomocí protokolu SSH

SSH můžete použít pro příkazového řádku přístup k vaší malin pí. Článek [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje postup konfigurace SSH na vaše malin platformy a jak se připojit z [Windows] [ lnk-ssh-windows] nebo [ Linux & Mac OS][lnk-ssh-linux].

Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>Volitelné: Sdílené složky na vaše malin platformy

Volitelně můžete sdílet složky na vaše malin pí s prostředí plochy. Sdílení složky vám umožní použít upřednostňované plochy textový editor (například [Visual Studio Code](https://code.visualstudio.com/) nebo [Sublime Text](http://www.sublimetext.com/)) Chcete-li upravit soubory na vaše malin platformy místo použití `nano` nebo `vi`.

Sdílení složky s Windows, konfigurace serveru Samba na malin pí. Můžete taky použít integrované [SFTP](https://www.raspberrypi.org/documentation/remote-access/) serveru s klientem SFTP na ploše.

### <a name="enable-spi"></a>Povolit SPI

Než spustíte ukázkovou aplikaci, je nutné povolit sběrnici sériové periferní rozhraní (SPI) na malin pí. Malin platformy zařízení senzor BME280 komunikuje přes sběrnici SPI. Chcete-li upravit konfigurační soubor, použijte následující příkaz:

```sh
sudo nano /boot/config.txt
```

Vyhledejte řádek:

`#dtparam=spi=on`

- Chcete-li Odkomentujte řádek, odstraňte `#` na začátku.
- Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor (**Ctrl-X**).
- Pokud chcete povolit SPI, restartování malin pí. Restartování odpojí terminálu, musíte se přihlásit znovu, pokud restartování malin platformy:

  ```sh
  sudo reboot
  ```


[img-connection-diagram]: media/iot-suite-v1-raspberry-pi-kit-prepare-pi/rpi2_remote_monitoring.png

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/