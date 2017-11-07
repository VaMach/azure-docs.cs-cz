## <a name="prepare-your-raspberry-pi"></a>Příprava vašeho Malinová platformy

### <a name="install-raspbian"></a>Nainstalujte Raspbian

Pokud používáte vaše platformy malin poprvé, musíte nainstalovat operační systém Raspbian pomocí NOOBS na kartu SD. součástí sady. [Malin pí softwaru průvodce] [ lnk-install-raspbian] popisuje postup instalace operačního systému na vaše malin pí. Tento kurz předpokládá, že jste nainstalovali Raspbian operačního systému na vaše malin pí.

> [!NOTE]
> Používání SD karet součástí [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] již má nainstalované NOOBS. Můžete spustit pí malin z této karty a zvolit instalaci operačního systému Raspbian.

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

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/