## <a name="install-the-prerequisites"></a>Nainstalujte součásti

Postup v tomto kurzu se předpokládá, že používáte Ubuntu Linux.

K instalaci požadovaných balíčků, otevřete prostředí a spusťte následující příkazy:

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake pkg-config libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

V prostředí spusťte následující příkaz, který naklonujte úložiště Azure IoT Edge GitHub do místního počítače:

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-to-build-the-sample"></a>Postup pro sestavení ukázky

Teď můžete sestavit IoT Edge runtime a ukázky na místním počítači:

1. Otevřete prostředí.

1. Přejděte do kořenové složky místní kopie úložiště **iot-edge**.

1. Spusťte skript sestavení následujícím způsobem:

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

Tento skript vytvoří v kořenové složce místní kopie úložiště **iot-edge** pomocí nástroje **cmake** složku **build** a vygeneruje soubor pravidel. Skript potom sestaví řešení a přeskočí při tom testy jednotek a celkové testy. Pokud chcete vytvořit a spouštění testování částí, přidat `--run-unittests` parametr. Pokud chcete sestavit a spustit testy koncová, přidat `--run-e2e-tests`.

> [!NOTE]
> Pokaždé když spustíte skript **build.sh**, odstraní a potom znovu vytvoří složku **build** v kořenové složce místní kopie úložiště **iot-edge**.