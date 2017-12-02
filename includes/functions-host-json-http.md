```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|Parametr routePrefix|rozhraní api|Předpona trasy, která platí pro všechny trasy. K odebrání výchozí předponu použijte prázdný řetězec. |
|maxOutstandingRequests|-1|Maximální počet nevyřízených požadavků, které jsou uložené v daném okamžiku. Toto omezení se vztahuje požadavků, které jsou zařazeny do fronty, ale nebyla spuštěna, a také všechny v průběhu spuštění. Všechny příchozí požadavky přes tento limit byly zamítnuty 429 "Zaneprázdněn" odpovědi. Který umožňuje volajícím využívat strategie založené na čase opakování a také vám může pomoct řídit maximální požadavek latenci. To je řízeno jen služby Řízení front, ke kterému dochází v této cestě hostitele provádění skriptu. Další fronty například fronty požadavků ASP.NET bude stále v platnosti a toto nastavení nemá vliv. Výchozí hodnota je bez vazby.|
|maxConcurrentRequests|-1|Maximální počet funkce protokolu http, které budou spuštěny současně. To umožňuje souběžnosti ovládací prvek, který může pomoct spravovat využití prostředků. Například můžete mít http funkce, která používá velké množství systémových prostředků (procesoru/paměti nebo sockets) tak, aby ho způsobuje problémy při souběžnosti je příliš vysoká. Nebo můžete mít funkci, která umožňuje odchozí požadavky na třetí strany služby a tyto volání musí být míra omezené. V těchto případech může pomoci použití omezení sem. Výchozí hodnota je bez vazby.|
|dynamicThrottlesEnabled|False|Když je povolené, budou tato nastavení způsobí čítače jako připojení, vlákna a procesy nebo paměti nebo procesoru/další kanálu zpracování požadavků na výkon systému pravidelně kontrolovat a pokud některá z těchto čítačů nad integrovanou vysokou prahovou hodnotou (80 %), požadavky odmítne kvůli 429 "Zaneprázdněn" odpověď, dokud counter(s) vrátit do normálního úrovně.|
