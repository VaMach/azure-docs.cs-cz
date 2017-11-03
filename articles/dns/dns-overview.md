---
title: "Přehled Azure DNS | Microsoft Docs"
description: "Přehled systému DNS, který je hostitelem služby v Microsoft Azure. Hostitel doménu v Microsoft Azure."
services: dns
documentationcenter: na
author: KumudD
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: kumud
ms.openlocfilehash: 890c00f3349abd52294e92d27f1b42ab38fe287a
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="azure-dns-overview"></a>Přehled Azure DNS

Systému názvů domény nebo DNS, zodpovídá za překladu (nebo vyřešení) název webu nebo služby na jeho IP adresu. Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure. Pokud svoje domény hostujete v Azure, můžete spravovat svoje DNS záznamy pomocí stejných přihlašovacích údajů, rozhraní API a nástrojů a za stejných fakturačních podmínek jako u ostatních služeb Azure. Azure DNS teď také podporuje privátní domén DNS. Další informace najdete v tématu [pomocí Azure DNS pro domény privátní](private-dns-overview.md).

![Přehled systému DNS](./media/dns-overview/scenario.png)

## <a name="features"></a>Funkce

* **Spolehlivost a výkon** -domén DNS v Azure DNS jsou hostované v Azure globální síti názvových serverů DNS. Azure DNS používá všesměrového vysílání sítě tak, aby každý dotaz DNS je zodpovězen nejbližší server DNS k dispozici. To poskytuje vysoký výkon a vysokou dostupnost vaší domény.

* **Bezproblémová integrace** – služba Azure DNS umožňuje spravovat záznamy DNS pro služeb Azure a slouží k poskytování DNS pro vaše externí prostředky. Azure DNS je integrovaná v portálu Azure a používá stejné přihlašovací údaje, fakturaci a podporu kontrakt jako jinými službami Azure.

* **Zabezpečení** – služba Azure DNS je založena na Azure Resource Manager. Jako takový výhody z funkce služby Správce prostředků například řízení přístupu na základě rolí, protokoly auditu a uzamčení prostředků. Doménách a záznamech lze spravovat prostřednictvím portálu Azure, rutin prostředí Azure PowerShell a rozhraní příkazového řádku Azure napříč platformami. Aplikace, které potřebují Automatická správa DNS může integrovat službu pomocí REST API a sady SDK.

Azure DNS aktuálně nepodporuje nákup názvů domén. Pokud chcete zakoupit domén, budete muset použít doménového registrátora názvu domény třetí strany. Registrátor obvykle účtuje malý roční poplatek. Pro správu záznamů DNS, může být domény pak hostovaný v Azure DNS. V tématu [delegování domény do Azure DNS](dns-domain-delegation.md) podrobnosti.

## <a name="pricing"></a>Ceny

Fakturace DNS je založena na počtu zónách DNS hostovaných v Azure a počet dotazů DNS. Další informace o cenách najdete na [Azure DNS ceny](https://azure.microsoft.com/pricing/details/dns/).

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy k Azure DNS, najdete v článku [Azure DNS – nejčastější dotazy](dns-faq.md).

## <a name="next-steps"></a>Další kroky

Další informace o zóny DNS a záznamy, navštivte stránky: [DNS zóny a zaznamenává přehled](dns-zones-records.md).

Zjistěte, jak [vytvořit zónu DNS](./dns-getstarted-create-dnszone-portal.md) v Azure DNS.

Informace o některých dalších klíčových [možnostech sítě](../networking/networking-overview.md) v Azure.

