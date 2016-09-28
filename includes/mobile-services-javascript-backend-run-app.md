
Závěrečná fáze tohoto kurzu je sestavení a spuštění nové aplikace.

1. Přejděte do umístění, kam jste uložili komprimované soubory projektu, rozbalte soubory do počítače a otevřete soubor řešení ve Visual Studiu.

2. Stisknutím klávesy **F5** projekt znovu sestavte a aplikaci spusťte.

3. V aplikaci zadejte smysluplný text, například *Dokončit kurz*, v části **Vložení úkolu** a klikněte na **Uložit**.

    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí pomocí mobilní služby a v druhém sloupci aplikace se zobrazí data.

4. (Volitelné) V univerzálním řešení pro Windows změňte výchozí spouštěný projekt na druhou aplikaci a znovu spusťte aplikaci.

    Všimněte si, že po spuštění aplikace se data uložená v předchozím kroku načtou z mobilní služby.
 
4. Zpátky na [portálu Azure Classic](https://manage.windowsazure.com/) klikněte na kartu **Data** a potom klikněte na tabulku **TodoItems**.

    Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.

    ![](./media/mobile-services-javascript-backend-run-app/mobile-data-browse.png)

<!--HONumber=Sep16_HO3-->


