Z důvodu probíhající vývoj verze sady SDK pro Android nainstalovaná v Android Studio nemusí shodovat s verzí v kódu. V tomto kurzu odkazuje SDK pro Android je verze 23, nejnovější v době psaní textu. Číslo verze může zvýšit objeví nové verze sady SDK, a doporučujeme používat na nejnovější dostupnou verzi.

Dvěma příznaky neshoda verze jsou:

- Při vytvoření nebo znovu sestavte projekt, může dojít k Gradle chybové zprávy jako "**se nepodařilo najít cílový Google Inc.:Google APIs:n**".
- Standardní Android objekty v kódu, který by měl směrovat na základě `import` příkazy může být generování chybové zprávy.

Pokud se zobrazí některá z nich, nemusí odpovídat verzi sady SDK pro Android nainstalovaná v Android Studio SDK cíl staženého projektu. Pokud chcete ověřit verzi, proveďte následující změny:

1. V Android Studio, klikněte na **nástroje** > **Android** > **SDK Manager**. Pokud jste nenainstalovali nejnovější verzi sady SDK platformy, klikněte na tlačítko ji nainstalovat. Poznamenejte si číslo verze.
2. Na **Project Exploreru** v části **Gradle skripty**, otevřete soubor **build.gradle (modeule: aplikace)**. Ujistěte se, že **compileSdkVersion** a **buildToolsVersion** jsou nastaveny na nainstalovanou nejnovější verzi sady SDK. Značky může vypadat například takto:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. V prohlížeči projektu Android Studio, klikněte pravým tlačítkem na uzel projektu, zvolte **vlastnosti**a v levém sloupci vyberte **Android**. Ujistěte se, že **cíl sestavení projektu** je nastaven na stejnou verzi sady SDK, jako **targetSdkVersion**.

V nástroji Android Studio souboru manifestu už slouží k určení cíle SDK a minimální verze SDK, na rozdíl od případu se Eclipse.
