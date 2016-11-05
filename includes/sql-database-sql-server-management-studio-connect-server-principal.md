

## Připojení ke službě Azure SQL Database pomocí Ověřování serveru SQL
Následující kroky ukazují, jak se připojit do serveru a databázi Azure SQL pomocí aplikace SSMS. Pokud server a databázi ještě nemáte, podle postupu v tématu [Vytvoření databáze SQL během pár minut](../articles/sql-database/sql-database-get-started.md) ji vytvořte.

1. Do pole vyhledávání ve Windows zadejte **Microsoft SQL Server Management Studio** a poté kliknutím na desktopovou aplikaci spusťte aplikaci SSMS.
2. V okně **Připojit k serveru** zadejte následující informace (pokud je aplikace SSMS již spuštěná, okno **Připojit k serveru** otevřete kliknutím na **Připojit > Databázový stroj**):
   
   * **Typ serveru**: Výchozí hodnotou je databázový stroj. Tuto hodnotu nemůžete změnit.
   * **Název serveru**: Zadejte plně kvalifikovaný název svého serveru služby Azure SQL Database v následujícím formátu: *&lt;název_serveru>*.**database.windows.net**
   * **Typ ověřování**: Tento článek popisuje připojení pomocí **Ověřování serveru SQL**. Podrobnosti o připojení k Azure Active Directory najdete v tématech [Připojení pomocí integrovaného ověřování Active Directory](../articles/sql-database/sql-database-aad-authentication.md#connect-using-active-directory-integrated-authentication), [Připojení pomocí ověřování hesla Active Directory](../articles/sql-database/sql-database-aad-authentication.md#connect-using-active-directory-password-authentication) a [Připojení pomocí univerzálního ověřování Active Directory](../articles/sql-database/sql-database-ssms-mfa-authentication.md).
   * **Uživatelské jméno**: Zadejte jméno uživatele s přístupem k databázi na serveru (například *správce serveru*, kterého jste nastavili při vytváření serveru). 
   * **Heslo**: Zadejte heslo daného uživatele (například *heslo*, které jste nastavili při vytváření serveru).
     
       ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-sql-server-management-studio-connect-server-principal/connect.png)
3. Klikněte na **Připojit**.
4. Ve výchozím nastavení nemají nové servery definována žádná [pravidla brány firewall](../articles/sql-database/sql-database-firewall-configure.md), takže je připojování klientů zpočátku blokováno. Pokud váš server zatím neobsahuje pravidlo brány firewall, které umožňuje připojení vaší konkrétní IP adresy, aplikace SSMS vám nabídne, že pravidlo brány firewall na úrovni serveru vytvoří za vás.
   
    Kliknutím na **Přihlásit se** vytvoříte pravidlo brány firewall na úrovni serveru. Abyste mohli vytvořit pravidlo brány firewall na úrovni serveru, musíte být správce Azure.
   
       ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-sql-server-management-studio-connect-server-principal/newfirewallrule.png)
5. Po úspěšném připojení k Azure SQL Database se otevře **Průzkumník objektů**. Nyní již máte přístup k databázi a můžete [provádět úlohy správy nebo dotazovat data](../articles/sql-database/sql-database-manage-azure-ssms.md).
   
     ![nová brána firewall na úrovni serveru](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-5.png)

## Řešení potíží s připojením
Nejčastější příčiny selhání připojení jsou chyby v názvu serveru a problémy s připojením k síti. Nezapomeňte, že <*název_serveru*> je název serveru, ne databáze, a že je potřeba zadat plně kvalifikovaný název: `<servername>.database.windows.net`

Ověřte také, že uživatelské jméno ani heslo neobsahují překlepy nebo mezery (uživatelská jména nerozlišují malá a velká písmena, ale hesla ano). 

Kromě názvu serveru můžete také explicitně nastavit protokol a číslo portu, jako například: `tcp:servername.database.windows.net,1433`

Chyby a časové limity připojení mohou být způsobeny také problémy s připojením k síti. Řešením může být jednoduché zopakování pokusu o připojení (jestliže víte, že název serveru, přihlašovací údaje a pravidla brány firewall jsou v pořádku).

Podrobnosti a další informace o problémech s připojením najdete v článku [Řešení potíží, diagnostika a předcházení chybám připojení SQL a přechodným chybám pro službu SQL Database](../articles/sql-database/sql-database-connectivity-issues.md).

<!--HONumber=Oct16_HO1-->


