<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>Přejít do režimu údržby
1. V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**.
2. Zadejte heslo. Výchozí heslo je **Heslo1**.
3. Na příkazovém řádku zadejte
   
     `Enter-HcsMaintenanceMode`
4. Zobrazí se upozornění oznamující, že bude přerušit všechny vstupně-výstupní požadavky a severu připojení k portálu Azure classic režimu údržby a zobrazí se výzva k potvrzení. Typ **Y** vstoupit do režimu údržby.
   
    Oba řadiče se restartuje. Po dokončení restartování se zobrazí další zpráva označující, že zařízení je v režimu údržby.

