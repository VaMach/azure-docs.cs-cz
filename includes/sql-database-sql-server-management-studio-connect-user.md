## Připojení ke službě Azure SQL Database jako uživatel

Pomocí následujícího postupu se můžete v aplikaci SSMS připojit ke službě Azure SQL Database jako uživatel.

1. Do pole vyhledávání ve Windows zadejte text „Microsoft SQL Server Management Studio“ a potom kliknutím na desktopovou aplikaci spusťte aplikaci SSMS.

2. V okně Připojení k serveru zadejte následující informace:

- **Typ serveru**: Výchozí hodnotou je databázový stroj. Tuto hodnotu nemůžete změnit.
 - **Název serveru**: Zadejte název serveru, který je hostitelem služby SQL Database. Použijte následující formát: *&lt;název_serveru >*.**Database.Windows.NET**.
 - **Typ ověřování**: Pokud teprve začínáte, vyberte Ověřování SQL. Pokud jste pro logický server služby SQL Database povolili službu Active Directory, můžete si vybrat Ověřování hesla Active Directory nebo Integrované ověřování Active Directory.
 - **Uživatelské jméno**: Pokud jste vybrali Ověřování SQL nebo Ověřování hesla Active Directory, zadejte jméno uživatele, který má přístup k databázi na serveru.
 - **Heslo**: Pokud jste vybrali Ověřování SQL nebo Ověřování hesla Active Directory, zadejte heslo uvedeného uživatele.
   
       ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-sql-server-management-studio-connect-user/connect-user-1.png)

3. Klikněte na **Možnosti** a určete databázi, ke které se chcete připojit.

      ![SQL Server Management Studio: Připojení k serveru služby SQL Database.](./media/sql-database-sql-server-management-studio-connect-user/connect-user-2.png)
 
4. V **Připojení k databázi** vyberte databázi, ke které se chcete připojit.

     ![SQL Server Management Studio: Připojení k serveru služby SQL Database](./media/sql-database-sql-server-management-studio-connect-user/connect-user-3.png)

5. Klikněte na **Připojit**.
 
6. Pokud IP adresa vašeho klienta nemá přístup k logickému serveru služby SQL Database, zobrazí se výzva k přihlášení k účtu Azure a k vytvoření pravidla brány firewall na úrovni serveru. Pokud jste správci předplatného Azure, klikněte na **Přihlásit** a vytvořte pravidlo brány firewall na úrovni serveru. Pokud ne, požádejte správce o vytvoření pravidla brány firewall na úrovni serveru nebo pravidla brány firewall na úrovni databáze v databázi, ke které se snažíte připojit.
 
      ![SQL Server Management Studio: Připojení k serveru služby SQL Database.](./media/sql-database-sql-server-management-studio-connect-user/connect-user-4.png)
 
7. Pokud vám vaše přihlašovací údaje umožní přístup k zadané databázi, otevře se Průzkumník objektů a vy teď můžete provádět úlohy správy nebo se dotazovat na data, podle toho, jaká máte uživatelská oprávnění.
  
      ![SQL Server Management Studio: Připojení k serveru služby SQL Database.](./media/sql-database-sql-server-management-studio-connect-user/connect-user-5.png)
      
 
## Řešení potíží s připojením

Nejčastější příčinou chyb připojení jsou chyby v názvu serveru (nezapomeňte, že <*název_serveru*> je název logického serveru a ne název databáze), chybné uživatelské jméno nebo heslo a také situace, kdy server z bezpečnostních důvodů připojení neumožňuje. 





<!---HONumber=Aug16_HO4-->


