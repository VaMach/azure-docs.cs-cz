
Kód pro všechny funkce v konkrétní funkce aplikace se nachází v kořenové složce, která obsahuje konfigurační soubor hostitele a jeden nebo více jejích podsložkách. Každé podsložky obsahuje kód pro samostatné funkce, jako v následujícím příkladu:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Soubor host.json obsahuje některé konfigurace specifické pro modul runtime a nachází v kořenové složce funkce aplikace. Informace o nastavení, které jsou k dispozici, najdete v článku [host.json odkaz](../articles/azure-functions/functions-host-json.md).

Jednotlivé funkce má složku, která obsahuje jeden nebo více souborů, function.json konfiguraci a další závislosti.

