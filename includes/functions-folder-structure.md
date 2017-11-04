
Kód pro všechny funkce v aplikaci pro danou funkci žije v kořenové složce, která obsahuje konfigurační soubor hostitele a jeden nebo více podsložek, z nichž každý obsahuje kód pro samostatné funkce, jako v následujícím příkladu:

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

*Host.json* soubor obsahuje některé konfigurace specifické pro modul runtime a nachází v kořenové složce funkce aplikace. Informace o nastaveních, které jsou k dispozici, najdete v části [host.json odkaz](../articles/azure-functions/functions-host-json.md).

Jednotlivé funkce má složku, která obsahuje jeden nebo více souborů, function.json konfiguraci a další závislosti.

