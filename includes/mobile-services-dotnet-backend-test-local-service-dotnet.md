
Projekt mobilní služby, který stáhnete, vám umožní spustit novou mobilní službu přímo na místním počítači nebo virtuálním počítači. To usnadňuje ladění kódu služby před jeho publikací v Azure.

V této části provedete testování nové aplikace v lokálně spuštěné mobilní službě.

1. Přejděte do umístění, kam jste uložili komprimované soubory projektu, rozbalte soubory do počítače a otevřete soubor řešení ve Visual Studiu.

2. V Průzkumníku řešení v sadě Visual Studio klikněte pravým tlačítkem myši na svůj projekt služby, klikněte na **Nastavit jako spouštěný projekt** a potom stiskněte klávesu **F5**, aby se projekt sestavil a spustila se místní mobilní služba.

    ![](./media/mobile-services-dotnet-backend-test-local-service-dotnet/mobile-service-startup.png)

    Po úspěšném spuštění mobilní služby se zobrazí webová stránka.

3. Pokud chcete testovat aplikaci pro Store, klikněte pravým tlačítkem myši na projekt klientské aplikace, klikněte na **Nastavit jako spouštěný projekt** a potom stiskněte klávesu **F5**, aby se projekt znovu sestavil a spustila se aplikace.

    Tím se spustí aplikace, která se připojí k místní instanci mobilní služby.   

4. V aplikaci zadejte smysluplný text, třeba _Dokončit kurz_, do části **Vložení úkolu** a klikněte na **Uložit**.

    Tím odešlete do místní mobilní služby požadavek POST. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí pomocí mobilní služby a v druhém sloupci aplikace se zobrazí data.


<!--HONumber=Aug16_HO4-->


