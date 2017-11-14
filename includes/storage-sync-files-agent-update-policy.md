Aktualizace agenta synchronizace souboru Azure budou vydány v pravidelných intervalech přidat další nové funkce a chcete-li vyřešit potíže, které byly zjištěny. Doporučujeme nakonfigurovat Microsoft Update a načíst všechny aktualizace agenta synchronizace souboru Azure jako vydáváme nich. Chápeme, že některé organizace jako výhradně řídit a otestovat aktualizace. 

Pro nasazení, které používají starší verze agenta synchronizace souboru Azure:

- Synchronizační služba úložiště ctí předchozí hlavní verzi dobu tří měsíců po počáteční vydání nové hlavní verze. Synchronizační služba úložiště by například podporovat verze 1. \* až tři měsíců po vydání verze 2. \*.
- Až po uplynutí tří měsíců, služby Sync úložiště začne blokovat registrované servery, které používají vypršela platnost verze ze synchronizace s jejich synchronizace skupiny.
- V rámci tři měsíce pro předchozí hlavní verzi přejděte všechny opravy chyb jenom na aktuální hlavní verze.

> [!Note]  
> Pokud budete používat verzi služby Azure File Sync, jejíž platnost během následujících tří měsíců vyprší, upozorníme vás na to informační zprávou na webu Azure Portal.