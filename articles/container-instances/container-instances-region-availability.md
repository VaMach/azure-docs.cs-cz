---
title: "Azure instancí kontejnerů oblasti a dostupnosti zdrojů | Dokumentace Azure"
description: "Zjistit, které oblasti Azure podporu nasazení kontejner instancí a omezení procesoru a paměti pro tyto instance."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 2b9b1b864bbfd73383759212dd7d91f8e4941544
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Dostupnost v oblastech Azure kontejner instancí

Verzi Preview instancí kontejnerů Azure jsou k dispozici v následujících oblastech s zadaný procesoru a paměti omezení.

| Umístění | Operační systém | Procesor | Paměť (GB) |
| -------- | -- | :---: | :-----------: |
| Západní Evropa, západní USA, východní USA | Linux | 2 | 7 |
| Západní Evropa, západní USA, východní USA | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Dostupnost prostředků

Kontejner instancí v rámci těchto limitů prostředků vytvořili podléhají dostupnosti v rámci nasazení oblasti. Při oblast je přetížen, může dojít k selhání při nasazování instancí.

Pro zmírnění selhání nasazení, zkuste nasazení instancí s nižší procesoru a paměti nastavení, nebo zkuste nasazení později.

## <a name="next-steps"></a>Další kroky

Další informace o řešení potíží s nasazením instance kontejneru najdete v tématu [řešení potíží s nasazením s instancemi Azure kontejneru](container-instances-troubleshooting.md).