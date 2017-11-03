---
title: "Oznámení vytváření sestav Azure Active Directory"
description: "Jak používat Azure Active Directory, vytváření sestav oznámení pro podezřelé přihlášení in."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: e561061cadd88e2c5670e27f2a66ef21002e30b0
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-reporting-notifications"></a>Oznámení vytváření sestav Azure Active Directory
## <a name="what-reports-generate-email-notifications"></a>Jaké sestavy generovat e-mailových oznámení
V tomto okamžiku pouze nestandardní přihlašovací aktivita sestavy aktivační události e-mailová oznámení.

## <a name="what-is-an-irregular-sign-in"></a>Co je "nestandardní přihlašovací"?
Nestandardní přihlášení jsou ty, které byly identifikovány naše algoritmů strojového učení, na základě podmínku "neuskutečnitelná cesta" v kombinaci s neobvyklá přihlášení, umístění a zařízení. To může znamenat, že se hacker se se pokoušel přihlásit pomocí tohoto účtu.

## <a name="who-receives-the-email-notifications"></a>Kdo obdrží e-mailová oznámení?
E-mail je odeslán do všichni globální správci, kteří mají přiřazený licenci služby Active Directory Premium. Zajistěte, aby že se doručí, jsme poslat ho správcům alternativní e-mailovou adresu i. Správci by měla obsahovat aad-alerts-noreply@mail.windowsazure.com v jejich seznamu bezpečných odesílatelů, takže nemáte promeškají e-mailu.

## <a name="how-often-are-these-emails-sent"></a>Jak často jsou tyto e-maily odesílané?
E-mail je odeslán, pokud 10 nové nestandardní přihlašovací aktivity, ke kterým došlo v posledních 30 dnů nebo od odeslání poslední e-mailu, podle toho, která je menší.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Přístupu zprávu podle e-mailu
Když kliknete na odkaz, budete přesměrováni na stránku sestavy v rámci portálu Azure classic. Chcete-li získat přístup k sestavě, musíte nastavit na obojí:

* Na správce nebo spolusprávce předplatného Azure
* Globální správce v adresáři a přiřadit licenci služby Active Directory Premium. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Můžete vypnout těchto e-mailů?
Ano, chcete-li vypnout oznámení související s neobvyklých přihlášení v rámci portálu Azure classic, klikněte na tlačítko **konfigurace**a potom vyberte **zakázané** pod **oznámení** části.

## <a name="whats-next"></a>Kam dál
* Chcete zjistit, jaké sestavy zabezpečení, auditování a aktivity jsou k dispozici? Podívejte se na [zabezpečení Azure AD, auditování a protokolování aktivit](active-directory-view-access-usage-reports.md)
* [Začínáme se službou Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu](active-directory-add-company-branding.md)

