```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|maxBatchSize|64|Maximální velikost události počet přijatých za receive smyčky.|
|prefetchCount|neuvedeno|Výchozí hodnota PrefetchCount, který se použije základní knihovny EventProcessorHost.| 
|batchCheckpointFrequency|1|Počet událostí zpracovávaných dávek před vytvořením kontrolního bodu Centrum EventHub kurzoru.| 
