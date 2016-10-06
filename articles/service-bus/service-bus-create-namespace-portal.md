<properties
    pageTitle="Vytvoření oboru názvů služby Service Bus pomocí portálu Azure Portal | Microsoft Azure"
    description="Abyste mohli začít používat Service Bus, budete potřebovat obor názvů. Zde zjistíte, jak ho můžete vytvořit pomocí portálu Azure Portal."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>


# Vytvoření oboru názvů služby Service Bus pomocí portálu Azure Portal

Obor názvů je společný kontejner pro všechny součásti zasílání zpráv. Součástí jednoho oboru názvů může být několik front a témat, přičemž obory názvů často slouží jako kontejnery aplikací. Obory názvů služby Service Bus je v současnosti možné vytvořit dvěma způsoby.

1.  Portál Azure Portal (tento článek)

2.  [Šablony Resource Manageru][create-namespace-using-arm]

## Vytvoření oboru názvů na portálu Azure Portal

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Blahopřejeme! Právě jste vytvořili obor názvů zasílání zpráv služby Service Bus.

## Další kroky

Podívejte se na naše [úložiště GitHub](https://github.com/Azure-Samples/azure-servicebus-messaging-samples][github-samples) s ukázkami, které představují některé pokročilejší funkce zasílání zpráv služby Azure Service Bus.

[create-namespace-using-arm]: ../service-bus-messaging/service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!--HONumber=Sep16_HO4-->


