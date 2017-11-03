---
title: "Osvědčené postupy pro Azure App Service"
description: "Zjistěte, osvědčené postupy a řešení problémů pro službu Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: dariagrigoriu
ms.openlocfilehash: a65b50a90a67b718f2a0cdd8657194d9740b3bd4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-for-azure-app-service"></a>Osvědčené postupy pro Azure App Service
Tento článek shrnuje doporučené postupy pro používání [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Společné umístění
Při sestavování řešení například webovou aplikaci a databázi prostředků Azure jsou umístěny v různých oblastech mohou zahrnovat následující důsledky:

* Zvýší latence při komunikaci mezi prostředky
* Peněžní poplatky za odchozí data přenos mezi oblastmi uvedených na [Azure stránce s cenami](https://azure.microsoft.com/pricing/details/data-transfers).

Společné umístění ve stejné oblasti je nejvhodnější pro sestavování řešení například webovou aplikaci a účet databáze nebo úložiště používané pro udržení dat nebo obsahu prostředků Azure. Při vytváření prostředků, které byste měli jsou ve stejné oblasti Azure Pokud máte konkrétní obchodní nebo návrh důvod pro ně nejsou. Aplikace služby App Service můžete přesunout do stejné oblasti jako vaše databáze s využitím [služby App Service funkce klonování](app-service-web-app-cloning.md) aktuálně dostupné pro plán aplikační služby Premium aplikace.   

## <a name="memoryresources"></a>Pokud aplikace vyžaduje více paměti, než se očekávalo
Pokud jste si všimli spotřebovává více paměti, než se očekávalo, jak je indikován při monitorování aplikace nebo služby doporučení zvažte [aplikace služby Automatické opravy funkce](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Jednu z možností pro funkci Automatické opravy trvá vlastní akce podle prahové hodnoty paměti. Akce span spektra z e-mailová oznámení k vyšetřování prostřednictvím výpis stavu paměti pro zmírnění dopadů na místě ve recykluje pracovní proces. Automatické opravy lze nakonfigurovat pomocí souboru web.config a prostřednictvím popisný uživatelské rozhraní dle instrukcí v tomto příspěvku na blogu pro [rozšíření lokality podporu služby aplikace](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Když aplikace spotřebovávat více procesorů, než se očekávalo
Pokud jste Všimněte si, že aplikace využívá více procesorů než očekávané nebo vyskytne opakuje vzroste využití procesoru indikován monitorování nebo doporučení zvažte vertikálním navýšení kapacity nebo škálování plán služby App Service. Pokud vaše aplikace statefull, vertikálním navýšení kapacity je jedinou možností, zatímco, pokud je vaše aplikace bezstavové, škálování na více systémů vám poskytne větší flexibilitu a vyšší potenciální škálování. 

Další informace o "bezstavové" aplikace "statefull" vs můžete přehrát toto video: [plánování škálovatelné začátku do konce vícevrstvé aplikace na webové aplikace Microsoft Azure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Další informace o možnostech škálování a automatické škálování služby App Service najdete v tématu: [škálování webové aplikace v Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>Když vyčerpání prostředků soketů
Obvyklým důvodem vyčerpává odchozí připojení TCP je použití knihoven klienta, které nejsou implementované znovu použít připojení TCP nebo v případě protokolu vyšší úrovni jako je například – udržování připojení HTTP není využít. Přečtěte si dokumentaci pro každou z knihoven odkazuje aplikace, které váš plán služby App Service k zajištění jejich nakonfigurován nebo získat přístup v kódu pro efektivní opakované použití odchozí připojení. Také podle pokynů uvedených knihovna dokumentace pro správné vytvoření a vydání nebo čištění, aby se zabránilo úniku připojení. Tato vyšetřování knihovny klienta se může být zmírnit dopad průběh škálování na více instancí.  

## <a name="appbackup"></a>Pokud vaše aplikace Zálohování spustí selhání
Dva nejčastější příčiny, proč se nezdaří zálohování aplikace: úložiště nejsou platné nastavení a konfiguraci databáze je neplatný. Tyto chyby obvykle dojít, když jsou změny zdrojů úložiště nebo databáze nebo změny jak získat přístup k tyto prostředky (například pověření aktualizované vybraná v nastavení zálohování databáze). Zálohování obvykle spouštět podle plánu a vyžadují přístup k úložišti (pro výstup zálohované soubory) a databází (pro kopírování a načítání obsahu, které mají být zahrnuty do zálohování). Výsledek neúspěšně snaží o přístup buď tyto prostředky by chyby konzistentní zálohování. 

Při selhání zálohování, přečtěte si poslední výsledky pochopit, jaký typ selhání se děje. V případě selhání úložiště přístup zkontrolujte a aktualizujte nastavení úložiště používané v konfiguraci zálohování. V případě selhání přístupu k databázi zkontrolujte a aktualizovat vaše připojení řetězce jako součást nastavení aplikace; přejděte k aktualizaci konfigurace zálohování správně zahrnout požadované databází. Další informace o zálohování aplikace najdete [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md) dokumentaci.

## <a name="nodejs"></a>Když se nasadí nové aplikace Node.js do Azure App Service
Výchozí konfigurace Azure App Service pro aplikace Node.js se má nejlépe vyhovovat potřebám nejčastěji používané aplikace. Pokud konfigurace pro aplikaci Node.js by těžit z přizpůsobené ladění pro zlepšení výkonu nebo optimalizovat využití prostředků procesoru a paměti nebo síťových prostředků, může zkontrolovat Naše osvědčené postupy a řešení potíží. V tomto článku dokumentace popisuje nastavení modulu iisnode možná muset nakonfigurovat pro vaši aplikaci Node.js, popisuje různé scénáře nebo že vaše aplikace může být čelí a ukazuje, jak tyto problémy řeší problémy: [osvědčených postupů a Průvodci odstraňováním potíží uzlu aplikace v Azure App Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   

