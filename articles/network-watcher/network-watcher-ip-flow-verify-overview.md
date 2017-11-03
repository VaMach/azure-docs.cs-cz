---
title: "Úvod do IP toku ověřit v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled IP sledovací proces sítě ověřte tok funkce"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b2ad45e76320c59d18dce7b39166679801b4170a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Úvod do IP toku ověřit v sledovací proces sítě Azure

Tok IP ověřte kontroluje, zda paket povolený nebo zakázaný do nebo z virtuálního počítače na základě informací o 5 řazené kolekce členů. Tyto informace se skládá z směr, protokol, místní IP, vzdálené IP, místního portu a vzdáleného portu. Pokud paketu je zakázané skupiny zabezpečení, je vrácen název pravidla, které odepřen paketu. Zatímco můžete vybrat všechny zdrojové i cílové adresy IP, tato funkce vám pomůže správci rychle diagnostikovat problémy s připojením z nebo na Internet a z nebo na místním prostředí.

Tok IP ověřte cílem síťové rozhraní virtuálního počítače. Tok přenosů dat je pak ověřit podle nakonfigurovaného nastavení do nebo z rozhraní sítě. Tato možnost je užitečná při potvrzení, pokud pravidla v skupinu zabezpečení sítě blokuje příchozí nebo odchozí provoz nebo z virtuálního počítače.

Instance sledovací proces sítě musí být vytvořen ve všech oblastech, které chcete spustit IP tok ověření. Sledovací proces sítě je služba, místní a může být spustili jenom s prostředky ve stejné oblasti. Tato nemá není mohla ovlivnit výsledky IP toku ověřte podle postupu spojeného s síťový adaptér bude stále vrácen.

![1][1]

## <a name="next-steps"></a>Další kroky

Najdete v následujícím článku se dozvíte, pokud je paket povolený nebo zakázaný pro konkrétní virtuální počítač prostřednictvím portálu. [Zkontrolovat, pokud provoz je povolené na virtuálním počítači s IP tok ověření pomocí portálu](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












