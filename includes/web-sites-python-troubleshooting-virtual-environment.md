Pokud skript nasazení zjistí, že kompatibilní virtuální prostředí už existuje, vytvoření virtuálního prostředí v Azure přeskočí.  To může nasazení značně urychlit.  Systém pip přeskočí balíčky, které jsou už nainstalované.

V určitých situacích může být vhodné vynutit odstranění daného virtuálního prostředí.  Bude to nutné udělat, pokud se rozhodnete zahrnout virtuální prostředí jako součást úložiště.  Může to také být vhodné, pokud se rozhodnete zbavit určitých balíčků nebo testovat změny souboru requirements.txt.

Existující virtuální prostředí je možné v Azure spravovat pomocí několika možností:

### <a name="option-1-use-ftp"></a>Možnost 1: Pomocí protokolu FTP
Pomocí klienta FTP se připojte k serveru a budete moct odstranit složku env.  Upozornění: Někteří klienti FTP (například webové prohlížeče) mohou umožňovat přístup jen ke čtení a neumožňují odstraňovat složky. Proto je nutné použít klienta, který má tuto funkci.  Název hostitele FTP a uživatele se zobrazuje v okně vaší webové aplikace na webu [Azure Portal](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Možnost 2: Přepnutí modulu runtime
Jedná se o alternativní postup využívající fakt, že skript nasazení odstraní složku env, pokud neodpovídá požadované verzi Pythonu.  Výsledkem je odstranění existujícího prostředí a vytvoření nového.

1. Přepněte na jinou verzi Pythonu (pomocí souboru runtime.txt nebo okna **Nastavení aplikace** na webu Azure Portal).
2. Pomocí příkazu git push proveďte nějaké změny (pokud dojde k chybám instalace systému pip, ignorujte je).
3. Přepněte zpět na původní verzi Pythonu.
4. Opět proveďte nějaké změny pomocí příkazu git push.

### <a name="option-3-customize-deployment-script"></a>Možnost 3: Přizpůsobení skriptu nasazení
Pokud jste přizpůsobili skript nasazení, můžete změnit kód v souboru deploy.cmd a vynutit, aby odstranil složku env.

