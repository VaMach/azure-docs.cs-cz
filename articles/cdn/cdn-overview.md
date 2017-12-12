---
title: "CDN Azure – Přehled | Dokumentace Microsoftu"
description: "Zjistěte, co je Síť pro doručování obsahu (CDN) Azure, a jak ji používat k doručování širokopásmového obsahu díky ukládání objektů blob a statického obsahu do mezipaměti."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: 866e0c30-1f33-43a5-91f0-d22f033b16c6
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/10/2017
ms.author: v-semcev
ms.openlocfilehash: cdcf07b6af2bd915345361c0bda2dcd9abe5486e
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="overview-of-the-azure-content-delivery-network"></a>Přehled sítě Azure Content Delivery Network

Azure Content Delivery Network (CDN) ukládá statický webový obsah do mezipaměti na strategicky umístěných místech, a tak poskytuje maximální propustnost pro bezpečné doručování obsahu uživatelům. CDN nabízí vývojářům globální řešení pro rychlé doručování širokopásmového obsahu díky ukládání obsahu do mezipaměti na fyzických uzlech po celém světě. 

> [!NOTE]
> Tento článek popisuje Azure CDN, jak funguje a funkce jednotlivých produktů Azure CDN. Pokud chcete tyto informace přeskočit a zobrazit kurz vytvoření koncového bodu CDN, přečtěte si téma [Začínáme s Azure CDN](cdn-create-new-endpoint.md). Pokud chcete zobrazit aktuální seznam umístění uzlů CDN, přečtěte si téma [Lokality POP v Azure CDN](cdn-pop-locations.md).

Mezi výhody používání sítě CDN k ukládání webových prostředků do mezipaměti patří:

* Lepší výkon a zdokonalené uživatelské prostředí pro koncové uživatele, zejména při používání aplikací, ve kterých se k načtení obsahu vyžaduje vícenásobný přenos.
* Velké škálování pro lepší zvládání náhlého vysokého zatížení, například při uvádění produktu na trh.
* Distribuce uživatelských požadavků a poskytování obsahu přímo ze serverů Edge pro zajištění nižšího provozu ke zdroji.

## <a name="how-it-works"></a>Jak to funguje
![Přehled CDN](./media/cdn-overview/cdn-overview.png)

1. Uživatel (Alice) požaduje soubor (také označovaný jako prostředek) pomocí adresy URL se speciálním názvem domény, například `<endpointname>.azureedge.net`. DNS přesměruje požadavek na nejvýkonnější umístění POP (Point-of-Presence), což je obvykle POP, které je geograficky nejblíže uživateli.
2. Pokud servery Edge v POP nemají soubor v mezipaměti, server Edge si ho vyžádá ze zdroje.  Zdrojem může být webová aplikace Azure, cloudová služba Azure, účet Azure Storage nebo jakýkoli veřejně přístupný webový server.
3. Zdroj vrátí soubor serveru Edge včetně volitelných hlaviček protokolu HTTP, které popisují hodnotu TTL (Time to Live) souboru.
4. Server Edge soubor uloží do mezipaměti a vrátí ho původnímu žadateli (Alici).  Soubor zůstane v mezipaměti na serveru Edge, dokud nevyprší platnost hodnoty TTL.  Pokud zdroj nezadal hodnotu TTL, je výchozí hodnota TTL sedm dní.
5. Další uživatelé mohou následně požadovat stejný soubor pomocí stejné adresy URL a budou přesměrováni na stejný POP.
6. Pokud hodnota TTL souboru nevypršela, vrátí server Edge soubor z mezipaměti. Tento proces má za následek rychlejší a rychleji reagující uživatelské prostředí.

## <a name="azure-cdn-features"></a>Funkce Azure CDN
Existují tři produkty Azure CDN: **Azure CDN Standard od společnosti Akamai**, **Azure CDN Standard od společnosti Verizon** a **Azure CDN Premium od společnosti Verizon**.  V následující tabulce jsou uvedené funkce, které jsou k dispozici u každého produktu.

|  | Akamai Standard | Verizon Standard | Verizon Premium |
| --- | --- | --- | --- |
| __Funkce a optimalizace výkonu__ |
| [Akcelerace dynamického webu](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&amp;#x2713;**  | **&amp;#x2713;** | **&amp;#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  [Akcelerace dynamického webu – adaptivní komprese obrázků](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&amp;#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  [Akcelerace dynamického webu – předběžné načtení objektů](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&amp;#x2713;**  |  |  |
| [Optimalizace streamování videa](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&amp;#x2713;**  | \* |  \* |
| [Optimalizace velkých souborů](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&amp;#x2713;**  | \* |  \* |
| [Vyrovnávání zatížení globálního serveru (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Rychlé vyprázdnění](cdn-purge-endpoint.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Předběžné načítání prostředku](cdn-preload-endpoint.md) | |**&amp;#x2713;** |**&amp;#x2713;** |
| Nastavení mezipaměti nebo hlaviček (pomocí [pravidel ukládání do mezipaměti](cdn-caching-rules.md)) |**&amp;#x2713;** |**&amp;#x2713;** | |
| Nastavení mezipaměti nebo hlaviček (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |
| [Ukládání řetězce dotazu do mezipaměti](cdn-query-string.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| Duální sada protokolů IPv4/IPv6 |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Podpora HTTP/2](cdn-http2.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| __Zabezpečení__ |
| Podpora HTTPS s koncovými body CDN |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [HTTPS pro vlastní doménu](cdn-custom-ssl.md) | |**&amp;#x2713;** |**&amp;#x2713;** |
| [Podpora vlastních názvů domén](cdn-map-content-to-custom-domain.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Geografická filtrování](cdn-restrict-access-by-country.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Ověření tokenu](cdn-token-auth.md)|  |  |**&amp;#x2713;**| 
| [Ochrana proti útoku DDoS](https://www.us-cert.gov/ncas/tips/ST04-015) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| __Analýzy a generování sestav__ |
| [Diagnostické protokoly Azure](cdn-azure-diagnostic-logs.md) | **&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Základní sestavy od Verizonu](cdn-analyze-usage-patterns.md) | |**&amp;#x2713;** |**&amp;#x2713;** |
| [Vlastní sestavy od Verizonu](cdn-verizon-custom-reports.md) | |**&amp;#x2713;** |**&amp;#x2713;** |
| [Rozšířené sestavy HTTP](cdn-advanced-http-reports.md) | | |**&amp;#x2713;** |
| [Statistiky v reálném čase](cdn-real-time-stats.md) | | |**&amp;#x2713;** |
| [Výkon hraničních uzlů](cdn-edge-performance.md) | | |**&amp;#x2713;** |
| [Výstrahy v reálném čase](cdn-real-time-alerts.md) | | |**&amp;#x2713;** |
| __Snadné použití__ |
| Snadná integrace se službami Azure, jako jsou [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service/app-service-web-tutorial-content-delivery-network.md) a [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| Správa prostřednictvím [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) nebo [prostředí PowerShellu](cdn-manage-powershell.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Přizpůsobitelný modul doručování obsahu založený na pravidlech](cdn-rules-engine.md) | | |**&amp;#x2713;** |
| Přesměrování nebo přepsání adresy URL (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |
| Pravidla mobilních zařízení (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |

\* Verizon podporuje přímé doručování velkých souborů a médií prostřednictvím Obecného doručování webu.


> [!TIP]
> Víte o funkci, kterou byste uvítali v CDN Azure?  [Sdělte nám svůj názor](https://feedback.azure.com/forums/169397-cdn)! 
> 
> 

## <a name="next-steps"></a>Další kroky
Pokud chcete začít se sítí CDN, přečtěte si téma [Začínáme s Azure CDN](cdn-create-new-endpoint.md).

Pokud jste stávající zákazník CDN, můžete nyní spravovat koncové body CDN prostřednictvím portálu [Microsoft Azure Portal](https://portal.azure.com) nebo [PowerShellu](cdn-manage-powershell.md).

Pokud chcete vidět síť CDN v akci, podívejte se na [video z konference Build 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Přečtěte si informace o automatizaci Azure CDN s [.NET](cdn-app-dev-net.md) nebo [Node.js](cdn-app-dev-node.md).

Informace o cenách najdete v tématu [Ceny služby Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).

