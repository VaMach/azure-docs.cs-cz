<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>Pro instalaci regulární oprav hotfix prostřednictvím Windows Powershellu pro StorSimple
1. Připojte ke konzole sériového portu zařízení. Další informace najdete v tématu [krok 1: připojení ke konzole sériového portu](../articles/storsimple/storsimple-update-device.md#step1).
2. V nabídce konzoly sériového portu, vyberte možnost 1, **přihlásit úplný přístup**. Zadejte heslo. Výchozí heslo je **Heslo1**.
3. Na příkazovém řádku zadejte:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Tento příkaz se vztahuje pouze na regulární opravy hotfix. Tento příkaz spustit na jenom jeden řadič, ale oba řadiče se pak zaktualizuje.
    > Může dojít k selhání řadiče během procesu aktualizace. převzetí služeb při selhání však nebude mít vliv dostupnosti systému nebo operace.

4. Po zobrazení výzvy zadejte cestu k sdílené síťové složce, která obsahuje soubory oprav hotfix.
5. Zobrazí se výzva k potvrzení. Typ **Y** pokračovat v instalaci oprav hotfix.

