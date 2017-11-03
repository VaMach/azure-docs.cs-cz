### <a name="prerequisites"></a>Požadavky
* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Azure SQL Database](../articles/sql-database/sql-database-get-started.md) se svými informacemi o připojení, včetně názvu serveru, názvu databáze a uživatelského jména a hesla. Tato informace je obsažena v připojovacím řetězci SQL Database:
  
    Server = tcp:*yoursqlservername*. database.windows.net,1433;Initial katalogu =*yourqldbname*; Zachovat bezpečnostní údaje = False; ID uživatele = {your_username}; Heslo = {your_password}; MultipleActiveResultSets = False; Šifrování = True; TrustServerCertificate = False; Časový limit připojení = 30;
  
    Další informace o [databází SQL Azure](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> Při vytváření databáze SQL Azure, můžete také vytvořit ukázkové databáze součástí SQL. 
> 
> 

Před použitím vaší databázi SQL Azure v aplikaci logiky se připojte k vaší databázi SQL. Můžete k tomu snadno v rámci aplikace logiky na portálu Azure.  

Připojte k vaší databázi SQL Azure pomocí následujících kroků:  

1. Vytvoření aplikace logiky. V Návrháři Logic Apps přidejte aktivační událost a potom přidat akci. Vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu a potom zadejte do vyhledávacího pole "sql". Vyberte jednu z akcí:  
   
    ![Krok vytvoření připojení SQL Azure](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Pokud jste dosud nevytvořili žádné připojení k databázi SQL, budete vyzváni k podrobnosti připojení:  
   
    ![Krok vytvoření připojení SQL Azure](./media/connectors-create-api-sqlazure/connection-details.png) 
3. Zadejte podrobnosti databáze SQL. Vyžadují se vlastnosti s hvězdičkou.
   
   | Vlastnost | Podrobnosti |
   | --- | --- |
   | Připojit prostřednictvím brány |Nechte zaškrtnuté políčko. Používá se při připojování k serveru SQL na místě. |
   | Název připojení * |Zadejte libovolný název pro připojení. |
   | Název systému SQL Server * |Zadejte název serveru; což je něco podobného jako *servername.database.windows.net*. Název serveru se zobrazí ve vlastnostech SQL Database na portálu Azure a zobrazí také v připojovacím řetězci. |
   | Název databáze SQL * |Zadejte název dáte vaší databázi SQL. Položka je uvedena ve vlastnostech SQL Database v připojovacím řetězci: Initial Catalog =*yoursqldbname*. |
   | Uživatelské jméno * |Zadejte uživatelské jméno, které jste vytvořili při vytvoření databáze SQL. Položka je uvedena ve vlastnostech SQL Database na portálu Azure. |
   | Heslo * |Zadejte heslo, které jste vytvořili při vytvoření databáze SQL. |
   
    Tyto přihlašovací údaje se používají k autorizaci aplikace logiky připojení a přístup k datům SQL. Po dokončení podrobné informace o připojení vypadat nějak takto:  
   
    ![Krok vytvoření připojení SQL Azure](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. Vyberte **Vytvořit**. 
5. Všimněte si, že existuje připojení. Nyní pokračujte další kroky v aplikaci logiky: 
   
    ![Krok vytvoření připojení SQL Azure](./media/connectors-create-api-sqlazure/table.png)

