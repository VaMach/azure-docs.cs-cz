---
title: "Podporované hostované operační systémy pro Azure zásobníku | Microsoft Docs"
description: "V zásobníku Azure lze tyto hostované operační systémy."
services: azure-stack
documentationcenter: 
author: Brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: c9f5bee38772623fb79fa081be8eaece981cc8ab
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Hostované operační systémy podporované v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

## <a name="windows"></a>Windows
Zásobník Azure podporuje následující Windows hostované operační systémy. Obrázky v Marketplace nejsou k dispozici ke stažení do protokolů Azure. Bitových kopií systému Windows klienta nejsou k dispozici na webu Marketplace.

Během nasazení zásobník Azure zajistí, že je vhodná verze agenta hosta vložit do bitové kopie.

| Operační systém | Popis | Vydavatel | OS Type | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64 bitů | Microsoft | Windows | Datové centrum |
| Windows Server 2012 | 64 bitů | Microsoft | Windows | Datové centrum |
| Windows Server 2012 R2 | 64 bitů | Microsoft | Windows | Datové centrum |
| Windows Server 2016 | 64 bitů | Microsoft | Windows | Datacenter, Datacenter základní datového centra s kontejnery |
| Windows 7 | 64-bit, Pro a Enterprise | Microsoft | Windows | Ne |
| Windows 8.1 | 64-bit, Pro a Enterprise | Microsoft | Windows | Ne |
| Windows 10 *(viz poznámka 1)* | 64-bit, Pro a Enterprise | Microsoft | Windows | Ne |

***Poznámka 1:****k nasazení Windows 10 klientské operační systémy v zásobníku Azure, musí mít [Windows licence vázané na uživatele](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) nebo nakupovat prostřednictvím kvalifikovaný hostitele víceklientské ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)).* 


## <a name="linux"></a>Linux

Linuxových distribucích tady zahrnují nezbytné Windows Azure Linux Agent (WALA).

> [!NOTE]   
> Bitové kopie vytvořené s WALA verze starší než 2.2.3 *není* nepodporuje a není pravděpodobné, že chcete nasadit. Některé verze agenta WALA se ví, není funkce na virtuálních počítačích Azure zásobníku, včetně verze 2.2.12 a 2.2.13.


| Distribuce | Popis | Vydavatel | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Kontejner Linux |  64 bitů | CoreOS | Stable |
| Na základě centOS 6.9 | 64 bitů | Podvodný Wave | Ano |
| Distribuce založené na CentOS 7.3 | 64 bitů | Podvodný Wave | Ano |
| Na základě centOS 7.4 | 64 bitů | Podvodný Wave | Ano |
| Debian 8 "Klára" | 64 bitů | credativ |  Ano |
| Debian 9 "Stretch" | 64 bitů | credativ | Ano |
| Oracle Linux | 64 bitů | Oracle | Ne |
| Red Hat Enterprise Linux 7.x | 64 bitů | Red Hat | Ne |
| SLES 11SP4 | 64 bitů | SUSE | Ano |
| SLES 12SP3 | 64 bitů | SUSE | Ano |
| Ubuntu 14.04-LTS | 64 bitů | Canonical | Ano |
| Ubuntu 16.04-LTS | 64 bitů | Canonical | Ano |

Další Linuxových distribucích může být podporovaná v budoucnu.
