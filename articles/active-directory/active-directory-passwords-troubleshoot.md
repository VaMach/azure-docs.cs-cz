---
title: "Řešení potíží: Azure AD SSPR | Microsoft Docs"
description: "Řešení problémů s Azure AD samoobslužného resetování hesel"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 2eeb49cb6ab39c78612e0f1b3e08130ba88cf356
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="how-to-troubleshoot-self-service-password-reset"></a>Řešení potíží s samoobslužné resetování hesla

Pokud máte problémy s samoobslužné resetování hesla, položky, které následují můžete získat věcí práce rychle.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-may-see"></a>Řešení chyb resetování hesla pomocí samoobslužné služby, které mohou se zobrazit uživatele

| Chyba | Podrobnosti | Technické podrobnosti |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože váš správce zakázal resetování hesla pro vaši organizaci. Neexistuje žádná další akce, které můžete provést k vyřešení této situaci. Obraťte se na správce a požádejte jej o povolení této funkce. Další informace, přečtěte si [nápovědy, zapomenuté heslo Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: Zjistili jsme, že nebylo správce povoleno vytvoření nového hesla. Obraťte se na správce a požádejte jej o povolení obnovení hesla pro vaši organizaci. |
| WritebackNotEnabled = 10 |Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože správce nepovolil nezbytné služby pro vaši organizaci. Neexistuje žádná další akce, které můžete provést k vyřešení této situaci. Obraťte se na správce a požádejte je o Zkontrolujte konfiguraci vaší organizace. Další informace o této potřebné služby, přečtěte si [konfigurace zpětný zápis hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-writeback#configuring-password-writeback). | SSPR_0010: Zjistili jsme, že nebylo povoleno zpětný zápis hesla. Obraťte se na správce a požádejte jej o povolení zpětného zápisu hesla. |
| SsprNotEnabledInUserPolicy = 11 | Je nám líto  <br> V tuto chvíli nelze obnovit heslo, protože váš správce nenakonfiguroval resetování hesla pro vaši organizaci. Neexistuje žádná další akce, které můžete provést k vyřešení této situaci. Obraťte se na správce a požádejte je, aby konfigurace resetování hesla. Další informace o heslo resetování konfigurace najdete v článku [rychlý start: resetování hesla pomocí samoobslužné služby Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Vaše organizace nedefinoval zásady resetování hesel. Obraťte se na správce a požádejte je o definovat zásady resetování hesel. |
| UserNotLicensed = 12 | Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože požadované licence chybí z vaší organizace. Neexistuje žádná další akce, které můžete provést k vyřešení této situaci. Obraťte se na správce a požádejte je o zkontrolujte přiřazení licence. Další informace o licencování najdete v článku [resetovat Licensing požadavky pro hesla pomocí samoobslužné služby Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: Vaše organizace nemá požadované licence nezbytným k provedení resetování hesla. Obraťte se na správce a požádejte je o kontrolu přiřazením licencí. |
| UserNotMemberOfScopedAccessGroup = 13 | Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože váš správce nenakonfiguroval váš účet se má použít resetování hesla. Neexistuje žádná další akce, které můžete provést k vyřešení této situaci. Obraťte se na správce a požádejte je o konfiguraci vašeho účtu pro resetování hesla. Další informace o konfiguraci účtu pro resetování hesla najdete v článku [resetování hesla pro uživatele zavádět](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012: Nejste členem skupiny povolen pro resetování hesla. Obraťte se na správce a požadavek, který se má přidat do skupiny. |
| UserNotProperlyConfigured = 14 | Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože chybí požadované informace z vašeho účtu. Neexistuje žádná další akce, které můžete provést k vyřešení této situaci. Vás kontaktovat správce a požádejte je, aby vám heslo resetoval. Až budete mít přístup ke svému účtu znova, dozvíte, jak zaregistrovat nezbytné informace podle kroků v článku [zaregistrovat pro resetování hesla pomocí samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-reset-register). | SSPR_0014: K resetování hesla se vyžaduje další bezpečnostní údaje. Chcete-li pokračovat, obraťte se na správce a požádejte je o resetování hesla. Až budete mít přístup ke svému účtu můžete registrovat další bezpečnostní údaje na adrese https://aka.ms/ssprsetup. Správce můžete přidat další bezpečnostní údaje k účtu pomocí kroků v [sady a data čtení ověřování pro resetování hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired = 29 | Je nám líto <br> Nemůžeme resetovat heslo v tuto chvíli z důvodu problému s konfigurací resetování hesla vaší organizace. Neexistuje žádná další akce, které můžete provést k vyřešení této situaci. Obraťte se na správce a požádejte je o prozkoumat. Další informace o potenciálně problém, přečtěte si článek [řešení zpětného zápisu hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: Nepovedlo se nám k resetování hesla došlo k chybě v konfiguraci na místě. Obraťte se na správce a požádejte je o prozkoumat. |
| OnPremisesConnectivityError = 30 | Je nám líto <br> Nemůžeme resetovat heslo v tuto chvíli kvůli potížím s připojením k vaší organizaci. Neexistuje žádné akci nyní provést, ale může být problém vyřešen, pokud to zkusíte znovu později. Pokud potíže potrvají, obraťte se na správce a požádejte je o prozkoumat. Další informace o čtení problémy s připojením k [připojení k řešení potíží s zpětný zápis hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Nemůžeme resetovat heslo kvůli nízký připojení s místním prostředí. Obraťte se na správce a požádejte je o prozkoumat.|


## <a name="troubleshoot-password-reset-configuration-in-the-azure-portal"></a>Řešení potíží s konfigurací resetování hesla na portálu Azure

| **Chyba** | Řešení |
| --- | --- |
| Nejsou **resetování hesla** části v rámci Azure AD na portálu Azure | To může nastat, když máte Azure AD Premium nebo základní licenci přiřazené správci provádění této operace. <br> To lze vyřešit přiřazení licence k účtu správce v dotyčných pomocí článek [přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Možnost konkrétní konfigurace se nezobrazí | Mnoho prvků uživatelského rozhraní jsou skryté. dokud nebude potřeba. Zkuste povolit všechny možnosti, které chcete zobrazit. |
| Se nezobrazí **integrace v místním** karta | Tato možnost jenom se zobrazí, pokud máte stáhnout Azure AD Connect a nakonfigurovat zpětný zápis hesla. Další informace o tomto tématu najdete v článku [Začínáme s Azure AD Connect pomocí expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Řešení potíží s reporting resetování hesla

| **Chyba** | Řešení |
| --- | --- |
| Všechny typy aktivit správy hesla se zobrazí v se nezobrazí **Samoobslužná správa hesel** kategorie události auditu | To může nastat, když máte Azure AD Premium nebo základní licenci přiřazené správci provádění této operace. <br> To lze vyřešit přiřazení licence k účtu správce v dotyčných pomocí článek [přiřadit, ověření a vyřešit problémy s licencemi] |
| Registrace uživatele zobrazit více než jednou. | Aktuálně když se uživatel zaregistruje, jsme aktuálně protokolu každé jednotlivé část dat, která je registrována jako samostatná událost. <br> Pokud chcete shromažďovat tato data, můžete stáhnout sestavu a otevřete data jako kontingenční tabulky v aplikaci excel tak, aby měl větší flexibilitu.

## <a name="troubleshoot-password-reset-registration-portal"></a>Řešení potíží s registrační portál pro resetování hesla

| **Chyba** | Řešení |
| --- | --- |
| Adresář není povolen pro resetování hesla **správce vám nepovolil se pomocí této funkce** | Přepínač **samoobslužném resetování hesla služby povolena** příznak, který **skupinu** nebo **každý uživatel** a klikněte na tlačítko **uložit** |
| Uživatel nemá Azure AD Premium nebo Basic přiřazenou licenci **správce vám nepovolil se pomocí této funkce** | To může nastat, když máte Azure AD Premium nebo základní licenci přiřazené správci provádění této operace. <br> To lze vyřešit přiřazení licence k účtu správce v dotyčných pomocí článek [přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Chyba při zpracování žádosti | To může být způsobeno řadu problémů, ale obecně tato chyba je způsobená buď problém se službou výpadku nebo konfigurace. Pokud se zobrazí tato chyba a jeho je ovlivňující vaši firmu, kontaktujte podporu Microsoftu o další pomoc. |

## <a name="troubleshoot-password-reset-portal"></a>Řešení potíží s portálu pro resetování hesel

| **Chyba** | Řešení |
| --- | --- |
| Adresář není povolen pro resetování hesla. | Přepínač **samoobslužném resetování hesla služby povolena** příznak, který **skupinu** nebo **každý uživatel** a klikněte na tlačítko **uložit** |
| Uživatel nemá Azure AD Premium nebo Basic přiřazenou licenci | To může nastat, když máte Azure AD Premium nebo základní licenci přiřazené správci provádění této operace. <br> To lze vyřešit přiřazení licence k účtu správce v dotyčných pomocí článek [přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Adresář je povolen pro resetování hesla, ale uživatel má chybí nebo je vytvořen špatný informace o ověřování | Zajistěte, aby že tento uživatel má ve správném formátu kontaktní údaje na soubor v adresáři, než budete pokračovat. Další informace o tomto tématu najdete v článku [dat používá Azure AD funkce samoobslužného resetování hesla](active-directory-passwords-data.md). |
| Adresář je povolen pro resetování hesla, ale uživatel má pouze jeden prvek kontaktní dat v souboru v případě zásada nastavená tak, aby vyžadovala dva kroky pro ověření | Ujistěte se, tento uživatel má alespoň dva způsoby kontaktování správně nakonfigurované (Příklad: mobilní telefon **a** Telefon do kanceláře) než budete pokračovat. |
| Adresář je povolen pro resetování hesla a správně nakonfigurován, ale uživatel se nemůže kontaktovat | To může být způsobeno dočasné chybě nebo nesprávné kontaktní data, která jsme se nezdařilo správně zjistit. <br> <br> Pokud uživatel vyčká 10 sekund, zkuste to znovu a zobrazí se odkaz "obraťte se na správce". Kliknutím na tlačítko zkuste to znovu opakovat volání, zatímco kliknutím na tlačítko "obraťte se na správce" pošle e-mailu s formuláři správcům požadujícím heslo resetovat provést pro tento uživatelský účet. |
| Uživatel obdrží nikdy resetování hesla služby SMS nebo telefonní hovor | To může být výsledek vytvořen špatný telefonní číslo v adresáři. Zkontrolujte, zda telefonní číslo je ve formátu "+ xxxyyyzzzzXeeee kopie". <br> <br> Resetování hesla nepodporuje rozšíření, i v případě, že zadáváte jednu v adresáři (jsou se odstraní předtím, než se volání odeslaných). Zkuste použít číslem bez přípony, nebo do telefonní číslo ve vašem Ústřednou integrací rozšíření. |
| Uživatel nikdy obdrží e-mail pro resetování hesla | Nejčastější příčinou tohoto problému je, že je zpráva odmítnut filtr proti spamu. Zkontrolujte nevyžádané pošty, nevyžádanou poštou nebo odstraněných položek složku pro e-mailu. <br> Ujistěte se také, že při kontrole správná e-mailová zpráva. |
| Nastavení zásad resetování hesel, ale pokud účet správce používá resetování hesla, není použita zásad | Spravuje Microsoft a ovládací prvky zásadu a zajistit tak nejvyšší úroveň zabezpečení pro resetování hesla správce. |
| Uživateli zabránit v pokusu o vytvoření nového příliš mnohokrát za den | Implementaci mechanismus automatického omezení uživatelům bloku v pokusu o vytvoření nového hesla příliš mnohokrát v krátké době. V takovém případě: <br> 1. Uživatel se pokusí ověřit telefonní číslo 5krát za jednu hodinu. <br> 2. Uživatel se pokusí použít bránu otázky zabezpečení 5krát za jednu hodinu. <br> 3. Uživatel se pokusí resetovat heslo pro stejným uživatelským účtem 5krát za jednu hodinu. <br> Chcete-li odstranit tento problém, požádejte uživateli počkejte 24 hodin po poslední pokus o a uživatel bude moci obnovit své heslo. |
| Uživateli se zobrazí k chybě při ověření svého telefonního čísla | K této chybě dojde, pokud zadané telefonní číslo neodpovídá telefonního čísla na soubor. Ujistěte se, že uživatel zadá úplné telefonní číslo, včetně kódu oblasti a země, při pokusu o použití metody založené na telefonu pro resetování hesla. |
| Chyba při zpracování žádosti | To může být způsobeno řadu problémů, ale obecně tato chyba je způsobená buď problém se službou výpadku nebo konfigurace. Pokud se zobrazí tato chyba a jeho je ovlivňující vaši firmu, kontaktujte podporu Microsoftu o další pomoc. |

## <a name="troubleshoot-password-writeback"></a>Řešení potíží s zpětný zápis hesla

| **Chyba** | Řešení |
| --- | --- |
| Služba resetování hesel místní s chybou 6800 v protokolu událostí aplikace na Azure AD Connect počítač nespustí. <br> <br> Po synchronizaci registrace, federovaný nebo hodnoty hash hesla uživatele nelze resetovat vlastní hesla. | Pokud je povolen zpětný zápis hesla, synchronizační modul volá knihovnu zpětný zápis do provedení konfigurace (registrace) rozhovoru ke cloudové službě registrace. Všechny chyby došlo během registrace nebo při spouštění koncový bod WCF pro zpětný zápis hesla, které jsou výsledkem chyby v protokolu událostí v protokolu událostí vašeho počítače Azure AD Connect. <br> <br> Při restartování služby ADSync Pokud bylo nakonfigurované zpětný zápis, koncový bod WCF je spuštění. Ale pokud se nezdaří spuštění koncového bodu, jsme bude protokolu událostí 6800 a umožní spuštění synchronizační služby. Přítomnost Tato událost znamená, že koncový bod nebyla spuštěna, až zpětný zápis hesla. Podrobnosti protokolu událostí pro tuto událost (6800) společně s položky protokolu událostí vygenerujte PasswordResetService součást označuje, proč nebylo možné spustit koncový bod. Přečtěte si tyto chyby v protokolu událostí a pokuste se službu Azure AD Connect restartovat, pokud zpětný zápis hesla stále nepracuje. Pokud potíže potrvají, zkuste zakázat a znovu povolit zpětný zápis hesla.
| Když se uživatel pokusí resetovat heslo nebo odemknout účet s povolen zpětný zápis hesla, operace se nezdaří. <br> <br> Kromě toho se zobrazí události v protokolu událostí na Azure AD Connect obsahující: "synchronizačního modulu vrátila chyba hr = 800700CE, zpráva = Název souboru nebo rozšíření je příliš dlouhá" po odemčení operace probíhá. | Najděte účet služby Active Directory pro Azure AD Connect a resetování hesla tak, aby obsahovala více než 127 znaků. Synchronizační služba poté otevřete z nabídky Start. Přejděte na spojnice a najděte konektor služby Active Directory. Vyberte ho a klikněte na položku Vlastnosti. Přejděte na stránku přihlašovací údaje a zadejte nové heslo. Kliknutím na tlačítko OK zavřete stránku. |
| V posledním kroku procesu instalace Azure AD Connect zobrazí se chyba oznamující, že nejde nakonfigurovat zpětný zápis hesla. <br> <br> V protokolu událostí aplikace Azure AD Connect obsahuje chyby 32009 s textem "Chyba při získávání auth token". | K této chybě dojde v těchto dvou případů:<br> <br> a. Zadali jste nesprávné heslo pro zadaný na začátku procesu instalace Azure AD Connect účet globálního správce.<br> b. Pokusili jste se použít federovaného uživatele pro účet globálního správce zadaný na začátku procesu instalace Azure AD Connect.<br> <br> Odstranění této chyby, ujistěte se, že nejste pomocí federovaný účet pro globální správce, jste zadali na začátku procesu instalace Azure AD Connect a zadané heslo je správné. |
| V protokolu událostí Azure AD Connect počítače obsahuje chyby 32002 vyvolané PasswordResetService. <br> <br> Chyba čtení: "Chyba připojení k službě sběrnice, zprostředkovatel tokenu se nepodařilo poskytnout token zabezpečení..." | Vaše místní prostředí není možné se připojit k koncový bod service bus v cloudu. Tato chyba je obvykle způsobena pravidlo brány firewall blokuje odchozí připojení na konkrétní port nebo webovou adresu. V tématu [připojení požadavky](./connect/active-directory-aadconnect-prerequisites.md) Další informace. Jakmile jste aktualizovali tato pravidla, restartovat počítač Azure AD Connect a zpětný zápis hesla by se měl spustit znovu funguje. |
| Po synchronizaci pracovní pro některé čas, federovaný nebo hodnoty hash hesla uživatele nelze resetovat vlastní hesla. | Ve výjimečných případech službu zpětný zápis hesla podařit k restartování při restartování Azure AD Connect. V těchto případech nejdřív zkontrolujte, zda se zdá být povoleno v-místním prostředí zpětný zápis hesla To lze provést pomocí Průvodce Azure AD Connect nebo prostředí powershell (HowTos najdete v části výše). Pokud se zobrazí tuto funkci povolit, zkuste povolit nebo zakázat funkci znovu buď prostřednictvím uživatelského rozhraní nebo Powershellu. Pokud to nepomůže, zkuste úplně odinstalovat a znova nainstalovat Azure AD Connect. |
| Federovaný nebo heslo hash synchronizované uživatele, kteří pokus o resetování hesla v chybě po odeslání hesla, což značí, že došlo k chybě služby. <br ><br> Kromě toho během operace resetování hesla, můžete je vidět, že chybu týkající se agenta pro správu byl odepřen přístup v protokolech událostí v místním. | Pokud se zobrazí tyto chyby v protokolu událostí, potvrďte, že účet AD MA (která byla zadaná v průvodci v době konfigurace) musí mít potřebná oprávnění pro zpětný zápis hesla. <br> <br> **Jakmile se toto oprávnění je zadána, může trvat až 1 hodinu oprávnění k skapat dolů prostřednictvím sdprop úlohy na pozadí na řadiči domény.** <br> <br> Oprávnění pro resetování hesel k práci, musí být opatřena popisovač zabezpečení objektu uživatele, jejichž hesla je obnovena. Dokud nebude tato oprávnění se zobrazí na objekt uživatele, resetování hesla i nadále dojde k chybě přístup odepřen. |
| Federovaný nebo heslo hash synchronizované uživatele, kteří pokus o resetování hesla v chybě po odeslání hesla, což značí, že došlo k chybě služby. <br> <br> Kromě toho během operace resetování hesla, může se zobrazit chybu v protokolech událostí ze služby Azure AD Connect indikující chybu "Objekt nebyl nalezen". | Tato chyba obvykle označuje, že synchronizační modul se nepodařilo najít buď objekt uživatele v prostoru konektoru AAD nebo propojené MV nebo objektu prostoru konektoru AD. <br> <br> Chcete-li tento problém odstranit, ujistěte se, že uživatel skutečně synchronizovaných z místní aad prostřednictvím aktuální instance služby Azure AD Connect a zkontrolovat stav objekty v prostor konektoru a MV. Zkontrolujte, zda je objekt služby AD CS konektoru k objektu MV prostřednictvím pravidlo "Microsoft.InfromADUserAccountEnabled.xxx".|
| Federovaný nebo heslo hash synchronizované uživatele, kteří pokus o resetování hesla v chybě po odeslání hesla, což značí, že došlo k chybě služby. <br> <br> Kromě toho během operace resetování hesla, může se zobrazit chybu v protokolech událostí ze služby Azure AD Connect indikující chybu "Nalezeno více odpovídajících". | To znamená, že synchronizační modul zjistil, že objektu MV je připojen k více než jeden objektů služby AD CS prostřednictvím "Microsoft.InfromADUserAccountEnabled.xxx". To znamená, zda má uživatel aktivovaný účet ve více než jedné doménové struktuře. **Tento scénář není podporován pro zpětný zápis hesla.** |
| Operace s heslem se nezdaří s chybou konfigurace. Obsahuje v protokolu událostí aplikace <br> <br> Azure AD Connect chyba 6329 s textem: 0x8023061f (operace se nezdařila, protože na tento Agent pro správu není povolena synchronizace hesel.) | Proběhne, že pokud se změnila konfigurace Azure AD Connect můžete přidat novou doménovou strukturu AD (nebo můžete odebrat a readd existující doménové struktury) po již byla povolena funkce zpětného zápisu hesla. Operace s heslem pro uživatele v například nově přidaná selhání doménové struktury. Chcete-li problém vyřešit, zakažte a znovu povolit funkci zpětného zápisu hesla, až dokončíte změny konfigurace doménové struktury. |

## <a name="password-writeback-event-log-error-codes"></a>Kódy chyb v protokolu událostí zpětný zápis hesla

Osvědčeným postupem při řešení potíží s zpětný zápis hesla je ke kontrole tohoto protokolu událostí aplikace na počítači pro Azure AD Connect. Tento protokol událostí neobsahuje události ze dvou zdrojů, které vás zajímají pro zpětný zápis hesla. Zdroj PasswordResetService popisuje operace a problémy související s operaci zpětného zápisu hesla. Zdroj ADSync popisuje operace a problémy související s nastavení hesla v prostředí AD.

### <a name="source-of-event-is-adsync"></a>Zdroj události je ADSync

| Kód | Název/zprávy | Popis |
| --- | --- | --- |
| 6329 | NICHŽ: MMS(4924) 0x80230619 – "omezení zabrání heslo se změnil na aktuální zadanému." | K této události dojde, když se pokusí nastavení hesla na vaše místní adresář, který nesplňuje stáří hesla, historie, složitost nebo filtrování požadavky domény zpětný zápis hesla služby. <br> <br> Pokud máte nejmenší stáří hesla a v nedávné době změnili heslo v rámci dané okno času, nemůžete se moct změnit heslo znovu, dokud nedosáhne zadané stáří ve vaší doméně. Pro účely testování by měla minimální věk nastavena na hodnotu 0. <br> <br> Pokud máte požadavky na heslo historie povolena, pak je nutné vybrat heslo, které nebyl použit v posledních N krát, kde N je nastavení historie hesel. Pokud vyberete heslo, které používá se v poslední dobu N, potom můžete zobrazit informace o selhání v tomto případě. Pro účely testování historie musí být nastavená na hodnotu 0. <br> <br> Pokud máte požadavky na složitost hesla, všechny z nich se vynucují, když se uživatel pokusí o změnu nebo resetování hesla. <br> <br> Pokud máte povolené filtry heslo a uživatel vybere heslo, které splňují kritéria filtrování, pak resetovat nebo změnit operace selže. |
| 6329 | MMS(3040): admaexport.cpp(2837): server neobsahuje ovládacího prvku LDAP heslo zásad. | K tomuto problému dochází, pokud LDAP_SERVER_POLICY_HINTS_OID řízení (1.2.840.113556.1.4.2066) není povoleno na řadičích domény. Pokud chcete používat funkce zpětného zápisu hesla, je nutné povolit ovládacího prvku. Uděláte to tak, musí být řadiče domény na Windows Server 2008 (s nejnovější SP) nebo novější. Pokud jsou vaše řadiče domény v 2008 (pre-R2), tak také musíte použít opravu hotfix [KB2386717](http://support.microsoft.com/kb/2386717). |
| HR 8023042 | Synchronizační modul vrácena chyba hr = 80230402, zpráva = pokus o získání objektu se nezdařilo, protože existují duplicitní položky s stejné ukotvení | K této události dojde, pokud stejné id uživatele je povoleno ve více doménách. Například pokud se synchronizuje doménových struktur účtu nebo prostředků a mít stejné id uživatele existuje a povolené v každé, této chybě může dojít. <br> <br> Tato chyba může vyskytnout, pokud používáte atribut jedinečný anchor (jako alias, nebo hlavní název uživatele) a dva uživatelé sdílet tento stejný atribut ukotvení. <br> <br> K vyřešení tohoto problému, zkontrolujte, že nemáte žádné duplicitní uživatele v rámci domény a, že používáte atribut ukotvení jedinečný pro každého uživatele. |

### <a name="source-of-event-is-passwordresetservice"></a>Zdroj události je PasswordResetService

| Kód | Název/zprávy | Popis |
| --- | --- | --- |
| 31001 | PasswordResetStart | Tato událost označuje, že místní službu zjistil, že žádost o pocházející z cloudu hash synchronizované uživatele federované nebo heslo pro resetování hesla. Tato událost je, že první událost v každé heslo resetovat operace zpětného zápisu. |
| 31002 | PasswordResetSuccess | Tato událost označuje, že vybraný uživatel nové heslo během operace resetování hesla zjistili jsme, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byla úspěšně zapsána zpět do místního prostředí AD. |
| 31003 | PasswordResetFail | Tato událost označuje, že vybraný uživatel heslo a že hesla byly úspěšně přijaty do místního prostředí, ale když jsme se pokusili nastavení hesla v místním prostředí AD, došlo k selhání. To může dojít z několika důvodů: <br> <br> Pro uživatele heslo nesplňuje stáří, historie, složitost, nebo filtrovat požadavky pro doménu. Zkuste tento problém vyřešíte tak zcela nové heslo. <br> <br> Účtu služby MA nemá příslušná oprávnění k nastavení nového hesla pro uživatelský účet v. <br> <br> Účet uživatele je v chráněné skupiny, jako je například domény nebo enterprise admins, která zakáže heslo množinové operace. |
| 31004 | OnboardingEventStart | Tato událost v případě povolení zpětného zápisu hesla službou Azure AD Connect a určuje, že jsme spuštěné registrace vaší organizace k webové službě zpětný zápis hesla. |
| 31005 | OnboardingEventSuccess | Tato událost ukazuje procesu registrace byla úspěšná a že schopností zpětný zápis hesla je připravený k použití. |
| 31006 | ChangePasswordStart | Tato událost označuje, že místní služby byla nalezena žádost o změnu hesla pro federované nebo heslo uživatele hash synchronizované pocházející z cloudu. Tato událost je první událost v každé operace zpětného zápisu změny hesla. |
| 31007 | ChangePasswordSuccess | Tato událost označuje, že uživatel vybrané nové heslo během operace změny hesla zjistili jsme, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byla úspěšně zapsána zpět do místního prostředí AD. |
| 31008 | ChangePasswordFail | Tato událost označuje, že vybraný uživatel heslo a že hesla byly úspěšně přijaty do místního prostředí, ale když jsme se pokusili nastavení hesla v místním prostředí AD, došlo k selhání. To může dojít z několika důvodů: <br> <br> Pro uživatele heslo nesplňuje stáří, historie, složitost, nebo filtrovat požadavky pro doménu. Zkuste tento problém vyřešíte tak zcela nové heslo. <br> <br> Účtu služby MA nemá příslušná oprávnění k nastavení nového hesla pro uživatelský účet v. <br> <br> Účet uživatele je v chráněné skupiny, jako je například domény nebo enterprise admins, která zakáže heslo množinové operace. |
| 31009 | ResetUserPasswordByAdminStart | Místní služba zjistil požadavek federované pro resetování hesla nebo hodnoty hash hesla synchronizované uživatele pocházející z správce jménem uživatele. Tato událost je první událost v každé operace zpětného zápisu resetování hesla spouštěná správce. |
| 31010 | ResetUserPasswordByAdminSuccess | Správce vybraná během operace resetování hesla správce spouštěná nové heslo, zjistili jsme, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byla úspěšně zapsána zpět do místního prostředí AD. |
| 31011 | ResetUserPasswordByAdminFail | Správce vybrané heslo jménem uživatele a toto heslo byly úspěšně přijaty do místního prostředí, ale když jsme se pokusili nastavení hesla v místním prostředí AD, došlo k selhání. To může dojít z několika důvodů: <br> <br> Pro uživatele heslo nesplňuje stáří, historie, složitost, nebo filtrovat požadavky pro doménu. Zkuste tento problém vyřešíte tak zcela nové heslo. <br> <br> Účtu služby MA nemá příslušná oprávnění k nastavení nového hesla pro uživatelský účet v. <br> <br> Účet uživatele je v chráněné skupiny, jako je například domény nebo enterprise admins, která zakáže heslo množinové operace. |
| 31012 | OffboardingEventStart | Tato událost dochází, pokud zakážete zpětný zápis hesla s Azure AD Connect a určuje, že jsme spuštěné odpojení vaší organizace k webové službě zpětný zápis hesla. |
| 31013| OffboardingEventSuccess| Tato událost označuje proces odpojení bylo úspěšné a že schopností zpětný zápis hesla se úspěšně vypnul. |
| 31014| OffboardingEventFail| Tato událost označuje, že proces odpojení nebyla úspěšná. Může to být kvůli chybě oprávnění na cloudu nebo na místní účet správce zadaný během konfigurace, nebo proto, že se pokoušíte použít globální správce federované cloudu při zakazování zpětný zápis hesla. Chcete-li odstranit tento problém, zkontrolujte na správu oprávnění a že nepoužíváte žádné federovaný účet při konfiguraci funkce zpětného zápisu hesla.|
| 31015| WriteBackServiceStarted| Tato událost označuje, že služba zpětný zápis hesla byla úspěšně spuštěna a je připravena přijímat žádosti o správu hesel z cloudu.|
| 31016| WriteBackServiceStopped| Tato událost označuje, že služba zpětný zápis hesla byla zastavena a všechny žádosti správu hesel z cloudu nebude úspěšná.|
| 31017| AuthTokenSuccess| Tato událost označuje, že jsme úspěšně načíst autorizační token pro globální správce. zadaný při instalaci Azure AD Connect k zahájení procesu odpojení nebo registrace.|
| 31018| KeyPairCreationSuccess| Tato událost označuje, že jsme vytvořili úspěšně heslo šifrovací klíč, který se používá k šifrování hesla z cloudu k odeslání do místního prostředí.|
| 32000| Neznámé chyby| Tato událost ukazuje na neznámou chybu během operace správy heslo. Podívejte se na text výjimky v události pro další podrobnosti. Pokud máte problémy, zkuste zakázání a opětovného povolení zpětného zápisu hesla. Pokud to nepomůže, obsahovat kopii protokolu událostí společně s zadané id sledování zevnitř na pracovníka podpory.|
| 32001| ServiceError| Tato událost označuje došlo k chybě připojení k cloudu heslo pro resetování služby a obecně nastane, když se nemohl připojit k webové službě resetování hesel místní služby.|
| 32002| ServiceBusError| Tato událost vyplývá, že došlo k chybě připojení k instanci sběrnice služby vašeho klienta. Toto může nastat, protože v místním prostředí blokujete odchozí připojení. Zkontrolujte bránu firewall, aby umožňovaly připojení přes TCP 443 a https://ssprsbprodncu-sb.accesscontrol.windows.net/ a akci opakujte. Pokud stále dochází k potížím, zkuste zakázání a opětovného povolení zpětného zápisu hesla.|
| 32003| InPutValidationError| Tato událost znamená, že vstup naše webového rozhraní API služby byla neplatná. Opakujte operaci znovu.|
| 32004| DecryptionError| Tato událost označuje, že došlo k chybě při dešifrování hesla, které byly přijaty z cloudu. Může to být způsobeno dešifrovací klíče neshodu mezi cloudové služby a v místním prostředí. To vyřešit, zakažte a znovu povolte zpětný zápis hesla v místním prostředí.|
| 32005| ConfigurationError| Během registrace uložíme informace specifické pro klienta v konfiguračním souboru v místním prostředí. Tato událost ukazuje došlo k chybě uložení tohoto souboru nebo že při spuštění služby došlo k chybě čtení souboru. Chcete-li tento problém vyřešit, zkuste zakázání a opětovné povolení zpětného zápisu hesla k vynucení přepsání tohoto souboru konfigurace.|
| 32007| OnBoardingConfigUpdateError| Během registrace odešleme data z cloudu na místní heslo pro resetování služby. Tato data se pak zapisovány do souboru v paměti před odesláním do služby sync k bezpečnému ukládání těchto informací na disk. Tato událost znamená problém s zápis nebo aktualizaci dat v paměti. Chcete-li tento problém vyřešit, zkuste zakázání a opětovné povolení zpětného zápisu hesla k vynucení přepsání této konfigurace.|
| 32008| ValidationError| Tato událost označuje, že jsme obdržel neplatnou odezvu z webovou službu resetování hesla. Chcete-li tento problém vyřešit, zkuste zakázání a opětovné povolení zpětného zápisu hesla.|
| 32009| AuthTokenError| Tato událost znamená, že jsme nelze získat o autorizační token pro účet globálního správce zadaný při instalaci Azure AD Connect. Tato chyba může být způsobeno chybné uživatelské jméno nebo heslo zadané pro účet globálního správce nebo federovaný vzhledem k tomu, že zadaný účet globálního správce. Chcete-li tento problém vyřešit, znovu spustit konfiguraci s správné uživatelské jméno a heslo a ujistěte se, že správce spravovaný účet (jenom pro cloud nebo synchronizovat hesla).|
| 32010| CryptoError| Tato událost ukazuje na došlo k chybě při generování hesla šifrovací klíč nebo dešifrovací heslo, které dorazí z cloudové služby. Tuto chybu pravděpodobně označuje potíže s vaším prostředím. Podívejte se na podrobnosti protokolu událostí pro další informace a tento problém vyřešit. Můžete také zkusit akci zakázání a opětovného povolení zpětného zápisu hesla služby to vyřešit.|
| 32011| OnBoardingServiceError| Tato událost označuje, že místní služby nelze správně komunikovat s webovou službu resetování hesla k zahájení procesu registrace. Může to být způsobeno pravidlo brány firewall nebo potížím při získávání tokenu ověřování pro vašeho klienta. Chcete-li odstranit tento problém, ověřte, zda nejsou blokování odchozí připojení přes TCP 443 a TCP 9350-9354 nebo https://ssprsbprodncu-sb.accesscontrol.windows.net/ a že účet správce AAD používáte zařadit do provozu není federovaný.|
| 32013| OffBoardingError| Tato událost označuje, že místní služby nelze správně komunikovat s webovou službu resetování hesla zahájíte proces odpojení. Může to být způsobeno pravidlo brány firewall nebo potížím při získávání autorizační token pro vašeho klienta. Chcete-li odstranit tento problém, zkontrolujte, zda neblokují odchozí připojení přes 443 nebo https://ssprsbprodncu-sb.accesscontrol.windows.net/ a, správce účtu AAD, že používáte k offboard není federovaný.|
| 32014| ServiceBusWarning| Tato událost naznačuje, když jsme se můžete znovu pokusit připojit k instanci sběrnice služby vašeho klienta. Za normálních podmínek to by nemělo být žádný problém, ale pokud se zobrazí tato událost mnohokrát, zkuste zkontrolovat síťové připojení k service bus, zvlášť pokud je to vysokou latencí nebo připojení s malou šířkou pásma.|
| 32015| ReportServiceHealthError| Chcete-li sledovat stav služby zpětný zápis hesla, jsme odesílání prezenčního signálu data na našich heslo resetovat webové služby každých 5 minut. Tato událost označuje, že došlo k chybě při odesílání této informace o stavu zpět k webové službě cloud. Tato informace o stavu nezahrnuje OII nebo PII data a je čistě prezenčního signálu a služby na úrovni basic statistiky tak, aby můžeme poskytnout informace o stavu služby v cloudu.|
| 33001| ADUnKnownError| Tato událost ukazuje na to, že došlo k neznámé chybě službou Active Directory, zkontrolujte události ze zdroje ADSync pro další informace o této chybě v protokolu událostí serveru Azure AD Connect.|
| 33002| ADUserNotFoundError| Tato událost označuje, že uživatel, který se pokouší resetovat nebo změnit heslo nebyl nalezen v místním adresáři. Tato situace může nastat, když uživatel byl odstraněný místní, ale není v cloudu, nebo pokud nastane problém se synchronizací. Najdete v protokolech synchronizace a poslední synchronizace několik spusťte podrobnosti o další informace.|
| 33003| ADMutliMatchError| Když pro vytvoření nového hesla nebo žádost o změnu pochází z cloudu, použijeme ukotvení cloudu zadaný během procesu instalace služby Azure AD Connect určit, jak propojit tento požadavek zpět na uživatele v místním prostředí. Tato událost označuje, že jsme nalezena dva uživatelé ve vaší místní adresář se stejným atributem ukotvení cloudu. Najdete v protokolech synchronizace a poslední synchronizace několik spusťte podrobnosti o další informace.|
| 33004| ADPermissionsError| Tato událost značí, že účet agenta pro správu služby nemá příslušná oprávnění na dotyčném nastavit nové heslo účtu. Zajistěte, aby MA účet v doménové struktuře uživatele resetování a změn hesel oprávnění pro všechny objekty v doménové struktuře. Další informace o tom, udělat na to, najdete v kroku 4: nastavení příslušných oprávnění služby Active Directory.|
| 33005| ADUserAccountDisabled| Tato událost označuje, že jsme se pokusili resetovat nebo změnit heslo pro účet, který byl zakázán místně. Zapnout účet a operaci opakujte.|
| 33006| ADUserAccountLockedOut| Událost označuje, že jsme se pokusili resetovat nebo změnit heslo pro účet, který byl uzamčen místně. Uzamčení může dojít, když má uživatel o změnu nebo resetování hesla operaci příliš mnohokrát v krátké době. Odemknout účet a operaci opakujte.|
| 33007| ADUserIncorrectPassword| Tato událost označuje, zda uživatel zadal nesprávný aktuální heslo při provádění heslo změnit operaci. Zadejte správné aktuální heslo a zkuste to znovu.|
| 33008| ADPasswordPolicyError| K této události dojde, když se pokusí nastavení hesla na vaše místní adresář, který nesplňuje stáří hesla, historie, složitost nebo filtrování požadavky domény zpětný zápis hesla služby. <br> <br> Pokud máte nejmenší stáří hesla a v nedávné době změnili heslo v rámci dané okno času, nemůžete se moct změnit heslo znovu, dokud nedosáhne zadané stáří ve vaší doméně. Pro účely testování by měla minimální věk nastavena na hodnotu 0. <br> <br> Pokud máte požadavky na heslo historie povolena, pak je nutné vybrat heslo, které nebyl použit v posledních N krát, kde N je nastavení historie hesel. Pokud vyberete heslo, které používá se v poslední dobu N, potom můžete zobrazit informace o selhání v tomto případě. Pro účely testování historie musí být nastavená na hodnotu 0. <br> <br> Pokud máte požadavky na složitost hesla, všechny z nich se vynucují, když se uživatel pokusí o změnu nebo resetování hesla. <br> <br> Pokud máte povolené filtry heslo a uživatel vybere heslo, které splňují kritéria filtrování, pak resetovat nebo změnit operace selže.|
| 33009| ADConfigurationError| Tato událost označuje, že došlo k problému zápis heslo zpět do místního adresáře z důvodu konfigurace problém se službou Active Directory. Zkontrolujte protokol událostí aplikace na Azure AD Connect počítač pro zprávy ze služby ADSync Další informace o jaké došlo k chybě.|

## <a name="troubleshoot-password-writeback-connectivity"></a>Řešení potíží s připojením zpětný zápis hesla

Pokud dochází k přerušení poskytování služeb s komponentou zpětný zápis hesla služby Azure AD Connect, zde jsou některé rychlé kroky, které můžete provést to vyřešit:

* [Zkontrolujte připojení k síti](#confirm-network-connectivity)
* [Restartování Azure AD Connect synchronizační služby](#restart-the-azure-ad-connect-sync-service)
* [Zakázat a znovu povolit funkci zpětného zápisu hesla](#disable-and-re-enable-the-password-writeback-feature)
* [Nainstalovat nejnovější verzi Azure AD Connect](#install-the-latest-azure-ad-connect-release)
* [Řešení potíží se zpětným zápisem hesla](#troubleshoot-password-writeback)

Obecně platí doporučujeme provést tyto kroky v pořadí výše způsobem nejvíce rychlé obnovení služby.

### <a name="confirm-network-connectivity"></a>Zkontrolujte připojení k síti

Nejběžnější bodem selhání je, aby brána firewall a nebo jsou nesprávně nakonfigurované porty proxy a časové limity nečinnosti. Přečtěte si požadavky připojení v článku [požadavky pro Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) Další informace.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Restartování Azure AD Connect synchronizační služby

Restartování službu Azure AD Connect Sync vám může pomoct vyřešit problémy s připojením nebo jiné přechodné problémy se službou.

1. Jako správce, klikněte na tlačítko **spustit** na serveru se systémem **Azure AD Connect**.
2. Typ **"services.msc"** vyhledávacího pole a stiskněte **Enter**.
3. Vyhledejte **Microsoft Azure AD Sync** položku.
4. Klikněte pravým tlačítkem na položku služby, klikněte na tlačítko **restartujte**a počkejte na dokončení operace.

   ![Restartujte službu Azure AD Sync][Service Restart]

Tyto kroky znovu navázat připojení s cloudovou službou a vyřešte všechny přerušení, které máte. Pokud restartování služby synchronizace váš problém nevyřeší, doporučujeme pokusu zakázat a znovu povolit funkci zpětného zápisu hesla jako další krok.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Zakázat a znovu povolit funkci zpětného zápisu hesla

Zakázání a opětovného povolení zpětného zápisu hesla funkce vám může pomoct vyřešit problémy s připojením.

1. Jako správce, otevřete **Průvodce konfigurací služby Azure AD Connect**.
2. Na **připojit k Azure AD** dialogové okno, zadejte vaše **přihlašovací údaje globálního správce Azure AD**
3. Na **připojit ke službě AD DS** dialogové okno, zadejte vaše **přihlašovací údaje Správce služby AD Domain Services**.
4. Na **Jednoznačná identifikace uživatelů** dialogové okno, klikněte na **Další** tlačítko.
5. Na **volitelné funkce** dialogové okno, zrušte zaškrtnutí políčka **zpětný zápis hesla** zaškrtávací políčko.
6. Klikněte na tlačítko **Další** prostřednictvím zbývající stránky dialogové okno beze změny jakéhokoli až na **připraveno ke konfiguraci** stránky.
7. Ujistěte se, že konfigurace stránky zobrazuje **možnost zpětný zápis hesla jako zakázané** a pak klikněte na tlačítko zeleným **konfigurace** tlačítko potvrzení změny.
8. Na **dokončeno** dialogové okno, zrušte výběr **synchronizovat** a potom klikněte na **Dokončit** zavřete průvodce.
9. Otevřete **Průvodce konfigurací služby Azure AD Connect**.
10. **Opakujte kroky 2-8**, s výjimkou si zajistěte **zaškrtněte možnost zpětný zápis hesla** na **volitelné funkce** službu znovu povolit na obrazovce.

Tyto kroky znovu navázat připojení s naší cloudové služby a vyřešte přerušení, které máte.

Pokud zakázat a znovu povolit funkci zpětného zápisu hesla váš problém nevyřeší, doporučujeme pokusí znovu nainstalovat Azure AD Connect jako další krok.

### <a name="install-the-latest-azure-ad-connect-release"></a>Nainstalovat nejnovější verzi Azure AD Connect

Konfigurace a problémy s připojením mezi naší cloudové služby a místního prostředí AD může vyřešit opětovné instalace Azure AD Connect.

Doporučujeme, tento krok provést až po pokusu o první dva kroky popsané výše.

> [!WARNING]
> Pokud jste upravili mimo pole synchronizační pravidla, **tyto zálohování před pokračováním upgradu a znovu je ručně, až budete hotovi**.

1. Stáhnout nejnovější verzi služby Azure AD Connect z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
2. Vzhledem k tomu, že jste již nainstalovali Azure AD Connect, budete muset provést místní upgrade k aktualizaci instalace Azure AD Connect na nejnovější verzi.
3. Spuštění staženého balíčku a postupujte podle na obrazovce pokyny aktualizujte počítač Azure AD Connect.

Tyto kroky znovu navázat připojení s naší cloudové služby a vyřešte všechny přerušení, které máte.

Pokud instalaci nejnovější verze server Azure AD Connect váš problém nevyřeší, doporučujeme vám, zkuste zakázání a opětovné povolení zpětného zápisu hesla v posledním kroku po instalaci nejnovější verze.

## <a name="verify-whether-azure-ad-connect-has-the-required-permission-for-password-writeback"></a>Ověřte, zda má Azure AD Connect požadované oprávnění pro zpětný zápis hesla
Azure AD Connect vyžaduje AD **resetovat heslo** oprávnění k provedení zpětný zápis hesla. Chcete zjistit, jestli Azure AD Connect má oprávnění pro danou místní AD uživatelský účet, můžete použít funkci Windows efektivní oprávnění:

1. Přihlaste se k serveru Azure AD Connect a spustit **Synchronization Service Manager** (Start → synchronizace Service).
2. V části **konektory** vyberte místní **AD connector.** a klikněte na tlačítko **vlastnosti**.  
![Efektivní oprávnění – krok 2](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
3. V dialogovém okně automaticky otevírané okno, vyberte **připojit k doménové struktuře služby Active Directory** kartě a poznamenejte si **uživatelské jméno** vlastnost. Toto je účet služby AD DS použít k provedení synchronizace adresářů přes Azure AD Connect. Azure AD Connect provést zpětný zápis hesla musí mít účet služby AD DS oprávnění resetovat heslo.  
![Efektivní oprávnění – krok 3](./media/active-directory-passwords-troubleshoot/checkpermission02.png) 
4. Přihlaste se k řadiči domény místní a spustit **Active Directory Users and Computers** aplikace.
5. Klikněte na tlačítko **zobrazení** a zajistěte, aby **upřesňující funkce** je povolena možnost.  
![Efektivní oprávnění – krok 5](./media/active-directory-passwords-troubleshoot/checkpermission03.png) 
6. Podívejte se na AD uživatelský účet, který chcete ověřit. Klikněte pravým tlačítkem na účet a vyberte **vlastnosti**.  
![Efektivní oprávnění – krok 6](./media/active-directory-passwords-troubleshoot/checkpermission04.png) 
7. V dialogovém okně automaticky otevírané okno, přejděte na **zabezpečení** a klikněte na **Upřesnit**.  
![Efektivní oprávnění - krok 7](./media/active-directory-passwords-troubleshoot/checkpermission05.png) 
8. V dialogovém okně Upřesnit nastavení zabezpečení automaticky otevírané okno, přejděte na **platný přístup** kartě.
9. Klikněte na **vyberte uživatele** a vyberte účet služby AD DS používaný službou Azure AD Connect (viz krok 3). Pak klikněte na tlačítko **zobrazit platný přístup**.  
![Efektivní oprávnění - kroku 9](./media/active-directory-passwords-troubleshoot/checkpermission06.png) 
10. Posuňte se dolů a vyhledejte **resetovat heslo**. Pokud položka je zaškrtnuto, znamená to, zda má účet služby AD DS oprávnění resetovat heslo vybraného účtu uživatele AD.  
![Efektivní oprávnění - krok 10](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Fóra pro Azure AD

Pokud máte obecný dotaz týkající se Azure AD a resetování hesla pomocí samoobslužné služby, můžete požádat komunitou o pomoc na [fóra Azure AD](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Členové komunity služby zahrnují Engineers, správce produktu, MVP a hlavě nehodí Odborníci v oblasti IT.

## <a name="contact-microsoft-support"></a>Obraťte se na podporu společnosti Microsoft

Pokud nemůžete najít odpověď na problém jsou vždy k dispozici další vám pomůže našimi týmy podpory.

Správně, jsme položte zadejte množství podrobností nejdříve při otevírání případu včetně:

* **Obecný popis chyby** -co je chyba? Jak se chování, které bylo zaznamenali? Jak jsme chybu reprodukovat? Nejdříve zadejte množství podrobností.
* **Stránka** – které stránce jste byli na když jste si všimli chybu? Uveďte adresu URL, pokud budete moci a snímek.
* **Podpora kódu** – jak se vygeneruje, když uživatel viděli chyba kód podporu?
    * To najdete reprodukujte chyba, potom klikněte na odkaz podporují kód v dolní části obrazovky a odeslat pracovník podpory identifikátor GUID, která způsobí, že.
    ![Podpora kód v dolní části obrazovky][Support Code]
    * Pokud jste na stránce bez podpory kódu v dolní části, stiskněte klávesu F12 a vyhledejte SID a CID a odesílat tyto dva výsledky pracovníkovi podpory.
* **Datum, čas a časové pásmo** -uveďte přesné datum a čas **s časové pásmo** , k níž došlo k chybě.
* **ID uživatele** -kdo byl uživatel, který viděli chybu? (user@contoso.com)
    * Je to federovaného uživatele?
    * Je toto heslo hash synchronizované uživatele?
    * Je to cloudu jenom uživatele?
* **Licencování** -má uživatel přiřazenou licenci Azure AD Premium nebo Azure AD Basic?
* **Protokolu událostí aplikace** – Pokud používáte zpětný zápis hesla a chyba je v infrastruktuře můžete místní kopii protokolu událostí aplikací ze serveru Azure AD Connect komprimované vložit při kontaktování podpory.



[Service Restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Restartujte službu Azure AD Sync"
[Support Code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Podpora kódu se nachází v pravém dolním rohu okna"

## <a name="next-steps"></a>Další kroky

Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [Jak dokončení úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetovat nebo změnit heslo](active-directory-passwords-update-your-own-password.md).
* [Registrace pro resetování hesla pomocí samoobslužné služby](active-directory-passwords-reset-register.md).
* [Máte dotaz Licensing?](active-directory-passwords-licensing.md)
* [Jaká data používá SSPR a jaká data by měla můžete naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Jaké metody ověřování jsou k dispozici pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad s SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč je starat o něm?](active-directory-passwords-writeback.md)
* [Jak sestavy na aktivitu v SSPR](active-directory-passwords-reporting.md)
* [Co jsou všechny možnosti v SSPR a co znamená, že?](active-directory-passwords-how-it-works.md)
* [Je nutné zadat otázku, která nebyla někde zahrnutých, jinak](active-directory-passwords-faq.md)
