<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-download-hotfixes"></a>Stažení oprav hotfix
Proveďte následující kroky stáhnout aktualizace softwaru.

1. Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Pokud na tomto počítači používáte Katalog služby Microsoft Update poprvé, po zobrazení výzvy k instalaci doplňku Katalog služby Microsoft Update klikněte na **Nainstalovat**.
    ![Nainstalujte katalogu](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)
3. Do vyhledávacího pole katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) opravy hotfix, které chcete stáhnout, například **3063418**a potom klikněte na **vyhledávání**.
4. Zobrazí se **StorSimple aktualizace 1.2 zařízení aktualizace** sady. Klikněte na tlačítko **Přidat**. Aktualizace bude přidána do košíku.
5. Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce (**3043005** a **3063416**) a přidejte všechny košíku.
6. Klikněte na **Zobrazit košík**.
   
    ![Zobrazit košík](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. Klikněte na **Stáhnout**. Zadejte místní umístění, do kterého chcete aktualizace stáhnout, nebo do něj přejděte pomocí tlačítka **Procházet**. Aktualizace se stáhnou do zadaného umístění do podsložky se stejným názvem, jako má aktualizace. Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.

> [!NOTE]
> Opravy hotfix musí být dostupný z obou řadičích ke zjištění potenciálních chybové zprávy z druhé strany řadiče.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Instalace a ověření oprav hotfix běžného režimu
Proveďte následující kroky k instalaci a ověření opravy hotfix regular režimu. Pokud jste již nainstalovali pomocí portálu Azure, přeskočit na [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).

1. K instalaci aktualizace softwaru, přístup k rozhraní Windows PowerShell na konzole sériového portu zařízení StorSimple. Postupujte podle podrobných pokynů v článku [Připojení ke konzole sériového portu pomocí klienta PuTTy](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Na příkazovém řádku stiskněte **Enter**.
2. Vyberte **Možnost 1** a přihlaste se k zařízení s úplným přístupem.
3. Chcete-li instalaci balíčku aktualizace, na příkazovém řádku zadejte:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    V cestě ke sdílené složce v předchozím příkazu používejte IP adresu místo DNS. Parametr Credential se používá pouze pro přístup ke sdílené složce s nutností ověření.
   
    Parametr Credential doporučujeme používat pro přístup ke sdíleným složkám. I sdílené složky otevřené všem uživatelům obvykle nejsou otevřené pro neověřené uživatele.
   
    Ukázkový výstup najdete níž.
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts the hotfix installation and could reboot one or
    both of the controllers. If the device is serving I/Os, these will not
    be disrupted. Are you sure you want to continue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. Po zobrazení výzvy k potvrzení instalace opravy hotfix zadejte **Y**.
5. Průběh aktualizace můžete sledovat pomocí rutiny `Get-HcsUpdateStatus`.
   
    Následující ukázkový výstup ukazuje probíhající aktualizaci. Když aktualizace probíhá, hodnota `RunInprogress` bude `True`.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Následující ukázkový výstup ukazuje dokončení aktualizace. Když se aktualizace dokončí, hodnota `RunInProgress` bude `False`.

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > Rutina občas hlásí `False`, i když aktualizace stále probíhá. Pokud chcete zkontrolovat, že se oprava hotfix dokončila, počkejte několik minut, znovu spusťte tento příkaz a ověřte, že hodnota `RunInProgress` je `False`. Pokud ano, oprava hotfix byla dokončena.
   > 
   > 
6. Po dokončení aktualizace softwaru zkontrolujte verze systémového softwaru. Zadejte následující příkaz:
   
    `Get-HcsSystem`
   
    Měly by se zobrazit následující verze:
   
   * HcsSoftwareVersion: 6.3.9600.17584
   * CisAgentVersion: 1.0.9049.0
   * MdsAgentVersion: 26.0.4696.1433
     
     Pokud číslo verze se nezmění po instalaci aktualizace, znamená to, že se nepodařilo použít opravu hotfix. Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-contact-microsoft-support.md).
7. Zopakujte kroky 3 až 5 pro instalaci oprav hotfix zbývající regular režimu (KB3043005).

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Instalace a ověření oprav hotfix režimu údržby
Použití KB3063416 k instalaci aktualizace firmwaru disku. Tyto jsou rušivý aktualizace a trvat zhruba 30 – 45 minut. Můžete se rozhodnout je nainstalovat během naplánovaného časového období údržby pomocí připojení ke konzole sériového portu zařízení.

Pokud chcete nainstalovat aktualizace firmwaru disku, postupujte podle následujících pokynů.

1. Umístíte zařízení do režimu údržby. Všimněte si, že byste neměli používat vzdálenou komunikaci prostředí Windows PowerShell, při připojení k zařízení v režimu údržby. Musíte se tato rutina spustit na řadiči zařízení při připojení prostřednictvím konzole sériového portu zařízení. Zadejte:
   
    `Enter-HcsMaintenanceMode`
   
    Ukázkový výstup najdete níž.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Oběma řadičům potom restartujte do režimu údržby.
2. Pokud chcete nainstalovat aktualizaci firmwaru disku, zadejte:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Ukázkový výstup najdete níž.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Průběh instalace můžete sledovat pomocí příkazu `Get-HcsUpdateStatus`. Když se `RunInProgress` změní na `False`, aktualizace je dokončena.
4. Po dokončení instalace bude nutné restartovat řadiče, na kterém byla nainstalována oprava hotfix režimu údržby. Přihlaste se jako Možnost 1 s úplným přístup a zkontrolujte verzi firmwaru disku. Zadejte:
   
   `Get-HcsFirmwareVersion`
   
   Očekávané verze firmwaru disku jsou:
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   Na druhém kontroleru spusťte příkaz `Get-HcsFirmwareVersion` a ověřte, že došlo k aktualizaci verze softwaru. Potom můžete ukončit režim údržby. Zadejte následující příkaz pro každý řadič zařízení:
   
   `Exit-HcsMaintenanceMode`
5. V řadičích restartovat po ukončení režimu údržby. Po úspěšné instalaci aktualizací firmwaru disku a ukončení režimu údržby na zařízení se vraťte na portál Azure Classic. Všimněte si, že nemusí zobrazit na portálu, že jste nainstalovali aktualizace režimu údržby po dobu 24 hodin.

