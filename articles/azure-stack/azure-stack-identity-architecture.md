---
title: "Architektura identity pro Azure zásobníku | Microsoft Docs"
description: "Další informace o architektuře Identity, které můžete použít s Azure zásobníku."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/28/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 7f2ec78da38f3c97fde810fb8fc965cfbb6fda08
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Architektura identity pro Azure zásobníku
Než si zvolíte zprostředkovatele identity pro použití s Azure zásobníku, pochopit důležité rozdíly mezi možnosti služby Azure Active Directory (Azure AD) a Active Directory Federated Services (AD FS). 

## <a name="capabilities-and-limitations"></a>Možnosti a omezení 
Možnosti, včetně podpory pro více klientů můžete omezit zprostředkovatele identity, které zvolíte. 

  

|Funkce nebo scénáře        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|Připojení k Internetu     |Ano       |Nepovinné|
|Podpora pro více klientů     |Ano       |Ne      |
|Syndikace Marketplace.       |Ano       |Ano – vyžaduje použití [offline syndikace Marketplace](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) nástroj.|
|Podpora služby Active Directory Authentication Library (ADAL) |Ano |Ano|
|Podpora pro nástroje, jako je rozhraní příkazového řádku Azure (CLI), Visual Studio (VS) a prostředí PowerShell  |Ano |Ano|
|Vytvořit objekty služby prostřednictvím portálu     |Ano |Ne|
|Vytvořit objekty služby s certifikáty      |Ano |Ano|
|Vytvořit objekty služby s tajné klíče (klíče)    |Ano |Ne|
|Aplikace můžete použít službu grafu           |Ano |Ne|
|Aplikace můžete použít zprostředkovatele identity pro přihlášení |Ano |Ano – vyžaduje aplikace vytvořit federaci s místní služby AD FS. |

## <a name="topologies"></a>Topologie
Následující části obsahují informace o topologiích identity, které můžete použít.

### <a name="azure-ad--single-tenant"></a>Azure AD – jednoho klienta 
Když nainstalujete zásobník Azure a Azure AD, Azure zásobníku ve výchozím nastavení používá topologii jednoho klienta. 

Topologie jednoho klienta je užitečné, když:
- Všichni uživatelé jsou součástí stejné klienta.
- Poskytovatel služeb hostitelem instance Azure zásobníku v organizaci.  

![Azure zásobníku topologie pomocí topologii jednoho klienta Azure AD](media/azure-stack-identity-architecture/single-tenant.png)

S touto topologií:
- Azure zásobníku zaregistruje všechny aplikace a služby pro stejnou službou Azure AD klienta adresáře. 
- Azure zásobníku ověřuje uživatele a aplikací z adresáře, včetně tokeny. 
- Identity pro správce (operátorům cloudu) a uživatelů klienta jsou ve stejném adresáři klientovi. 
- Chcete-li uživatel z jiného adresáře pro přístup k tomuto prostředí zásobníku Azure, musíte [pozvat uživatele jako Host](azure-stack-identity-overview.md#guest-users) k adresáři klienta.  

### <a name="azure-ad--multi-tenant"></a>Azure AD – více klientů
Operátoři cloudu můžete nakonfigurovat zásobník Azure pro povolení přístupu k aplikacím klienty z jednoho nebo více organizací. Uživatelé přístup k aplikacím prostřednictvím portálu user portal. V této konfiguraci portálu pro správu (používá operátor cloudu) je omezená na uživatele z jednoho adresáře. 

Topologie s více klienty je užitečné, když:
- Poskytovatel služeb chce uživatelům z několik organizací, které umožní přístup k Azure zásobníku.

![Azure zásobníku topologie, topologie s více klienty pomocí Azure AD](media/azure-stack-identity-architecture/multi-tenant.png)

S touto topologií:
- Přístup k prostředkům by měla být na základě za organizace. 
- K udělení přístupu k prostředkům pro uživatele, kteří jsou mimo jejich organizace by neměla mít uživatelé z jedné organizace.  
- Identity pro správce (operátorům cloudu) může být v samostatné directory klienta než identity pro uživatele. Toto oddělení zajišťuje izolaci účet na úrovni zprostředkovatele identity. 
 
### <a name="ad-fs"></a>AD FS  
Topologie služby AD FS je nutné, když je splněna jedna z následujících podmínek:
- Azure zásobníku se nepřipojí k Internetu.
- Azure zásobníku může připojit k Internetu, ale chcete použít pro zprostředkovatele identity služby AD FS.
  
![Azure zásobníku topologie pomocí služby AD FS](media/azure-stack-identity-architecture/adfs.png)

S touto topologií:
- Pro podporu použití v produkčním prostředí, je potřeba integrovat předdefinované instanci služby Azure zásobníku AD FS s existující instancí služby AD FS zálohovaný ve službě Active Directory (AD) přes důvěryhodnost federace. 
- Službu grafu v zásobníku Azure můžete integrovat s vaší stávající AD.  Můžete také použít protokol OData na základě rozhraní Graph API služby, který podporuje rozhraní API, které jsou konzistentní s Azure AD Graph API.  

  Pro interakci s služby AD, rozhraní Graph API vyžaduje pověření uživatele ze služby AD, které mají oprávnění jen pro čtení služby AD. 
  - Integrované služby AD FS je založena na Server 2016. 
  - Vaše AD FS a musí být založený na Server 2012 nebo novější.  
  
  Mezi služby AD a integrované služby AD FS interakce nejsou omezena na OpenID Connect a můžete použít libovolný protokol pro vzájemně podporované.  
  - Uživatelské účty se vytváří a spravují v místní AD.
  - Objekty služby a registrace pro aplikace se spravují v předdefinované AD.



## <a name="next-steps"></a>Další postup
- [Identity – přehled](azure-stack-identity-overview.md)   
- [Integrace Datacenter - Identity](azure-stack-integrate-identity.md)