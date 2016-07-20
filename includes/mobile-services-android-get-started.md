Závěrečná fáze tohoto kurzu je sestavení a spuštění nové aplikace.

### Načtení projektu do Android Studia a synchronizace nástroje Gradle

1. Přejděte do umístění, kam jste uložili komprimované soubory projektu, a rozbalte soubory do počítače do adresáře projektů Android Studia.

2. Otevřete Android Studio. Pokud pracujete s projektem a projekt se otevře, zavřete ho (File (Soubor) => Close Project (Zavřít projekt)).

3. Vyberte **Open an existing Android Studio project** (Otevřít existující projekt Android Studia), přejděte do umístění projektu a potom klikněte na tlačítko **OK.** Tím projekt načtete a spustíte synchronizaci s Gradlem.

    ![](./media/mobile-services-android-get-started/android-studio-import-project.png)

4. Počkejte na dokončení synchronizace Gradlu. Pokud uvidíte chybu Failed to find target (Nepodařilo se najít cíl), je to kvůli odlišné verzi použité v Android Studiu, která neodpovídá ukázce. Nejjednodušším způsobem řešení je kliknout na odkaz **Install missing platform(s) and sync project** (Nainstalovat chybějící platformy a synchronizovat projekt) v chybové zprávě. Může se stát, že se zobrazí další chybové zprávy verze. V takovém případě stačí tento postup jednoduše opakovat tak dlouho, až se chyby přestanou zobrazovat.
    - Pokud chcete používat nejnovější verzi Androidu, existuje ještě jiný způsob řešení. Můžete aktualizovat hodnotu **targetSdkVersion** v souboru *build.gradle*, který se nachází v adresáři *app*, aby odpovídala verzi nainstalované v počítači. Tu zjistíte kliknutím na ikonu **SDK Manager** (Správce sad SDK). Potom stiskněte **Sync Project with Gradle Files** (Synchronizovat projekt se soubory Gradle). Verze sestavovacích nástrojů může vyvolat chybovou zprávu, kterou vyřešíte stejným způsobem.

### Spuštění aplikace

Aplikaci můžete spustit pomocí emulátoru nebo pomocí samotného zařízení.

1. Pokud ji chcete spustit na zařízení, připojte ho k počítači pomocí kabelu USB. Zařízení je nutné [přepnout do vývojového režimu](https://developer.android.com/training/basics/firstapp/running-app.html). Pokud vyvíjíte na počítači s Windows, musíte si stáhnout a nainstalovat ovladač USB.

2. Abyste mohli aplikaci spustit v emulátoru Android, musíte definovat alespoň jedno virtuální zařízení s Androidem (AVD). Kliknutím na ikonu správce AVD můžete tato zařízení vytvořit a spravovat.

3. V nabídce **Run** (Spustit) kliknutím na tlačítko **Run** (Spustit) spusťte projekt a v zobrazeném dialogu vyberte zařízení nebo emulátor.

4. Když se aplikace zobrazí, zadejte smysluplný text, například _Dokončit kurz_, a klikněte na **Add** (Přidat).

    ![](./media/mobile-services-android-get-started/mobile-quickstart-startup-android.png)

    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí pomocí mobilní služby a v seznamu se zobrazí data.

    > [AZURE.NOTE] Na kód, který přistupuje k mobilní službě pro dotazování a vkládání dat, se můžete podívat v souboru ToDoActivity.java.

8. Zpátky na portálu Azure Classic klikněte na kartu **Data** a potom na tabulku **TodoItems**.

    ![](./media/mobile-services-android-get-started/mobile-data-tab1.png)

    Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.

    ![](./media/mobile-services-android-get-started/mobile-data-browse.png)



<!--HONumber=Jun16_HO2-->


