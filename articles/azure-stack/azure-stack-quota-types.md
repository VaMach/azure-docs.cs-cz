---
title: "Typy kvót v zásobníku Azure | Microsoft Docs"
description: "Projděte si různé kvóty typy, které jsou dostupné pro služby a prostředky v Azure zásobníku."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 8/23/2017
ms.author: erikje
ms.openlocfilehash: d9bb048ece32bf5b34e05d7459488aa0f24d0d44
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="quota-types-in-azure-stack"></a>Typy kvót v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

[Kvóty](azure-stack-plan-offer-quota-overview.md#plans) definovat omezení prostředků, které uživatel předplatné můžete zřizovat nebo využívat. Například může kvótu umožnit uživateli vytvořit až pět virtuální počítače. Každý prostředek může mít svůj vlastní typy kvót.

## <a name="compute-quota-types"></a>Výpočetní kvóty typy
| **Typ** | **Výchozí hodnota** | **Popis** |
| --- | --- | --- |
| Maximální počet virtuálních počítačů | 20 | Maximální počet virtuálních počítačů, které předplatné můžete vytvořit v tomto umístění. |
| Maximální počet jader virtuálního počítače | 50 | Maximální počet jader, které předplatné můžete vytvořit v tomto umístění (například virtuální počítač A3 má čtyři jádra). |
| Nastaví maximální počet dostupnosti | 10 | Maximální počet sad dostupnosti, které lze vytvořit v tomto umístění. |
| Nastaví maximální počet škálování virtuálních počítačů | 20 | Maximální počet sady škálování virtuálního počítače, které lze vytvořit v tomto umístění. |

> [!NOTE]
> Výpočetní kvóty nejsou vynucená v této verzi technical preview.
> 
> 

## <a name="storage-quota-types"></a>Typy kvótu úložiště
| **Položka** | **Výchozí hodnota** | **Popis** |
| --- | --- | --- |
| Maximální kapacita (GB) |500 |Celkový úložný kapacity, které mohou být spotřebovávána předplatné v tomto umístění. |
| Celkový počet účtů úložiště |20 |Maximální počet účtů úložiště, které předplatné můžete vytvořit v tomto umístění. |

## <a name="network-quota-types"></a>Typy síťových kvóty
| **Položka** | **Výchozí hodnota** | **Popis** |
| --- | --- | --- |
| Maximální počet veřejných IP adres |50 |Maximální počet veřejných IP adres, které předplatné můžete vytvořit v tomto umístění. |
| Maximální počet virtuálních sítí |50 |Maximální počet virtuálních sítí, které předplatné můžete vytvořit v tomto umístění. |
| Maximální počet brány virtuální sítě |1 |Maximální počet brány virtuální sítě (VPN Gateway), které předplatné můžete vytvořit v tomto umístění. |
| Maximální počet síťových připojení |2 |Maximální počet připojení sítě (typu point-to-point nebo site-to-site), která může vytvořit odběr pro všechny brány virtuální sítě v tomto umístění. |
| Maximální počet nástroje pro vyrovnávání zatížení |50 |Maximální počet nástroje pro vyrovnávání zatížení, které předplatné můžete vytvořit v tomto umístění. |
| Maximální počet síťových karet |100 |Maximální počet síťových rozhraní, které předplatné můžete vytvořit v tomto umístění. |
| Maximální počet skupin zabezpečení sítě |50 |Maximální počet skupin zabezpečení sítě, které předplatné můžete vytvořit v tomto umístění. |

## <a name="view-an-existing-quota"></a>Zobrazit existující kvótu
1. Klikněte na tlačítko **další služby** > **zprostředkovatelé prostředků**.
2. Vyberte službu s kvótu, kterou chcete zobrazit.
3. Klikněte na tlačítko **kvóty**a vyberte kvóty, které chcete zobrazit.

## <a name="next-steps"></a>Další kroky
[Další informace o plánech, nabídky a kvóty.](azure-stack-plan-offer-quota-overview.md)

[Vytvoření kvóty při vytváření plánu.](azure-stack-create-plan.md)
