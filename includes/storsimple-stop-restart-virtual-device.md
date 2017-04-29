#### <a name="to-stop-and-start-a-virtual-device"></a>Zastavení a spuštění virtuálního zařízení
Pokud chcete zastavit virtuální zařízení, klikněte na jeho název a potom klikněte na **Vypnout**. Když se virtuální zařízení vypíná, zobrazuje se u něj stav **Zastavování**. Po zastavení virtuálního zařízení se zobrazuje stav **Zastaveno**.

K zastavení a spuštění virtuálního zařízení slouží následující rutiny.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-virtual-device"></a>Restartování virtuálního zařízení
Když běží virtuální zařízení a vy ho chcete restartovat, klikněte na jeho název a potom klikněte na **Restartovat**. Když se virtuální zařízení restartuje, má stav **Restartování**. Když je virtuální zařízení připravené k použití, má stav **Spuštěno**.

K restartování virtuálního zařízení slouží následující rutina.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

