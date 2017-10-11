---
title: "Koncepční přehled vlastních názvů domén v Azure Active Directory | Microsoft Docs"
description: "Vysvětluje koncepční framework pro používání vlastních názvů domén v Azure Active directory, včetně federace pro jednotné přihlašování"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fd0c5def-0da2-43af-81bc-76f4cfe86afd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 3c591680160101a91174868714392674c9aa7178
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Koncepční přehled vlastních názvů domén v Azure Active Directory
Název domény může být důležité identifikátor pro mnoho directory prostředky, jako součást:

* Pro uživatele uživatelského jména nebo e-mailové adresy
* Adresa pro skupinu
* Identifikátor ID URI aplikace pro aplikaci

Prostředek v Azure Active Directory (Azure AD) může zahrnovat název domény, který je již ověřit vlastnit adresář, který obsahuje prostředek. Globální správce můžete provádět úlohy správy domény ve službě Azure AD.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. Jak spravovat názvy domén v Centru správy služby Azure AD, najdete v článku [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).

Názvy domén ve službě Azure AD jsou globálně jedinečný. Vlastní název domény lze pouze jeden klient Azure AD v čase. Pokud jeden adresář Azure AD ověří název domény, můžete žádné další adresář Azure AD ověřit nebo použít stejný název domény.

## <a name="initial-and-custom-domain-names"></a>Názvy počáteční a vlastní domény
Každý název domény ve službě Azure AD je buď počáteční název domény, nebo vlastní název domény.

Každý Azure AD se dodává s počáteční název domény v contoso.onmicrosoft.com formuláře. Tento název třetí úrovně domény, v tomto příkladu "contoso.onmicrosoft.com," byl vytvořen, když adresář byl vytvořen, obvykle správce, který vytvořil adresář. Název domény pro adresář nelze změnit ani odstranit. Název domény, zatímco plně funkční, je určený hlavně má být použit jako mechanismus samozaváděcí, dokud je ověřit vlastní název domény.

Ve většině produkční prostředí adresář má alespoň jeden ověřené vlastní domény, například "contoso.com", a je tento vlastní doménu, která je viditelná pro koncové uživatele. Vlastní název domény je název domény, který je ve vlastnictví a použít dané organizace, jako je například "contoso.com", pro účely, například hostování svého webu. Tento název domény je dobře známé zaměstnancům, protože je součástí uživatelské jméno, které používají pro přihlášení k podnikové síti, nebo k odesílat a přijímat e-mailu.

Před použitím Azure AD, vlastní název domény musí přidat do vašeho adresáře a ověřit.

## <a name="verified-and-unverified-domain-names"></a>Názvy ověřeny a neověřených domén
Název domény pro adresář je implicitně vyhodnotit, jak ověřit pomocí služby Azure AD. Když správce přidá vlastního názvu domény do Azure AD, je nejprve v neověřené stavu. Azure AD neumožní všechny prostředky directory použít název neověřené domény. To zajistí, že pouze jeden adresář můžete použít název konkrétní domény a že organizace používá název domény, ve skutečnosti vlastní název domény.

Azure AD ověřuje vlastnictví názvu domény tak, že vyhledá konkrétní položky v souboru zóny domény name service (DNS) pro název domény. Pokud chcete ověřit vlastnictví názvu domény, správce získá položky DNS z Azure AD, Azure AD bude hledat a přidá tento záznam do souboru zóny DNS pro název domény. Souboru zóny DNS se udržuje registrátorem názvu domény pro tuto doménu. Postup ověření domény jsou uvedené v článku pro [přidání vlastní domény k adresáři služby Azure AD](active-directory-add-domain.md).

Přidání položky DNS do souboru zóny pro název domény nemá vliv na jiné domény služby, například e-mailu nebo publikování na webu.

## <a name="federated-and-managed-domain-names"></a>Názvy domén federované a spravované
Umožnit uživateli federovaného přihlašování v prostředí mezi vaší místní služby Active Directory a Azure AD můžete nakonfigurovat vlastní název domény ve službě Azure AD. Konfigurace domény pro federaci vyžaduje aktualizace, které privilegované prostředky ve službě Azure AD a také do vašeho systému Windows Server Active Directory. Konfiguruje federované domény musí být dokončena z Azure AD Connect nebo pomocí prostředí PowerShell. Federaci vlastní domény nejde inicializovat z portálu Azure classic. [Podívejte se na toto video, které další informace o konfiguraci služby AD FS pro uživatele přihlásit se pomocí Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domény, které nejsou federované se někdy označuje jako spravované domény. Počáteční domény pro adresář služby Azure AD je implicitně vyhodnocena jako spravované domény.

## <a name="primary-domain-names"></a>Názvy primární doménu
Název primární domény pro adresář je název domény, který je předem vybraný jako výchozí hodnota pro část, domény, uživatelské jméno, když správce vytvoří nový uživatel v [portál Azure](https://portal.azure.com/), nebo jiný portál, jako je například portál pro správu Office 365 nebo portálu Microsoft Intune. Adresář může mít pouze jeden název domény. Správce můžete změnit název primární domény jako žádné ověřeny vlastní doménu, která není federovaný, nebo pro počáteční domény.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Názvy domén ve službě Azure AD a dalších služeb Microsoft Online Services
Název domény musí ověřit ve službě Azure AD, než mohou být využívána jiné Online službu Microsoftu, jako je Exchange Online, SharePoint Online a Intune. Tyto další služby obvykle vyžadují správci přidat jeden nebo více záznamy DNS, které jsou konkrétní službě.

Webové aplikace Azure používá k ověření vlastnictví domény vlastní mechanismus. Domény musí ověřit pro použití s Azure AD i v případě, že dříve ověření pro použití ve webové aplikace Azure v rámci předplatného, které jsou závislé na této službě Azure AD. Webové aplikace Azure můžete použít název domény, který byl ověřen v jiném adresáři z adresáře, který zabezpečuje webové aplikace.

## <a name="managing-domain-names"></a>Správa názvů domén
Úlohy správy domény je možné dokončit, z portálu Azure classic a z prostředí PowerShell. Mnoho úloh je možné provést pomocí rozhraní Azure AD Graph API.

* [Přidání a ověření vlastního názvu domény](active-directory-add-domain.md)
* [Správa domény na portálu Azure classic](active-directory-add-manage-domain-names.md)
* [Použití Powershellu ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Pomocí Azure AD Graph API ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

