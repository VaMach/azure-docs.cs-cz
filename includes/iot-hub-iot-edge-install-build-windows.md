## <a name="install-the-prerequisites"></a>Nainstalujte součásti

1. Nainstalujte [Visual Studio 2015 nebo 2017](https://www.visualstudio.com). Bezplatná edice Community můžete použít, pokud splňují požadavky na licencování. Nezapomeňte zahrnout Visual C++ a Správce balíčků NuGet.

1. Nainstalujte [git](http://www.git-scm.com) a zajistěte, aby git.exe můžete spustit z příkazového řádku.

1. Nainstalujte [CMake](https://cmake.org/download/) a zajistěte, aby cmake.exe můžete spustit z příkazového řádku. CMake verze 3.7.2 nebo novější je doporučené. **.Msi** instalační program je nejjednodušší možnost v systému Windows. Přidání softwaru CMake k CESTĚ pro alespoň aktuálního uživatele, když instalační program zobrazí výzvu.

1. Nainstalujte [Python 2.7](https://www.python.org/downloads/release/python-27). Ověřte, že je přidat Python pro vaše `PATH` proměnné prostředí. Přejděte na **ovládací panely** > **systém a zabezpečení** > **systému** > **Upřesnit nastavení systému**  >  **Proměnné prostředí**. Přidat `C:\Python27` pro vaši cestu. 

1. Na příkazovém řádku spusťte následující příkaz, který naklonujte úložiště Azure IoT Edge GitHub do místního počítače:

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-to-build-the-sample"></a>Postup pro sestavení ukázky

Teď můžete sestavit IoT Edge runtime a ukázky na místním počítači:

1. Otevřete **příkazový řádek vývojáře pro VS 2015** nebo **příkazový řádek vývojáře pro VS 2017**, v závislosti na vaší verzi.

1. Přejděte do kořenové složky místní kopie úložiště **iot-edge**.

1. Spusťte skript sestavení následujícím způsobem:

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

Tento skript vytvoří soubor řešení sady Visual Studio a vytvoří řešení. Můžete najít řešení v sadě Visual Studio **sestavení** složky ve vaší místní kopii **iot hranou** úložiště. Pokud chcete vytvořit a spouštění testování částí, přidat `--run-unittests` parametr. Pokud chcete sestavit a spustit testy koncová, přidat `--run-e2e-tests`.

> [!NOTE]
> Pokaždé, když spustíte **build.cmd** skriptu, se odstraní a potom znovu vytvoří **sestavení** složky v kořenové složce místní kopii **iot hranou** úložiště.