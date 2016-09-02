Fakturuje se využívání Azure Storage podle vašeho účtu úložiště. Náklady na úložiště se odvíjí od těchto faktorů: oblast/umístění, typ účtu, kapacita úložiště, schéma replikace, transakce úložiště a odchozí data.

- Oblast znamená zeměpisnou oblast, ve které se účet nachází.
- Typ účtu odkazuje na to, jestli používáte obecný účet úložiště, nebo účet služby Blob Storage. S účtem Blob Storage úroveň přístupu také určuje fakturační model účtu.
- Kapacita úložiště znamená, kolik plnění svého účtu úložiště využíváte k uložení dat.
- Replikace udává, kolik kopií vašich dat se spravuje zároveň a v jakých umístěních.
- Transakce znamenají všechny operace čtení a zápisu v Azure Storage.
- Odchozí data zahrnují data přenesená ven z oblasti Azure. Když k datům v účtu úložiště přistupuje aplikace, která neběží ve stejné oblasti, budou se vám účtovat odchozí data. (Ve službách Azure můžete svoje data seskupit do datových center a tak snížit nebo úplně zabránit odchozím datům a s nimi spojeným nákladům.)

Na stránce [Ceny za Azure Storage](https://azure.microsoft.com/pricing/details/storage/) najdete podrobné informace o cenách na základě typu účtu, kapacity úložiště, replikace a transakcí. Na stránce [Podrobné informace o cenách přenosů dat](https://azure.microsoft.com/pricing/details/data-transfers/) najdete podrobné informace o sazbách za odchozí data. Představu o nákladech můžete získat v [Cenové kalkulačce pro Azure Storage](https://azure.microsoft.com/pricing/calculator/?scenario=data-management).



<!--HONumber=Aug16_HO4-->


