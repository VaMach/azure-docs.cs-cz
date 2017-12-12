---
title: Latence sestav Azure Active Directory | Microsoft Docs
description: "Další informace o dobu potřebnou pro události vytváření sestav objeví na portálu Azure"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/18/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: f4b3e59cb6cec5ac4ca771818e5eca8509a43d82
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-reporting-latencies"></a>Latence sestav Azure Active Directory.

S [reporting](active-directory-preview-explainer.md) ve službě Azure Active Directory, získáte všechny informace, které potřebujete k určení, jak je to vaše prostředí. Dobu potřebnou pro vytváření sestav dat objeví na portálu Azure se taky říká latence. 

Toto téma obsahuje informace o latenci pro všechny sestavy kategorií v portálu Azure. 


## <a name="activity-reports"></a>Sestavy aktivit

Existují dvě oblasti aktivity generování sestav:

- **Aktivity přihlašování** – informace o použití spravovaných aplikací a aktivitách přihlašování uživatelů
- **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů

Následující tabulka uvádí informace o protokolování aktivit latenci.

| Sestava | Minimální | Průměr | Maximální počet |
| :-- | --- | --- | --- |
| Protokoly auditu             | 30 minut  | 1 hodina | 2 hodiny     |
| Přihlášení               | 15 minut  | 2 hodiny * | 24 hodin  |

>[!NOTE]
> Pro některá data aktivit přihlášení pocházející z aplikací Office starší verze může zobrazení dat pro generování sestav trvat až 8 hodin. 


## <a name="security-reports"></a>Sestavy zabezpečení

Existují dvě oblasti generování sestav zabezpečení:

- **Riziková přihlášení** – Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu. 
- **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. 

Následující tabulka uvádí informace o zabezpečení sestavy latenci.

| Sestava | Minimální | Průměr | Maximální počet |
| :-- | --- | --- | --- |
| Ohrožených uživatelích          | 5 minut   | 15 minut  | 2 hodiny  |
| Rizikové přihlášení         | 5 minut   | 15 minut  | 2 hodiny  |

## <a name="risk-events"></a>Riziko události

Azure Active Directory používá algoritmy adaptivní strojového učení a heuristiky ke zjištění podezřelé akce, které se vztahují k vaší uživatelské účty. Všechny zjištěné podezřelé akce je uložený v události zavolat riziko záznamu.

Následující tabulka uvádí informace o rizikových událostech latenci.

| Sestava | Minimální | Průměr | Maximální počet |
| :-- | --- | --- | --- |
| Přihlášení z anonymních IP adres |5 minut |15 minut |2 hodiny |
| Přihlášení z neznámých míst |5 minut |15 minut |2 hodiny |
| Uživatelé s uniklé přihlašovací údaje |2 hodiny |4 hodiny |8 hodin |
| Nemožná cesta do netypických míst |5 minut |1 hodina |8 hodin  |
| Přihlášení z nakažených zařízení |2 hodiny |4 hodiny |8 hodin  |
| Přihlášení z IP adres s podezřelou aktivitou |2 hodiny |4 hodiny |8 hodin  |



## <a name="next-steps"></a>Další kroky

Pokud chcete získat další informace o protokolování aktivit na portálu Azure, najdete v části:

- [Přihlašovací aktivity sestav na portálu Azure Active Directory](active-directory-reporting-activity-sign-ins.md)
- [Sestavy auditu aktivity na portálu Azure Active Directory](active-directory-reporting-activity-audit-logs.md)

Pokud chcete získat další informace o zabezpečení sestav na portálu Azure, najdete v části:

- [Uživatelé na riziko zabezpečení sestav na portálu Azure Active Directory](active-directory-reporting-security-user-at-risk.md)
- [Sestava rizikové přihlášení na portálu Azure Active Directory](active-directory-reporting-security-risky-sign-ins.md)

Pokud chcete získat další informace o rizikových událostech, přečtěte si téma [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).
