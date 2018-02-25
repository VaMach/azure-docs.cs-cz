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
ms.date: 02/22/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: 3eceb740b8115d2eaca517017f6158744d6e8e58
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Hostované operační systémy podporované v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

## <a name="windows"></a>Windows
Azure zásobníku podporuje hostované operační systémy Windows, které jsou uvedeny v následující tabulce: obrázky v Marketplace nejsou k dispozici ke stažení pro Azure zásobník. Bitových kopií systému Windows klienta nejsou k dispozici na webu Marketplace.

Během nasazení vloží Azure zásobníku vhodná verze agenta hosta do bitové kopie.

| Operační systém | Popis | Vydavatel | OS Type | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64 bitů | Microsoft | Windows | Datové centrum |
| Windows Server 2012 | 64 bitů | Microsoft | Windows | Datové centrum |
| Windows Server 2012 R2 | 64 bitů | Microsoft | Windows | Datové centrum |
| Windows Server 2016 | 64 bitů | Microsoft | Windows | Datacenter, Datacenter základní datového centra s kontejnery |
| Windows 10 *(viz poznámka 1)* | 64-bit, Pro a Enterprise | Microsoft | Windows | Ne |

***Poznámka 1:****k nasazení Windows 10 klientské operační systémy v zásobníku Azure, musí mít [Windows licence vázané na uživatele](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) nebo nakupovat prostřednictvím kvalifikovaný hostitele víceklientské ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)).* 


## <a name="linux"></a>Linux

Linuxových distribucích tady zahrnují nezbytné Windows Azure Linux Agent (WALA).

> [!NOTE]   
> Bitové kopie vytvořené s WALA verze starší než 2.2.3 *není* nepodporuje a není pravděpodobné, že chcete nasadit. Některé verze agenta WALA se ví, není funkce na virtuálních počítačích Azure zásobníku, včetně verze 2.2.12 a 2.2.13.
>
> [init cloudu](https://cloud-init.io/) je podporován pouze na Ubuntu distribuce v zásobníku Azure.

| Distribuce | Popis | Vydavatel | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Kontejner Linux |  64 bitů | CoreOS | Stable |
| Na základě centOS 6.9 | 64 bitů | Podvodný Wave | Ano |
| Na základě centOS 7.4 | 64 bitů | Podvodný Wave | Ano |
| Debian 8 "Klára" | 64 bitů | credativ |  Ano |
| Debian 9 "Stretch" | 64 bitů | credativ | Ano |
| Red Hat Enterprise Linux 7.x (čekání na schválení) | 64 bitů | Red Hat | Ne |
| SLES 11SP4 | 64 bitů | SUSE | Ano |
| SLES 12SP3 | 64 bitů | SUSE | Ano |
| Ubuntu 14.04-LTS | 64 bitů | Canonical | Ano |
| Ubuntu 16.04-LTS | 64 bitů | Canonical | Ano |

Další Linuxových distribucích může být podporovaná v budoucnu.
