---
title: "Najít cloudu nespravované aplikace s Cloud App Discovery v Azure Active Directory | Microsoft Docs"
description: "Poskytuje informace o hledání a Správa aplikací pomocí Cloud App Discovery, jaké jsou výhody a jak to funguje."
services: active-directory
keywords: "cloud app discovery,. Správa aplikací"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 35b898aa3c03aeef914a7df574ac65a22a6c7bec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Najít cloudu nespravované aplikace s Cloud App Discovery
## <a name="summary"></a>Souhrn

Cloud App Discovery je funkce služby Azure Active Directory Premium, můžete zjišťovat nespravované cloudové aplikace se používaných lidmi ve vaší organizaci. V rámci moderní firmy IT oddělení nemají často informace všech cloudových aplikací, které členové své organizace používat ke své práci. Je snadno zjistit, proč by správci mají obavy o neoprávněný přístup k podnikovým datům, možné úniku a další bezpečnostní rizika. Tento nedostatek povědomí o můžete nastavit, vytváření plánu pro práci s těchto rizik zabezpečení pravděpodobně složitý.

> [!TIP] 
> Podívejte se na vylepšení k Cloud App Discovery v Azure Active Directory (Azure AD), které jsou vylepšit určením [integraci s Microsoft Cloud App Security](https://portal.cloudappsecurity.com).

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


## <a name="next-steps"></a>Další kroky
* [Cloud App Discovery zabezpečení a důležité informace o ochraně osobních údajů](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Nastavení registru Cloud App Discovery na Proxy servery s vlastní porty](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Protokol změn agenta cloud App Discovery.](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

