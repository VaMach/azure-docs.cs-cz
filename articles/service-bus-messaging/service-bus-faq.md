---
title: "Azure Service Bus nejčastější dotazy (FAQ) | Microsoft Docs"
description: "Odpovídá na některé často kladené otázky týkající se Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 1403184d96388cb03b2c767c4da342ec1c6fe236
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="service-bus-faq"></a>Nejčastější dotazy k Service Bus
Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se služby Microsoft Azure Service Bus. Další informace získáte [podporu nejčastější dotazy k Azure](http://go.microsoft.com/fwlink/?LinkID=185083) obecné Azure – ceny a podporu informace.

## <a name="general-questions-about-azure-service-bus"></a>Obecné otázky o Azure Service Bus
### <a name="what-is-azure-service-bus"></a>Co je Azure Service Bus?
[Azure Service Bus](service-bus-messaging-overview.md) je asynchronní zasílání zpráv Cloudová platforma, která umožňuje odesílání dat mezi odpojeného systémy. Společnost Microsoft poskytuje jako služba, což znamená, že není potřeba žádný svůj vlastní hardware hostitele odborných tuto funkci.

### <a name="what-is-a-service-bus-namespace"></a>Co je obor názvů Service Bus?
A [obor názvů](service-bus-create-namespace-portal.md) poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace. Vytvořit je nutné použít Service Bus a bude jeden z první kroků v části Začínáme.

### <a name="what-is-an-azure-service-bus-queue"></a>Co je fronty Azure Service Bus?
A [fronty Service Bus](service-bus-queues-topics-subscriptions.md) je entita, ve kterém jsou uložené zprávy. Fronty jsou užitečné, když máte více aplikací, nebo z několika součástí distribuované aplikace, které potřebují ke komunikaci mezi sebou. Fronta je podobná distribučního centra v, že více produktů (zpráv) jsou přijata a pak se odešle z tohoto umístění.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Co jsou témata a předplatné Azure Service Bus?
Téma můžete vizualizovat jako fronty a při použití více předplatných, začne bohatší zasílání zpráv modelu; v podstatě nástroj na více komunikace. Tento model publikování a přihlášení k odběru (nebo *pub nebo sub*) umožňuje aplikaci, která odešle zprávu do tématu s více odběry tak, aby měl tento zpráv přijatých více aplikací.

### <a name="what-is-a-partitioned-entity"></a>Co je dělené entity?
Konvenční fronta nebo téma zpracování zprostředkovatelem jedné zprávy a uloženy v úložišti jeden zasílání zpráv. A [oddílů fronta nebo téma](service-bus-partitioning.md) zpracovává více zpráv zprostředkovatelé a uložená v úložištích více zasílání zpráv. To znamená, že celkovou propustnost oddílů fronta nebo téma už není omezené podle výkonu zprostředkovatele jedné zprávy nebo úložišti pro přenos zpráv. Kromě toho dočasnému výpadku zasílání zpráv úložiště nevykresluje oddílů fronta nebo téma není k dispozici.

Všimněte si, že řazení není zajištěna při použití rozdělení entity. V případě, že oddíl není k dispozici, můžete i nadále odesílat a přijímat zprávy z jiných oddílů.

## <a name="best-practices"></a>Osvědčené postupy
### <a name="what-are-some-azure-service-bus-best-practices"></a>Jaké jsou některé z osvědčených postupů Azure Service Bus?
* [Osvědčené postupy pro zlepšení výkonu pomocí služby Service Bus] [ Best practices for performance improvements using Service Bus] – Tento článek popisuje, jak optimalizovat výkon při výměně zpráv.

### <a name="what-should-i-know-before-creating-entities"></a>Co je třeba vědět před vytvořením entity?
Následující vlastnosti frontu a téma se nedá změnit. Proveďte to v úvahu při zřizování vaší entity jako to nemůže být upraven, bez vytvoření nové entity nahrazení.

* Velikost
* Dělení
* Relace
* Detekce duplicitních
* Expresní entity

## <a name="pricing"></a>Ceny
Tato část odpovídá na některé často kladené otázky týkající se služby Service Bus cenové struktury.

[Service Bus ceny a fakturace](service-bus-pricing-billing.md) článek vysvětluje fakturace měřidla v Service Bus a informace o cenách možností Service Bus, najdete v části [Service Bus podrobnosti o cenách](https://azure.microsoft.com/pricing/details/service-bus/).

Další informace získáte [Azure podporu nejčastější dotazy k](http://go.microsoft.com/fwlink/?LinkID=185083) pro obecné Azure informace o cenách. 

### <a name="how-do-you-charge-for-service-bus"></a>Jak se vám účtují pro Service Bus?
Úplné informace o cenách služby Service Bus, najdete v tématu [Service Bus podrobnosti o cenách][Pricing overview]. Kromě ceny uvedené budou se vám účtovat pro přenosy přidružená data o sazbách za odchozí mimo datové centrum, ve kterém je zajištěna vaší aplikace.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Jaké využití služby Service Bus podléhá přenos dat? Co není?
Bez poplatků, stejně tak i všechny příchozí přenosy dat je k dispozici žádné přenos dat v dané oblasti Azure. Přenos dat mimo oblast je předmětem poplatky za odchozí data, které můžou být nalezen [zde](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Účtují sběrnice pro úložiště?
Ne, sběrnice není účtují pro úložiště. Je však kvótu omezení maximální množství dat, která můžete nastavit jako trvalý, za fronta nebo téma. Najdete v části Nejčastější dotazy pro další.

## <a name="quotas"></a>Kvóty

Seznam kvót a omezení služby Service Bus, najdete v článku [přehled kvóty Service Bus][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Má Service Bus žádné kvóty využití?
Ve výchozím nastavení pro všechny cloudové služby společnosti Microsoft nastaví agregační měsíční využití kvóta, která je vypočtená ve všech předplatných zákazníka. Chápeme, bude pravděpodobně vyžadovat více než tato omezení, protože oddělení služeb zákazníkům kdykoli tak, aby jsme pochopení potřeb a odpovídajícím způsobem nastavit tyto limity. Služba Service Bus kvóty agregační využití je 5 miliardy zpráv za měsíc.

Když jsme si vyhrazuje právo k zakázání účtu zákazníka, která byla překročena kvóty jeho využití v daném měsíci, poskytujeme bude e-mailová oznámení a provádění více pokusů ke kontaktování zákazníka před provedením jakékoli akce. Zákazníci překročení těchto kvót stále je zodpovědná za poplatky, které překračují kvóty.

Stejně jako u jiných služeb v Azure Service Bus vynucuje sadu konkrétní kvóty pro zajištění správného využití prostředků. Můžete najít další podrobnosti o těchto kvót v [přehled kvóty Service Bus][Quotas overview].

## <a name="troubleshooting"></a>Řešení potíží
### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Jaké jsou některé výjimky generované API pro Azure Service Bus a jejich doporučované akce?
Seznam možných výjimek Service Bus najdete v tématu [výjimky přehled][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Co je sdíleného přístupového podpisu a jazyky, které podporuje generování podpis?
Sdílené přístupové podpisy jsou mechanismus ověřování založené na SHA-256 zabezpečené hodnoty hash nebo identifikátory URI. Informace o tom, jak generovat vlastní podpisy v uzlu, PHP, Java a C\#, najdete v článku [sdílené přístupové podpisy] [ Shared Access Signatures] článku.

## <a name="subscription-and-namespace-management"></a>Správa předplatného a obor názvů
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Jak migrace oboru do jiného předplatného Azure

Můžete přesunout oboru názvů z jedno předplatné do druhého, buď pomocí [portál Azure](https://portal.azure.com) nebo příkazů prostředí PowerShell. Aby bylo možné provést operaci, musí být obor názvů již aktivní. Uživatel provádění příkazů, které musí být správce na zdrojovém i cílovém odběry.

#### <a name="portal"></a>Portál

Portál Azure pro migraci oborů názvů Service Bus do jiného předplatného, postupujte podle pokynů [zde](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

Následující sekvence příkazů prostředí PowerShell přesune do jiného oboru názvů z jedno předplatné. K provedení této operace, obor názvů musí již být aktivní, a uživatel, který spouští příkazy prostředí PowerShell musí být správce na zdrojovém i cílovém odběry.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Další kroky
Pokud se o službě Service Bus chcete dozvědět víc, pročtěte si následující témata.

* [Představení Azure Service Bus Premium (příspěvek na blogu)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Představení služby Azure Service Bus Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Přehled služby Service Bus](service-bus-messaging-overview.md)
* [Přehled architektury služby Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
