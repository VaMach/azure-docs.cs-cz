
Rychlé starty Azure Functions obsahují funkční kód, a proto můžete novou funkci hned otestovat.

1. Na kartě **Vývoj** si prohlédněte okno **Kód** a všimněte si, že tento kód v Node.js očekává žádost HTTP s hodnotou *name* předanou buď v textu zprávy, nebo v řetězci dotazu. Když je funkce spuštěná, tato hodnota je vrácená v odpovědi.
   
2. Kliknutím na **Otestovat** zobrazíte integrované podokno testovací žádosti HTTP pro tuto funkci.
 
    ![](./media/functions-quickstart-test/function-app-develop-tab-testing.png)

3. V textovém poli **Text žádosti** změňte hodnotu vlastnosti *name* na své jméno a klikněte na **Spustit**. Uvidíte, že provedení se aktivuje testovací žádostí HTTP, do protokolů se zapíšou informace a ve **výstupu** se zobrazí odpověď „hello...“. 

4. Pokud chcete aktivovat provedení té samé funkce z testovacího nástroje protokolu HTTP nebo z jiného okna prohlížeče, zkopírujte hodnotu **adresy URL funkce** na kartě **Vývoj** a vložte ji do nástroje nebo adresního řádku prohlížeče. Připojte k adrese URL hodnotu řetězce dotazu `&name=yourname` a proveďte požadavek. Všimněte si, že stejné informace se zapisují do protokolů a stejný řetězec je obsažený v těle zprávy s odpovědí.

    ![](./media/functions-quickstart-test/function-app-browser-testing.png)


<!--HONumber=Feb17_HO2-->


