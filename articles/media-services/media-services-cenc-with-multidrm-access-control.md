---
title: "Šifrování CENC s více technologiemi DRM a řízením přístupu: referenční návrhu a implementace v Azure a Azure Media Services | Microsoft Docs"
description: "Další informace o tom k licencování Microsoft® technologie Smooth Streaming klienta portování Kit."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 7814739b-cea9-4b9b-8370-538702e5c615
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;kilroyh;yanmf;juliako
ms.openlocfilehash: 730917b6859f8dbd800ef2cb141062f45d7779ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC využívající Multi-DRM a Access Control: Referenční návrh a implementace v prostředí Azure a Azure Media Services
 
## <a name="introduction"></a>Úvod
Je dobře známé je složité úlohy pro návrh a vytváření DRM subsystém pro OTT nebo online řešení datového proudu. A je běžnou praxí pro operátory, online poskytovatelů videa využívající tuto část poskytovatelům služeb specializované DRM. Cílem tohoto dokumentu je poskytnout odkaz návrhu a implementace začátku do konce DRM subsystému v OTT nebo streamování řešení online.

Cílové čtečky tohoto dokumentu se technici práce v subsystému DRM OTT nebo streamování nebo více-screen řešení online nebo všechny zájem o DRM subsystém čtečky. Předpokladem je, že čtečky obeznámeni s alespoň jedním z technologiemi DRM na trhu, jako je například technologie PlayReady, Widevine, FairPlay nebo Adobe Access.

Pomocí DRM jsme také zahrnovat CENC (Common Encryption) s více technologiemi DRM. Hlavní trend v online streamování a OTT odvětví je pro použití CENC s více-native technologiemi DRM na různých platformách klienta, což je shift z předchozí trend pomocí jednoho DRM a jeho klientskou sadou SDK pro různé platformy klienta. Při použití šifrování CENC s více native DRM, technologie PlayReady a Widevine jsou šifrované podle [Common Encryption (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) specifikace.

Výhody CENC s více technologiemi DRM jsou následující:

1. Snižuje náklady, protože zpracování jedné šifrování se používá cílení na různých platformách s nativní technologiemi DRM; šifrování
2. Snižuje náklady na správu šifrované prostředky, protože je vyžadována pouze jedna kopie šifrované prostředků;
3. Eliminuje DRM klientů na licence náklady, protože je obvykle volné v jeho nativní platforma Nativní klient DRM.

Microsoft byl aktivní vykonavatel DASH a šifrování CENC společně s některé přehrávače hlavní odvětví. Microsoft Azure Media Services má poskytování podpory DASH a šifrování CENC. Poslední oznámení, najdete v tématu Mingfei na blozích: [služeb doručování licence Google Widevine uvedení ve službě Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/), a [Azure Media Services přidá balení Google Widevine pro doručení datový proud více technologiemi DRM](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Přehled tohoto článku
Cílem tohoto článku zahrnuje následující položky:

1. Poskytuje referenční návrhu podsystému DRM pomocí šifrování CENC s více technologiemi DRM;
2. Poskytuje implementaci odkaz na platformě Microsoft Azure nebo Azure Media Services;
3. Popisuje některé témata týkající se návrhu a implementace.

V následujícím článku se zabývá "více technologiemi DRM" následující:

1. Microsoft PlayReady
2. Google Widevine
3. Apple FairPlay 

Následující tabulka shrnuje nativní platforma/nativní aplikaci a prohlížeče nepodporuje každý DRM.

| **Klientské platformy** | **Podpora nativní DRM** | **Aplikace nebo prohlížeče** | **Streamování formáty** |
| --- | --- | --- | --- |
| **Inteligentní televizní přijímače, operátor externích zařízení, externích OTT zařízení** |PlayReady především se stává, nebo Widevine, nebo jiné |Linux, Opera, WebKit, jiné |Různými formáty |
| **Zařízení s Windows 10 (počítače s Windows, tablety s Windows, Windows Phone, Xbox)** |PlayReady |MS Edge/IE11/EME<br/><br/><br/>UWP |DASH (pro HLS, technologie PlayReady není podporována.)<br/><br/>DASH, technologie Smooth Streaming (HLS pro, PlayReady není podporována.) |
| **Zařízení se systémem Android (telefon, Tablet, TV)** |Widevine |Chrome nebo EME |DASH, HLS |
| **iOS (iPhone, iPad), klienty OS X a Apple TV** |FairPlay |Safari 8 +/ EME |HLS |


Vzhledem k tomu, aktuální stav nasazení pro každý DRM obvykle chtít službu implementovat 2 nebo 3 technologiemi DRM a ujistěte se, že adresa všechny typy koncových bodů nejlepším způsobem.

Je kompromis mezi složitosti logice služby a složitost na straně klienta k dosažení určitá úroveň činnost koncového uživatele na různých klientech.

Chcete-li váš výběr, mějte na paměti tyto skutečnosti:

* PlayReady je nativně implementované v každé zařízení s Windows, na některých zařízeních se systémem Android a dostupný prostřednictvím sady SDK softwaru na prakticky jakékoli platformě
* Widevine je nativně implementována v každé zařízení se systémem Android, Chrome a některá další zařízení
* FairPlay je k dispozici jenom v iOS a Mac OS klienty nebo prostřednictvím iTunes.

Proto by byl typické více technologiemi DRM:

* Možnost 1: PlayReady a Widevine
* Možnost 2: PlayReady, Widevine a FairPlay

## <a name="a-reference-design"></a>Návrh odkazu
V této části jsme nabídne návrhu odkaz, který nerozlišuje technologie použít k implementaci.

Subsystém DRM může obsahovat následující součásti:

1. Správa klíčů
2. Balení DRM
3. Doručování licencí DRM
4. Kontrola oprávnění
5. Ověřování/autorizace
6. Player
7. Původ nebo CDN

Následující diagram znázorňuje nejvyšších úrovní interakce mezi jednotlivými součástmi v subsystému DRM.

![Subsystému DRM s CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Existují tři základní "vrstvy" v návrhu:

1. Zálohování vrstvy office (v černé), které nejsou vystaveny externě;
2. Vrstva "Hraniční sítě" (modrá) obsahující všechny koncové body, kterým čelí veřejné;
3. Veřejné internetové vrstvy (světla modrá) obsahující CDN a přehrávače s přenosy přes veřejný Internet.

Měla by existovat nástroj správy obsahu, k nimž ochrany DRM, bez ohledu na to je statické nebo dynamické šifrování. Vstupy pro šifrování DRM by měla obsahovat:

1. Video obsah MBR;
2. Klíč obsahu;
3. Licence získání adresy URL.

Během doby přehrávání nejvyšší úrovni tok je:

1. Uživatel je ověřen;
2. Autorizační token se vytvoří pro uživatele.
3. DRM chráněný obsah (manifestu) se stáhne do player;
4. Přehrávač odešle žádost o získání licence na licenční servery společně s klíče ID a autorizace tokenu.

Než budete pokračovat na další téma pár slov o návrhu služby správy klíčů.

| **ContentKey – prostředek** | **Scénář** |
| --- | --- |
| 1 – 1 |Nejjednodušším případě. Poskytuje nejlepší ovládacího prvku. Ale obecně výsledkem nejvyšší doručení náklady na licence. Požadavek na minimální jednu licenci je požadované pro každý chráněný prostředek. |
| 1 –-mnoho |Můžete použít stejný klíč k obsahu pro více prostředků. Například pro všechny prostředky v logické skupiny, jako je genre nebo podmnožinu genre (nebo gen film), můžete použít jeden klíč obsahu. |
| N – 1 |Více klíčů k obsahu, je potřeba pro každý prostředek. <br/><br/>Například pokud se mají použít dynamické ochranu CENC s více technologiemi DRM pro MPEG-DASH a dynamické šifrování AES-128 pro HLS, je třeba dva samostatné obsahu klíče, každou s vlastním ContentKeyType. (Pro klíč obsahu používat dynamické šifrování CENC ochrany, musí být ContentKeyType.CommonEncryption používá, když pro klíč k obsahu pro dynamické šifrování AES-128, by měl použít ContentKeyType.EnvelopeEncryption.)<br/><br/>Další příklad v CENC ochrany obsahu DASH, teoreticky jeden klíč obsahu lze použít k ochraně datový proud videa a jiné klíč obsahu k ochraně zvukový stream. |
| Mnoho – pro - mnoho |Kombinace výše uvedených dvou scénářích: jednu sadu obsahu klíče se používají pro každou z více prostředků ve stejném asset "skupina". |

Dalším důležitým faktorem vzít v úvahu je použití trvalé a dočasnou licencí.

Tyto aspekty jsou důležité

Pokud používáte veřejného cloudu pro doručování licencí mají přímý vliv na doručování náklady na licence. Zvažte těchto dvou různých případech pro ilustraci:

1. Měsíční předplatné: použití trvalé licence a obsahu klíč asset mapování 1: n. Například pro děti filmy použijeme jeden klíč obsahu pro šifrování. V takovém případě:

    Celkový počet licencí # požadované pro všechny děti filmy a zařízení = 1
2. Měsíční předplatné: použít dočasnou licence a mapování 1: 1 mezi klíč obsahu a asset. V takovém případě:

    Celkový počet licencí # požadované pro všechny děti filmy a zařízení = [# filmy sledovaná] x [relací #]

Jak můžete snadno vidět, dvou různých návrzích vést k velmi jiné licenční vzory požadavku proto doručování licencí náklady Pokud službu doručování licencí poskytuje veřejného cloudu, jako je například Azure Media Services.

## <a name="mapping-design-to-technology-for-implementation"></a>Mapování návrhu technologie pro implementaci
V dalším kroku jsme mapovat naše obecné návrhu technologie na platformě Microsoft Azure nebo Azure Media Services, zadáním technologii, kterou chcete použít pro každý stavební blok.

V následující tabulce jsou uvedeny mapování:

| **Stavební blok.** | **Technologie** |
| --- | --- |
| **Player** |[Přehrávač médií Azure](https://azure.microsoft.com/services/media-services/media-player/) |
| **Zprostředkovatel identity (IDP)** |Azure Active Directory |
| **Služba tokenů zabezpečení (STS)** |Azure Active Directory |
| **Pracovní postup ochrany DRM** |Dynamické ochrany služby Azure Media Services |
| **Doručování licencí DRM** |1. Azure Media Services licence doručení (PlayReady, Widevine, FairPlay), nebo <br/>2. Axinom licenční Server, <br/>3. Vlastní PlayReady licenčního serveru |
| **Počátek** |Koncový bod streamování služby Azure Media Services |
| **Správa klíčů** |Není skutečně potřeba pro odkaz na implementaci |
| **Správa obsahu** |Konzolovou aplikaci C# |

Jinými slovy zprostředkovatele Identity (IDP) a zabezpečené tokenu služby (STS) bude Azure AD. Přehrávač, použijeme [rozhraní API služby Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Azure Media Services a Azure Media Player podporovat DASH a šifrování CENC s více technologiemi DRM.

Následující diagram znázorňuje celkové struktuře a toku s výše mapování technologie.

![Šifrování CENC na AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Pokud chcete nastavit dynamické šifrování CENC, použije nástroj pro správu obsahu následující vstupy:

1. Otevřete obsahu;
2. Klíč obsahu z klíče generace nebo správu;
3. Adresy URL získávání licencí;
4. Seznam informací ze služby Azure AD.

Výstup nástroje pro správu obsahu se bude:

1. ContentKeyAuthorizationPolicy obsahující specifikace na tom, jak doručování licencí ověří JWT token a specifikace licence DRM;
2. AssetDeliveryPolicy obsahující specifikace na streamování formátu, ochrany DRM a adresy URL získání licence.

Během doby běhu, tok je jako níže:

1. Po ověření uživatele se generuje JWT token;
2. Jedna z deklarací obsažených v tokenu JWT je "skupiny" deklarace obsahující ID objektu skupiny "EntitledUserGroup". Tento požadavek se použije pro předávání "Kontrola nárocích".
3. Přehrávač stahování klienta manifest CENC chráněného obsahu a "vidí" následující:

   1. klíč ID,
   2. obsah je chráněný, CENC
   3. Licence získání adresy URL.
4. Přehrávač odešle požadavek získání licence založené na prohlížeči nebo DRM podporované. V žádosti o získání licence klíče ID a taky bude možné odeslat JWT token. Službu doručování licencí ověří JWT token a deklarace obsažené před vydáním licence, které jsou potřebné.

## <a name="implementation"></a>Implementace
### <a name="implementation-procedures"></a>Implementace postupů
Implementace bude zahrnovat následující kroky:

1. Příprava testovacího majetek: kódování nebo balíček testovací video, které více přenosovými rychlostmi fragmentovaný soubor MP4 ve službě Azure Media Services. Tento prostředek není DRM chráněný. DRM ochrany se provádí dynamické ochrany později.
2. Vytvoření klíče ID a obsah klíče (volitelně od počáteční hodnoty klíčů). Pro naše účely není vyžadována systémem správy klíčů, protože jsme se zabývají pouze jedinou sadu klíčů ID a klíč obsahu pro několik prostředků testovací;
3. Slouží ke konfiguraci služeb doručování licencí DRM více pro prostředek test rozhraní API pro AMS. Pokud používáte vlastní licenčních serverů ve vaší společnosti nebo vaše společnost dodavatelé místo licenční služby ve službě Azure Media Services, můžete tento krok přeskočit a zadat licenční získání adresy URL v kroku ke konfiguraci doručování licencí. Rozhraní API pro AMS je potřeba zadat, že některé podrobné konfigurace, například omezení zásady autorizace, šablony odpovědi licence pro různé služby licence DRM, atd. V tuto chvíli portál Azure ještě neposkytuje rozhraní potřebných pro tuto konfiguraci. Můžete najít informace o úroveň rozhraní API a ukázkový kód v dokumentu Dita Kornich: [pomocí PlayReady nebo Widevine běžného dynamického šifrování](media-services-protect-with-drm.md).
4. Konfigurace zásad doručení assetu pro prostředek test pomocí rozhraní API AMS. Můžete najít informace o úroveň rozhraní API a ukázkový kód v dokumentu Dita Kornich: [pomocí PlayReady nebo Widevine běžného dynamického šifrování](media-services-protect-with-drm.md).
5. Vytvořit a nakonfigurovat klienta služby Azure Active Directory v Azure;
6. Vytvořit několik uživatelských účtů a skupin v klientovi služby Azure Active Directory: je třeba vytvořit alespoň "EntitledUser" skupiny a přidat uživatele do této skupiny. Uživatelé v této skupině předá kontrolu oprávnění v získání licence a uživatelé není v této skupině se nepodaří předat kontrola ověřování a nebude možné získat žádné licence. Je členem této skupiny "EntitledUser" je taková deklarace požadované "skupiny" v tokenu JWT vydaného Azure AD. Tento požadavek deklarace identity musí být zadán v konfiguraci krok služeb doručování licence více technologiemi DRM.
7. Vytvoření aplikace ASP.NET MVC, který bude hostitelem vaší přehrávání videa. Tato aplikace ASP.NET bude chránit pomocí ověřování uživatele pomocí služby klienta Azure Active Directory. Správné deklarací identity bude součástí přístupových tokenů získat po ověření uživatele. Pro tento krok se doporučuje OpenID Connect rozhraní API. Je potřeba nainstalovat následující balíčky NuGet:
   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Instalovat balíček Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
8. Vytvoření přehrávač pomocí [rozhraní API služby Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). [Rozhraní API Azure Media Player ProtectionInfo](http://amp.azure.net/libs/amp/latest/docs/) vám umožní určit technologii, která DRM používat na jiné platformě DRM.
9. Testovací matrice:

| **DRM** | **Prohlížeč** | **Výsledek nárok uživatele** | **Výsledek pro zrušení nárok uživatele** |
| --- | --- | --- | --- |
| **PlayReady** |MS Edge nebo IE11 v systému Windows 10 |Úspěšné |Selhání |
| **Widevine** |Chrome ve Windows 10 |Úspěšné |Selhání |
| **FairPlay** |Bude doplněno | | |

George Trifonov Azure Media Services týmu byl zápis blog, který poskytuje podrobné pokyny k nastavení Azure Active Directory pro aplikaci ASP.NET MVC player: [integrovat Azure Media Services OWIN MVC na základě aplikace pomocí Azure Active Directory a omezit obsah na základě deklarací JWT doručení klíče](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George také zapisují blog na [ověření pomocí tokenu JWT v Azure Media Services a dynamickým šifrováním](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). A tady je svůj [ukázku v Azure AD integraci s Azure Media Services doručení klíče](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Informace o službě Azure Active Directory:

* Můžete najít informace pro vývojáře v [Azure Active Directory – Příručka vývojáře](../active-directory/active-directory-developers-guide.md).
* Můžete najít informace pro správce v [spravovat váš adresář Azure AD](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Některé gotchas v implementaci
V implementaci nejsou některé "gotchas". Zpravidla v následujícím seznamu "gotchas" vám může pomoci řešení problémů v případě, že narazíte na problémy.

1. **Vystavitel** adresa URL musí končit **"/"**.  

    **Cílová skupina** by měl být ID klienta aplikace přehrávače a měli byste také přidat **"/"** na konci URL vystavitele.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    V [JWT Decoder](http://jwt.calebb.net/), měli byste vidět **oblast** a **iss** jako níže v tokenu JWT:

    ![1. gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)
2. Přidejte oprávnění k aplikaci v AAD (na kartě Konfigurace aplikace). To je potřeba pro každou aplikaci (místní a nasazené verze).

    ![gotcha 2.](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)
3. Použijte správné vystavitele v nastavení dynamické šifrování CENC ochrany:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Nebudou fungovat následující:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    Identifikátor GUID je ID klienta AAD. Identifikátor GUID naleznete v automaticky otevřeném okně koncových bodů na portálu Azure.
4. Členství ve skupině udělit deklarací oprávnění. Zajistěte, aby v souboru manifestu aplikace AAD, že máme následující

    "groupMembershipClaims": "Vše", (výchozí hodnota je null)
5. Nastavení správné TokenType při vytváření omezení požadavků.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Od přidání podpory JWT (AAD) kromě SWT (ACS), je výchozí TokenType TokenType.JWT. Pokud používáte SWT/ACS, je nutné nastavit na TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Další témata pro implementaci
Potom jsme se disk uss některá další témata v našem návrhu a implementace.

### <a name="http-or-https"></a>Protokol HTTP nebo HTTPS?
Přehrávač aplikace ASP.NET MVC, které vytvoříme, musí podporovat tyto věci:

1. Ověření uživatele prostřednictvím služby Azure AD, které musí být v rámci HTTPS;
2. Exchange tokenu JWT mezi klientem a Azure AD, která musí být v rámci HTTPS;
3. Získání licence DRM klientem, která musí být v rámci protokolu HTTPS, pokud se službou Azure Media Services poskytuje doručování licencí. Samozřejmě sady produktů PlayReady nenutí HTTPS pro doručování licencí. Pokud je server licence PlayReady mimo Azure Media Services, by bylo možné pomocí protokolu HTTP nebo HTTPS.

Proto aplikace přehrávače ASP.NET použije HTTPS jako osvědčený postup. To znamená, že Azure Media Player bude na stránce v rámci protokolu HTTPS. Ale pro streamování jsme raději HTTP, proto je potřeba vzít v úvahu smíšený obsah problém.

1. Prohlížeč nepovoluje smíšený obsah. Můžete ale povolit moduly plug-in, jako je modul plug-in Silverlight a OSMF pro protokol smooth a POMLČKY. Smíšený obsah je potíže se zabezpečením – je to z důvodu riziko, že možnost Vložit škodlivý JS, což může způsobit zákaznická data za ohrožené.  To prohlížeče blokovat ve výchozím nastavení a, pokud je jediný způsob, jak je obejít na straně serveru (původ), aby všechny domény (bez ohledu na to https nebo http). To je pravděpodobně není vhodné buď.
2. Jsme byste neměli smíšený obsah: jak používat protokol HTTP nebo oba používat protokol HTTPS. Při přehrávání smíšený obsah, technická silverlightSS uděláte zrušením smíšený obsah upozornění. Technická flashSS zpracovává smíšený obsah bez upozornění smíšený obsah.
3. Pokud před srpen 2014 byl vytvořen koncový bod streamování, nebudou podporovat protokol HTTPS. V takovém případě prosím vytvořit a použít nový koncový bod streamování pro protokol HTTPS.

V implementaci odkaz pro DRM chráněný obsah v části HTTTPS bude aplikace a vysílání datového proudu. Otevřít obsah přehrávač nemusí ověřování nebo licenci, takže liberty k použití protokolu HTTP nebo HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory podepisování výměna klíče
To je důležité vzít vzít v úvahu vaší implementace. Pokud jste to nepovažujte v implementaci, dokončené systému nakonec přestanou fungovat úplně do maximálně 6 týdnů.

Azure AD používá k navázání vztahu důvěryhodnosti mezi samostatně a aplikací pomocí služby Azure AD oborový standard. Konkrétně Azure AD používá podpisový klíč, který se skládá z pár veřejného a privátního klíče. Pokud Azure AD vytvoří token zabezpečení, který obsahuje informace o uživateli, je tento token podepsána Azure AD pomocí jeho privátní klíč, než budou odeslána zpět do aplikace. Pokud chcete ověřit, zda je token platný a z Azure AD ve skutečnosti původu, musíte ověřit aplikace je token podpis pomocí veřejného klíče vystavené Azure AD, která je součástí dokument federačních metadat klienta. Tento veřejný klíč – a podpisový klíč, ze kterého pochází – je stejný jako ten, používá se pro všechny klienty v Azure AD.

Podrobné informace o výměně klíčů Azure AD najdete v dokumentu: [důležité informace o podepisování výměny klíče ve službě Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Mezi [pár veřejného a privátního klíče](https://login.microsoftonline.com/common/discovery/keys/),

* Privátní klíč se používá služba Azure Active Directory pro vygenerování tokenu JWT;
* Veřejný klíč je aplikace, jako je například služeb doručování licencí DRM v AMS používají k ověření tokenu JWT;

Azure Active Directory za účelem zabezpečení, otočí tento certifikát pravidelně (každých 6 týdny). V případě porušení zabezpečení může dojít výměny klíčů, kdykoli. Proto služeb doručování licencí v AMS je potřeba aktualizovat veřejný klíč používaný jako Azure AD otočí páru klíčů, v opačném případě se nezdaří ověření pomocí tokenu v AMS a budou vydávat žádná licence.

Toho dosáhnete pomocí nastavení TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument při konfiguraci služeb doručování licencí DRM.

Tok tokenu JWT je jako níže:

1. Azure AD vydá JWT token s aktuální privátní klíč pro ověřené uživatele.
2. Když přehrávač uvidí šifrování CENC s více technologiemi DRM chráněný obsah, bude nejprve vyhledat tokenu JWT vydaného Azure AD.
3. Přehrávač odešle žádost o získání licence se JWT token služeb doručování licencí v AMS;
4. Služeb doručování licencí v AMS použije aktuální nebo platný veřejný klíč z Azure AD ověřit JWT token předtím, než vystavování licencí.

Služeb doručování licencí DRM bude kontrola vždy aktuální, platný veřejný klíč z Azure AD. Veřejný klíč předloží Azure AD bude klíč používaný k ověření JWT tokenem vydaným službou Azure AD.

Co v případě výměny klíčů se stane po generuje JWT token AAD, ale před token JWT token posílá přehrávače služeb na doručování licencí DRM v AMS pro ověření?

Vzhledem k tomu, že v každém okamžiku může být vrácena klíč, je vždy více než jeden platný veřejný klíč k dispozici v dokument federačních metadat. Doručování licencí Azure Media Services můžete použít některou z klíče určené v dokumentu, vzhledem k tomu, že jeden klíč může být vrácena brzy, jiné může být jeho nahrazení a tak dále.

### <a name="where-is-the-access-token"></a>Kde je Token přístupu?
Pokud se podíváte na tom, jak webová aplikace volá aplikaci API v části [identita aplikace s udělení pověření klienta OAuth 2.0](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), tok ověřování je jako níže:

1. Uživatel je přihlášený do služby Azure AD ve webové aplikaci (viz [webový prohlížeč k webové aplikaci](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
2. Koncový bod autorizace Azure AD přesměruje uživatelského agenta zpět do klientské aplikace s autorizační kód. Uživatelský agent vrátí autorizační kód identifikátor URI přesměrování klienta aplikace.
3. Webová aplikace musí získat přístupový token, aby mohli ověřit webové rozhraní API a načíst požadovaný prostředek. Vytvoří žádost na Azure AD koncovému bodu tokenu, poskytuje pověření, ID klienta a identifikátor ID URI webové rozhraní API aplikace. Představuje autorizační kód k prokázání, že uživatel souhlasí.
4. Azure AD ověřuje aplikace a vrátí přístupový token JWT, který se používá k volání webového rozhraní API.
5. Přes protokol HTTPS webová aplikace používá přístupový token vrácená JWT přidat řetězec JWT s označením "Nosiče" v hlavičce autorizace požadavku do webové rozhraní API. Webové rozhraní API pak ověří JWT token a pokud je ověření úspěšné, vrátí požadovaného prostředku.

V tomto toku "Identita aplikace" důvěřuje webové rozhraní API, webovou aplikaci ověřil daného uživatele. Z tohoto důvodu se tento vzor nazývá důvěryhodný subsystém. [Diagramu na této stránce](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) popisuje, jak funguje tok poskytování autorizačních kódů.

V získání licence s omezení s tokenem jsme postupujete podle stejného vzoru důvěryhodný subsystém. A službu doručování licencí ve službě Azure Media Services je webové rozhraní API prostředku "prostředek back-end" potřebuje přístup k webové aplikaci. Kde je proto tokenu přístupu?

Skutečně jsme se získání tokenu přístupu z Azure AD. Po úspěšné ověření uživatele je vrácen autorizační kód. Autorizační kód se pak používá, společně s klíčem klienta ID a aplikace, k systému exchange pro přístupový token. A přístupový token je pro přístup k aplikaci "ukazatel" ukazující nebo představuje službu doručování licencí Azure Media Services.

Je potřeba zaregistrovat a nakonfigurovat "ukazatel" aplikace ve službě Azure AD pomocí následujících kroků:

1. V klientovi Azure AD

   * Přidání aplikace (prostředků) s adresou URL přihlašování:

   https://[resource_name].azurewebsites.NET/ a

   * Adresa URL ID aplikace:

   https://[aad_tenant_name].onmicrosoft.com/[resource_name];
2. Přidejte nový klíč pro aplikaci prostředků;
3. Aktualizace souboru manifest aplikace, tak, aby vlastnost groupMembershipClaims s následující hodnotou: "groupMembershipClaims": "Vše",  
4. V aplikaci Azure AD odkazující na webovou aplikaci player v části "oprávnění k ostatním aplikacím", přidejte prostředků aplikace, která byla přidána v kroku 1 výše. V části "delegovaná oprávnění" zkontrolujte zaškrtnutí "Přístup [resource_name]". To dává oprávnění webové aplikace k vytvoření přístupové tokeny pro přístup k prostředku aplikace. Měli byste to provést pro místní i nasazené verze webové aplikace Pokud vyvíjíte pomocí sady Visual Studio a Azure webové aplikace.

Proto tokenu JWT vydaného Azure AD, je skutečně přístupového tokenu pro přístup k tomuto prostředku "ukazatel".

### <a name="what-about-live-streaming"></a>Co se chystáte živě Streamovat?
Ve výše má byla tato diskuse zaměřovat na prostředky na vyžádání. Co se chystáte živé streamování?

Dobrá zpráva je, že můžete používat přesně stejnou návrh a implementaci pro ochranu živé streamování ve službě Azure Media Services tak, že považuje asset přidružený program jako "VOD asset".

Konkrétně je dobře známý, uděláte živé vysílání datového proudu ve službě Azure Media Services, musíte vytvořit kanál a potom program v kanálu. Pokud chcete vytvořit program, potřebujete vytvořit prostředek, který bude obsahovat za provozu archivu programu. Chcete-li poskytovat CENC s více technologiemi DRM ochrany obsahu za provozu, všechny je potřeba udělat, je použít stejné nastavení nebo zpracování pro daný prostředek jako by byl "VOD asset" před spuštěním programu.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Co licenčních serverů mimo Azure Media Services?
Zákazníci mohou často investovaly do licence serverové farmy buď v svá vlastní data center nebo hostované poskytovateli služeb DRM. Naštěstí Azure Media Services obsahu Protection umožňuje pracovat v režimu hybridní: obsah hostované a dynamicky chráněné v Azure Media Services, zatímco licence DRM se dodávají servery mimo Azure Media Services. V takovém případě existují následující aspekty změny:

1. Služba tokenů zabezpečení je potřeba vystavovat tokeny, které jsou přijatelné a lze ověřit pomocí licence serverové farmy. Například servery licence Widevine poskytované Axinom vyžaduje konkrétní token JWT, který obsahuje "nárocích zprávu". Proto je potřeba mít služby tokenů zabezpečení vystavit takový token JWT. Autoři dokončili takové implementace a podrobnosti najdete v následujícím dokumentu v [centru dokumentace Azure](https://azure.microsoft.com/documentation/): [pomocí Axinom pro doručování licence na Widevine do služby Azure Media Services](media-services-axinom-integration.md).
2. Už je nutné nakonfigurovat službu doručování licencí (ContentKeyAuthorizationPolicy) ve službě Azure Media Services. Co musíte udělat je poskytnout licence získání adresy URL (pro PlayReady, Widevine a FairPlay) při konfiguraci AssetDeliveryPolicy v nastavení šifrování CENC s více technologiemi DRM.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Co dělat, když chcete používat vlastní službu tokenů zabezpečení?
Může dojít z několika důvodů, které si zákazník může vybrat používat vlastní tokenů zabezpečení (Secure Token Service) k poskytování tokenů JWT. Některé z nich jsou:

1. Zprostředkovatel Identity (IDP), které jsou využívaných zákazníkem nepodporuje službu tokenů zabezpečení. Vlastní službu tokenů zabezpečení v takovém případě může být možnost.
2. Zákazník může být nutné flexibilní nebo náročnější ovládací prvek v integraci služby tokenů zabezpečení do odběratele zákazníka účetním systémem. Operátor MVPD může například nabízí více balíčků OTT odběratele, jako jsou premium a basic, sportu, atd. Operátor může chtít tak, aby odpovídaly deklarace do tokenu s balíčkem odběratele tak, aby pouze obsah v pravém balíčku je k dispozici. V takovém případě vlastní službu tokenů zabezpečení poskytuje potřebné flexibilitu a řízení.

Dvě změny nutné provést při použití vlastních tokenů zabezpečení:

1. Při konfiguraci služby doručování licencí pro určitý prostředek, budete muset zadat klíč zabezpečení používané pro ověřování pomocí vlastních tokenů zabezpečení (Další informace níž), místo aktuální klíč ze služby Azure Active Directory.
2. Když se vygeneruje JTW token, je určen klíč zabezpečení místo privátní klíč pro aktuální X509 certifikátu ve službě Azure Active Directory.

Existují dva typy bezpečnostních klíčů:

1. Symetrický klíč: stejný klíč se používá pro generování a ověřování token JWT;
2. Asymetrický klíč: páru veřejného a privátního klíče RSA ve X509 certifikát se používá s privátním klíčem pro šifrování nebo generování JWT token a veřejný klíč k ověření tokenu.

#### <a name="tech-note"></a>Technická poznámka
Pokud používáte rozhraní .NET Framework nebo C# jako vývoj platformy, X509 certifikát používaný pro zabezpečení asymetrický klíč musí mít klíč délku aspoň 2048. Toto je požadavek třídy System.IdentityModel.Tokens.X509AsymmetricSecurityKey v rozhraní .NET Framework. Jinak bude vyvolána následující výjimky:

IDX10630: 'System.IdentityModel.Tokens.X509AsymmetricSecurityKey' pro podepisování nemůže být menší než '2048' bits.

## <a name="the-completed-system-and-test"></a>Dokončené systému a testování
Vám ukážeme v dokončené systému začátku do konce několik scénářů, tak, aby čtečky může mít základní "formát" chování před získáním účet přihlášení.

Přehrávač webové aplikace a její přihlášení najdete [zde](https://openidconnectweb.azurewebsites.net/).

Pokud je třeba je "zjistili" scénář: video prostředky, které jsou hostované v Azure Media Services, které jsou buď nechráněné nebo chráněné DRM, ale bez ověření pomocí tokenu (vystavování licenci pro kdo o to požádá), můžete otestovat ji bez přihlášení (při přechodu HTTP Pokud video streamování přes protokol HTTP).

Pokud je třeba je integrovaný scénář začátku do konce: video prostředky je pod dynamické DRM ochrany ve službě Azure Media Services pomocí tokenu ověření a token JWT generován službou Azure AD, musíte se přihlásit.

### <a name="user-login"></a>Přihlášení uživatele
Chcete-li otestovat integrovaný systém DRM začátku do konce, je potřeba mít "účet", nebo přidat.

Který účet?

I když služba Azure původně umožňovala přístup pouze uživatelé s účtem Microsoft, nyní umožňuje přístup uživatelům z obou systémů. Provedli jsme to tak, že jsme všechny vlastnosti služby Azur nechali ověřovat službou Azure AD, která ověřuje organizační uživatele, a také jsme vytvořili federační vztah, kde Azure AD nechává spotřebitelské uživatele ověřovat systémem identit uživatelů účtů Microsoft. V důsledku toho dokáže služba Azure AD ověřit hostitelské účty Microsoft i nativní účty Azure AD.

Vzhledem k tomu, že Azure AD důvěřuje domény účtu Microsoft (MSA), můžete přidat všechny účty z jakéhokoli z následujících domén do vlastní Azure AD klienta a použít účet pro přihlášení:

| **Název domény** | **Domény** |
| --- | --- |
| **Doména klienta vlastní Azure AD** |somename.onmicrosoft.com |
| **Podnikové doméně** |Microsoft.com |
| **Účet Microsoft (MSA) domény** |live.com, Outlook.com, hotmail.com |

Autoři tak, aby měl účet vytvořit nebo přidat pro vás může kontaktovat.

Níže jsou snímky obrazovky stránky jiné přihlašovací údaje používané účty v jiné doméně.

**Vlastní Azure AD klienta účet domény**: V tomto případě se zobrazí stránka přizpůsobené přihlášení vlastní doménu klienta služby Azure AD.

![Účet domény klienta vlastní Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft doménový účet s čipovou kartu**: V tomto případě se zobrazí přihlašovací stránky přizpůsobený podle Microsoft podnikové IT s dvoufaktorové ověřování.

![Účet domény klienta vlastní Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Účet Microsoft (MSA)**: V tomto případě se zobrazí stránka přihlášení Account Microsoft pro spotřebitele.

![Účet domény klienta vlastní Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Pomocí rozšíření šifrované média pro PlayReady
Moderní prohlížeč s šifrované rozšíření média (EME) pro podporu technologie PlayReady, jako je například Internet Exploreru 11 na Windows 8.1 nebo vyšší a prohlížeč Microsoft Edge ve Windows 10 bude PlayReady základní DRM pro EME.

![Použití EME pro PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Oblasti tmavý player je skutečnost, že PlayReady ochrana chrání jeden z provedení snímek obrazovky chráněné videa.

Následující obrazovka ukazuje player modulů plug-in a MSE/EME podporu.

![Použití EME pro PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME v Microsoft Edge a aplikace Internet Explorer 11 ve Windows 10 umožňuje vyvolání z [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) na zařízení s Windows 10, které ji podporují. PlayReady SL3000 odemkne toku obsahu rozšířené premium (4K, záhlaví, atd.) a nové modely doručování obsahu (časná okna pro rozšířený obsah).

Zaměřit se na zařízení se systémem Windows: PlayReady je pouze DRM v HW dostupná na zařízeních s Windows (PlayReady SL3000). Streamování služby můžete použít PlayReady prostřednictvím EME nebo aplikaci UWP a nabízí vyšší kvalitu videa pomocí PlayReady SL3000 než jiné DRM. Obvykle 2K obsahu budou procházet skrz Chrome nebo Firefox a 4K obsahu prostřednictvím Microsoft Edge/IE11 nebo aplikace UPW do stejného zařízení (v závislosti na nastavení služby a implementaci).

#### <a name="using-eme-for-widevine"></a>Použití EME pro Widevine
Moderní prohlížeč s podporou EME nebo Widevine, jako je například Chrome 41 + na Windows 10, Windows 8.1, Mac OSX Yosemite a Chrome na Android bodu 4.4.4 je Google Widevine DRM za EME.

![Použití EME pro Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Všimněte si, že Widevine nebrání z provedení snímek obrazovky chráněné videa.

![Použití EME pro Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Není nárok uživatelé
Pokud uživatel není členem skupiny "Uživatelé s názvem", uživatel nebude možné předat "Kontrola nárocích" a více technologiemi DRM licenční služby bude odmítnout vydání licence, které jsou požadované, jak je uvedeno níže. Podrobný popis je "získat licenci se nezdařilo", což je tak, jak má.

![Zrušení nárok uživatelé](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Spuštění vlastního tokenu služby Zabezpečené
Scénář spuštěných vlastní zabezpečení tokenu služby (STS) JWT token bude vydat vlastní službu tokenů zabezpečení pomocí buď symetrický, nebo asymetrický klíč.

Případ použití symetrického klíče (pomocí chromu):

![Spuštění vlastní službu tokenů zabezpečení](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

V případě pomocí asymetrického klíče prostřednictvím X509 certifikátu (s použitím moderní prohlížeče Microsoft).

![Spuštění vlastní službu tokenů zabezpečení](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

V obou případech zůstává ověření uživatele stejný – prostřednictvím služby Azure AD. Jediným rozdílem je, že tokeny JWT vydal vlastní službu tokenů zabezpečení místo Azure AD. Samozřejmě, při konfiguraci ochrany dynamické šifrování CENC, omezení službu doručování licencí Určuje typ tokenu JWT, buď symetrický, nebo asymetrický klíč.

## <a name="summary"></a>Souhrn
V tomto dokumentu jsme probrali CENC s více native DRM a řízení přístupu prostřednictvím tokenu ověřování: návrh a jeho implementace pomocí Azure, Azure Media Services a Azure Media Player.

* Návrh odkazu se zobrazí, který obsahuje všechny komponenty potřebné v subsystému DRM nebo CENC;
* Odkaz na implementaci na Azure, Azure Media Services a Azure Media Player.
* Některá témata přímo zahrnutých v návrhu a implementace jsou také popsány.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
