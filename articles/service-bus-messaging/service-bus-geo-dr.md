---
title: "Azure Service Bus Geo-havárii | Microsoft Docs"
description: "Jak používat zeměpisné oblasti převzetí služeb při selhání a proveďte obnovení po havárii v Azure Service Bus"
services: service-bus-messaging
documentationcenter: 
author: christianwolf42
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: sethm
ms.openlocfilehash: 49f2992245d694f85b7b1f1c34339f1445c9d699
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="azure-service-bus-geo-disaster-recovery-preview"></a>Azure Service Bus Geo-havárii (preview)

Regionální datová centra dojde k výpadku, je důležité pro zpracování dat pro i nadále fungovat v jiné oblasti nebo datového centra. Jako takový *geograficky havárii* a *geografická replikace* jsou důležité funkce pro všechny organizace. Azure Service Bus podporuje havárii geografické obnovení a geografická replikace, na úrovni oboru názvů. 

Ve verzi preview havárii geografické obnovení je aktuálně k dispozici pouze ve dvou oblastech (**– Sever střední USA** a **jihu USA)**.

## <a name="outages-and-disasters"></a>Výpadky a havárií

[Osvědčené postupy pro izolační aplikace proti výpadkům Service Bus a havárií](service-bus-outages-disasters.md) článku rozlišuje "výpadky" a "havárie", což je důležité si uvědomit. *Výpadku* je dočasné nedostupnosti Azure Service Bus a může mít vliv na některé součásti služby, jako je zasílání zpráv úložiště nebo i celého datového centra. Ale po napravení problému, Service Bus opět k dispozici. Výpadek obvykle nezpůsobí ztrátu zprávy nebo jiná data. Příkladem takových výpadku může být výpadku napájení v datovém centru.

A *po havárii* je definován jako trvalé, nebo dlouhodobější ztrátu Service Bus [jednotky škálování](service-bus-architecture.md#service-bus-scale-units) nebo datacenter. Datacentru může nebo nemusí opět k dispozici nebo může být mimo provoz pro hodin nebo dnů. Příkladem takových havárie jsou ještě efektivněji, zahlcení nebo zemětřesení. Po havárii, který se stane trvalé může způsobit ztrátu některé zprávy nebo jiná data. Ale ve většině případů by měla být nedošlo ke ztrátě dat a zprávy lze obnovit po zálohování datového centra.

Funkce obnovení geograficky havárie Azure Service Bus je řešení zotavení po havárii. Koncepty a pracovní postup popsaný v tomto článku použít po havárii scénáře a nikoli výpadků přechodný nebo dočasné.  

## <a name="basic-concepts-and-terms"></a>Základními koncepcemi a termíny

Funkce obnovení po havárii implementuje zotavení po havárii metadata a spoléhá na primárním serverem a obory názvů obnovení po havárii sekundární. Upozorňujeme, že je k dispozici pro funkci havárii geografické obnovení [Premium obory názvů](service-bus-premium-messaging.md) pouze. Není nutné žádné změny připojovací řetězec, jako je připojení přes alias.

V tomto článku se používají následující termíny:

-  *Alias*: hlavní připojovací řetězec.

-  *Obor názvů primární a sekundární*: Popisuje obory názvů, které odpovídají alias. Primární "aktivní" a přijímá zprávy, sekundární je "pasivní" a nepřijímá zprávy. Metadata mezi oběma se synchronizace, tak i může bezproblémově přijmout zprávy beze změn kódu aplikace.

-  *Metadata*: vaše reprezentace objektů v Azure Service Bus. Momentálně podporujeme pouze metadata.

-  *Převzetí služeb při selhání*: proces aktivace sekundární obor názvů. Musíte načítat zprávy z vaší dříve primární obor názvů, až bude opět k dispozici a pak odstraňte oboru názvů. Pokud chcete vytvořit jinou převzetí služeb při selhání, přidejte nový sekundární obor názvů pro spárování. Pokud chcete znovu použít předchozí primární obor názvů po selhání, musíte nejprve odebrat všechny existující entity z oboru názvů. Ujistěte se, že než tak učiníte přijímat všechny zprávy.

## <a name="failover-workflow"></a>Pracovní postup převzetí služeb při selhání

V následující části je přehled celý proces nastavení počáteční převzetí služeb při selhání a jak pokračovat od tohoto okamžiku.

![1][]

Nejdřív nastavit primární a sekundární obor názvů a potom vytvořit párování. Tato párování vám dává alias, který můžete použít pro připojení. Vzhledem k tomu, že používáte alias, nemáte změnit připojovací řetězce. Pouze nové obory názvů mohou být přidány do vaší párování převzetí služeb při selhání. Nakonec je třeba přidat některé logiku aktivační události (například některé obchodní logiky, která zjistí, pokud není k dispozici obor názvů a zahájí převzetí služeb při selhání). Můžete zkontrolovat pomocí dostupnosti oboru názvů [zpráva procházení](message-browsing.md) funkce služby Service Bus.

Poté, co jste nastavili jak monitorování a zotavení po havárii, můžete si prohlédnout proces převzetí služeb při selhání. Pokud aktivační událost zahájí převzetí služeb při selhání, nebo ručně spustit převzetí služeb při selhání, vyžadují se dva kroky:

1. V případě jiné výpadku chcete mít možnost převzetí služeb při selhání znovu. Proto nastavit druhý pasivní obor názvů a aktualizujte párování. 
2. Jakmile je k dispozici nový obor názvů pro vyžádání obsahu zprávy z dříve primární oboru názvů. Potom znovu použít nebo odstraňte staré primární oboru názvů.

![2][]

## <a name="set-up-disaster-recovery"></a>Nastavení zotavení po havárii

Tato část popisuje, jak vytvořit vlastní kód pro obnovení Service Bus Geo-po havárii. K tomu budete potřebovat dva obory názvů v nezávislých umístění; například USA – jih a Sever střední USA. Následující příklad používá Visual Studio 2017.

1.  Vytvořte novou **konzoly App(.Net Framework)** projektu v sadě Visual Studio a zadejte jeho název, například **SBGeoDR**.

2.  Nainstalujte následující balíčky NuGet:
    1.  Microsoft.IdentityModel.Clients.ActiveDirectory
    2.  Microsoft.Azure.Management.ServiceBus

3. Ujistěte se, že je verze balíčku Newtonsoft.Json NuGet, který používáte verze 10.0.3.

3.  Přidejte následující `using` příkazy kódu:

    ```csharp
    using System.Threading;
    using Microsoft.Azure.Management.ServiceBus;
    using Microsoft.Azure.Management.ServiceBus.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

4. Upravit vaše `main()` metody přidat dva obory názvů premium:

    ```csharp
    // 1. Create primary namespace (optional).

    var namespaceParams = new SBNamespace()
    {
        Location = "South Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    var namespace1 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, namespaceParams);

    // 2. Create secondary namespace (optional if you already have an empty namespace available).

    var namespaceParams2 = new SBNamespace()
    {
        Location = "North Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    // If you re-run this program while namespaces are still paired this operation will fail with a bad request.
    // This is because we block all updates on secondary namespaces once it is paired.

    var namespace2 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRSecondaryNS, namespaceParams2);
    ```

5. Povolit párování mezi dva obory názvů a získat alias, který později použít pro připojení k vaší entity:

    ```csharp
    // 3. Pair the namespaces to enable DR.

    ArmDisasterRecovery drStatus = client.DisasterRecoveryConfigs.CreateOrUpdate(
        resourceGroupName,
        geoDRPrimaryNS,
        alias,
        new ArmDisasterRecovery { PartnerNamespace = geoDRSecondaryNS });

    // A similar loop can be used to check if other operations (Failover, BreakPairing, Delete) 
    // mentioned below have been successful.
    while (drStatus.ProvisioningState != ProvisioningStateDR.Succeeded)
    {
        Console.WriteLine("Waiting for DR to be set up. Current state: " +
        drStatus.ProvisioningState);
        drStatus = client.DisasterRecoveryConfigs.Get(
        resourceGroupName,
        geoDRPrimaryNS,
        alias);

        Thread.CurrentThread.Join(TimeSpan.FromSeconds(30));
    }
    ```

Úspěšně jste nastavili dva spárované obory názvů. Nyní můžete vytvořit entity k synchronizaci metadat sledovat. Pokud chcete provést převzetí služeb okamžitě později, měli byste povolit nějakou dobu metadata synchronizovat. Můžete přidat krátký režimu spánku po, například:

```csharp
client.Topics.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", new SBTopic());
client.Subscriptions.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", "myTopic-Sub1", new SBSubscription());

// sleeping to allow metadata to sync across primary and secondary
Thread.Sleep(1000 * 60);
```

V tomto okamžiku můžete přidávat entity prostřednictvím portálu nebo pomocí Azure Resource Manager a v tématu Jak synchronizaci. Pokud váš plán je ruční převzetí služeb při selhání, měli byste vytvořit aplikaci, která monitoruje primární obor názvů a zahájí převzetí služeb při selhání, pokud bude k dispozici. 

## <a name="initiate-a-failover"></a>Zahájení převzetí služeb při selhání

Následující kód ukazuje, jak k zahájení převzetí služeb při selhání:

```csharp
// Note that this failover operation is always run against the secondary namespace 
// (because primary might be down at time of failover).

client.DisasterRecoveryConfigs.FailOver(resourceGroupName, geoDRSecondaryNS, alias);
```

Po spustíte převzetí služeb při selhání, přidejte nový obor názvů pasivní a znovu vytvořit párování. Kód k vytvoření nové párování se zobrazí v předchozí části. Kromě toho je nutné odebrat zprávy z původního primárního oboru názvů, po dokončení převzetí. Příklady, jak přijmout zprávy z fronty, najdete v části [začít pracovat s fronty](service-bus-dotnet-get-started-with-queues.md).

## <a name="how-to-disable-geo-disaster-recovery"></a>Jak zakázat geografická havárii

Následující kód ukazuje, jak zakázat obor názvů párování:

```csharp
client.DisasterRecoveryConfigs.BreakPairing(resourceGroupName, geoDRPrimaryNS, alias);
```

Následující kód odstraní alias, který jste vytvořili:

```csharp
// Delete the DR config (alias).
// Note that this operation must run against the namespace to which the alias is currently pointing.
// If you break the pairing and want to delete the namespaces afterwards, you must delete the alias first.

client.DisasterRecoveryConfigs.Delete(resourceGroupName, geoDRPrimaryNS, alias);
```

## <a name="steps-after-a-failover-failback"></a>Kroky následující po selhání (navrácení služeb po obnovení)

Po selhání proveďte následující dva kroky:

1.  Vytvořte nový pasivní sekundární obor názvů. Kód se zobrazí v předchozí části.
2.  Odeberte zbývající zprávy z fronty.

## <a name="alias-connection-string-and-test-code"></a>Alias připojovací řetězec a testování kódu

Pokud chcete testovat proces převzetí služeb při selhání, můžete napsat ukázkové aplikace, který uloží zprávy do primární používání alias oboru názvů. Uděláte to tak, ujistěte se, získat připojovací řetězec aliasu z aktivní obor názvů. V aktuální verzi preview není k dispozici žádné další rozhraní přímo získat připojovací řetězec. Následující příklad kódu připojí před a po převzetí služeb při selhání:

```csharp
var accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRPrimaryNS, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;

if(aliasPrimaryConnectionString == null)
{
    accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRSecondaryNS, "RootManageSharedAccessKey");
    aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
    aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
}
```

## <a name="next-steps"></a>Další kroky

- Najdete v části geografická havárii [zde odkazu k REST API](/rest/api/servicebus/disasterrecoveryconfigs).
- Spustit obnovení geograficky havárii [ukázku na Githubu](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Najdete v části geografická havárii [ukázku, která odesílá zprávy do alias](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Další informace o zasílání zpráv Service Bus, najdete v následujících článcích:

* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md)
* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak používat témata a odběry Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [Rozhraní REST API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
