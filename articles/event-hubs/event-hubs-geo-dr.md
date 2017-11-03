---
title: "Obnovení geograficky havárie Azure Event Hubs | Microsoft Docs"
description: "Jak používat zeměpisné oblasti převzetí služeb při selhání a proveďte obnovení po havárii v Azure Event Hubs"
services: event-hubs
documentationcenter: 
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: sethm
ms.openlocfilehash: 94c2782b3166fbc65ae755291a82a2a14556b96f
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/14/2017
---
# <a name="azure-event-hubs-geo-disaster-recovery-preview"></a>Azure Event Hubs havárii geografické obnovení (preview)

Regionální datová centra dojde k výpadku, je důležité pro zpracování dat pro i nadále fungovat v jiné oblasti nebo datového centra. Jako takový *geograficky havárii* a *geografická replikace* jsou důležité funkce pro všechny organizace. Azure Event Hubs podporuje havárii geografické obnovení a geografická replikace, na úrovni oboru názvů. 

Funkce obnovení geograficky havárii služby Azure Event Hubs je řešení zotavení po havárii. Koncepty a pracovní postup popsaný v tomto článku použít po havárii scénáře a nikoli výpadků přechodný nebo dočasné.

Podrobnou diskuzi o zotavení po havárii v Microsoft Azure, najdete v části [v tomto článku](/azure/architecture/resiliency/disaster-recovery-azure-applications). 

## <a name="terminology"></a>Terminologie

**Párování**: primární obor názvů se označuje jako *active* a přijímá zprávy. Obor názvů převzetí služeb při selhání je *pasivní* a nepřijímá zprávy. Metadata mezi oběma se synchronizace, tak i může bezproblémově přijmout zprávy beze změn kódu aplikace. Vytvoření konfigurace obnovení po havárii mezi oblast aktivní a pasivní oblasti se označuje jako *párování* ty oblasti.

**Alias**: název konfigurace obnovení po havárii, které jste nastavili. Alias poskytuje jeden stabilní připojovací řetězec plně kvalifikovaný název domény (FQDN). Aplikace použít tento připojovací řetězec aliasu se připojit k oboru názvů.

**Metadata**: odkazuje na názvy centra událostí, skupiny uživatelů, oddíly, jednotky propustnosti, entit a vlastnosti, které jsou spojeny s oborem názvů.

## <a name="enable-geo-disaster-recovery"></a>Povolit obnovení geo-po havárii

Povolíte služby Event Hubs havárii geografické obnovení 3 kroků: 

1. Vytvořte geo párování, který vytvoří řetězec připojení alias a poskytuje za provozu metadata replikace. 
2. Aktualizujte existující připojovací řetězce klienta na alias vytvořili v kroku 1.
3. Zahájit převzetí: párování geograficky je poškozený a alias odkazuje na sekundární oboru názvů jako jeho nový primární obor názvů.

Následující obrázek ukazuje tento pracovní postup:

![Párování geograficky toku][1] 

### <a name="step-1-create-a-geo-pairing"></a>Krok 1: vytvoření geograficky párování

Pokud chcete vytvořit párování mezi dvou oblastí, musíte jmenného prostoru primární a sekundární oboru názvů. Pak vytvoříte na formuláři geo pár alias. Jakmile jsou obory názvů spárovat s aliasem, metadata se pravidelně replikují v oba obory názvů. 

Následující kód ukazuje, jak to udělat:

```csharp
ArmDisasterRecovery adr = await client.DisasterRecoveryConfigs.CreateOrUpdateAsync(
                                    config.PrimaryResourceGroupName,
                                    config.PrimaryNamespace,
                                    config.Alias,
                                    new ArmDisasterRecovery(){ PartnerNamespace = config.SecondaryNamespace});
```

### <a name="step-2-update-existing-client-connection-strings"></a>Krok 2: aktualizovat existující klient připojovací řetězce

Po dokončení párování geograficky připojovací řetězce, které odkazují na primární obory názvů musí být aktualizovány tak, aby odkazoval na alias připojovací řetězec. Získejte připojovací řetězce, jak je znázorněno v následujícím příkladu:

```csharp
var accessKeys = await client.Namespaces.ListKeysAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
```

### <a name="step-3-initiate-a-failover"></a>Krok 3: zahájení převzetí služeb při selhání

Pokud dojde k havárii, nebo pokud se rozhodnete k zahájení převzetí služeb při selhání do sekundárního oboru názvů, budou metadat a dat odesílaných do oboru názvů sekundární spustit. Protože aplikace použijte řetězce připojení s alias, není nutné, žádná další akce, protože automatické spuštění čtení a zápis do centra událostí v sekundární oboru názvů. 

Následující kód ukazuje, jak aktivovat převzetí služeb:

```csharp
await client.DisasterRecoveryConfigs.FailOverAsync(config.SecondaryResourceGroupName,
                                                   config.SecondaryNamespace, config.Alias);
```

Po dokončení převzetí a potřebujete data, která je obsažená v primární oboru názvů, jak extrahovat data je nutné použít explicitní připojovací řetězec do centra událostí v primární oboru názvů.

### <a name="other-operations-optional"></a>Jiné operace (volitelné)

Můžete také rozdělit párování geograficky nebo odstranit alias, jak je znázorněno v následujícím kódu. Všimněte si, že pokud chcete odstranit připojení řetězec aliasu, musíte nejprve zrušit geo parkovací:

```csharp
// Break pairing
await client.DisasterRecoveryConfigs.BreakPairingAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, config.Alias);

// Delete alias connection string
// Before the alias is deleted, pairing must be broken
await client.DisasterRecoveryConfigs.DeleteAsync(config.PrimaryResourceGroupName,
                                                 config.PrimaryNamespace, config.Alias);
```

## <a name="considerations-for-public-preview"></a>Důležité informace pro verzi public preview

Pozorně si projděte následující informace pro tuto verzi:

1. Funkce obnovení geo-po havárii je dostupná jenom v oblastech severní jihu USA a Jižní střední USA. 
2. Tato funkce je podporována pouze pro nově vytvořené obory názvů.
3. Ve verzi preview je povolená pouze metadata replikace. Skutečná data se nereplikují.
4. Ve verzi preview je bez nákladů pro povolení funkce. Ale primární i sekundární obory názvů bude platit poplatky pro jednotky vyhrazenou propustností.

## <a name="next-steps"></a>Další kroky

* [Ukázku na Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) provede jednoduché pracovní postup, který vytvoří geograficky párování a zahájí převzetí služeb při selhání pro scénáře zotavení po havárii.
* [Odkazu k REST API](/rest/api/eventhub/disasterrecoveryconfigs) popisuje rozhraní API pro provádění konfigurace obnovení Geo-po havárii.

Další informace o službě Event Hubs naleznete pod těmito odkazy:

* Úvodní [Kurz služby Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)
* [Ukázkové aplikace, které používají službu Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/eh-geodr1.png

