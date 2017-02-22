Existují některá omezení počtu metrik a událostí na aplikaci (tj. na instrumentační klíč). Omezení závisí na zvoleném [cenovém plánu](https://azure.microsoft.com/pricing/details/application-insights/).

| **Prostředek** | **Výchozí omezení** | **Poznámka**
| --- | --- | --- |
| Celkem dat za den | 500 GB | Objem dat jde snížit nastavením limitu. Když potřebujete víc, napište na adresu AIDataCap@microsoft.com.
| Volná data za měsíc<br/> (Základní cenový plán) | 1 GB | Další data se účtují za gigabajt.
| Omezování | 32&000; událostí za sekundu | Omezení se měří se po minutách.
| Uchovávání dat | 90 dnů | Tento prostředek je pro funkce [Vyhledávání](../articles/application-insights/app-insights-diagnostic-search.md), [Analýza](../articles/application-insights/app-insights-analytics.md) a [Průzkumník metrik](../articles/application-insights/app-insights-metrics-explorer.md).
| [Vícekrokový test dostupnosti](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) – uchování podrobných výsledků | 90 dnů | Tento prostředek poskytuje podrobné výsledky každého kroku.
| Délka názvu vlastnosti a metriky | 150 |
| Délka řetězce hodnoty vlastnosti | 8&192; |
| Délka zprávy trasování a výjimky | 10 000 |
| [Testy dostupnosti](../articles/application-insights/app-insights-monitor-web-app-availability.md) – počet na aplikaci  | 10 |

Další informace najdete v tématu [Ceny a kvóty ve službě Application Insights](../articles/application-insights/app-insights-pricing.md).


<!--HONumber=Feb17_HO2-->


