Azure určí verzi jazyka Python, která se má použít pro virtuální prostředí, pomocí tohoto pořadí priorit:

1. Verze zadaná v souboru runtime.txt v kořenové složce
2. Verze zadaná v nastavení jazyka Python v konfiguraci webové aplikace (okno **Nastavení** > **Nastavení aplikace** pro danou webovou aplikaci na webu Azure Portal)
3. Pokud není uvedená žádná z výše uvedených verzí, použije se výchozí verze python-2.7

Platné hodnoty obsahu souboru 

    \runtime.txt

jsou následující:

* python-2.7
* python-3.4

Pokud je uvedená mikroverze (třetí číslice), bude se ignorovat.

<!--HONumber=Sep16_HO3-->


