---
title: "Představení služby Azure Active Directory"
description: "Pomocí Azure Active Directory rozšířit vaše stávající místních identit do cloudu, nebo vyvíjet aplikace, integraci služby Azure AD."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: mtillman
ms.reviewer: jsnow
ms.author: jeffgilb
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.custom: it-pro
ms.openlocfilehash: e5eafd4d25d2638ab5d9f904a7e0c00b36501d68
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-azure-active-directory"></a>Představení služby Azure Active Directory
Azure Active Directory (Azure AD) je Microsoft je více klientů, cloudu na základě adresáře a identity management service. Azure AD kombinuje základní adresářové služby, pokročilé identity zásad správného řízení a správa přístupu aplikací. Azure AD také nabízí bohatou a založených na standardech platformu, která umožňuje vývojářům k poskytování řízení přístupu k jejich aplikacím na základě centralizovaných zásad a pravidla. 

Pro správce IT, Azure AD poskytuje cenově dostupné a snadno použitelný řešení poskytnout zaměstnancům a obchodními partnery jednotné přihlašování (SSO) přístup [tisíce cloudové aplikace SaaS](active-directory-saas-tutorial-list.md) , jako třeba Office 365, Salesforce.com, DropBox a Concur.

Pro vývojáře aplikací Azure AD vám umožní soustředit na budování aplikaci tím, že rychlé a jednoduché integrovat řešení správy identity – třída world používané miliony organizací po celém světě.

Azure AD také zahrnuje úplná sada funkcí správy identit, včetně vícefaktorového ověřování, registrace zařízení, správu hesla pomocí samoobslužné služby, Samoobslužná správa skupin, správu privilegovaného účtu, řízení přístupu, sledování využití aplikací, bohaté auditování a zabezpečení monitorování a výstrahy na základě role. Tyto možnosti můžete pomůže zabezpečené cloudové aplikace, zjednodušit procesy IT, snížit náklady a ujistěte se, zda jsou splněny dodržování podnikových předpisů cíle.

Kromě toho se právě [čtyři kliknutí](./connect/active-directory-aadconnect-get-started-express.md), Azure AD se dá integrovat s existující Windows Server Active Directory, což umožní využít svých stávajících místních organizace investice identity můžete spravovat přístup do cloudu na základě aplikací SaaS.

Pokud jste zákazník Office 365, Azure nebo Dynamics CRM Online, nemusí Uvědomte si, že už používáte Azure AD. Každému klientovi Office 365, Azure a Dynamics CRM je ve skutečnosti již klient služby Azure AD. Vždy, když chcete, že můžete začít používat ke správě tohoto klienta přístup k tisícům jiných cloudových aplikací Azure AD se integruje se službou!

![Sada komponent Azure AD Connect](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="how-reliable-is-azure-ad"></a>Jak spolehlivé je Azure AD?
Návrh víceklientské, zeměpisné polohy, vysokou dostupnost služby Azure AD znamená, že můžete spoléhat na pro vaše nejdůležitější obchodní potřeby. Systém mimo 28 datových centrech po celém světě s automatické převzetí služeb při selhání, budete mít pohodlí zároveň budete vědět, že Azure AD je vysoce spolehlivé a to i v případě, že datové centrum ocitne mimo provoz kopie adresáře data jsou v za provozu alespoň dva další regionální rozptýlená v datových centrech a k dispozici pro okamžitý přístup.

Další podrobnosti najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Zvolte edice
Všechny firemní služby Microsoft Online spoléhají na Azure Active Directory (Azure AD) pro přihlášení a je třeba další identity. Pokud se přihlásíte k některému z firemní služby Microsoft Online (například Office 365 nebo Microsoft Azure), bude docházet k Azure AD s přístupem ke všem funkcím volné. S edice Azure Active Directory Free můžete spravovat uživatele a skupiny, proveďte synchronizaci s místních adresářů, získat jednotné přihlašování v Azure, Office 365 a tisícům oblíbených aplikací SaaS jako Salesforce, Workday, Concur, DocuSign, Google Apps, pole, ServiceNow, Dropbox a další. 

K vylepšení služby Azure Active Directory, můžete přidat placené možnosti pomocí edice Azure Active Directory Basic, Premium P1 a Premium P2. Azure Active Directory placené edice jsou postavená na adresáře existující volné poskytování podnikové třídy pokrývání uzlů samoobslužné služby, rozšířené monitorování, vytváření sestav zabezpečení, služba Multi-Factor Authentication (MFA) a zabezpečený přístup pro mobilních pracovníků.

> [!NOTE]
> Cenová možnosti tyto edice najdete v tématu [Azure Active Directory ceny](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, Premium P2 a Azure Active Directory Basic v Číně aktuálně nepodporují. Kontaktujte nás na fóru Azure Active Directory pro další informace.
>

* **Azure Active Directory Basic** -navržený pro úkolové pracovníky s potřebami cloudu první, tato edice poskytuje cloudové řešení správy identity zaměřená na aplikace pro přístup a samoobslužné služby. Azure Active Directory ve verzi Basic vám poskytne funkce pro zvýšení produktivity a snížení nákladů, jako je správa přístupu na základě skupin, samoobslužné obnovení hesel pro cloudové aplikace a aplikační proxy Azure Active Directory (aby se pomocí Azure Active Directory daly publikovat místní webové aplikace), a to všechno se smlouvou SLA na podnikové úrovni s 99,9procentní dostupností.
* **Azure Active Directory Premium P1** – navržené pro organizace s náročnější umožnit správu identit a přístupu musí, Azure Active Directory Premium edition přidá funkce pro správu identit podnikové úrovni bohaté funkce a umožňuje uživatelům hybridní bezproblémově přistupovat k místnímu a funkce v cloudu. Tato verze obsahuje všechno, co informační pracovníci a správci identit potřebují v hybridním prostředí k zajištění přístupu k aplikacím, samoobslužné správě identit a přístupu (IAM), ochraně identit a zabezpečení v cloudu. Podporuje rozšířené správy a delegování prostředky jako dynamických skupin a samoobslužnou správu skupin. Zahrnuje Microsoft Identity Manager (místní přístupu a identit a správy sady) a poskytuje cloudu zpětný zápis funkce povolení řešení, jako jsou samoobslužné resetování hesla pro místní uživatele.
* **Azure Active Directory Premium P2** -navržený s rozšířenou ochranu pro všechny uživatele a správce, tato nová nabídka zahrnuje všechny funkce v Azure AD Premium P1 a také naše nové Identity Protection a Privileged Identity Management. Azure Active Directory Identity Protection využívá až miliardy signály, které poskytují podmíněného přístupu na základě riziko k vašim aplikacím a důležitá firemní data. Nám také pomáhají spravovat a chránit privilegovaných účtů s Azure Active Directory Privileged Identity Management, můžete zjistit, omezit a sledovat správci a jejich přístup k prostředkům a poskytují přístup za běhu v případě potřeby.  

> [!NOTE]
> Počet možností Azure Active Directory jsou k dispozici prostřednictvím "platím průběžně" edice:
>
> * Active Directory B2C je řešení správy identit a přístupu pro aplikace určených. Další podrobnosti najdete v tématu [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Azure Multi-Factor Authentication umožňuje použít pro uživatele, nebo v zprostředkovatele ověřování. Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
>

## <a name="how-can-i-get-started"></a>Jak můžu začít?

**Pokud jste správce IT:**

* [Vyzkoušejte si to!](https://azure.microsoft.com/trial/get-started-active-directory/) -můžete zaregistrujte se ke bezplatné 30denní zkušební verze dnes a nasadit řešení první cloudu pomocí tohoto odkazu v části 5 minut.

* Čtení [Začínáme s Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) pro klienta spuštěná rychlé tipy a triky na získávání Azure AD

**Pokud jste vývojář:**
 
* Podívejte se na naše [Příručka pro vývojáře](active-directory-developers-guide.md) do Azure Active Directory

* [Spuštění zkušební verze](https://azure.microsoft.com/trial/get-started-active-directory/) – zaregistrujte se ke bezplatné 30denní zkušební verze dnes a spusťte integrace aplikací s Azure AD

## <a name="next-steps"></a>Další kroky
[Další informace o základní informace o Azure správu identit a přístupu](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
