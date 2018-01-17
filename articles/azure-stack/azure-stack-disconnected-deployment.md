---
title: "Rozhodnutí o Azure odpojené nasazení pro Azure zásobníku integrované systémy | Microsoft Docs"
description: "Určení plánování rozhodnutí pro nasazení na víc uzlů připojená k Azure zásobník Azure nasazení."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 8708f13109767c7cb9e4a1bf0d2797d7c4fb9a23
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Plánování rozhodnutí pro Azure zásobník Azure odpojené nasazení integrované systémy
Poté, co jste se rozhodli [jak bude zásobník Azure integrovat do cloudového prostředí hybridní](azure-stack-deployment-decisions.md), pak můžete dokončit svoje rozhodnutí o nasazení Azure zásobníku.

S odpojeném pomocí volby nasazení Azure, můžete nasadit a používat zásobník Azure bez připojení k Internetu. K odpojené nasazení jste však omezená na úložiště identity služby AD FS a model fakturace na základě kapacity. 

Tuto možnost zvolte, pokud je:
- Máte zabezpečení nebo jiná omezení, které vyžadují, abyste nasazení Azure zásobníku v prostředí, který není připojen k Internetu.
- Chcete blokovat data (včetně dat o využití) se z odesílány do Azure.
- Chcete použít Azure zásobníku čistě jako řešení privátního cloudu, který je nasazen na podnikovém intranetu a nejsou zajímají hybridní scénáře.

> [!TIP]
> V některých případech tohoto typu prostředí jsou také označovány jako "Podmořský scénář".

Odpojené nasazení neznamená výhradně, nemůžete později vaší instanci Azure zásobníku do Azure připojit pro hybridní scénáře virtuálních počítačů klienta. Znamená to, že nemáte připojení do Azure během nasazení nebo nechcete použít jako úložiště identit Azure Active Directory. Ale pokud chcete mít připojení k Azure po nasazení, bez ohledu na to, co chcete použít jako úložiště identit, měli byste vybrat připojte se k nasazení Azure možnost. 

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Funkce, které jsou zasažená nebo není k dispozici v odpojeném nasazení 
Azure zásobníku byl navržený tak, aby fungují lépe, když je připojený k Azure, takže je důležité si uvědomit, že se některé funkce a funkce, které jsou narušena nebo zcela v odpojeném režimu k dispozici. 

|Funkce|Dopad v odpojeném režimu|
|-----|-----|
|Nasazení virtuálního počítače s rozšířením DSC ke konfiguraci nasazení virtuálního počítače post|Narušen - rozšíření DSC vypadá k Internetu pro poslední verzi WMF.|
|Nasazení virtuálního počítače s rozšířením Docker ke spuštění příkazů Docker|Narušen – Docker zkontroluje Internetu na nejnovější verzi a tato kontrola se nezdaří.|
|Odkazy na dokumentaci na webu Azure Portal zásobníku|Není k dispozici – odkazy například odeslat zpětnou vazbu, pomoc, startu atd využívající internetové adresy URL nebude fungovat.|
|Výstrahy nápravy nebo zmírnění, odkazující online nápravy Průvodce|Není k dispozici – nápravy škod způsobených výstrahy odkazy využívající internetové adresy URL nebude fungovat.|
|Syndikace Marketplace – možnost a vyberte a přidejte Galerie balíčky přímo z Azure Marketplace|Není k dispozici – tato funkce vyžaduje připojení k Azure a účet služby Azure Active Directory.|
|Použití Azure Active Directory federation účty ke správě nasazení služby Azure zásobníku|Není k dispozici – tato funkce vyžaduje připojení k Azure. Místo toho je nutné použít služby AD FS s místní instancí Active Directory.|
|Zprostředkovatelé prostředků, jako je například WebApps a SQL|Není k dispozici - poskytovatelů prostředků, jako je například WebApps a SQL vyžadují přístup k Internetu pro obsah.|
|Rozhraní příkazového řádku (CLI)|Narušen – rozhraní příkazového řádku omezila funkce z hlediska ověřování a zřizování zásad služby.|
|Visual Studio – Cloud discovery|Narušen – Cloud Discovery bude zjišťovat buď jiný cloudy nebo nefungují vůbec.|
|Visual Studio – služby AD FS|Narušen – pouze pro Visual Studio Enterprise podporuje služby AD FS.
Telemetrická data|Není k dispozici – Telemetrická data pro zásobník Azure jako a jakékoliv balíčky Galerie třetích stran, které jsou závislé na telemetrická data.|
|Certifikáty|Není k dispozici – připojení k Internetu je vyžadována pro služby seznamu odvolaných certifikátů (CRL) a protokol protokolu (Online Certificate Status OSCP) v kontextu protokolu HTTPS.|
|Key Vault|Narušen – běžně používá pro Key Vault je tak, aby měl čtení tajných klíčů v době běhu aplikace. Pro tuto aplikaci musí objekt služby v adresáři. V Azure Active Directory běžní uživatelé (bez oprávnění správce) jsou ve výchozím nastavení povolené, chcete-li přidat objekty služby. Ve službě AD (pomocí služby AD FS) nejsou. Protože jeden musí vždy projít správce directory přidat všechny aplikace to mezní umístí do prostředí začátku do konce.| 

## <a name="learn-more"></a>Další informace
- Informace o případy použití, nákup, partneři a OEM výrobci hardwaru najdete v tématu [zásobník Azure](https://azure.microsoft.com/overview/azure-stack/) stránky produktu.
- Informace o plán a geografická dostupnosti pro zásobník Azure integrované systémy, najdete v dokumentu white paper: [Azure zásobník: rozšíření Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Další informace o Microsoft Azure zásobníku balení a ceny [stáhnout .pdf](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

