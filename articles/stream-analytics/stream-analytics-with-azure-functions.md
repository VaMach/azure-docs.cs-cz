---
title: "Spusťte Azure Functions s úlohy Azure Stream Analytics | Microsoft Docs"
description: "Informace o konfiguraci funkce Azure jako výstupní jímku pro datový proud analytické úlohy."
keywords: "data výstup, streamování dat Azure – funkce"
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/19/2017
ms.author: sngun
ms.openlocfilehash: adc147fc9f47e78cc0a2fcaf53f064bcfa5eee2c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="run-azure-functions-with-azure-stream-analytics-jobs"></a>Spusťte Azure Functions s úlohy Azure Stream Analytics 
 
> [!IMPORTANT]
> Tato funkce je ve verzi preview.

Azure Functions můžete spustit pomocí služby Azure Stream Analytics konfigurací Azure Functions jako jeden z jímky výstup do úlohy Stream Analytics. Azure funkce je událost řízené, výpočetní na vyžádání prostředí, které umožňuje implementovat kód, který je aktivovaného událostmi, ke kterým dochází v Azure nebo služby třetích stran. Tato schopnost funkce Azure reagovat na aktivační události je přirozené výstup do úlohy Azure Stream Analytics.

Azure Stream Analytics vyvolá funkce Azure prostřednictvím protokolu HTTP aktivační události. Adaptér pro výstup funkce Azure umožňuje uživatelům připojit Azure Functions do služby Stream Analytics tak, že události můžete spustit založená na dotazech Stream Analytics. 

Tento kurz ukazuje, jak připojit Azure Stream Analytics k [Azure Redis Cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) pomocí [Azure Functions](../azure-functions/functions-overview.md). 

## <a name="configure-stream-analytics-job-to-run-an-azure-function"></a>Konfigurace úlohy služby Stream Analytics ke spuštění funkce Azure 

V této části ukazuje, jak nakonfigurovat spuštění funkce Azure, který zapisuje data do Azure Redis Cache úlohy Stream Analytics. Úloha Stream Analytics načítá události z centra událostí, provede dotaz, který volá funkci Azure. Tato funkce Azure načte data z úlohy Stream Analytics a zapíše ho do Azure Redis Cache.

![Obrázek znázorňující kurz](./media/stream-analytics-with-azure-functions/image1.png)

Následující kroky jsou nezbytné pro dosažení této úlohy:
* [Vytvořte úlohu služby Stream Analytics s centra událostí jako vstup.](#create-stream-analytics-job-with-event-hub-as-input)  
* [Vytvoření Azure Redis Cache.](#create-an-azure-redis-cache)  
* [Vytvoření funkce Azure, který může zapisovat data do mezipaměti Redis.](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Aktualizujte úlohu datového proudu analytické funkce Azure jako výstup.](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Zkontrolujte výsledky Redis Cache.](#check-redis-cache-for-results)  

## <a name="create-stream-analytics-job-with-event-hub-as-input"></a>Vytvořit úlohu služby Stream Analytics s centra událostí jako vstup

Postupujte podle [odhalování podvodů v reálném čase](stream-analytics-real-time-fraud-detection.md) kurz pro vytvoření centra událostí, spusťte aplikaci generátor událostí a vytvoření Azure Stream Analytics (přeskočit provede kroky k vytvoření dotaz a výstup, bude místo toho instalace Azure Functions výstup v další části.)

## <a name="create-an-azure-redis-cache"></a>Vytvoření Azure Redis Cache

1. Vytvoření Azure Redis Cache pomocí kroků popsaných v [vytvoření mezipaměti](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) článku Redis Cache.  

2. Po vytvoření mezipaměti Redis, přejděte na vytvořenou mezipaměti > **přístupové klíče** > a poznamenejte si **primární připojovací řetězec**.

   ![Připojovací řetězec mezipaměti redis](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-an-azure-function-that-can-write-data-to-the-redis-cache"></a>Vytvoření funkce Azure, který může zapisovat data do mezipaměti Redis

1. Použití [vytvořit aplikaci function app](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) části dokumentace Azure Functions vytvořit aplikaci funkce Azure a [funkce aktivované protokolem HTTP Azure](../azure-functions/functions-create-first-azure-function.md#create-function) (neboli Webhooku) pomocí **CSharp** jazyk.  

2. Přejděte na **run.csx** funkce a aktualizovat ji následujícím kódem (Nezapomeňte nahradit "\<místo připojovacího řetězce redis cache\>" textu s Redis Cache primární připojovací řetězec jestli jste získali v předchozí části):  

   ```c#
   using System;
   using System.Net;
   using System.Threading.Tasks;
   using StackExchange.Redis;
   using Newtonsoft.Json;
   using System.Configuration;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
      // Get the request body
      dynamic dataArray = await req.Content.ReadAsAsync<object>();

      // Throw an HTTP Request Entity Too Large exception when the incoming batch(dataArray) is greater than 256 KB. Make sure that the size value is consistent with the value entered in the Stream Analytics portal.

      if (dataArray.ToString().Length > 262144)
      {        
         return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
      var connection = ConnectionMultiplexer.Connect("<your redis cache connection string goes here>");
      log.Info($"Connection string.. {connection}");
    
      // Connection refers to a property that returns a ConnectionMultiplexer
      IDatabase db = connection.GetDatabase();
      log.Info($"Created database {db}");
    
      log.Info($"Message Count {dataArray.Count}");

      // Perform cache operations using the cache object. For example, the following code block adds few integral data types to the cache
      for (var i = 0; i < dataArray.Count; i++)
      {
        string time = dataArray[i].time;
        string callingnum1 = dataArray[i].callingnum1;
        string key = time + " - " + callingnum1;
        db.StringSet(key, dataArray[i].ToString());
        log.Info($"Object put in database. Key is {key} and value is {dataArray[i].ToString()}");
       
      // Simple get of data types from the cache
      string value = db.StringGet(key);
      log.Info($"Database got: {value}");
      }

      return req.CreateResponse(HttpStatusCode.OK, "Got");
}    

   ```

   Když Azure Stream Analytics přijme 413 (Http požadavku Entity příliš velký) výjimky z funkce Azure, snižuje velikost dávky, který odesílá Azure Functions. V Azure funkce použijte následující kód zkontrolujte, že Azure Stream Analytics neodešle nadměrné velikosti dávky. Ujistěte se, že jsou konzistentní s hodnotou zadanou v portálu Stream Analytics batch maximální počet a velikost hodnoty používané ve funkci.

   ```c#
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. V textovém editoru podle vaší volby, vytvořte soubor JSON s názvem **project.json** následujícím kódem a uložte ho do místního počítače. Tento soubor obsahuje závislosti balíčku NuGet, požadované funkce jazyka c#.  
   
   ```json
       {
         "frameworks": {
             "net46": {
                 "dependencies": {
                     "StackExchange.Redis":"1.1.603",
                     "Newtonsoft.Json": "9.0.1"
                 }
             }
         }
     }

   ```
 
4. Přejděte zpět na portálu Azure > přejde na Azure funkce > z **funkce** kartě > klikněte na **App Service Editor** umístěné v **nástroje pro vývoj**. 
 
   ![Aplikace služby editor obrazovky](./media/stream-analytics-with-azure-functions/image3.png)

5. V editoru služby aplikace, klikněte pravým tlačítkem na kořenový adresář a nahrajte **project.json** souboru. Po úspěšném odeslání aktualizujte stránku, měli byste vidět automaticky generovaný soubor s názvem **project.lock.json**.  Automaticky generovaný soubor obsahuje odkazy na knihovny DLL, které jsou určené v souboru Project.json.  

   ![Nahrát soubor project.json](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytic-job-with-azure-function-as-output"></a>Aktualizace jako výstup datového proudu analytické úlohy s funkce Azure

1. Spusťte úlohu Azure Stream Analytics na portálu Azure.  

2. Přejděte na Azure funkce > **přehled** > **výstupy** > **přidat** nový výstupní > vyberte **funkce Azure** pro možnost jímky. Adaptér pro výstup nové funkce Azure je k dispozici s následujícími vlastnostmi:  

   |**Název vlastnosti**|**Popis**|
   |---|---|
   |Alias pro výstup| Popisný název, který používáte v dotazu úlohy k odkazu na výstup. |
   |Možnost importu| Můžete použít azure funkce z aktuálního předplatného, nebo ručně zadejte nastavení, pokud se funkce nachází v jiné předplatné. |
   |Aplikace zajišťující funkci| Název aplikace Azure funkce. |
   |Funkce| Název funkce v aplikaci funkce (název funkce run.csx).|
   |Velikost dávky maximální|Tato vlastnost se používá k nastavení maximální velikosti pro každou dávku výstup, která se posílají funkce Azure. Ve výchozím nastavení je tato hodnota nastavena na 256 KB.|
   |Maximální počet dávek|Tato vlastnost umožňuje určit maximální počet událostí v každé dávce, které je odesláno funkce Azure. Počet maximální dávky výchozí hodnota je 100. Tato vlastnost je nepovinná.|
   |Klíč|Tato vlastnost umožňuje používat funkci Azure z jiné předplatné. Zadejte hodnotu klíče pro přístup k vaší funkci. Tato vlastnost je nepovinná.|

3. Zadejte název pro alias pro výstup. V tomto kurzu jsme název **saop1** (můžete použít jiný název zvoleného) a vyplňte další podrobnosti.  

4. Otevřete úlohu služby Stream Analytics a aktualizujte dotaz pro následující (Nezapomeňte nahradit text "saop1" Pokud výstupní jímku mít jiný název):  

   ```sql
    SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        INTO saop1
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
           JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum
   ```

5. Spustit aplikaci telcodatagen.exe spuštěním následujícího příkazu na příkazovém řádku (příkaz přijímá formát - `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`)  
   
   **telcodatagen.exe 1000.2 2**
    
6.  Spuštění úlohy Stream Analytics.

## <a name="check-redis-cache-for-results"></a>Zkontrolujte výsledky Redis Cache

1. Přejděte na portál Azure a najít mezipaměť Redis > vyberte **konzoly**.  

2. Použití [Redis cache příkazy](https://redis.io/commands) k ověření, že vaše data jsou v mezipaměti Redis. (Příkaz přijímá Get formátu {klíč}). Příklad:

   **Získat "12/19/2017 21:32:24-123414732"**

   Tento příkaz má tisk hodnoty pro zadaný klíč:

   ![Výstupní mezipaměti redis](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Známé problémy

* Na portálu Azure, při pokusu o resetování velikost dávky maximální / počet dávek maximální hodnotu empty(default), změní zpět na dříve zadanou hodnotu na Uložit. Záměrně zadejte výchozí hodnoty pro tato pole v tomto případě.

## <a name="next-steps"></a>Další postup
