```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|maxPollingInterval|60000|Maximální dobu v milisekundách mezi dvěma fronty.| 
|visibilityTimeout|0|Časový interval mezi opakovanými pokusy při zpracování zprávy, které se nezdaří.| 
|batchSize|16|Počet zpráv fronty, které Functions runtime současně načte a zpracuje paralelně. Když počet zpracovávaných získá dolů na `newBatchThreshold`, modul runtime získá další dávku a spustí zpracování těchto zpráv. Proto je maximální počet souběžných zpráv, které jsou zpracovány za funkce `batchSize` plus `newBatchThreshold`. Toto omezení platí pro jednotlivé funkce aktivované fronty samostatně. <br><br>Pokud chcete, aby se zabránilo paralelní zpracování zprávy přijaté v jedné frontě, můžete nastavit `batchSize` na 1. Toto nastavení však eliminuje souběžnosti pouze tak dlouho, dokud běží vaše aplikace funkce jednoduchého virtuálního počítače (VM). Pokud aplikaci funkce horizontálně navýší kapacitu na víc virtuálních počítačů, může každý virtuální počítač spustit jednu instanci každé funkce aktivované fronty.<br><br>Maximální počet `batchSize` je 32. | 
|maxDequeueCount|5|Počet pokusů, před přesunutím do fronty poškozených zpracování zprávy.| 
|newBatchThreshold|batchSize/2|Vždy, když počet zpráv zpracovává souběžně získá na toto číslo, načte modul runtime jiná dávka.| 



