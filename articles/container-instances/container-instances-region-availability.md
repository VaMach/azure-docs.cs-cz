---
title: "Dostupnost Azure Container Instances v oblastech a dostupnost prostředků | Dokumentace Azure"
description: "Zjistěte, které oblasti Azure podporují nasazování instancí kontejnerů a jaká jsou omezení těchto instancí z hlediska procesoru a paměti."
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
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Dostupnost Azure Container Instances v oblastech

Během období Preview je služba Azure Container Instances dostupná v následujících oblastech s uvedenými omezeními procesoru a paměti.

| Umístění | Operační systém | Procesor | Paměť (GB) |
| -------- | -- | :---: | :-----------: |
| Západní Evropa, Západní USA, Východní USA | Linux | 2 | 7 |
| Západní Evropa, Západní USA, Východní USA | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Dostupnost prostředků

Pro instance kontejnerů vytvořené v mezích těchto omezení prostředků platí určitá omezení z hlediska dostupnosti v rámci oblasti nasazení. V případě velkého zatížení oblasti může při nasazování instancí docházet k selhání.

Pokud chcete selhání nasazení zmírnit, zkuste nasazení instancí s nižšími nastaveními procesoru a paměti nebo zkuste nasazení později.

## <a name="next-steps"></a>Další kroky

Další informace o řešení potíží s nasazením instance kontejneru najdete v tématu [Řešení potíží s nasazením pomocí Azure Container Instances](container-instances-troubleshooting.md).