---
title: "Azure Cloud Services Def. Schéma NetworkTrafficRules | Microsoft Docs"
ms.custom: 
ms.date: 04/14/2015
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 351b369f-365e-46c1-82ce-03fc3655cc88
caps.latest.revision: "17"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 37ec6c771ad83be680d95e9b5597d8da3cace9ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Definice schématu NetworkTrafficRules služby Azure Cloud Services
`NetworkTrafficRules` Uzel je volitelný element v souboru definice služby, která určuje, jak se role vzájemně komunikovat. Omezuje přístup vnitřních koncových bodů konkrétní role jednotlivé role. `NetworkTrafficRules` Není element samostatné; se zkombinuje s dvěma nebo více rolí v definičním souboru služby.

Výchozí přípona souboru definice služby je .csdef.

> [!NOTE]
>  `NetworkTrafficRules` Uzlu je pouze dostupné pomocí sady Azure SDK verze 1.3 nebo vyšší.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Definice schématu základní služby pro pravidla pro provoz sítě
Základní formát souboru definice služby obsahující definice provozu sítě je následující.

```xml
<ServiceDefinition …>
   <NetworkTrafficRules>
      <OnlyAllowTrafficTo>
         <Destinations>
            <RoleEndpoint endpointName="<name-of-the-endpoint>" roleName="<name-of-the-role-containing-the-endpoint>"/>
         </Destinations>
         <AllowAllTraffic/>
         <WhenSource matches="[AnyRule]">
            <FromRole roleName="<name-of-the-role-to-allow-traffic-from>"/>
         </WhenSource>
      </OnlyAllowTrafficTo>
   </NetworkTrafficRules>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Prvky schématu
`NetworkTrafficRules` Uzlu souboru definice služby zahrnuje tyto prvky, které jsou podrobně popsány v následujících částech v tomto tématu:

[NetworkTrafficRules Element](#NetworkTrafficRules)

[OnlyAllowTrafficTo Element](#OnlyAllowTrafficTo)

[Element cíle](#Destinations)

[RoleEndpoint Element](#RoleEndpoint)

[AllowAllTraffic Element](#AllowAllTraffic)

[WhenSource Element](#WhenSource)

[FromRole Element](#FromRole)

##  <a name="NetworkTrafficRules"></a>NetworkTrafficRules Element
`NetworkTrafficRules` Element určuje, jaké role může komunikovat s kterému koncovému bodu na jinou roli. Služba může obsahovat jednu `NetworkTrafficRules` definice.

##  <a name="OnlyAllowTrafficTo"></a>OnlyAllowTrafficTo Element
`OnlyAllowTrafficTo` Element popisuje kolekci cílové koncové body a rolí, které mohou komunikovat s nimi. Můžete určit více `OnlyAllowTrafficTo` uzlů.

##  <a name="Destinations"></a>Element cíle
`Destinations` Element popisuje kolekci RoleEndpoints, než můžete oznamovat s.

##  <a name="RoleEndpoint"></a>RoleEndpoint Element
`RoleEndpoint` Element popisuje koncový bod na roli a povolit komunikaci s. Můžete určit více `RoleEndpoint` elementů, pokud existuje více než jeden koncový bod v roli.

| Atribut      | Typ     | Popis |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Povinná hodnota. Název koncového bodu pro povolení přenosu.|
| `roleName`     | `string` | Povinná hodnota. Název webovou roli na umožňují komunikaci s.|

## <a name="allowalltraffic-element"></a>AllowAllTraffic Element
`AllowAllTraffic` Element je pravidlo, které umožní komunikovat s koncovými body definované ve všech rolí `Destinations` uzlu.

##  <a name="WhenSource"></a>WhenSource Element
`WhenSource` Element popisuje kolekci rolí, než může komunikovat s koncovými body definované v `Destinations` uzlu.

| Atribut | Typ     | Popis |
| --------- | -------- | ----------- |
| `matches` | `string` | Povinná hodnota. Určuje pravidlo platilo při povolení komunikace. Jediná platná hodnota je aktuálně `AnyRule`.|
  
##  <a name="FromRole"></a>FromRole Element
`FromRole` Element určuje role, které mohou komunikovat s koncové body definované v `Destinations` uzlu. Můžete určit více `FromRole` elementů, pokud existuje víc než jedné role, který může komunikovat s koncových bodů.

| Atribut  | Typ     | Popis |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Povinná hodnota. Název role, ze kterého chcete povolit komunikaci.|

## <a name="see-also"></a>Viz také
[Cloudové služby (klasické) definice schématu](schema-csdef-file.md)