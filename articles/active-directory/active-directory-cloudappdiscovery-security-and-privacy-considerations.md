---
title: "Cloud App Discovery zabezpečení a důležité informace o ochraně osobních údajů | Microsoft Docs"
description: "Toto téma popisuje aspekty zabezpečení a ochrany osobních údajů související s Cloud App Discovery."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 2fce5c82-d3de-4097-808f-40214768df9e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: cec3d2cb02dd34dd5ac631e572936cfd7c8de033
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery zabezpečení a důležité informace o ochraně osobních údajů
Toto téma vysvětluje, jak data jsou shromažďována, zpracování a zabezpečené v rámci Azure Active Directory Cloud App Discovery. Společnost Microsoft se zavazuje chránit vaše osobní údaje a zabezpečení vašich dat. Microsoft dodržuje zabezpečit vývoj softwaru životního cyklu postupy pro provoz služby. Zabezpečení a ochrana dat je nejvyšší prioritou ve společnosti Microsoft.

> [!TIP] 
> Podívejte se nové bez agenta Cloud App Discovery v Azure Active Directory (Azure AD), které jsou vylepšit určením [integraci s Microsoft Cloud App Security](https://portal.cloudappsecurity.com). 

## <a name="overview"></a>Přehled
Cloud App Discovery je funkce služby Azure AD a je hostován v Microsoft Azure.  
Cloud App Discovery endpoint agent se používá ke shromažďování dat zjišťování aplikací z IT spravovaných počítačů. Shromážděná data posílá bezpečně přes šifrovaný kanál služby Azure AD Cloud App Discovery. Cloud App Discovery data v organizaci, je pak viditelný na portálu Azure. 

![Jak funguje Cloud App Discovery.](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png) 

V následujících částech použijte zabezpečené toku informací z vaší organizace ke službě Cloud App Discovery a nakonec i na portálu Cloud App Discovery.

## <a name="collecting-data-from-your-organization"></a>Shromažďování dat z vaší organizace
Chcete-li získat přehled o aplikace používané zaměstnanci ve vaší organizaci pomocí funkce zjišťování cloudové aplikace Azure Active Directory, je třeba nejprve nasadit Azure AD Cloud App Discovery endpoint agent. k počítačům ve vaší organizaci.

Správci klienta Azure Active Directory (nebo jejich delegáta) mohou stáhnout instalační balíček agenta z portálu Azure. Agenta můžete buď ručně nainstalovány nebo nainstalován napříč více počítačů v organizaci pomocí SCCM nebo zásad skupiny.

Další pokyny o možnostech nasazení najdete v tématu [cloudové aplikace zjišťování skupiny zásad Deployment Guide](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).


### <a name="data-collected-by-the-agent"></a>Údaje shromážděné agentem
Informace uvedené v následujícím seznamu se shromažďují pomocí agenta po připojení k webové aplikaci. Informace se shromažďují pouze u aplikací, které je správce nakonfigurován pro zjišťování. Můžete upravit seznam cloudové aplikace, které agent monitoruje prostřednictvím nástroje Cloud App Discovery ve službě Azure Active Directory Microsoft [portál Azure](https://portal.azure.com/)v části **nastavení**->**shromažďování dat**  -> **Seznam kolekcí aplikace**. 

**Informace o kategorii**: informace o uživateli  
**Popis**: uživatelské jméno systému Windows procesu, který vytvořil žádost k cíli webové aplikace (například doména\uživatelské jméno) a také Windows identifikátor zabezpečení (SID) uživatele.

**Informace o kategorii**: zpracovat informace  
**Popis**: název procesu, který vytvořil požadavek do cílové webové aplikace (například iexplore.exe)

**Informace o kategorii**: počítač informace  
**Popis**: na počítači, na kterém je nainstalován agent názvu rozhraní NetBIOS.

**Informace o kategorii**: informace o provozu aplikaci.  
**Popis**: následující informace o připojení:

* Zdroj (místní počítač) a cílové IP adresy a čísla portů
* Veřejnou IP adresu organizace, přes které prochází žádosti.
* Čas požadavku
* Objem provozu, odesílání a přijímání
* Verze IP (4 nebo 6)
* Pro připojení protokolu TLS pouze: název hostitele cílového z rozšíření indikace názvu serveru nebo certifikát serveru.

Následující informace protokolu HTTP:

* Metoda (GET, POST atd.)
* Protokol (HTTP/1.1, atd.)
* Řetězec uživatelského agenta
* Název hostitele
* Cílový identifikátor URI (s výjimkou řetězce dotazu)
* Informace o typu obsahu
* Informace o adresách URL odkazující server (s výjimkou řetězce dotazu)

> [!NOTE]
> Výše uvedené HTTP informace se shromažďují pro všechna připojení bez šifrování.
> Pro připojení protokolu TLS se tyto informace zaznamená pouze při nastavení, při hloubkové kontrole' zapnuté na portálu. Toto nastavení je ve výchozím nastavení "ON".
> Další podrobnosti najdete v tématu níže, a [získávání spuštěna s Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 

Kromě dat, které agent shromažďuje o síťové aktivity je také shromažďuje anonymní informace o
* Konfigurace softwaru a hardwaru
* Zprávy o chybách
* Údaje o tom, jak je používán agent.


### <a name="how-the-agent-works"></a>Jak funguje agenta
Instalace agenta obsahuje dvě součásti:

* Součást uživatelského režimu
* Součást ovladače režimu jádra (ovladače Windows Filtering Platform)

Při první instalaci agenta ukládá specifické pro počítač důvěryhodný certifikát na počítači, který pak použije k vytvoření zabezpečeného spojení se službou Cloud App Discovery. Agent pravidelně načte konfiguraci zásad ze služby Cloud App Discovery přes tento zabezpečené připojení. Zásady obsahuje informace o které cloudové aplikace pro monitorování a jestli automatické aktualizace by měla být povolená, mimo jiné.

Jako webový provoz se odesílají a přijímají na počítači z Internet Exploreru a Chrome, Cloud App Discovery agent analyzuje provoz a extrahuje relevantní metadata (najdete v článku **Data shromážděná pomocí agenta** část výše).  
Každou minutu, agent odešle shromážděných metadat ke službě Cloud App Discovery přes šifrovaný kanál.

Součást ovladače zachycuje šifrovaný provoz a sám vloží do šifrované datového proudu. Další podrobnosti najdete **zachycení dat ze šifrované připojení (při hloubkové kontrole)** části níže.

### <a name="respecting-user-privacy"></a>Bere ohledy na soukromí uživatelů
Naším cílem je zajistit správci nástroje pro nastavení rovnováhu mezi podrobné kabel do aplikace využití a uživatele o ochraně osobních údajů v závislosti na jejich organizace. Za tímto účelem poskytujeme následující knoflíky na stránce nastavení na portálu:

* **Shromažďování dat**: mohou správci zadejte kategorie aplikací, které chtějí získat data zjišťování nebo aplikace.
* **Při hloubkové kontrole**: Správci můžete zvolit k určení, pokud agent shromažďuje přenos HTTP pro připojení protokolem SSL/TLS (neboli **'Hloubkové kontrole'**). Další informace o to v další části.
* **Souhlas možnosti**: Správci mohou pomocí portálu Cloud App Discovery. Vyberte, zda chcete upozornit uživatele shromažďování dat pomocí agenta, a jestli uživatel musí vyjádřit souhlas. před spuštěním agenta shromažďování dat uživatele.

Cloud App Discovery endpoint agent shromažďuje informace popsané v jenom **Data shromážděná pomocí agenta** část výše.

### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Zachycení dat ze šifrované připojení (při hloubkové kontrole)
Jak jsme už zmínili dřív, správci mohou nakonfigurovat agenta monitorování dat z šifrované připojení ('hloubkové kontrole'). Protokol TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) je jedním nejběžnější protokolů v použití na Internetu ještě dnes. Šifrování komunikace s TLS, klient může vytvořit zabezpečený a privátní komunikaci s webového serveru; Protokol TLS poskytuje základní ochranu pro předávání pověření ověřování a zabránit úniku citlivých informací.

Při začátku do konce zabezpečený šifrovaný kanál poskytované TLS umožňuje důležité zabezpečení a ochrana osobních údajů, protokol je často zneužít pro účely škodlivý nebo nefarious. Mnohem tedy ve skutečnosti, že protokol TLS je často označuje jako (universal obcházení brány firewall protocol). Kořenové problému je, že většinu bran firewall nelze zkontrolovat komunikace TLS, protože aplikační vrstvě data se šifrují pomocí SSL. Zároveň budete vědět, útočníci často využívají TLS k poskytování zlými úmysly datových částí uživateli přesvědčeni, že jsou úplně skrytá TLS i nejvíce inteligentního aplikační vrstvě bránu firewall a musí jednoduše předávání TLS komunikace mezi hostiteli. Koncoví uživatelé často využívají TLS obejít vynucené podle podnikových bran firewall a proxy servery, řízení přístupu pomocí připojit veřejná proxy a pro protokoly – protokol TLS přes bránu firewall, která může být jinak blokovaly zásadami pro tunelové propojení.

Při hloubkové kontrole může agent Cloud App Discovery tak, aby fungoval jako důvěryhodné man-in-the-middle. Pokud požadavek klienta Přišla žádost o přístup prostředku HTTPS chráněné, ovladač Endpoint Agent zachycuje připojení a určuje, že nové připojení k cílovému serveru k načte svůj certifikát SSL jménem klienta. Agent pak ověřuje, že certifikát důvěryhodné (Kontrola, zda nebyl odvolaný a provádět další kontroly certifikátu), a pokud tyto průchodu Endpoint Agent pak zkopíruje informace z certifikátu serveru a vytvoří vlastní server certifikát – známý jako certifikát zachycení – pomocí těchto informací. Zachycení certifikát je podepsaný na průběžně endpoint Agent kořenový certifikát, který je nainstalován v úložišti důvěryhodných certifikátů systému Windows. Tento certifikát podepsaný svým držitelem kořenové je označený jako jiný exportovatelný a seznamu ACL museli správci. Je určena k nikdy neopustí počítače, v němž byla vytvořena. Když koncového uživatele je klientská aplikace obdrží certifikát zachycení, bude ho ho důvěřujete, protože může úspěšně ověřit řetěz certifikátů, abyste kořenový certifikát. Tento proces je ve většině případů transparentní z Koncoví uživatele hlediska se několik aspektů, jak je popsáno níže.

Povolením hloubkové kontrole může Cloud App Discovery Endpoint Agent dešifrovat a zkontrolovat TLS zašifrovaná komunikace, umožňuje služba snížení šumu a poskytnou přehled o využití šifrované cloudových aplikací.

#### <a name="a-word-of-caution"></a>Word varování
Před zapnutím hloubkové kontroly, důrazně doporučujeme komunikovat vaše záměry právní a Personálního oddělení a získat jejich souhlasu. Zkontrolujete privátní šifrování komunikace koncový uživatel může být citlivé předmět, zřejmé důvodů. Než produkční zavádění hloubkové kontroly Ujistěte se, že vaše podnikové zabezpečení a byly aktualizovány podmínky použití zásad k označení, že bude prověřovány šifrovanou komunikaci. Oznámení pro uživatele a výjimky webů považují za citlivé (například bankovnictví a lékařské webů) může být také nutné, pokud nakonfigurujete Cloud App Discovery k monitorování je. Jak je uvedeno nahoře, mohou správci portálu Cloud App Discovery zvolte, zda oznámení uživatelům shromažďování dat pomocí agenta a jestli se má vyžadovat souhlas uživatele před spuštěním agenta shromažďování dat uživatele.

### <a name="known-issues-and-drawbacks"></a>Známé problémy a nevýhody
Existuje několik případů, kdy zachycení TLS může mít vliv na prostředí koncového uživatele:

* Rozšířené certifikáty ověření (EV) vykreslit zelená tak, aby fungoval vizuálně indikují, že navštívíte důvěryhodného webu adresního řádku webového prohlížeče. TLS kontroly nelze duplicitní EV v certifikátu, které vystavuje klientovi, aby webové servery, které používají certifikáty EV fungovat normálně, ale nezobrazí adresním řádku zeleně.  
* Veřejné klíče Připnutí (také označované jako Připnutí certifikátu) slouží k ochraně proti útokům man-in-the-middle uživatelů a podvodné certifikačních autorit. Pokud kořenový certifikát pro lokalitu definovaného neodpovídá žádné známé dobré CA, prohlížeč odmítá připojení s chybou. Vzhledem k tomu, že protokol TLS zachycení je ve skutečnosti man-in-the-middle, tato připojení se nezdaří.
* Pokud uživatelé klikněte na ikonu zámku v prohlížeči panelu Adresa prohlížeče zkontrolovat informace o lokalitě, neuvidí řetěz končící na certifikační autorita používaná k podepsání certifikátu webu, ale místo toho důvěryhodný řetěz certifikátů s Windows úložiště certifikátů.

Pokud chcete zkrátit výskyty tyto problémy, jsme sledování od cloudových služeb a aplikací klienta známé použít rozšířené ověření nebo veřejného klíče Připnutí a vyzvat Endpoint Agent. aby se zabránilo brání ovlivněné připojení. I v těchto případech bude nadále získávat sestavy použití těchto cloudových aplikací a objem přenášených dat, ale vzhledem k tomu, že nejsou hloubkové prověřovány, bude k dispozici žádné podrobnosti o použití aplikace byly.

## <a name="sending-data-to-cloud-app-discovery"></a>Odesílání dat do Cloud App Discovery
Jakmile se metadata jsou shromážděná agentem, do mezipaměti na počítači pro až 1 minutu, nebo dokud se data uložená v mezipaměti dosáhne velikosti 5 MB. Pak je komprimované a odešlou přes zabezpečené připojení ke službě Cloud App Discovery.

Pokud agenta nemůže komunikovat se službou Cloud App Discovery z jakéhokoli důvodu, shromážděných metadata se ukládají do mezipaměti místního souboru, který lze přistupovat pouze mohou uživatelé s oprávněním na počítači (například skupiny Administrators).  
Agenta se automaticky pokusí znovu odeslat metadata uložená v mezipaměti, dokud úspěšně byla přijata službou Cloud App Discovery.

## <a name="receiving-the-data-at-the-service-end"></a>Přijetí dat na konci služby
Agenti ověření Cloud App Discovery služby pomocí certifikátu ověřování klienta konkrétní počítač výše uvedené a předává data přes šifrovaný kanál.  
Službě Cloud App Discovery analytického kanálu zpracovávání metadata pro každého zákazníka a to samostatně logicky vytváření oddílů všechny fázemi kanálu analýzy.
Analyzovaných metadata řídí různé sestavy na portálu.

Nezpracované metadata a analyzovaných metadata jsou uloženy po dobu 180 dnů. Kromě toho tato volba se zaznamenat analyzovaných metadat v účtu úložiště objektů blob v Azure podle vlastního uvážení.
To je užitečné pro offline analýzu metadat, jakož i déle uchovávání dat.

## <a name="accessing-the-data-using-the-azure-portal"></a>Přístup k datům pomocí portálu Azure
Ve snaze zabezpečit metadata shromážděných ve výchozím nastavení jenom globální Správci klienta mít přístup k funkci Cloud App Discovery na portálu Azure.  
Však mohou správci delegovat tento přístup k jiné uživatele nebo skupiny.

> [!NOTE]
> Další podrobnosti najdete v tématu [získávání spuštěna s Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 


Každý uživatel přístup k datům na portálu, musí mít licenci s licencí Azure AD Premium.

## <a name="additional-resources"></a>Další zdroje
* [Jak může zjišťovat nedovolené cloudové aplikace, které se používají v rámci Moje organizace](active-directory-cloudappdiscovery-whatis.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

