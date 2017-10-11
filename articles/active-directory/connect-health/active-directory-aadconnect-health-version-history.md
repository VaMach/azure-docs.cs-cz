---
title: "Historie verzí služby Azure AD Connect Health"
description: "Tento dokument popisuje verze pro Azure AD Connect Health a co byl zahrnut v těchto verzích."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6c990a184d44771c78330f54f518bb4c35a36a35
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Historie vydaných verzí
Tým služby Azure Active Directory pravidelně aktualizuje Azure AD Connect Health se nové funkce a funkce. V tomto článku jsou uvedené verze a funkce, které byly vydány.

## <a name="october-2016"></a>Října 2016
**Aktualizace agenta:**

* Agent Azure AD Connect Health pro službu AD FS \(verze 2.6.408.0\)
  1. Vylepšení detekce klientských IP adres v žádosti o ověření
  2. Opravy chyb, které souvisí s výstrahami
* Agent Azure AD Connect Health pro službu AD DS (verze 2.6.408.0)
  1. Opravy chyb, které souvisí s výstrahami.
* Agent Azure AD Connect Health pro synchronizaci (verze 2.6.353.0) vydané službou Azure AD Connect verze 1.1.281.0
  1. Zadejte požadovaná data pro sestavy chyb synchronizace
  2. Opravy chyb, které souvisí s výstrahami

**Nové funkce verze preview:**

* Zprávy o chybách synchronizace pro Azure AD Connect

**Nové funkce:**

* Azure AD Connect Health pro AD FS – pole IP adresy je k dispozici v sestavě o nejčastějších 50 uživatelů s chybné uživatelské jméno a heslo.

## <a name="july-2016"></a>Července 2016
**Nové funkce verze preview:**

* [Azure AD Connect Health pro službu AD DS](active-directory-aadconnect-health-adds.md).

## <a name="january-2016"></a>leden 2016
**Aktualizace agenta:**

* Agent Azure AD Connect Health pro službu AD FS (verze 2.6.91.1512)

**Nové funkce:**

* [Nástroj pro testování připojení pro Azure AD Connect stavu agentů](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>listopad 2015
**Nové funkce:**

* Podpora pro [řízení přístupu podle rolí](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)

**Nové funkce verze preview:**

* [Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md).

**Opravené problémy:**

* Opravy chyb pro chyby můžou vyskytnout během registrace agenta.

## <a name="september-2015"></a>Září 2015
**Nové funkce:**

* Nesprávný uživatelské jméno hesla sestav pro službu AD FS
* Podpora konfigurace neověřené Proxy protokolu HTTP
* Podpora konfigurace agenta na jádru serveru
* Vylepšení výstrah pro službu AD FS
* Vylepšení v agenta Azure AD Connect Health pro službu AD FS pro připojení a data nahrát.

**Opravené problémy:**

* Opravy chyb v přehledy využití pro typy chyb AD FS.

## <a name="june-2015"></a>Červen 2015
**Počáteční verze služby Azure AD Connect Health pro AD FS a Proxy služby AD FS.**

**Nové funkce:**

* Výstrahy sledování serverů služby AD FS a Proxy služby AD FS s e-mailová oznámení.
* Snadný přístup k topologie služby AD FS a vzory v čítače výkonu služby AD FS.
* Trend úspěšné žádosti o tokeny na serverech služby AD FS, které jsou seskupené podle aplikací, metody ověřování, žádosti o síťové umístění atd.
* Trendy v chybných požadavků na serverech služby AD FS, které jsou seskupené podle aplikací, chyba typy atd.
* Jednodušší nasazení agenta pomocí přihlašovacích údajů Azure AD globálního správce.  

## <a name="next-steps"></a>Další kroky
Další informace o [monitorování vaší místní infrastruktury identit a synchronizaci služeb v cloudu](active-directory-aadconnect-health.md).

