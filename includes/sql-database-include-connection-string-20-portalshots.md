
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Získání připojovacího řetězce z portálu Azure
Použití [portál Azure](https://portal.azure.com/) k získání připojovacího řetězce, které jsou nezbytné pro váš klientský program pro interakci s Azure SQL Database: 

1. Klikněte na tlačítko **Procházet** > **databází SQL**.
2. Zadejte název vaší databáze do textového pole filtru téměř horní nalevo od **databází SQL** okno.
3. Klikněte na řádek pro vaši databázi.
4. Po zobrazí v okně vaší databáze, pro jednoduchost visual, můžete kliknutím minimalizaci standardní ovládací prvky sbalit okna, které jste použili pro procházení a filtrování databáze. 
   
    ![Filtr izolovat vaše databáze][10-FilterDatabase]
5. V okně vaší databáze, klikněte na **zobrazit databázové připojovací řetězce**.
6. Pokud máte v úmyslu použít knihovnu připojení ADO.NET, zkopírujte řetězec s názvem bez přípony **ADO**. 
   
    ![Zkopírujte připojovací řetězec pro vaši databázi ADO][20-CopyAdoConnectionString]
7. Do jednoho formátu nebo jiné vložte informace o připojovacím řetězci do váš klientský kód programu.

Další informace naleznete v tématu:<br/>[Připojovací řetězce a konfigurační soubory](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
