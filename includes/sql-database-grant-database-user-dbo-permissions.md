

## Udělení oprávnění db_owner novému uživateli databáze

Pomocí následujícího postupu můžete stávajícímu uživateli databáze udělit oprávnění db_owner.

Tento postup předpokládá, že jste v Průzkumníku objektů v aplikaci SSMS připojení ke službě SQL Database a jste připojení ke svému logickému serveru SQL Database jako hlavní správce na úrovni serveru nebo pomocí uživatelského účtu s oprávněním k udělování oprávnění uživatelům. 

1. V Průzkumníku objektů rozbalte uzel Databáze a vyberte databázi obsahující uživatele, kterému chcete udělit oprávnění dbo.

     ![SQL Server Management Studio: Připojení k serveru služby SQL Database.](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Klikněte pravým tlačítkem myši na vybranou databázi a potom klikněte na **Dotaz**.

     ![SQL Server Management Studio: Připojení k serveru služby SQL Database.](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. V okně dotazu úpravou a použitím následujícího příkazu Transact-SQL udělte zadanému uživateli oprávnění dbo. 

    ```ALTER ROLE db_owner ADD MEMBER user1;
    ```

     ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-grant-database-user-dbo-permissions/sql-database-grant-database-user-dbo-permissions-1.png)





<!---HONumber=Aug16_HO4-->


