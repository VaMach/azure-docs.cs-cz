<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>K instalaci aktualizací režimu údržby pomocí prostředí Windows PowerShell pro StorSimple
1. Pokud jste tak ještě neučinili, přístup k konzoly sériového portu zařízení a vyberte možnost 1, **přihlásit úplný přístup**. 
2. Zadejte heslo. Výchozí heslo je **Heslo1**.
3. Na příkazovém řádku zadejte:
   
     `Get-HcsUpdateAvailability` 
4. Zobrazí se upozornění, pokud jsou k dispozici aktualizace, a zda jsou aktualizace rušivý nebo omezovaly. Na rušivý aktualizace, musíte převést zařízení do režimu údržby. V tématu [krok 2: Zadejte údržby režimu](../articles/storsimple/storsimple-update-device.md#step2) pokyny.
5. Když je zařízení v režimu údržby, na příkazovém řádku zadejte:`Start-HcsUpdate`
6. Zobrazí se výzva k potvrzení. Po ověření, aktualizace, budou nainstalovány na řadiči, který se právě používají. Po instalaci aktualizace, řadičem se restartuje. 
7. Monitorujte stav aktualizace. Zadejte:
   
    `Get-HcsUpdateStatus`
   
    Pokud `RunInProgress` je `True`, aktualizaci stále probíhá. Pokud `RunInProgress` je `False`, znamená to, že aktualizace byla dokončena.  
8. Pokud je aktualizace nainstalována na řadiči aktuální a jeho restartování, připojit k jiné řadiče a proveďte kroky 1 až 6.
9. Po aktualizaci oba řadiče ukončení režimu údržby. V tématu [krok 4: režim údržby ukončení](../articles/storsimple/storsimple-update-device.md#step4) pokyny.

