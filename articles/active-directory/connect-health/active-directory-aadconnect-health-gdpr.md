---
title: "Azure AD Connect Health a nařízení ochrany dat obecné | Microsoft Docs"
description: "Tento dokument popisuje, jak zajistit dodržování předpisů GDPR službou Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2018
ms.author: billmath
ms.openlocfilehash: b9a0b9027bbead00300040186e453933b3a7f46b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="gdpr-compliance-and-azure-ad-connect-health"></a>GDPR dodržování předpisů a Azure AD Connect Health 

[Obecné Data Protection nařízení (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm) je data protection a o ochraně osobních údajů zákon Evropské unie (EU). GDPR ukládá nové pravidel na společnosti, organizace státní správy, bez zisku a jinými organizacemi, které nabízejí zboží a služeb na osoby ve EU, nebo že shromažďovat a analyzovat data svázané s obyvatele Evropské unie. 

Produkty a služby Microsoftu jsou dnes, které vám pomůžou GDPR požadavkům. Další informace o zásadách Microsoft Privacy na [Centrum zabezpečení](https://www.microsoft.com/trustcenter).

Azure AD Connect Health monitoruje služby infrastruktury a synchronizace místního identity. Také poskytuje přehledy a povrchy výstrahy. Společnost Microsoft se zavazuje GDPR nedodržování předpisů napříč cloudové služby může 2018 zahájení vynucení a zajistit záruky související GDPR v jeho smluvními závazky. 

>[!NOTE] 
> Tento článek popisuje GDPR dodržování předpisů v Azure AD Connect Health. Informace o dodržování předpisů GDPR v Azure AD Connect, najdete v části [GDPR dodržování předpisů a Azure AD Connect](../../active-directory/connect/active-directory-aadconnect-gdpr.md).

## <a name="gdpr-classification"></a>GDPR klasifikace
Azure AD Connect Health, které patří do **procesor dat** kategorie klasifikace GDPR. Jako procesor dat kanál poskytuje služby zpracování dat služby a klíče partnery a koncoví uživatelé. Azure AD Connect Health negeneruje uživatelská data a nemá žádnou nezávislé kontrolu nad jaké osobní údaje se shromažďují a jak se používají. Načtení dat, agregace, analýzu a vytváření sestav v Azure AD Connect Health jsou založeny na existující místní data. 

## <a name="data-retention-policy"></a>Zásady uchovávání dat
Azure AD Connect Health není generování sestav, provádět analýzy nebo nabízejí přehled za 30 dní. Azure AD Connect Health proto není ukládání, zpracovat nebo zachování všech dat za 30 dní. Tento návrh je kompatibilní s předpisy GDPR, Microsoft o ochraně osobních údajů předpisy a zásad uchovávání dat služby Azure AD. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Zakázání shromažďování dat a monitorování v Azure AD Connect Health
Azure AD Connect Health umožňuje zastavit shromažďování dat pro každý monitorovaných server nebo instanci monitorované služby. Například můžete zastavit shromažďování dat pro jednotlivé servery služby AD FS (Active Directory Federation Services), které jsou monitorovány pomocí Azure AD Connect Health. Můžete také zastavit shromažďování dat pro instanci služby AD FS, který je monitorován pomocí Azure AD Connect Health. Pokud se rozhodnete tak učinit, odpovídající servery se odstraní z portálu Azure AD Connect Health po zastavení shromažďování dat. 

>[!IMPORTANT]
> Potřebujete oprávnění globální správce Azure AD nebo roli Přispěvatel v RBAC odstranění monitorované servery z Azure AD Connect Health.
>
> Odebrání serveru nebo instance služby z Azure AD Connect Health je reverzibilního akce. 

### <a name="what-to-expect"></a>Co můžete očekávat?
Když zastavíte shromažďování dat a monitorování pro jednotlivé monitorovaném serveru nebo instanci monitorované služby, vezměte na vědomí následující:

- Při odstranění instance monitorované služby instance se odebere ze seznamu monitorování služby Azure AD Connect Health na portálu. 
- Při odstranění monitorovaných server nebo instanci monitorované služby agenta stavu se neodinstalují nebo odebrat z vašich serverů. Agent stavu je nakonfigurován tak, aby posílat data do Azure AD Connect Health. Budete muset ručně odinstalovat agenta stavu na dříve monitorované servery.
- Pokud jste agenta stavu neodinstalovali před provedením tohoto kroku, může na serverech vztahujících se k agentovi stavu objevit události chyb.
- Všechna data patřící k instanci monitorované služby se odstraní podle zásad uchovávání dat Microsoft Azure.

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Zakázání shromažďování dat a monitorování pro monitorovaném serveru
V tématu [postup odebrání serveru z Azure AD Connect Health](active-directory-aadconnect-health-operations.md#to-delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Zakázat monitorování pro instanci monitorované služby a shromažďování dat
V tématu [postup odebrání instance služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).


## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Opětovné povolení shromažďování dat a monitorování v Azure AD Connect Health
Chcete-li znovu povolte sledování v Azure AD Connect Health pro dříve odstraněné monitorované služby, je nutné odinstalovat a [přeinstalujte agenta stavu](active-directory-aadconnect-health-agent-install.md) ve všech serverech.


## <a name="next-steps"></a>Další postup
* [Zkontrolujte nastavení zásad Microsoft Privacy na Centrum zabezpečení](https://www.microsoft.com/trustcenter)
* [Azure AD Connect a GDPR](../../active-directory/connect/active-directory-aadconnect-gdpr.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
