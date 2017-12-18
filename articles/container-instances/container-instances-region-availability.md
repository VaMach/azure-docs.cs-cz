---
title: "Dostupnost Azure Container Instances v oblastech a dostupnost prostředků"
description: "Zjistěte, které oblasti Azure podporují nasazování instancí kontejnerů a jaká jsou omezení těchto instancí z hlediska procesoru a paměti."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 08/31/2017
ms.author: marsma
ms.openlocfilehash: ace4eb6b284f2c1b2caeb54c1d686e68cacb1725
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
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