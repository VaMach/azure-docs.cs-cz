
1. Pro zvýšení oprávnění, zadejte:
   
        sudo -s
   
    Zadejte svoje heslo.
2. Chcete-li nainstalovat MySQL Community Server edition, zadejte:
   
        zypper install mysql-community-server
   
    Počkejte, než MySQL se stáhne a nainstaluje.
3. Pokud chcete nastavit MySQL spustit při spuštění systému, zadejte:
   
        insserv mysql
4. Démon MySQL (mysqld) spusťte ručně pomocí tohoto příkazu:
   
        rcmysql start
   
    Chcete-li zkontrolovat stav démon MySQL, zadejte:
   
        rcmysql status
   
    Chcete-li zastavit démon MySQL, zadejte:
   
        rcmysql stop
   
   > [!IMPORTANT]
   > Po instalaci MySQL kořenové heslo je prázdné ve výchozím nastavení. Doporučujeme spustit **mysql\_zabezpečené\_instalace**, skript, který pomáhá zabezpečené MySQL. Skript vyzve k změnit kořenové heslo MySQL, odeberte anonymní uživatelské účty, zakažte kořenové vzdáleného přihlášení, odebrat testovací databáze a znovu načíst tabulky oprávnění. Doporučujeme Ano odpovědět, aby se všechny tyto možnosti a kořenové heslo změnit.
   > 
   > 
5. Zadejte to pro spuštění skriptu MySQL instalační skript:
   
        mysql_secure_installation
6. Přihlaste se k MySQL:
   
        mysql -u root -p
   
    Zadejte hesla kořenového MySQL (který jste změnili v předchozím kroku) a uvedené jsou s výzvou, kde můžete použít příkazy SQL pro interakci s databází.
7. Pokud chcete vytvořit nového uživatele databáze MySQL, spusťte následující příkaz **mysql >** řádku:
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Poznámka: středníkem (;) na konci řádku je zásadní pro ukončení příkazu.
8. K vytvoření databáze a udělit `mysqluser` oprávnění uživatele, vydávání následující příkazy:
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Všimněte si, že databáze uživatelská jména a hesla používají pouze skripty s připojením k databázi.  Názvy databáze uživatelských účtů nutně nepředstavuje skutečný uživatelské účty v systému.
9. Chcete-li se přihlásit z jiného počítače, zadejte:
   
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    kde `ip-address` je IP adresa počítače, ze kterého se můžete připojit k MySQL.
10. Ukončete nástroj pro správu databáze MySQL, zadejte:
    
        quit

## <a name="add-an-endpoint"></a>Přidání koncového bodu
1. Po instalaci MySQL, budete potřebovat ke konfiguraci koncového bodu vzdálený přístup MySQL. Přihlaste se k [portál Azure][AzurePortal]. Klikněte na tlačítko **virtuální počítače**, klikněte na název nového virtuálního počítače a pak klikněte na tlačítko **koncové body**.
2. Klikněte na tlačítko **přidat** v dolní části stránky.
3. Přidat koncový bod s názvem "MySQL" s protokolem **TCP**, a **veřejné** a **privátní** porty nastavena na "3306".
4. Chcete-li vzdáleně připojit k virtuálnímu počítači z vašeho počítače, zadejte:
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    Například používání virtuálních počítačů, které jste vytvořili v tomto kurzu, zadejte tento příkaz:
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://portal.azure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
