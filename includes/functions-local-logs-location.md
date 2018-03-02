Když hostitel funkce běží místně, zapisuje protokoly do následujícího umístění:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

V systému Windows `<DefaultTempDirectory>` je první nalezené hodnoty USERPROFILE TMP TEMP, proměnné prostředí nebo adresáři Windows.
V systému MacOS nebo Linux `<DefaultTempDirectory>` je proměnná prostředí TMPDIR.

[!Note]
Při spuštění funkce hostitele, přepíše stávající strukturu souborů v adresáři.