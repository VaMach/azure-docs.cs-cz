Pravidelně se budou vydávat aktualizace agenta Azure File Sync, které budou přidávat nové funkce a řešit případné nalezené problémy. Doporučujeme na webu Microsoft Update povolit získávání všech vydaných aktualizací agenta Azure File Sync. Přesto rozumíme požadavkům některých organizací na striktní kontrolu a testování aktualizací. Pro nasazení používající starší verze agenta Azure File Sync:

- Služba synchronizace úložiště bude podporovat předchozí hlavní verzi po dobu tří měsíců od prvotního vydání nové hlavní verze. Například verze 1.\* bude podporovaná službou synchronizace úložiště po dobu tří měsíců od vydání verze 2.\*.
- Po uplynutí tří měsíců začne služba synchronizace úložiště pomocí skupin synchronizace blokovat synchronizaci registrovaných serverů používajících verzi, jejíž platnost vypršela.
- Během těchto tří měsíců se všechny opravy chyb v předchozí hlavní verzi dostanou pouze do aktuální hlavní verze.

> [!Note]  
> Pokud budete používat verzi služby Azure File Sync, jejíž platnost během následujících tří měsíců vyprší, upozorníme vás na to informační zprávou na webu Azure Portal.