---
title: "Získání klienta Azure AD | Dokumentace Microsoftu"
description: "Získání klienta Azure Active Directory pro registraci a vytváření aplikací."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: b45e9479fa2e06f2c15ace9138e48ced94bacbda
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Získání klienta Azure Active Directory
V Azure Active Directory (Azure AD) představuje [klient](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) zástupce organizace.  Je vyhrazenou instancí služby Azure AD, kterou obdrží a vlastní organizace, když si zaregistruje cloudovou službu společnosti Microsoft, například Azure, Microsoft Intune nebo Office 365.  Každý klient Azure AD se odlišuje a je oddělený od ostatních klientů Azure AD.  

V klientovi se nachází uživatelé a společnosti a informace o nich – hesla, data uživatelského profilu, oprávnění atd.  Obsahuje také skupiny, aplikace a další informace týkající se organizace a jejího zabezpečení.

Pokud chcete uživatelům Azure AD povolit přihlášení do aplikace, je třeba aplikaci zaregistrovat do vašeho klienta.  Publikování aplikace v klientovi Azure AD je **zcela zdarma**.  Většina vývojářů dokonce pro účely experimentování, vývoje, přípravy a testování vytváří několik klientů a aplikací.  Organizace, které si zaregistrují a používají vaši aplikaci, si mohou volitelně vybrat nákup licencí, pokud si přejí využívat výhod pokročilých funkcí adresáře.

Jak tedy můžete získat klienta Azure AD?  Proces se může mírně lišit, pokud:

* [Máte stávající předplatné služeb Office 365](#use-an-existing-office-365-subscription)
* [Máte stávající předplatné Azure spojené s účtem Microsoft](#use-an-msa-azure-subscription)
* [Máte stávající předplatné Azure spojené s účtem organizace](#use-an-organizational-azure-subscription)
* [Nemáte nic z výše uvedeného a chcete začít od nuly](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Použití stávajícího předplatného služeb Office 365
Pokud máte stávající předplatné Office 365, již tenanta služby Azure AD máte! Můžete se přihlásit na webu [Azure Portal](https://portal.azure.com) pomocí svého účtu O365 a začít používat službu Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Použití předplatného MSA Azure
Pokud jste si již dříve zaregistrovali předplatné služby Azure pomocí svého individuálního účtu Microsoft, již klienta máte!  Při přihlášení na webu [Azure Portal](https://portal.azure.com) budete automaticky přihlášeni ke svému výchozímu tenantu. Tohoto klienta můžete klidně používat podle svých potřeb – ale možná si budete chtít vytvořit účet správce organizace.

To provedete podle těchto kroků.  Případně můžete vytvořit nového klienta a vytvořit v něm správce podle podobného postupu.

1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) pomocí svého individuálního účtu.
2. Na portálu přejděte do části Azure Active Directory (nachází se na levém navigačním panelu v části **Další služby**).
3. Měli byste být automaticky přihlášeni k výchozímu adresáři. Pokud ne, můžete adresář změnit kliknutím na název účtu v horním pravém rohu.
4. V části **Rychlé úlohy** zvolte **Přidat uživatele**.
5. Ve formuláři pro přidání uživatele zadejte následující podrobnosti:

   * Název: (zvolte příslušnou hodnotu)
   * Uživatelské jméno: (zvolte uživatelské jméno pro tohoto správce.)
   * Profil: (vyplňte příslušné hodnoty pro Jméno, Příjmení, Pozice a Oddělení)
   * Role: globální správce
6. Po vyplnění formuláře pro přidání uživatele obdržíte dočasné heslo pro nového správce, které si nezapomeňte někam zapsat. Bude nutné k přihlášení tohoto nového uživatele, abyste mohli heslo změnit. Pomocí alternativní e-mailové adresy můžete také heslo odeslat přímo uživateli.
7. Kliknutím na **Vytvořit** vytvořte nového uživatele.
8. Pokud chcete dočasné heslo změnit, přihlaste se na adrese [https://login.microsoftonline.com](https://login.microsoftonline.com) pomocí tohoto nového uživatelského účtu a po výzvě heslo změňte.

## <a name="use-an-organizational-azure-subscription"></a>Použití organizačního předplatného Azure
Pokud jste si již dříve zaregistrovali předplatné služby Azure pomocí účtu organizace, již klienta máte!  Na webu [Azure Portal](https://portal.azure.com) byste měli tenanta najít v části Další služby a Azure Active Directory.  Tohoto klienta můžete volně používat podle svých potřeb.

## <a name="start-from-scratch"></a>Začátek od nuly
Pokud vám výše uvedené informace připadají nesrozumitelné, nemusíte si dělat starosti.  Navštivte stránku [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) a zaregistrujte se do služby Azure s novou organizací.  Jakmile proces dokončíte, bude mít svého vlastního klienta Azure AD s názvem domény, který jste zvolili při registraci.  Na webu [Azure Portal](https://portal.azure.com) tenanta najdete v části Azure Active Directory v nabídce vlevo.

Součástí registrace do služby Azure je zadání údajů z platební karty.  Můžete bez obav pokračovat – za publikování aplikací v Azure AD ani vytváření nových klientů vám nebude nic účtováno.
