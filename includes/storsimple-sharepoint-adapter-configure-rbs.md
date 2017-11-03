<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> Při provádění změn StorSimple adaptéru pro konfiguraci RBS služby SharePoint, musíte být přihlášeni pomocí uživatelského účtu, který patří do skupiny Domain Admins. Stránka konfigurace musí navíc přístup z prohlížeče spuštěna na stejném hostiteli jako centrální správy.
> 
> 

#### <a name="to-configure-rbs"></a>Ke konfiguraci RBS
1. Otevřít stránku Centrální správa SharePoint a přejděte do **nastavení systému**. 
2. V **Azure StorSimple** klikněte na tlačítko **nakonfigurovat adaptér StorSimple**.
   
    ![Nakonfigurujte adaptér StorSimple](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. Na **nakonfigurovat adaptér StorSimple** stránky:
   
   1. Ujistěte se, že **povolení úprav cesta** je zaškrtnuté políčko.
   2. Do textového pole zadejte cestu Universal Naming Convention (UNC) úložiště objektů BLOB.
      
      > [!NOTE]
      > Svazek úložiště objektů BLOB musí být hostované na jednotce iSCSI nakonfigurované v zařízení StorSimple.

   3. Klikněte **povolit** tlačítko pod každý z databáze obsahu, které chcete nakonfigurovat pro vzdálené úložiště.
      
      > [!NOTE]
      > Úložiště objektů BLOB musí být sdílené a dostupné všechny (WFE) servery front-end webové a uživatelský účet, který je nakonfigurován pro serverové farmy služby SharePoint musí mít přístup ke sdílené složce.
      
      ![Povolit zprostředkovatele RBS](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      Když povolíte nebo zakážete RBS, zobrazí se také následující zpráva.
      
      ![Konfigurace zařízení StorSimple adaptéru povolit zakázat](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Klikněte **aktualizace** tlačítko použít v konfiguraci. Když kliknete **aktualizace** tlačítko Stav konfigurace RBS bude aktualizována na všech serverech WFE a celou farmu budou RBS povolena. Zobrazí se následující zpráva.
      
      ![Zpráva Konfigurace adaptéru](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Pokud konfigurujete RBS pro farmu služby SharePoint s velmi velký počet databází (větší než 200), může webové stránce Centrální správa SharePoint vypršení časového limitu. Pokud k tomu dojde, aktualizujte stránku. Proces konfigurace to nemá vliv.

4. Ověření konfigurace:
   
   1. Přihlaste se k webu Centrální správa SharePoint a přejděte do **nakonfigurovat adaptér StorSimple** stránky.
   2. Zkontrolujte podrobnosti o konfiguraci a ujistěte se, jestli se shoduje s nastavením, které jste zadali. 
5. Ověřte, že RBS funguje správně:
   
   1. Uložení dokumentu do služby SharePoint. 
   2. Přejděte k cestě UNC, která jste nakonfigurovali. Ujistěte se, zda byl vytvořen RBS adresářovou strukturu a obsahuje objekt nahrané.
6. (Volitelné) Můžete použít Microsoft RBS `Migrate()` rutiny prostředí PowerShell, které jsou součástí služby SharePoint k migraci stávajícího obsahu objektu BLOB do zařízení StorSimple. Další informace najdete v tématu [migraci obsahu do nebo z RBS v SharePoint 2013] [ 6] nebo [migraci obsahu do nebo z RBS (SharePoint Foundation 2010)] [7].
7. (Volitelné) Na zkušební instalace můžete ověřit, že byly objekty BLOB přesunuty z databáze obsahu: 
   
   1. Spusťte SQL Management Studio.
   2. Spusťte dotaz ListBlobsInDB_2010.sql nebo ListBlobsInDB_2013.sql následujícím způsobem.
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      Pokud RBS bylo správně nakonfigurované, by se zobrazit hodnotu NULL ve sloupci SizeOfContentInDB pro libovolný objekt, který byl odeslán a úspěšně externalized s RBS.
8. (Volitelné) Po konfiguraci RBS a přesunout všechny obsah objektu BLOB do zařízení StorSimple, můžete přesunout databázi obsahu do zařízení. Pokud budete chtít přesunout databázi obsahu, doporučujeme nakonfigurovat databázi obsahu úložiště v zařízení jako primární svazek. Pak použijte navázat systému SQL Server osvědčené postupy k migraci databáze obsahu do zařízení StorSimple. 
   
   > [!NOTE]
   > Přesunutí databáze obsahu do zařízení je podporována pouze pro řady StorSimple 8000 (není podporována pro řad 5000 a 7000).
   
   Pokud uchováváte objekty BLOB a databáze obsahu v samostatných svazcích v zařízení StorSimple, doporučujeme vám nakonfigurovat ve stejném kontejneru svazku. Tím se zajistí, že se budou zálohovány společně.
   
   > [!WARNING]
   > Pokud jste nepovolili RBS, nedoporučujeme přesunutí databáze obsahu do zařízení StorSimple. Toto je netestované konfigurace.
   
9. Přejděte k dalšímu kroku: [konfigurace uvolňování paměti](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
