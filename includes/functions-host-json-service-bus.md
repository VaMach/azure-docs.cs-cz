```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|maxConcurrentCalls|16|Maximální počet souběžných volání zpětné volání, které by měla iniciovat message pump. Ve výchozím nastavení procesů Functions runtime více zpráv současně. Přímé modulu runtime zpracovat jenom jeden fronta nebo téma zprávy najednou, nastavte `maxConcurrentCalls` na 1. | 
|prefetchCount|neuvedeno|Výchozí hodnota PrefetchCount, který se použije základní MessageReceiver.| 
|autoRenewTimeout|00:05:00|Maximální doba, ve kterém bude automaticky obnoveno zámek zprávy.| 
