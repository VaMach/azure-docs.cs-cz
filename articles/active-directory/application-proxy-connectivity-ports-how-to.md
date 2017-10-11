---
title: "Jak otevřít porty brány firewall, které jsou potřebné pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Zjistěte, jaké porty otevřít pro Azure AD Application Proxy fungovala správně"
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
ms.openlocfilehash: 8ecd6d7e666d362194126a4abba7a65f2c7b8b6b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Jak otevřít porty brány firewall, které jsou potřebné pro aplikaci Proxy aplikace

Pokud chcete zobrazit úplný seznam požadované porty a funkci každý port, najdete v části Požadavky [Proxy aplikace dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable). Všimněte si, že Proxy aplikace používá jenom Odchozí porty.

Můžete také zkontrolovat, jestli máte všechny požadované porty otevřete tak, že otevřete [nástroj pro testování porty konektor](https://aadap-portcheck.connectorporttest.msappproxy.net/) z vaší místní sítě. Další zelené značky zaškrtnutí znamená větší odolnost proti chybám. 

## <a name="app-proxy-regions"></a>Oblasti Proxy aplikace

Pracujeme na způsob, jak umožňují vědět, která z těchto oblastí musí být pro vás. Teď Ujistěte se, že všechny jsou. Také je vyžadována bez ohledu na to, které oblasti jsou ve střed USA.

Pokud chcete mít jistotu, že tento nástroj vám dává správné výsledky, nezapomeňte:

-   Otevřete nástroj v prohlížeči ze serveru, kam jste nainstalovali konektor.

-   Zajistěte, aby všechny proxy nebo brány firewall pro vaše konektor jsou také aplikováno na tuto stránku. To lze provést v aplikaci Internet Explorer tak, že přejdete do **nastavení**  - &gt; **Možnosti Internetu**  - &gt; **připojení**  - &gt; **nastavení místní sítě**. Na této stránce se zobrazí pole "Použití Proxy serveru pro vaše místní sítě". Zaškrtněte toto políčko a uveďte adresu proxy serveru do pole "Adresa".

## <a name="next-steps"></a>Další kroky
[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)
