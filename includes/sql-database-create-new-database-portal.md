
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## Vytvoření nové databáze SQL Azure

Pomocí následujícího postupu můžete na portálu Azure vytvořit novou databázi SQL Azure na novém nebo stávajícími logickém serveru služby SQL Database.

1. Pokud nejste připojeni, připojte se k webu [Azure Portal](http://portal.azure.com).
2. Klikněte na **Nový**, zadejte **SQL Database** a poté klikněte na **SQL Database (nová databáze)**.

     ![Nová databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Klikněte na **SQL Database (nová databáze)**.

     ![Nová databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Kliknutím na **Vytvořit** vytvořte ve službě SQL Database novou databázi.

     ![Nová databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. Zadejte hodnoty následujících vlastností serveru:

 - Název databáze
 - Předplatné: Platí pouze pokud máte více předplatných.
 - Skupina prostředků: Pokud teprve začínáte, použijte skupinu prostředků logického serveru.
 - Vyberte zdroj: Můžete zvolit prázdnou databázi, ukázková data nebo zálohu databáze Azure. Chcete-li migrovat místní databázi systému SQL Server nebo načíst data pomocí nástroje příkazového řádku BCP, na konci tohoto článku najdete odkazy.
 - Server: Nový nebo stávající logický server.
 - Přihlašovací jméno správce serveru
 - Heslo
 - Cenová úroveň: Pokud teprve začínáte, použijte výchozí hodnotu S0.
 - Kolace: Platí pouze pokud byla zvolena prázdná databáze.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Klikněte na možnost **Vytvořit**. V oznamovací oblasti uvidíte, že nasazení bylo zahájeno.

     ![Nová databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Než budete pokračovat na další krok, počkejte, až se nasazení dokončí.

     ![Nová databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)



<!--HONumber=sep16_HO1-->


