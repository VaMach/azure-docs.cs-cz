

## Vytvoření nového uživatele databáze pomocí aplikace SSMS

Pomocí následujících kroků vytvořte ve stávající databázi nového uživatele databáze pomocí aplikace SSMS. 

Tento postup předpokládá, že jste v Průzkumníku objektů v aplikaci SSMS připojení ke službě SQL Database a jste připojení ke svému logickému serveru SQL Database jako hlavní správce na úrovni serveru nebo pomocí uživatelského účtu s oprávněním k vytváření nových uživatelů. 

1. V Průzkumníku objektů rozbalte uzel Databáze a vyberte databázi, ve které chcete vytvořit nový uživatelský účet.

     ![SQL Server Management Studio: Připojení k serveru služby SQL Database.](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Klikněte pravým tlačítkem myši na vybranou databázi a potom klikněte na **Dotaz**.

     ![SQL Server Management Studio: Připojení k serveru služby SQL Database.](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. V okně dotazu úpravou a použitím následujícího příkazu Transact-SQL vytvořte uživatele ve své databázi uživatelů. 

    ```CREATE USER user1 WITH PASSWORD ='p@ssw0rd1';
    ```

     ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-3.png)






<!--HONumber=Aug16_HO4-->


