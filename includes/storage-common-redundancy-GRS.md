Geograficky redundantní úložiště (GRS) replikuje data do sekundární oblasti, která je stovky miles od primární oblasti. Pokud má váš účet úložiště GRS povoleno, vaše data byla odolná i v případě výpadku dokončení místní nebo havárii, ve kterém není použitelná pro obnovení primární oblasti.

Účet úložiště GRS povolena je nejprve aktualizace potvrzena pro primární oblasti, kde se replikují třikrát. Aktualizace se pak replikují asynchronně sekundární oblast, kde se také replikují třikrát.

S GRS primární a sekundární oblasti spravovat repliky v rámci domén samostatné selhání a upgradu domén v rámci jednotce škálování úložiště, jak je popsáno s LRS.

Aspekty:

* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, v případě havárie regionální je možné, že od primární oblasti nelze obnovit data budou ztracena změny, které ještě nebyla replikována do sekundární oblasti.
* Replika není k dispozici, pokud Microsoft zahájí převzetí služeb při selhání pro sekundární oblast. Pokud Microsoft zahájit převzetí služeb při selhání pro sekundární oblast, budete mít ke čtení a zápis do dat po převzetí služeb po dokončení. Další informace najdete v tématu [pokyny pro zotavení po havárii](../articles/storage/common/storage-disaster-recovery-guidance.md). 
* Pokud aplikace chce číst ze sekundární oblasti, uživatel by měl povolit RA-GRS.

Pokud vytvoříte účet úložiště, vyberte primární oblasti pro účet. Sekundární oblast je určen na základě primární oblasti a nelze změnit. V následující tabulce jsou uvedeny dvojice primární a sekundární oblast.

| Primární | Sekundární |
| --- | --- |
| Střed USA – sever | Střed USA – jih |
| Střed USA – jih | Střed USA – sever |
| Východ USA | Západní USA |
| Západní USA | Východ USA |
| USA – východ 2 | Střed USA |
| Střed USA | USA – východ 2 |
| Severní Evropa | Západní Evropa |
| Západní Evropa | Severní Evropa |
| Jihovýchodní Asie | Východní Asie |
| Východní Asie | Jihovýchodní Asie |
| Východní Čína | Severní Čína |
| Severní Čína | Východní Čína |
| Japonsko – východ | Japonsko – západ |
| Japonsko – západ | Japonsko – východ |
| Brazílie – jih | Střed USA – jih |
| Austrálie – východ | Austrálie – jihovýchod |
| Austrálie – jihovýchod | Austrálie – východ |
| Indie – jih | Indie – střed |
| Indie – střed | Indie – jih |
| Indie – západ | Indie – jih |
| USA (Gov) – Iowa | USA (Gov) – Virginia |
| USA (Gov) – Virginia | USA (Gov) – Texas |
| USA (Gov) – Texas | USA (Gov) – Arizona |
| USA (Gov) – Arizona | USA (Gov) – Texas |
| Střední Kanada | Východní Kanada |
| Východní Kanada | Střední Kanada |
| Spojené království – západ | Spojené království – jih |
| Spojené království – jih | Spojené království – západ |
| Německo – střed | Německo – severovýchod |
| Německo – severovýchod | Německo – střed |
| Západní USA 2 | Západní střed USA |
| Západní střed USA | Západní USA 2 |

Aktuální informace o oblastech podporovaných v Azure najdete v tématu [oblastí Azure](https://azure.microsoft.com/regions/).

>[!NOTE]  
> USA – verze pro státní správu Virginia sekundární oblast je Texas nám verze pro státní správu. Dříve nám verze pro státní správu Virginia využité nám verze pro státní správu Iowa jako sekundární oblasti. Účty úložiště stále využití nám verze pro státní správu Iowa jako sekundární oblasti se migruje nám verze pro státní správu Texas jako sekundární oblast. 
> 
> 