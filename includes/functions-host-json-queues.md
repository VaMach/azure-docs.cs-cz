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
|batchSize|16|Počet zpráv fronty k načtení a zpracovávají paralelně. Maximum je 32.| 
|maxDequeueCount|5|Počet pokusů, před přesunutím do fronty poškozených zpracování zprávy.| 
|newBatchThreshold|batchSize/2|Prahová hodnota, na kterém jsou načtených novou dávku zpráv.| 
