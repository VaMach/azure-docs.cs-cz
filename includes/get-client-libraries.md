### <a name="install-via-composer"></a>Nainstalovat prostřednictvím autora
1. Vytvořte soubor s názvem **composer.json** v kořenu projektu a přidejte do ní následující kód:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage": "*"
      }
    }
    ```
2. Stáhněte si  **[composer.phar] [ composer-phar]**  v kořenového adresáře projektu.
3. Otevřete příkazový řádek a spusťte následující příkaz v kořenového adresáře projektu
   
    ```
    php composer.phar install
    ```

Případně přejděte do [klientské knihovny pro Azure Storage PHP] [ php-sdk-github] na Githubu klonovat zdrojového kódu.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
