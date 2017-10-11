<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>Chcete-li nainstalovat pravidelné aktualizace pomocí prostředí Windows PowerShell pro StorSimple
1. Otevření konzoly sériového portu zařízení a vyberte možnost 1, **přihlásit úplný přístup**. Zadejte heslo. Výchozí heslo je *Heslo1*. 
2. Na příkazovém řádku zadejte:
   
     `Get-HcsUpdateAvailability`
   
    Zobrazí se upozornění, pokud jsou k dispozici aktualizace, a zda jsou aktualizace rušivý nebo omezovaly.
3. Na příkazovém řádku zadejte:
   
     `Start-HcsUpdate`
   
    Spustí se proces aktualizace.

> [!IMPORTANT]
> * Tento příkaz se vztahuje pouze na pravidelné aktualizace. Tento příkaz spustit na jenom jeden řadič, ale oba řadiče se pak zaktualizuje. 
> * Může dojít k selhání řadiče během procesu aktualizace. převzetí služeb při selhání však nebude mít vliv dostupnosti systému nebo operace.
> 
> 

