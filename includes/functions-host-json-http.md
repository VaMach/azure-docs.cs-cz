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
|maxOutstandingRequests|-1|Maximální počet nevyřízených požadavků, které bude možné uchovávat v každém okamžiku (-1 znamená bez vazby). Limit zahrnuje požadavky, které jsou zařazeny do fronty, ale nebyla spuštěna, a také všechny probíhající spuštěních. Všechny příchozí požadavky přes tento limit byly zamítnuty 429 "Zaneprázdněn" odpovědi. Volající můžete použít tuto odpověď využívat strategie opakování založené na čase. Určuje toto nastavení pouze služby Řízení front, dojde v této cestě hostitele provádění úlohy. Další fronty, jako je například fronty požadavků ASP.NET, je toto nastavení nemá vliv. |
|maxConcurrentRequests|-1|Maximální počet funkce protokolu HTTP, které budou spuštěny současně (-1 znamená bez vazby). Například můžete nastavit omezení Pokud funkce protokolu HTTP používat příliš mnoho systémových prostředků, když je souběžnosti vysoké. Nebo pokud funkcí provést odchozí požadavky služby třetích stran, těchto volání může být nutné míra limited.|
|dynamicThrottlesEnabled|False|Způsobí, že zpracování kanálu požadavku na pravidelně kontrolovat, čítače výkonu systému. Čítače zahrnují připojení, vláken, procesy, paměti a procesoru. Pokud některá z počítadla předdefinované nadměrného (80 %), požadavky byly zamítnuty 429 "Zaneprázdněn" odpovědi, dokud counter(s) vrátit do normálního úrovně.|
