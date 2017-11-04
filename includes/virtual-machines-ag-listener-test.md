V tomto kroku otestovat naslouchacího procesu skupiny dostupnosti pomocí klientské aplikace, která běží ve stejné síti.

Připojení klienta má následující požadavky:

* Připojení klientů k naslouchací proces musí pocházet z počítačů, které se nacházejí v jiné cloudové službě než ten, který je hostitelem repliky dostupnosti Always On.
* Pokud jsou repliky Always On v různých podsítích, musí klienti zadat *MultisubnetFailover = True* v připojovacím řetězci. Tato podmínka způsobí pokusy o připojení paralelní repliky v různých podsítích. Tento scénář zahrnuje nasazení mezi oblastmi vždy na dostupnosti skupiny.

Jedním z příkladů je pro připojení k naslouchací proces z jednoho z virtuálních počítačů ve stejné virtuální síti Azure (ale není ten, který je hostitelem repliky). Snadný způsob, jak dokončit tento test je pokuste se připojit k naslouchacího procesu skupiny dostupnosti SQL Server Management Studio. Jiné jednoduše je spuštění [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx), a to takto:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Pokud je hodnota EndpointPort *1433*, není nutné je zadat ve volání. Předchozí volání také předpokládá, že klientský počítač připojený ke stejné doméně a že volající byla udělena oprávnění k databázi pomocí ověřování systému Windows.
> 
> 

Při testování naslouchací proces, ujistěte se, že jste převzít služby skupiny dostupnosti a ujistěte se, že klienti můžou připojit k naslouchacímu procesu napříč převzetí služeb při selhání.

