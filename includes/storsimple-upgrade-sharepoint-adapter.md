<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>Upgradovat SharePoint 2010 pro službu SharePoint 2013 a potom nainstalovat adaptér StorSomple pro službu SharePoint
> [!IMPORTANT]
> Všechny soubory, které byly dříve přesunuty do externího úložiště prostřednictvím RBS nebudete mít k dispozici, dokud je dokončen upgrade a je RBS funkce znovu povolena. Pokud chcete omezit dopad uživatele, proveďte všechny upgrade nebo přeinstalaci během plánované údržby.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>K upgradu služby SharePoint 2010 na SharePoint 2013 a potom nainstalujte adaptér
1. Ve farmě služby SharePoint 2010 Všimněte si, cesta k úložišti objektů BLOB pro externalized objekty BLOB a databáze obsahu, pro které je RBS povolená. 
2. Nainstalujte a nakonfigurujte nové farmy serverů Sharepointu 2013. 
3. Přesun databáze, aplikace a kolekce webů z farmy SharePoint 2010 do nové farmy serverů Sharepointu 2013. Pokyny, přejděte na [přehled procesu upgradu na SharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. Instalace adaptéru StorSimple pro SharePoint v nové farmě. Přejděte na [instalovat adaptér StorSimple pro službu SharePoint](#install-the-storsimple-adapter-for-sharepoint) postupy.
5. Pomocí informací, které jste si poznamenali v kroku 1, povolte RBS pro stejnou sadu databází obsahu a zadat stejnou cestu úložiště objektů BLOB, která byla použita při instalaci serveru SharePoint 2010. Přejděte na [konfigurace RBS](#configure-rbs) postupy. Po dokončení tohoto kroku, dříve externalized soubory musí být přístupný z nové farmy. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>Upgrade adaptéru StorSimple pro službu SharePoint
> [!IMPORTANT]
> Měli byste naplánovat tento upgrade proběhnout během plánované údržby z následujících důvodů:
> 
> * Externalized obsah dříve nebudete mít k dispozici, dokud bude znovu nainstalován adaptér.
> * Veškerý obsah, nahrán do lokality po odinstalaci předchozí verze adaptéru StorSimple pro službu SharePoint, ale před instalací nové verze, bude uložen v databázi obsahu. Potřebujete přesunout obsah do zařízení StorSimple, po instalaci nový adaptér. Můžete použít Microsoft` RBS Migrate()` rutiny prostředí PowerShell, které jsou součástí služby SharePoint k přenosu obsahu. Další informace najdete v tématu [migraci obsahu do nebo z RBS](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>Upgrade adaptéru StorSimple pro službu SharePoint
1. Odinstalujte předchozí verzi zařízení StorSimple adaptéru pro službu SharePoint.
   
   > [!NOTE]
   > Tato akce automaticky zakáže RBS v databázích obsahu. Existující objekty BLOB, ale bude zůstanou v zařízení StorSimple. Protože je zakázán RBS a objekty BLOB nebyly migrována zpět do databáze obsahu, všechny požadavky pro tyto objekty BLOB se nezdaří. 
   > 
   > 
2. Nainstalujte nový adaptér StorSimple pro službu SharePoint. Nový adaptér databáze obsahu, které byly dříve povolit nebo zakázat pro RBS automaticky rozpozná a použije předchozí nastavení.

