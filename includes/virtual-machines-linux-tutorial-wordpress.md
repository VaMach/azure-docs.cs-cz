## <a name="install-wordpress"></a>Instalace aplikace WordPress

Pokud budete chtít zkusit do zásobníku, nainstalujte ukázkovou aplikaci. Jako příklad následující kroky instalace open source [WordPress](https://wordpress.org/) platformu pro vytváření webů a blogů. Zahrnout další úlohy a zkuste to [Drupal](http://www.drupal.org) a [Moodle](https://moodle.org/). 

Tato instalace WordPress je pro testování konceptu. Další informace a nastavení pro provozní instalace najdete v tématu [WordPress dokumentaci](https://codex.wordpress.org/Main_Page). 



### <a name="install-the-wordpress-package"></a>Instalovat balíček WordPress

Spusťte následující příkaz:

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a>Konfigurace WordPress

Konfigurace WordPress používání MySQL a PHP. Spusťte následující příkaz a otevřete textový editor podle svého výběru vytvoření souboru `/etc/wordpress/config-localhost.php`:

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
Zkopírujte následující řádky do souboru, nahraďte heslo k databázi pro *yourPassword* (nechte ostatní hodnoty beze změny). Pak soubor uložte.

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

V pracovním adresáři, vytvořte textový soubor `wordpress.sql` konfigurace WordPress databáze: 

```bash
sudo sensible-editor wordpress.sql
```

Přidejte následující příkazy, nahraďte heslo k databázi pro *yourPassword* (nechte ostatní hodnoty beze změny). Pak soubor uložte.

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


Spusťte následující příkaz k vytvoření databáze:

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

Po dokončení příkazu, odstraňte soubor `wordpress.sql`.

Přesunete instalaci WordPress na kořen dokumentu webového serveru:

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

Nyní můžete dokončit nastavení WordPress a publikovat na platformě. Otevřete prohlížeč a přejděte na `http://yourPublicIPAddress/wordpress`. Nahraďte veřejnou IP adresu vašeho virtuálního počítače. By měla vypadat podobně jako tento obrázek.

![Stránka Instalace WordPress](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)