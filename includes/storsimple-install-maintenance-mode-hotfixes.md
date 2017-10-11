<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Pro instalaci oprav hotfix režimu údržby pomocí prostředí Windows PowerShell pro StorSimple
> [!IMPORTANT]
> V režimu údržby musíte nejprve použít opravu hotfix na jednom řadiči a poté na jiný řadič.
> 
> 

1. Umístíte zařízení do režimu údržby. V tématu [krok 2: Zadejte údržby režimu](../articles/storsimple/storsimple-update-device.md#step2) pokyny o tom, jak přechod do režimu údržby.
2. Chcete-li použít opravu hotfix, zadejte:
   
     `Start-HcsHotfix` 
3. Po zobrazení výzvy zadejte cestu k sdílené síťové složce, která obsahuje soubory oprav hotfix.
4. Zobrazí se výzva k potvrzení. Typ **Y** pokračovat v instalaci oprav hotfix.
5. Po instalaci opravy hotfix na jednom řadiči, přihlaste se k jiné řadiče. Opravy hotfix, stejně jako u předchozí kontroleru.
6. Po použití opravy hotfix, ukončete režim údržby. V tématu [krok 4: režim údržby ukončení](../articles/storsimple/storsimple-update-device.md#step4) pokyny.

