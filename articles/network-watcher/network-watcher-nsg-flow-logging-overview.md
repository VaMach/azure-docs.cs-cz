---
title: "Úvod do toku protokolování pro skupiny zabezpečení sítě s sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak pomocí protokolů NSG tok funkce sledovací proces sítě Azure"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: be29b993592e494053353aac1067bfb7eff90ed7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Úvod do toku protokolování pro skupiny zabezpečení sítě

Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, která vám umožní zobrazit informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě. Tyto protokoly toku jsou zapsané ve formátu json a zobrazit příchozí a odchozí toky na základě pravidla na síťový adaptér tok se vztahuje na 5 řazené kolekce členů informace o toku (protokol IP zdroj nebo cíl, zdrojový nebo cílový Port), a pokud se povoluje nebo odepírá provoz.

![Přehled protokoly toku][1]

I když toku protokoluje cílové skupiny zabezpečení sítě, nejsou zobrazí stejné jako další protokoly. Tok protokoly se ukládají pouze v rámci účtu úložiště a následující cesta pro protokolování, jak je znázorněno v následujícím příkladu:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

Do protokolů toku použít stejné zásady uchovávání informací jako zobrazené na další protokoly. Protokoly mít zásady uchovávání informací, můžete nastavit od 1 den do 365 dní. Pokud zásady uchovávání nejsou nastavené, protokoly se ukládají navždy.

## <a name="log-file"></a>Soubor protokolu

Tok protokolů mají více vlastností. V následujícím seznamu je seznam vlastností, které jsou vráceny v protokolu toku NSG:

* **čas** – čas při protokolu byla zaznamenána událost
* **ID systému** -skupiny zabezpečení sítě ID prostředku.
* **kategorie** – kategorie události, je vždy jednat NetworkSecurityGroupFlowEvent
* **ResourceId** – prostředek Id NSG
* **operationName** -vždy NetworkSecurityGroupFlowEvents
* **vlastnosti** -kolekci vlastností toku
    * **Verze** -číslo verze schématu toku protokolu událostí
    * **toky** -kolekce toků. Tato vlastnost má několik záznamů pro různá pravidla
        * **pravidlo** -pravidla pro tyto toky jsou uvedeny
            * **toky** -kolekce toků
                * **Mac** – adresa MAC síťového adaptéru pro virtuální počítač, kde byl shromážděn toku
                * **flowTuples** -řetězec, který obsahuje více vlastností řazené kolekce členů toku ve formátu čárkami
                    * **Časové razítko** – tato hodnota je časové razítko, když toku došlo k chybě ve formátu UNIX EPOCH
                    * **Zdrojová adresa IP** -zdrojové IP adresy
                    * **Cílové IP** -cílovou IP adresu
                    * **Zdrojový Port** -zdrojový port
                    * **Cílový Port** – cílový Port
                    * **Protokol** -protokol toku. Platné hodnoty jsou **T** pro TCP a **U** pro UDP
                    * **Tok provozu** -směr toku přenosu. Platné hodnoty jsou **I** pro příchozí a **O** pro odchozí.
                    * **Provoz** – ať povolené nebo zakázané přenosy. Platné hodnoty jsou **A** pro povolené a **D** pro odepřen.


Následuje příklad toku protokolu. Jak vidíte, že existuje více záznamů, které následují seznam vlastností, které jsou popsané v předchozí části. 

> [!NOTE]
> Hodnoty ve vlastnosti flowTuples jsou seznam oddělený čárkami.
 
```json
{
    "records": 
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a>Další kroky

Informace o povolení toku protokoly navštivte stránky [povolení toku protokolování](network-watcher-nsg-flow-logging-portal.md).

Další informace o NSG protokolování, navštivte stránky [protokolu analýzy pro skupiny zabezpečení sítě (Nsg)](../virtual-network/virtual-network-nsg-manage-log.md).

Zjistěte, pokud je povolené nebo zakázané na virtuálním počítači, navštivte stránky přenosy [ověřte ověřte provoz s tokem IP](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

