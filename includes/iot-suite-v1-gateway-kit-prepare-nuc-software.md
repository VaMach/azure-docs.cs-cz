## <a name="build-iot-edge"></a>Sestavení IoT Edge

Tento kurz používá vlastní moduly IoT okraj ke komunikaci s předkonfigurovaného řešení vzdáleného monitorování. Proto potřebujete k vytváření IoT Edge moduly z vlastní zdrojového kódu. Následující části popisují postup instalace IoT okraj a vytvořit vlastní modul IoT okraj.

### <a name="install-iot-edge"></a>Nainstalujte IoT Edge

Následující kroky popisují postup instalace softwaru předem kompilovaném IoT Edge v Intel NUC:

1. Konfigurace úložiště požadovaný balíček inteligentní spuštěním následujících příkazů na Intel NUC:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Zadejte `y` když příkaz vás vyzve, abyste **zahrnout tento kanál?**.

1. Aktualizujte správce inteligentní balíčků tak, že spustíte následující příkaz:

    ```bash
    smart update
    ```

1. Instalovat balíček Azure IoT Edge tak, že spustíte následující příkaz:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. Ověření instalace spuštěním ukázky "Hello, world". Tato ukázka zapíše zprávu hello world k souboru log.txT každých pět sekund. Ukázku "Hello, world" spustit následující příkazy:

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Ignorovat jakékoli **neplatný argument** zprávy při zastavení vzorku.

    Chcete-li zobrazit obsah souboru protokolu, použijte následující příkaz:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Řešení potíží

Pokud se zobrazí chyba "žádný balíček zajišťuje util. linux dev", zkuste restartovat Intel NUC.
