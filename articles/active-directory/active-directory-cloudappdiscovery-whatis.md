---
title: "Najít cloudu nespravované aplikace s Cloud App Discovery v Azure Active Directory | Microsoft Docs"
description: "Poskytuje informace o hledání a Správa aplikací pomocí Cloud App Discovery, jaké jsou výhody a jak to funguje."
services: active-directory
keywords: "cloud app discovery,. Správa aplikací"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1d0ad06fc7eec07f8e1e0ba47121b6eec01c87df
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Najít cloudu nespravované aplikace s Cloud App Discovery
## <a name="summary"></a>Souhrn

Cloud App Discovery v Azure Active Directory teď poskytuje lepší bez agentů zjišťování používá technologii Microsoft Cloud App Security. Pokud chcete používat Cloud App Discovery, právě Přihlaste se pomocí přihlašovacích údajů Azure AD Premium P1. Tato aktualizace je dostupná pro všechny zákazníky využívající Azure AD Premium P1 bez dalších poplatků. Začínáme s článkem [nastavení ve službě Azure AD Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/cloudappdiscovery-get-started), pak vyzkoušet [Microsoft Cloud App Security](https://portal.cloudappsecurity.com/).

> [!IMPORTANT] 
> Aktuální Azure AD Cloud App Discovery zkušenosti se zjišťováním založené na agentovi je vypnuté na 5. března 2018, po jejímž uplynutí agenty bude zakázáno a data odstraněna. Proveďte akci před březnem 5. Chcete-li získat spuštěná na nové prostředí chcete zabránit přerušení služby.  
 
**Cloud App Discovery můžete:**

* Najít cloudové aplikace, který je používán a měřit toto využití počet uživatelů, objemu provozu nebo počet žádostí o webové aplikaci.
* Identifikujte uživatele, kteří používají aplikaci.
* Exportujte data pro offline analýzu.
* Nastavte pro tyto aplikace pod kontrolou IT a povolit jednotné přihlašování pro správu uživatelů.

## <a name="how-it-works"></a>Jak to funguje
1. Agenti využití aplikace jsou nainstalovány na počítače uživatelů.
2. Informace o využití aplikace zachycenou agentů je odeslána přes zabezpečené, šifrovaný kanál ke službě cloud app discovery.
3. Služba Cloud App Discovery vyhodnotí data a generuje sestavy.

![Diagram cloud App Discovery](./media/active-directory-cloudappdiscovery/cad01.png)


## <a name="next-steps"></a>Další postup
* [Cloud App Discovery zabezpečení a důležité informace o ochraně osobních údajů](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Nastavení registru Cloud App Discovery na Proxy servery s vlastní porty](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Protokol změn agenta cloud App Discovery.](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

