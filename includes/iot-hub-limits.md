Následující tabulka uvádí omezení spojená s různými úrovněmi služeb (S1, S2, S3, F1). Informace o nákladech na jednotlivé *jednotky* v každé úrovni najdete v tématu [Ceny služby IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

| Prostředek | S1 Standard | S2 Standard | S3 Standard | F1 Free |
| --- | --- | --- | --- | --- |
| Počet zpráv za den |400 000 |6 000 000 |300 000 000 |8 000 |
| Maximální počet jednotek |200 |200 |10 |1 |

> [!NOTE]
> Pokud očekáváte, že využijete více než 200 jednotek v centru úrovně S1, S2 nebo více než 10 jednotek v centru úrovně S3, kontaktujte podporu Microsoftu.
> 
> 

Následující tabulka uvádí omezení, která se vztahují na prostředky služby IoT Hub:

| Prostředek | Omezení |
| --- | --- |
| Maximální počet placených služeb IoT Hub na předplatné Azure |10 |
| Maximální počet bezplatných služeb IoT Hub na předplatné Azure |1 |
| Maximální počet identit zařízení<br/> vrácených v jednom volání |1000 |
| Maximální doba uchování zpráv typu zařízení-cloud ve službě IoT Hub |7 dní |
| Maximální velikost zprávy typu zařízení-cloud |256 kB |
| Maximální velikost dávky typu zařízení-cloud |256 kB |
| Maximální počet zpráv v dávce typu zařízení-cloud |500 |
| Maximální velikost zprávy typu cloud-zařízení |64 kB |
| Maximální hodnota TTL pro zprávy typu cloud-zařízení |2 dny |
| Maximální počet doručení zpráv typu <br/> cloud-zařízení |100 |
| Maximální počet doručení zpráv se zpětnou vazbou <br/> v reakci na zprávu typu cloud-zařízení |100 |
| Maximální hodnota TTL pro zprávy se zpětnou vazbou <br/> v reakci na zprávu typu cloud-zařízení |2 dny |
| Maximální velikost dvojčete zařízení <br/> (značky, ohlášené vlastnosti a požadované vlastnosti) | 8 kB |
| Maximální velikost řetězcové hodnoty dvojčete zařízení | 512 bajtů |
| Maximální hloubka objektu ve dvojčeti zařízení | 5 |
| Maximální velikost datové části přímé metody | 8 kB |
| Maximální doba uchování historie úlohy | 30 dní |
| Maximální počet souběžných úloh | 10 (pro S3), 5 (pro S2), 1 (pro S1) |
| Maximální počet dalších koncových bodů | 10 (pro S1, S2, S3) |
| Maximální počet pravidel směrování zpráv | 100 (pro S1, S2, S3) |


> [!NOTE]
> Pokud v předplatném Azure potřebujete více než 10 placených služeb IoT Hub, kontaktujte podporu Microsoftu.
> 
> 

Služba IoT Hub omezuje žádosti při překročení následujících kvót:

| Omezení | Hodnota na centrum |
| --- | --- |
| Operace registru identit <br/> (vytvoření, načtení, výpis, aktualizace, odstranění), <br/> jednotlivý nebo hromadný import/export |5 000/min/jednotku (pro S3) <br/> 100/min/jednotku (pro S1 a S2) |
| Připojení zařízení |6 000/s/jednotku (pro S3), 120/s/jednotku (pro S2), 12/s/jednotku (pro S1) <br/>Minimálně 100/s |
| Odesílání typu zařízení-cloud |6 000/s/jednotku (pro S3), 120/s/jednotku (pro S2), 12/s/jednotku (pro S1) <br/>Minimálně 100/s |
| Odesílání typu cloud-zařízení |5 000/min/jednotku (pro S3), 100/min/jednotku (pro S1 a S2) |
| Příjem typu cloud-zařízení |50 000/min/jednotku (pro S3), 1 000/min/jednotku (pro S1 a S2) |
| Operace nahrávání souborů |5 000 oznámení o nahrávání souborů/min/jednotku (pro S3), 100 oznámení o nahrávání souborů/min/jednotku (pro S1 a S2) <br/> Najednou může existovat&10; 000 identifikátorů URI SAS pro účet Azure Storage.<br/> Najednou může existovat&10; identifikátorů URI SAS/zařízení. |
| Přímé metody | 1500/s/jednotku (pro S3), 30/s/jednotku (pro S2), 10/s/jednotku (pro S1) |
| Čtení dvojčat zařízení | 50/s/jednotku (pro S3), maximálně 10/s nebo 1/s/jednotku (pro S2), 10/s (pro S1) |
| Aktualizace dvojčat zařízení | 50/s/jednotku (pro S3), maximálně 10/s nebo 1/s/jednotku (pro S2), 10/s (pro S1) |
| Operace úloh <br/> (vytvoření, aktualizace, výpis, odstranění) | 5 000/min/jednotku (pro S3), 100/min/jednotku (pro S2), 100/min/jednotku (pro S1) |
| Propustnost operací úloh jednotlivých zařízení | 50/s/jednotku (pro S3), maximálně 10/s nebo 1/s/jednotku (pro S2), 10/s (pro S1) |

<!--HONumber=Feb17_HO3-->


