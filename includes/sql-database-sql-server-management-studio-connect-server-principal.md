

## Připojení ke službě Azure SQL Database pomocí hlavního přihlášení na úrovni serveru

Pokud se chcete připojit ke službě Azure SQL Database pomocí aplikace SSMS s hlavním přihlášením na úrovni serveru, postupujte následovně.

1. Do pole vyhledávání ve Windows zadejte text „Microsoft SQL Server Management Studio“ a potom kliknutím na desktopovou aplikaci spusťte aplikaci SSMS.

2. V okně Připojení k serveru zadejte následující informace:

 - **Typ serveru**: Výchozí hodnotou je databázový stroj. Tuto hodnotu nemůžete změnit.
 - **Název serveru**: Zadejte název serveru, který je hostitelem služby SQL Database. Použijte následující formát: *&lt;název_serveru >*.**database.windows.net**
 - **Typ ověřování**: Pokud teprve začínáte, vyberte Ověřování SQL. Pokud jste pro logický server služby SQL Database povolili službu Active Directory, můžete si vybrat Ověřování hesla Active Directory nebo Integrované ověřování Active Directory.
 - **Uživatelské jméno**: Pokud jste vybrali Ověřování SQL nebo Ověřování hesla Active Directory, zadejte jméno uživatele, který má přístup k databázi na serveru.
 - **Heslo**: Pokud jste vybrali Ověřování SQL nebo Ověřování hesla Active Directory, zadejte heslo uvedeného uživatele.
   
       ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-1.png)

3. Klikněte na **Připojit**.
 
4. Pokud IP adresa vašeho klienta nemá přístup k logickému serveru služby SQL Database, budete vyzváni k přihlášení k účtu Azure a k vytvoření pravidla brány firewall na úrovni serveru. Pokud jste správci předplatného Azure, klikněte na **Přihlásit** a vytvořte pravidlo brány firewall na úrovni serveru. Pokud nejste správci, požádejte správce Azure, aby pravidlo brány firewall na úrovni serveru vytvořil za vás.
 
      ![SQL Server Management Studio: Připojení k serveru služby SQL Database.](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-2.png)
 
1. Pokud jste správci předplatného Azure a potřebujete se přihlásit, zadejte po zobrazení přihlašovací stránky přihlašovací údaje svého předplatného a přihlaste se.

      ![přihlášení](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-3.png)
 
1. Po úspěšném přihlášení do Azure zkontrolujte navrhované pravidlo brány firewall na úrovni serveru (můžete ho upravit, abyste povolili rozsah IP adres) a potom kliknutím na tlačítko **OK** vytvořte pravidlo brány firewall a dokončete připojení ke službě SQL Database.
 
      ![nová brána firewall na úrovni serveru](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-4.png)
 
5. Pokud vám vaše přihlašovací údaje umožní přístup, otevře se Průzkumník objektů a vy teď můžete provádět úlohy správy nebo se dotazovat na data. 
 
     ![nová brána firewall na úrovni serveru](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-5.png)
 
     
## Řešení potíží s připojením

Nejčastější příčinou chyb připojení jsou chyby v názvu serveru (nezapomeňte, že <*název_serveru*> je název logického serveru a ne název databáze), chybné uživatelské jméno nebo heslo a také situace, kdy server z bezpečnostních důvodů připojení neumožňuje. 





<!--HONumber=Aug16_HO4-->


