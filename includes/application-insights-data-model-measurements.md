Kolekce vlastních měření. Použijte tuto kolekci pro sestavy s názvem měření přidružené k položce telemetrie. Typické případy použití jsou:
- velikost Telemetrických závislostí datové části
- počet položek fronty zpracovávaných požadavků Telemetrie
- čas tohoto zákazníka trvalo dokončení kroku při dokončení průvodce krok události Telemetrie.

Můžete zadat dotaz [vlastní měření](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) v analýza aplikace:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Vlastní měření jsou přidružená k položce telemetrie, ke které patří. Jsou předmětem vzorkování s položkou telemetrie obsahující tyto měření. Chcete-li sledovat Měrná jednotka, která má hodnotu nezávislé na jiné typy telemetrických dat, použijte [metriky telemetrie](../articles/application-insights/app-insights-api-custom-events-metrics.md).

Maximální délka klíče: 150
