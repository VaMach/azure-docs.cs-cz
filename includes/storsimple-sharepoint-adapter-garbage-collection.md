<!--author=SharS last changed: 9/17/15-->

V tomto postupu budete:

1. [Příprava ke spuštění spustitelného souboru Funkce Maintainer](#to-prepare-to-run-the-maintainer) .
2. [Příprava databáze obsahu a Koš pro okamžité odstranění objektů BLOB osamocené](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Spustit Maintainer.exe](#to-run-the-maintainer).
4. [Obnovit databázi obsahu a nastavení koše](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="to-prepare-to-run-the-maintainer"></a>Příprava ke spuštění funkce Maintainer
1. Na webovém serveru front-end otevřete prostředí SharePoint 2013 Management Shell jako správce.
2. Přejděte do složky *spouštěcí jednotka*: \Program Files\Microsoft 10.50\Maintainer SQL vzdáleného úložiště objektů Blob\.
3. Přejmenování **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** k **web.config**.
4. Použití `aspnet_regiis -pdf connectionStrings` pro dešifrování souboru web.config.
5. V souboru web.config dešifrovaný v části `connectionStrings` uzlu přidat připojovací řetězec pro vaše instance SQL serveru a název databáze obsahu. Prohlédněte si následující příklad.
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Použití `aspnet_regiis –pef connectionStrings` k opětovnému zašifrování souboru web.config. 
7. Přejmenujte soubor web.config Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>Příprava obsahu databáze a Koš okamžitě Odstranit osamocené objekty BLOB
1. Na serveru SQL v SQL Management Studio, spusťte následující dotazy aktualizace pro cílovou databázi obsahu: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Na webovém serveru front-end v části **centrální správy**, upravit **obecné nastavení webové aplikace** pro požadovanou databázi obsahu k dočasnému zakázání funkce Koš. Tato akce také vyprázdníte koš pro všechny související kolekce webů. Chcete-li to provést, klikněte na tlačítko **centrální správy** -> **Správa aplikací** -> **webových aplikací (Správa webové aplikace)**  ->  **SharePoint - 80** -> **obecné nastavení aplikace**. Nastavte **Stav koše** k **OFF**.
   
    ![Obecné nastavení webové aplikace](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>Spuštění funkce Maintainer
* Na webovém serveru front-end v prostředí SharePoint 2013 Management Shell, spusťte Funkce Maintainer následujícím způsobem:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Pouze `GarbageCollection` operace je podporována pro zařízení StorSimple v tuto chvíli. Všimněte si také, zda jsou parametry vydán pro Microsoft.Data.SqlRemoteBlobs.Maintainer.exe velká a malá písmena. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>Můžete obnovit databázi obsahu a nastavení koše
1. Na serveru SQL v SQL Management Studio, spusťte následující dotazy aktualizace pro cílovou databázi obsahu:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Na webovém serveru front-end v **centrální správy**, upravit **obecné nastavení webové aplikace** pro databázi požadovaného obsahu znovu zapnout Koš. Chcete-li to provést, klikněte na tlačítko **centrální správy** -> **Správa aplikací** -> **webových aplikací (Správa webové aplikace)**  ->  **SharePoint - 80** -> **obecné nastavení aplikace**. Nastavit stav Bin recyklaci na **ON**.

