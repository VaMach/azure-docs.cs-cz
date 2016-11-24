#### <a name="to-create-a-virtual-device"></a>Vytvoření virtuálního zařízení
1. Na portálu Azure přejděte do služby **StorSimple Manager**.
2. Přejděte na stránku **Zařízení**. V dolní části stránky **Zařízení** klikněte na **Vytvořit virtuální zařízení**.
3. V dialogovém okně **Vytvořit virtuální zařízení** zadejte následující podrobnosti.
   
    ![Vytvoření virtuálního zařízení StorSimple](./media/storsimple-create-virtual-device-u2/CreatePremiumsva1.png)
   
   1. **Název** – jedinečný název virtuálního zařízení.
   2. **Model** – zvolte model virtuálního zařízení. Toto pole se zobrazí, jenom pokud používáte verzi Update 2 nebo novější. Model zařízení 8010 nabízí úložiště Standard Storage 30 TB a model 8020 úložiště Premium Storage 64 TB. Pokud chcete nasadit zařízení pro scénáře obnovování položek ze záloh,
   3. zadejte 8010. Pokud chcete nasadit zařízení zajišťující úlohy s vysokými požadavky na výkon a nízkou latenci nebo sloužící jako sekundární zařízení pro zotavení po havárii, vyberte 8020.
   4. **Verze** – zvolte verzi virtuálního zařízení. Pokud je vybrán model zařízení 8020, uživateli se nezobrazí pole verze. Pokud všechna fyzická zařízení registrovaná ve službě používají verzi Update 1 (nebo novější), tato možnost chybí. Toto pole se zobrazí, jenom když ve stejné službě používáte kombinaci fyzických zařízení s verzí starší než Update 1 a s verzí Update 1 nebo novější. Protože verze virtuálního zařízení určuje, ze kterého virtuálního zařízení můžete převzít služby při selhání či klonovat, je důležité vybrat správnou verzi virtuálního zařízení. Vyberte:
      
      * Verzi Update 0.3, pokud budete provádět převzetí služeb při selhání nebo zotavení při havárii z fyzického zařízení s verzí Update 0.3 nebo starší. 
      * Verzi Update 1, pokud budete provádět převzetí služeb při selhání nebo klonování z fyzického zařízení s verzí Update 1 nebo novější. 
   5. **Virtuální síť** – zadejte virtuální síť, kterou chcete používat s tímto virtuálním zařízením. Při použití služby Premium Storage (Update 2 nebo novější) je nutné vybrat virtuální síť, která je podporována účtem služby Premium Storage. Nepodporované virtuální sítě se v seznamu zobrazí šedě. Pokud vyberete nepodporovanou virtuální síť, zobrazí se upozornění. 
   6. **Účet úložiště pro vytvoření virtuálního zařízení** – vyberte účet úložiště, který bude uchovávat image virtuálního zařízení během zřizování. Tento účet úložiště by měl být ve stejné oblasti jako virtuální zařízení a virtuální síť. Neměl by se používat k ukládání dat do fyzického ani virtuálního zařízení. Ve výchozím nastavení se za tímto účelem vytvoří nový účet úložiště. Pokud však víte, že už máte účet úložiště vhodný k tomuto účelu, můžete ho vybrat v seznamu. Při vytváření prémiového virtuálního zařízení se v rozevíracím seznamu zobrazí jenom účty služby Premium Storage. 
      
      > [!NOTE]
      > Virtuální zařízení můžete používat jenom s účty služby Azure Storage. Ostatní poskytovatele cloudových služeb, například Amazon, HP a OpenStack (které podporuje fyzické zařízení), virtuální zařízení StorSimple nepodporuje.
      > 
      > 
   7. Zaškrtnutím příslušného políčka potvrďte, že jste seznámeni s tím, že data uložená ve virtuálním zařízení budou hostována v datovém centru Microsoftu. Pokud používáte jenom fyzické zařízení, váš šifrovací klíč se uchovává se zařízením, a Microsoft proto nemůže data dešifrovat. 
      
       Při použití virtuálního zařízení jsou šifrovací i dešifrovací klíče uloženy v Microsoft Azure. Další informace najdete v článku o [bezpečnostních aspektech použití virtuálního zařízení](../articles/storsimple/storsimple-security.md#storsimple-virtual-device-security).
   8. Kliknutím na ikonu zaškrtnutí vytvořte virtuální zařízení. Zřízení zařízení může trvat zhruba 30 minut.
      
      ![Fáze vytváření virtuálního zařízení StorSimple](./media/storsimple-create-virtual-device-u2/StorSimple_VirtualDeviceCreating1M.png)



<!--HONumber=Nov16_HO3-->


