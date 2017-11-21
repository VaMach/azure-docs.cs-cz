Z důvodu probíhající vývoj verze sady SDK pro Android nainstalovaná v Android Studio nemusí shodovat s verzí v kódu. V tomto kurzu odkazuje SDK pro Android je verze 26, nejnovější v době psaní textu. Číslo verze může zvýšit objeví nové verze sady SDK, a doporučujeme používat na nejnovější dostupnou verzi.

Dvěma příznaky neshoda verze jsou:

- Při vytvoření nebo znovu sestavte projekt, může dojít k Gradle chybové zprávy jako `Gradle sync failed: Failed to find target with hash string 'android-XX'`.
- Standardní Android objekty v kódu, který by měl směrovat na základě `import` příkazy může být generování chybové zprávy.

Pokud se zobrazí některá z nich, nemusí odpovídat verzi sady SDK pro Android nainstalovaná v Android Studio SDK cíl staženého projektu. Pokud chcete ověřit verzi, proveďte následující změny:

1. V Android Studio, klikněte na **nástroje** > **Android** > **SDK Manager**. Pokud jste nenainstalovali nejnovější verzi sady SDK platformy, klikněte na tlačítko ji nainstalovat. Poznamenejte si číslo verze.

2. Na **Project Exploreru** v části **Gradle skripty**, otevřete soubor **build.gradle (modulu: aplikace)**. Ujistěte se, že **compileSdkVersion** a **targetSdkVersion** jsou nastaveny na nainstalovanou nejnovější verzi sady SDK. `build.gradle` Může vypadat například takto:

    ```gradle
    android {
        compileSdkVersion 26
        defaultConfig {
            targetSdkVersion 26
        }
    }
    ```
