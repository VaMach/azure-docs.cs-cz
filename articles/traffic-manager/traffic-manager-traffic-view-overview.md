---
title: "Přenosy dat zobrazení v Azure Traffic Manageru | Microsoft Docs"
description: "Úvod do zobrazení provozu Traffic Manageru"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 0a07ff578c6afeedc6f3806d52bfe5aef6945c04
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="traffic-manager-traffic-view"></a>Zobrazení provozu Traffic Manageru

>[!NOTE]
>Funkce zobrazení provoz v Traffic Manager je ve verzi Public Preview a nemusí mít stejnou úroveň dostupnost a spolehlivost jako verze funkce, které jsou obecné dostupnosti. Funkce není podporována, může mít omezené možnosti a nemusí být k dispozici ve všech Azure umístění. Nejaktuálnější upozornění na stav této funkce a dostupnost, zkontrolujte [Azure Traffic Manager aktualizuje](https://azure.microsoft.com/updates/?product=traffic-manager) stránky.

Traffic Manager poskytuje DNS úrovni směrování, aby vaši koncoví uživatelé jsou směrované na v pořádku koncové body založené na metodě směrování, které byla vytvořena při vytvoření profilu. To poskytuje zobrazení vaší uživatelské základny (na úrovni členitosti překladač DNs) a jejich provoz vzor Traffic Manager. Když povolíte provoz zobrazení, tyto informace zpracování poskytnout prakticky využitelné informace. 

Pomocí zobrazení provoz, můžete:
- pochopení, kde se nachází (až místní DNS překladač úrovně rozlišením) uživatelské základny.
- Zobrazit objem provozu (zjištěnými jako dotazy DNS zpracovává Azure Traffic Managerem) pocházející z těchto oblastí.
-  získáte přehled o co je reprezentativní latence, které tyto uživatele.
- Podrobné informace do vzory konkrétní přenosy ze všech těchto uživatelské základny k oblastem Azure, kde máte koncové body. 

Například můžete zobrazit provoz zjistit, které oblasti máte velký počet přenosů dat, ale trpí vyšší latence. Dále můžete tyto informace k plánování rozšíření vaší nároky na nové oblastí Azure tak, aby tito uživatelé může mít nižší latenci prostředí.

## <a name="how-traffic-view-works"></a>Jak funguje provoz zobrazení

Zobrazení provoz funguje tak, že Traffic Manager. podívejte se na příchozí dotazů přijatých za posledních sedmi dnech proti profil, který má povolení této funkce. Z těchto informací extrahuje zdrojovou IP Překladač DNS, který je pak použit jako reprezentace umístění uživatele. Tyto jsou pak seskupeny dohromady na úrovni rozlišením Překladač DNS vytvořit základní oblasti uživatele pomocí zeměpisné údaje udržované Traffic Managerem IP adres. Správce provozu se poté hledat v oblastech Azure, ke kterým dotaz byl směrován a vytvoří mapu tok provozu pro uživatele z těchto oblastí.
V dalším kroku Traffic Manager korelaci oblasti základní pro mapování oblast Azure s tabulkami intelligence latenci sítě, které udržuje pro jiný koncový uživatel sítě zjistit, které uživatelé z těchto oblastí Průměrná latence při připojování k oblastem Azure. Tyto výpočty se pak v tabulce, v úrovni místní DNS překladač IP předtím, než se zobrazí na vás. Tyto informace v pracovním postupu analytics zvoleného může zpracovat, můžete si taky stáhnout tyto informace do souboru CSV.
Pokud používáte zobrazení provoz, vám fakturují, na základě počtu datových bodů použít k vytvoření statistiky uvedené. Aktuálně je bod pouze datový typ používaný dotazů přijatých pro svůj profil Traffic Manageru. Další podrobnosti o cenách najdete na webu [Traffic Manager stránce s cenami](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Další kroky

- Další informace [fungování Traffic Manager](traffic-manager-overview.md)
- Další informace o [metody směrování provozu](traffic-manager-routing-methods.md) podporované nástrojem Traffic Manager
- Zjistěte, jak [vytvořit profil správce provozu](traffic-manager-create-profile.md)

