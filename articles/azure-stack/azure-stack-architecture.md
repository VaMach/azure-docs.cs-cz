---
title: "Architektura Microsoft Azure zásobníku Development Kit | Microsoft Docs"
description: "Zobrazte Architektura Microsoft Azure zásobníku Development Kit."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e8ff17e069bd9237bf06cd79b4c222c50a999eda
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Architektura Microsoft Azure zásobníku Development Kit

*Platí pro: Azure zásobníku Development Kit*

Azure zásobníku Development Kit je jedním uzlem nasazení Azure zásobníku. Všechny součásti jsou nainstalovány v virtuální počítače běžící na jeden hostitelský počítač. 

## <a name="logical-architecture-diagram"></a>Logická architektura diagram
Následující diagram znázorňuje Logická architektura sadě zásobník Azure pro vývoj a jeho součástí.

![](media/azure-stack-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Role virtuálních počítačů
V sadě Azure zásobníku development kit nabízí služby pomocí následující virtuální počítače na hostiteli:

| Name (Název) | Popis |
| ----- | ----- |
| **AzS ACS01** | Služba Azure zásobníku úložiště.|
| **AzS ADFS01** | Služba Active Directory Federation Services (ADFS).  |
| **AzS BGPNAT01** | Okraj směrovače a poskytuje možnosti NAT a VPN Azure zásobníku. |
| **AzS CA01** | Certifikační autorita služby pro služby role Azure zásobníku.|
| **AzS DC01** | Službě Active Directory, DNS a DHCP services pro Microsoft Azure zásobníku.|
| **AzS ERCS01** | Konzola nouzový obnovení virtuálního počítače. |
| **AzS GWY01** | Hraniční brána službami, jako je připojení site-to-site VPN pro sítě klienta.|
| **AzS NC01** | Síťový adaptér, který spravuje zásobník Azure síťové služby.  |
| **AzS SLB01** | Vyrovnávání multiplexor služby v Azure zásobníku pro klienty a služby infrastruktury Azure zásobníku zatížení.  |
| **AzS SQL01** | Vnitřní datové úložiště pro role infrastruktury Azure zásobníku.  |
| **AzS WAS01** | Portál pro správu Azure zásobníku a služby Správce prostředků Azure.|
| **AzS WASP01**| Portál Azure zásobník uživatele (klientů) a služby Správce prostředků Azure.|
| **AzS XRP01** | Řadič pro správu infrastruktury pro Microsoft Azure zásobníku, včetně poskytovateli prostředků výpočty, síť a úložiště.|


## <a name="next-steps"></a>Další kroky
[Nasazení Azure zásobníku](azure-stack-deploy.md)

[První scénáře pokusit](azure-stack-first-scenarios.md)

