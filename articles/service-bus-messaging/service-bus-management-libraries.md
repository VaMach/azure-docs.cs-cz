---
title: "Správa knihovny Azure Service Bus | Microsoft Docs"
description: "Správa oborů názvů Service Bus a entity z rozhraní .NET pro zasílání zpráv."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/18/2017
ms.author: sethm
ms.openlocfilehash: 3b7096a073b509217a6ed29b53f88f912e6613f6
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/19/2017
---
# <a name="service-bus-management-libraries"></a>Správa knihovny Service Bus

Správa knihovny Azure Service Bus můžete dynamicky zajišťují obory názvů Service Bus a entity. To umožňuje komplexní nasazení a scénáře zasílání zpráv a je možné programově určit, jaké entity zřídit. Tyto knihovny jsou aktuálně dostupné pro rozhraní .NET.

## <a name="supported-functionality"></a>Podporované funkce

* Namespace vytváření, aktualizaci, odstranění
* Vytvoření fronty, aktualizaci, odstranění
* Téma vytváření, aktualizaci, odstranění
* Vytvoření odběru, aktualizaci, odstranění

## <a name="prerequisites"></a>Požadavky

Abyste mohli začít používat správu knihovny Service Bus, je třeba ověřit pomocí služby Azure Active Directory (AAD). AAD vyžaduje ověřování jako hlavní název služby, která poskytuje přístup k prostředkům Azure. Informace o vytvoření objektu služby najdete v jednom z těchto článků:  

* [Pomocí portálu Azure k vytvoření aplikace Active Directory a objektu služby, které mají přístup k prostředkům](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Vytvoření instančního objektu pro přístup k prostředkům pomocí Azure PowerShellu](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Vytvoření instančního objektu pro přístup k prostředkům pomocí rozhraní příkazového řádku Azure](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Tyto kurzy vám poskytují `AppId` (ID klienta), `TenantId`, a `ClientSecret` (ověřovací klíč), které se používají pro ověřování pomocí knihovny správy. Musíte mít **vlastníka** oprávnění pro skupinu prostředků, na kterém chcete spustit.

## <a name="programming-pattern"></a>Vzor programování

Vzor k manipulaci s jakýmikoli prostředky sběrnice řídí společný protokol:

1. Získání tokenu z Azure Active Directory pomocí **Microsoft.IdentityModel.Clients.ActiveDirectory** knihovny.
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```
2. Vytvořte `ServiceBusManagementClient` objektu.

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Nastavte `CreateOrUpdate` parametry pro zadané hodnoty.

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Spusťte volání.

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Další kroky
* [Ukázka správy rozhraní .NET](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Odkaz na Microsoft.Azure.Management.ServiceBus rozhraní API](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
