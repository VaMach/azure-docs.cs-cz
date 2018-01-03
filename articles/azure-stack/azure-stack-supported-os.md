---
title: "Hostovaný operační systém podporující zásobník Azure | Microsoft Docs"
description: "V zásobníku Azure lze tyto hostované operační systémy."
services: azure-stack
documentationcenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: JeffGoldner
ms.openlocfilehash: af524b5f40fb79648d91deb54186826902aa8cd2
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Hostované operační systémy podporované v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

## <a name="windows"></a>Windows
Zásobník Azure podporuje následující Windows hostované operační systémy. Obrázky v Marketplace nejsou k dispozici ke stažení do protokolů Azure. Bitových kopií systému Windows klienta nejsou k dispozici na webu Marketplace.

Během nasazení zásobník Azure zajistí, že je vhodná verze agenta hosta vložit do bitové kopie.

| Operační systém | Popis | Vydavatel | OS Type | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64bitová verze | Microsoft | Windows | Datacentrum |
| Windows Server 2012 | 64bitová verze | Microsoft | Windows | Datacentrum |
| Windows Server 2012 R2 | 64bitová verze | Microsoft | Windows | Datacentrum |
| Windows Server 2016 | 64bitová verze | Microsoft | Windows | Datacenter, Datacenter základní datového centra s kontejnery |
| Windows 7 | 64-bit, Pro a Enterprise | Microsoft | Windows | Ne |
| Windows 8.1 | 64-bit, Pro a Enterprise | Microsoft | Windows | Ne |
| Windows 10 | 64-bit, Pro a Enterprise | Microsoft | Windows | Ne |

## <a name="linux"></a>Linux

Linuxových distribucích tady zahrnují nezbytné Windows Azure Linux Agent (WALA). 

> [!NOTE]
> Bitové kopie vytvořené s WALA verze starší než 2.2.3 *není* nepodporuje a není pravděpodobné, že chcete nasadit.

| Distribuce | Popis | Vydavatel | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Kontejner Linux |  64bitová verze | CoreOS | Stable |
| Na základě centOS 6.9 | 64bitová verze | Podvodný Wave | Ano |
| Distribuce založené na CentOS 7.3 | 64bitová verze | Podvodný Wave | Ano |
| Na základě centOS 7.4 | 64bitová verze | Podvodný Wave | Ano |
| Debian 8 "Klára" | 64bitová verze | credativ |  Ano |
| Debian 9 "Stretch" | 64bitová verze | credativ | Ano |
| Oracle Linux | 64bitová verze | Oracle | Ne |
| Red Hat Enterprise Linux 7.x | 64bitová verze | Red Hat | Ne |
| SLES 11SP4 | 64bitová verze | SUSE | Ano |
| SLES 12SP3 | 64bitová verze | SUSE | Ano |
| Ubuntu 14.04-LTS | 64bitová verze | Canonical | Ano |
| Ubuntu 16.04-LTS | 64bitová verze | Canonical | Ano |

Další Linuxových distribucích může být podporovaná v budoucnu.




