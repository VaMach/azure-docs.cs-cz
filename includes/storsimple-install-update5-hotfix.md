<!--author=alkohli last changed: 08/21/17-->

#### <a name="to-download-hotfixes"></a>Stažení oprav hotfix

Provedením následujících kroků si stáhněte aktualizace softwaru z Katalogu služby Microsoft Update.

1. Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Pokud na tomto počítači používáte Katalog služby Microsoft Update poprvé, po zobrazení výzvy k instalaci doplňku Katalog služby Microsoft Update klikněte na **Nainstalovat**.

    ![Instalace katalogu](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. Do vyhledávacího pole katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) opravy hotfix, které chcete stáhnout, například **4037264**a potom klikněte na **vyhledávání**.
   
    Zobrazí se seznam oprav hotfix, například **kumulativní softwaru sady aktualizací 5.0 pro zařízení StorSimple řady 8000**.
   
    ![Prohledávání katalogu](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. Klikněte na **Stáhnout**. Zadejte místní umístění, do kterého chcete aktualizace stáhnout, nebo do něj přejděte pomocí tlačítka **Procházet**. Klikněte na tlačítko soubory ke stažení do zadaného umístění a složky. Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.
5. Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce (**4037266**) a stáhněte soubory, odpovídající týkající se určitých složek, jak je uvedeno v předchozí tabulce.

> [!NOTE]
> Opravy hotfix musí být dostupný z obou řadičích ke zjištění potenciálních chybové zprávy z druhé strany řadiče.
>
> Opravy hotfix je nutné zkopírovat do 3 oddělených složek. Například aktualizace softwaru, SNS/MDS agenta zařízení je možné zkopírovat v _FirstOrderUpdate_ složku, všechny ostatní omezovaly aktualizace může kopírovat v _SecondOrderUpdate_ složku, a aktualizace režimu údržby zkopírovali v _ThirdOrderUpdate_ složky.

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Instalace a ověření oprav hotfix běžného režimu

Provedením následujících kroků nainstalujte a ověřte opravy hotfix běžného režimu. Pokud jste již nainstalovali pomocí portálu Azure, přeskočit na [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).

1. Pokud chcete nainstalovat opravy hotfix, v konzole sériového portu zařízení StorSimple spusťte rozhraní Windows PowerShell. Postupujte podle podrobných pokynů v článku [Připojení ke konzole sériového portu pomocí klienta PuTTy](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). Na příkazovém řádku stiskněte **Enter**.
2. Vyberte **Možnost 1** a přihlaste se k zařízení s úplným přístupem. Doporučujeme opravu hotfix nejprve nainstalovat na pasivním kontroleru.
3. Pokud chcete nainstalovat opravu hotfix, na příkazovém řádku zadejte:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    V cestě ke sdílené složce v předchozím příkazu používejte IP adresu místo DNS. Parametr Credential se používá pouze pro přístup ke sdílené složce s nutností ověření.
   
    Parametr Credential doporučujeme používat pro přístup ke sdíleným složkám. I sdílené složky otevřené všem uživatelům obvykle nejsou otevřené pro neověřené uživatele.
   
4. Po zobrazení výzvy zadejte heslo. Ukázkový výstup instalace aktualizací prvního řádu najdete níž. Pro první pořadí aktualizací budete muset přejděte na konkrétní soubor.

    >[!NOTE] 
    > Pokud nainstalujete _HcsSoftwareUpdate.exe_ první. Po dokončení této instalace poté nainstalujte _CisMdsAgentUpdate.exe_.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
5. Po zobrazení výzvy k potvrzení instalace opravy hotfix zadejte **Y**.
6. Průběh aktualizace můžete sledovat pomocí rutiny `Get-HcsUpdateStatus`. Aktualizace se nejdříve dokončí na pasivním kontroleru. Jakmile je pasivní kontroler aktualizovaný, proběhne převzetí služeb při selhání a potom se aktualizace nainstaluje i na druhý kontroler. Aktualizace je dokončena, když jsou aktualizované oba kontrolery.
   
    Následující ukázkový výstup ukazuje probíhající aktualizaci. `RunInprogress` Je `True` po aktualizaci v průběhu.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Následující ukázkový výstup ukazuje dokončení aktualizace. `RunInProgress` Je `False` po dokončení aktualizace.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > Rutina občas hlásí `False`, i když aktualizace stále probíhá. Pokud chcete zkontrolovat, že se oprava hotfix dokončila, počkejte několik minut, znovu spusťte tento příkaz a ověřte, že hodnota `RunInProgress` je `False`. Pokud ano, oprava hotfix byla dokončena.

7. Po dokončení aktualizace softwaru zkontrolujte verze systémového softwaru. Zadejte:
   
    `Get-HcsSystem`
   
    Měly by se zobrazit následující verze:
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   *  `HcsSoftwareVersion: 6.3.9600.17845`
   
    Pokud se číslo verze po instalaci aktualizace nezměnilo, znamená to, že se instalace opravy hotfix nezdařila. Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-8000-contact-microsoft-support.md).
     
    > [!IMPORTANT]
    > Je nutné restartovat řadič active prostřednictvím `Restart-HcsController` rutiny před použitím další aktualizace.
     
8. Opakujte kroky 3 až 6 pro instalaci _CisMDSAgentupdate.exe_ agenta stažena do vaší _FirstOrderUpdate_ složky.
8. Zopakujte kroky 3 až 6 pro instalaci druhé pořadí aktualizací. 

    > [!NOTE] 
    > Druhý pořadí aktualizací, lze nainstalovat víc aktualizací právě spuštěním `Start-HcsHotfix cmdlet` a přejdete na složku, kde jsou umístěny druhý pořadí aktualizací. Rutina spustí všechny aktualizace, které jsou ve složce k dispozici. Pokud je aktualizace již nainstalovaná, logika aktualizace to pozná a nebude ji instalovat.

    Jakmile budou nainstalované všechny opravy hotfix, použijte rutinu `Get-HcsSystem`. Verze by měly být:
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Instalace a ověření oprav hotfix režimu údržby

Použití KB4037263 k instalaci aktualizace firmwaru disku. Jedná se o narušující aktualizace a jejich dokončení trvá přibližně 30 minut. Můžete se rozhodnout je nainstalovat během naplánovaného časového období údržby pomocí připojení ke konzole sériového portu zařízení.

> [!NOTE] 
> Pokud už je aktuální firmware disku, musíte nebude tyto aktualizace nainstalujete. Z konzoly sériového portu zařízení spusťte rutinu `Get-HcsUpdateAvailability`, která zkontroluje dostupnost aktualizací a jestli se jedná o narušující (režim údržby) nebo nenarušující (běžný režim) aktualizace.

Pokud chcete nainstalovat aktualizace firmwaru disku, postupujte podle následujících pokynů.

1. Umístíte zařízení do režimu údržby. 

    > [!NOTE] 
    > Při připojování k zařízení v režimu údržby, nepoužívejte vzdálenou komunikaci prostředí Windows PowerShell. Místo toho spusťte tuto rutinu v řadiče zařízení při připojení prostřednictvím konzole sériového portu zařízení.

    Chcete-li kontroler převeďte do režimu údržby, zadejte:
   
    `Enter-HcsMaintenanceMode`
   
    Ukázkový výstup najdete níž.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Oba kontrolery se následně restartují do režimu údržby.
2. Pokud chcete nainstalovat aktualizaci firmwaru disku, zadejte:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Ukázkový výstup najdete níž.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Průběh instalace můžete sledovat pomocí příkazu `Get-HcsUpdateStatus`. Když se `RunInProgress` změní na `False`, aktualizace je dokončena.
4. Po dokončení instalace se kontroler, na který se instalovala oprava hotfix režimu údržby, restartuje. Přihlaste se jako Možnost 1 s úplným přístup a zkontrolujte verzi firmwaru disku. Zadejte:
   
   `Get-HcsFirmwareVersion`
   
   Očekávané verze firmwaru disku jsou:
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   Ukázkový výstup najdete níž.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    Na druhém kontroleru spusťte příkaz `Get-HcsFirmwareVersion` a ověřte, že došlo k aktualizaci verze softwaru. Potom můžete ukončit režim údržby. Uděláte to tak, že na obou kontrolerech zařízení zadáte následující příkaz:
   
   `Exit-HcsMaintenanceMode`

5. Kontrolery se restartují, jakmile ukončíte režim údržby. Po disk firmware se úspěšně aktualizace a zařízení ukončilo režim údržby, vraťte se na portálu Azure. Poznámka: Informace o instalaci aktualizací režimu údržby se na portálu může zobrazit až po 24 hodinách.

