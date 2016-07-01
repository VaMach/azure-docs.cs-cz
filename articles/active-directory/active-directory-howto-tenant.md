<properties
    pageTitle="Získání klienta Azure AD | Microsoft Azure"
    description="Získání klienta Azure Active Directory pro registraci a vytváření aplikací."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# Získání klienta Azure Active Directory

V Azure Active Directory (Azure AD) představuje [klient](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) zástupce organizace.  Je vyhrazenou instancí služby Azure AD, kterou obdrží a vlastní organizace, když si zaregistruje cloudovou službu společnosti Microsoft, například Azure, Microsoft Intune nebo Office 365.  Každý klient Azure AD se odlišuje a je oddělený od ostatních klientů Azure AD.  

V klientovi se nachází uživatelé a společnosti a informace o nich – hesla, data uživatelského profilu, oprávnění atd.  Obsahuje také skupiny, aplikace a další informace týkající se organizace a jejího zabezpečení.

Pokud chcete uživatelům Azure AD povolit přihlášení do aplikace, je třeba aplikaci zaregistrovat do vašeho klienta.  Publikování aplikace v klientovi Azure AD je **zcela zdarma**.  Většina vývojářů dokonce pro účely experimentování, vývoje, přípravy a testování vytváří několik klientů a aplikací.  Organizace, které si zaregistrují a používají vaši aplikaci, si mohou volitelně vybrat nákup licencí, pokud si přejí využívat výhod pokročilých funkcí adresáře.

Jak tedy můžete získat klienta Azure AD?  Proces se může mírně lišit, pokud:

- [Máte stávající předplatné služeb Office 365](#use-an-existing-office-365-subscription)
- [Máte stávající předplatné Azure spojené s účtem Microsoft](#use-an-msa-azure-subscription)
- [Máte stávající předplatné Azure spojené s účtem organizace](#use-an-organizational-azure-subscription)
- [Nemáte nic z výše uvedeného a chcete začít od nuly](#start-from-scratch)

## Použití stávajícího předplatného služeb Office 365
Pokud máte stávající předplatné služeb Office 365, ale nemáte předplatné Azure (a nemůžete se přihlásit k [portálu pro správu služby Azure](https://manage.windowsazure.com)), získáte přístup ke klientovi Azure AD podle [těchto pokynů](https://technet.microsoft.com/library/dn832618.aspx).

## Použití předplatného MSA Azure
Pokud jste si již dříve zaregistrovali předplatné služby Azure pomocí svého individuálního účtu Microsoft, již klienta máte!  Na [portálu pro správu služby Azure](https://manage.windowsazure.com) byste měli klienta s názvem Výchozí klient najít v seznamu Všechny položky a Active Directory.  Tohoto klienta můžete klidně používat podle svých potřeb – ale možná si budete chtít vytvořit účet správce organizace.

To provedete podle těchto kroků.  Případně můžete vytvořit nového klienta a vytvořit v něm správce podle podobného postupu.

1.  Přihlaste se k [portálu pro správu služby Azure](https://manage.windowsazure.com) pomocí individuálního účtu
2.  Přejděte do části Active Directory (nachází se na levém navigačního panelu)
3.  V seznamu dostupných adresářů vyberte položku Výchozí adresář
4.  Klikněte na odkaz Uživatelé v horní části stránky.  V seznamu se zobrazí jeden uživatel s hodnotou Účet Microsoft ve sloupci Zdrojem je
5.  Klikněte na tlačítko Přidat uživatele v dolní části stránky
6.  Ve formuláři pro přidání uživatele zadejte následující podrobnosti:
    - Typ uživatele: nový uživatel ve vaší organizaci
    - Uživatelské jméno: (zvolte uživatelské jméno pro tohoto správce.)
    - Jméno/příjmení/zobrazované jméno: (zvolte příslušné hodnoty.)
    - Role: globální správce
    - Alternativní e-mailová adresa: (zadejte příslušné hodnoty)
    - Volitelné: Povolení služby Multi-Factor Authentication
    - Nakonec kliknutím na zelené tlačítko VYTVOŘIT dokončíte vytvoření uživatele (a zobrazí se dočasné heslo).
7.  Po vyplnění formuláře pro přidání uživatele obdržíte dočasné heslo pro nového správce, které si nezapomeňte někam zapsat. Bude nutné k přihlášení tohoto nového uživatele, abyste mohli heslo změnit. Pomocí alternativní e-mailové adresy můžete také heslo odeslat přímo uživateli.
8.  Pokud chcete dočasné heslo změnit, přihlaste se na adrese https://login.microsoftonline.com pomocí tohoto nového uživatelského účtu a po výzvě heslo změňte.


## Použití organizačního předplatného Azure
Pokud jste si již dříve zaregistrovali předplatné služby Azure pomocí účtu organizace, již klienta máte!  Na [portálu pro správu služby Azure](https://manage.windowsazure.com) byste měli klienta najít v seznamu Všechny položky a Active Directory.  Tohoto klienta můžete volně používat podle svých potřeb.  Možná si budete chtít vytvořit nového klienta pomocí tlačítka Nový v levém dolním rohu portálu.


## Začátek od nuly
Pokud vám výše uvedené informace připadají nesrozumitelné, nemusíte si dělat starosti.  Navštivte stránku [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) a zaregistrujte se do služby Azure s novou organizací.  Jakmile proces dokončíte, bude mít svého vlastního klienta Azure AD s názvem domény, který jste zvolili při registraci.  Na [portálu pro správu služby Azure](https://manage.windowsazure.com) klienta najdete v části Active Directory v nabídce vlevo.

Součástí registrace do služby Azure je zadání údajů z platební karty.  Můžete bez obav pokračovat – za publikování aplikací v Azure AD ani vytváření nových klientů vám nebude nic účtováno.



<!--HONumber=Jun16_HO2-->


