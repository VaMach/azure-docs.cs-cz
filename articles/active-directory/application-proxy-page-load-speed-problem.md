---
title: "Aplikace Proxy aplikací trvá příliš dlouho načíst | Microsoft Docs"
description: "Řešení potíží s výkonem načítání stránky s Azure AD Application Proxy"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ce462c90746e6af0dc201686557121665b82b93d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Aplikace Proxy aplikací trvá příliš dlouho zatížení

Tento článek vám pomůže lépe porozumět proč Azure AD Application Proxy aplikace může trvat dlouhou dobu zatížení, a můžete provést k vyřešení tohoto problému.

## <a name="overview"></a>Přehled
Pokud vaše aplikace pracují, ale zobrazí dlouho latence, může být některé dílčí vylepšení v topologii sítě, můžete zvážit urychlit. Vyhodnocení topologiemi, najdete v článku [sítě aspekty dokumentu](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Pokud nemáte vám tyto aspekty, bohužel mít nemáme aktuálně Další doporučení pro optimalizaci výkonu. Jak služba Proxy aplikace zasahuje do více datových center, které mohou být blíž, můžete začít přímo zobrazíte zlepšení latence. Pokud chcete zobrazit úplný seznam datových center Azure, uvidíte [latence zkušební stránku](http://www.azurespeed.com/Azure/Latency). 

Datových centrech s Proxy aplikace služby lze najít pomocí [nástroj pro testování porty konektor](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Zpětná vazba týkající se umístění Proxy aplikací datových center 
Může být Azure datových center, které zatím neobsahují Proxy aplikace, ale vedlo ke zlepšování skvělé latence pro vás. Datovém centru umístění v < aadapfeedback@microsoft.com > , můžeme použít svůj názor na plánu, jak jsme rozbalte.

Jsme práce na některé další možnosti, které pomáhají zlepšit latenci pro klienty, kteří aktuálně najdete v části dlouho latenci a nezapomeňte sdílet dokumentaci, jakmile je k dispozici.

## <a name="next-steps"></a>Další kroky
[Práce s existující místní proxy servery](application-proxy-working-with-proxy-servers.md)
