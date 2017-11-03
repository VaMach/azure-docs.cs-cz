---
title: "Azure Active Directory PoC Playbook přísady | Microsoft Docs"
description: "Prozkoumejte a rychle implementovat scénáře identita a správa přístupu"
services: active-directory
keywords: "Azure active directory, scénářem, testování konceptu, PoC"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: dstefan
ms.openlocfilehash: 7d43be3cbfd63b6bc5f06426e9810a37bbf3d071
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a>Ověření služby Azure Active Directory koncept Playbook složek 

## <a name="theme"></a>Motiv
Azure AD poskytuje řešení identit a přístupu ve více oblastech v podnikové síti. Jsme klasifikovat scénáře v těchto oblastech: 

* [Velký počet aplikací, jednu identitu](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [Zvýšit zabezpečení](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [Škálování s Self Service](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

Definování motivu na rámce PoC umožňuje zaměřit úsilí chodem s obchodními cíli, což často jsou aktivačních událostí zájem testování konceptu na prvním místě. 

## <a name="environment"></a>Prostředí

Je důležité určit podrobnosti o prostředí, kde bude poskytovat fázi testování koncepce. V ideálním případě můžete vytvořit při ji po dokončení fázi testování koncepce. Byste měli najít rovnováhu mezi ho jako skutečné nejblíže a nároky na omezení nebo další důležité informace o cílovém prostředí je zásadní. Typické prostředí pro testovací prostředí jsou:
* **Produkční:** scénáře budou provedeny v prostředí za provozu a nasazené cloudové služby společnosti Microsoft (produkční AD, Office 365, Azure AD klienta nebo jednotného přihlašování k řešení). 
* **Uživatel přijetí testování uživateli nebo vývojová prostředí:** máte infrastruktury testovací (paralelní AD a potenciálně Azure AD klienta nebo jednotného přihlašování k řešení) s testovací data, která se podobá produkční. Testovací prostředí je obvykle sdílet mezi více projektů v podnikové síti.

Většina scénářů v této příručce jsou sčítání ve své podstatě. V důsledku toho je lze nasadit v provozním klientovi bez ovlivnění uživatelé mimo fázi testování koncepce. V tomto dokumentu jsme se volání na scénáře, které by neměl mít vliv na úrovni klienta. V takových případech můžete chtít zvážit mimo produkční prostředí. 


## <a name="target-users"></a>Cíloví uživatelé

Je důležité k určení cílové sady uživatelů, kteří budou uplatňovat scénáře, zejména v případě, že prostředí je produkčním i testovacím. Kategorie uživatelů cíl pro testování koncepce jsou:
* **Pilotní uživatelé:** skutečné uživatelů v rámci prostředí, který bude používat řešení s účtem, používají pro své každodenní úlohy funkce
* **Testovací uživatele:** testovací účty, které jsou vytvořeny v prostředí 

Většina scénářů v této příručce můžete provést podle uživatelů pilotního nasazení. V tomto dokumentu jsme se být volání si důležité informace o cílové uživatele v případě potřeby.


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]