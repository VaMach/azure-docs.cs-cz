---
title: "Povolit nebo zakázat HTTPS na vlastní domény služby Azure Content Delivery Network | Microsoft Docs"
description: "Zjistěte, jak chcete povolit nebo zakázat HTTPS na koncový bod Azure CDN s vlastní doménu."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: c92f1e20acf55b8bd791fad43f17e162a5cb3847
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="enable-or-disable-https-on-an-azure-content-delivery-network-custom-domain"></a>Povolit nebo zakázat HTTPS na vlastní domény služby Azure Content Delivery Network

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Podpora protokolu HTTPS pro vlastní domény Microsoft Azure obsah Delivery Network (CDN) umožňuje doručovat bezpečné obsahu přes SSL pomocí vlastní název domény pro zlepšení zabezpečení dat v průběhu přenosu. V pracovním postupu začátku do konce povolit HTTPS pro vaše vlastní doména je zjednodušený prostřednictvím povolování jedním kliknutím, kompletní certifikát správy a všechny s bez dalších nákladů.

Je důležité k zajištění ochrany osobních údajů a integritu dat všech s citlivými daty webové aplikace v průběhu přenosu. Pomocí protokolu HTTPS zajišťuje, aby citlivá data šifrovala při posílání přes internet. Poskytuje vztah důvěryhodnosti, ověřování a chrání vaše webové aplikace před útoky. Azure CDN v současné době podporuje protokol HTTPS na koncový bod CDN. Například pokud vytvoříte koncový bod CDN z Azure CDN (například https://contoso.azureedge.net), je ve výchozím nastavení povolené HTTPS. Nyní s vlastní doménu HTTPS, můžete povolit zabezpečené doručování pro vlastní doménu (například https://www.contoso.com) také. 

Mezi klíčové atributy HTTPS funkce patří:

- Bez dalších nákladů: existují neplatí pro získání certifikátů nebo obnovení a bez dalších nákladů pro komunikaci přes protokol HTTPS. Právě platíte za GB odchozí od CDN.

- Jednoduché povolování: jeden zřizování klikněte na tlačítko je k dispozici z [portál Azure](https://portal.azure.com). K povolení této funkce můžete také použít rozhraní REST API nebo jiné nástroje pro vývojáře.

- Dokončení správy certifikátů: všechna certifikátů nákup a můžete je zajištěná Správa. Certifikáty jsou automaticky zřizovat a obnovit před vypršením platnosti. Tím se úplně odebere rizika přerušení poskytování služeb v důsledku vypršení platnosti certifikátu.

>[!NOTE] 
>Před povolením podpora protokolu HTTPS, musí jste již vytvořili [vlastní doménu Azure CDN](./cdn-map-content-to-custom-domain.md).

## <a name="enabling-https"></a>Povolení protokolu HTTPS

Pokud chcete povolit protokol HTTPS, postupujte takto:

### <a name="step-1-enable-the-feature"></a>Krok 1: Povolení funkce 

1. V [portál Azure](https://portal.azure.com), přejděte na svůj profil Verizon standard nebo premium CDN.

2. V seznamu koncových bodů klikněte na koncový bod, který obsahuje vaše vlastní doména.

3. Klikněte na tlačítko vlastní doménu, pro který chcete povolit protokol HTTPS.

    ![Okno koncový bod](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Klikněte na tlačítko **na** a povolit protokol HTTPS, potom klikněte na **použít**.

    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


### <a name="step-2-validate-domain"></a>Krok 2: Ověření domény

>[!IMPORTANT] 
>Předtím, než HTTPS active na vaši vlastní doménu, je třeba provést ověření domény. Máte šest pracovních dnů ke schválení domény. Požadavky, které nejsou schválené do šesti dnů obchodní budou automaticky zrušeny. 

Povolíte-li HTTPS na vaši vlastní doménu, naše HTTPS poskytovatel certifikátu DigiCert ověří vlastnictví domény kontaktováním osob žádajících o registraci pro vaši doménu podle domény [WHOIS](http://whois.domaintools.com/) informace osob žádajících o registraci. Obraťte se provádí prostřednictvím e-mailovou adresu (ve výchozím nastavení) nebo telefonní číslo uvedené v registraci WHOIS. 

![Záznam WHOIS](./media/cdn-custom-ssl/whois-record.png)

Kromě toho DigiCert odešle ověřovací e-mail na následující adresy. Pokud je privátní informace WHOIS osob žádajících o registraci, ověřte, že můžete schválit přímo z těchto adres:

správce @&lt;vaše name.com domény&gt;  
správce @&lt;vaše name.com domény&gt;  
správce webového serveru @&lt;vaše name.com domény&gt;  
hostmaster @&lt;vaše name.com domény&gt;  
správce pošty @&lt;vaše name.com domény&gt;  

Měli byste obdržet e-mail za pár minut, podobně jako v následujícím příkladu, žádostí o schválení žádosti. Pokud používáte filtr proti spamu, přidejte admin@digicert.com k jeho seznamu povolených IP adres. Pokud jste neobdrželi e-mailu do 24 hodin, obraťte se na podporu společnosti Microsoft.
    
![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/domain-validation-email.png)

Když kliknete na odkaz schválení, budete přesměrováni na následující formulář online schválení: 
    
![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/domain-validation-form.png)

Postupujte podle pokynů na formuláři. máte dvě možnosti ověřování:

- Můžete schválit všechny budoucí objednávky zadané přes stejný účet pro stejné kořenové domény; například contoso.com. Toto je doporučený postup, pokud plánujete přidat další vlastní domény v budoucnosti pro stejnou kořenovou doménu.

- Můžete schválit pouze konkrétní název hostitele použít v této žádosti. Dodatečné schválení se bude vyžadovat pro následné požadavky.

Po schválení přidá DigiCert vlastního názvu domény do certifikátu alternativní názvy předmětu (SAN). Certifikát je platný po dobu jednoho roku a bude automaticky obnovovat před jeho platnost vypršela.

### <a name="step-3-wait-for-propagation"></a>Krok 3: Počkejte šíření

Po ověření názvu domény může trvat až 6 až 8 hodin pro funkci HTTPS vlastní domény tak, aby aktivovat. Po dokončení procesu stav "Vlastní HTTPS" na webu Azure portal je nastaven na "Povoleno" a kroky čtyři operace v okně vlastní domény HTTPS jsou označeny jako dokončené. Vaše vlastní doména je nyní připravena k používání protokolu HTTPS.

![Povolit HTTPS dialogové okno](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Průběh operace

Následující tabulka zobrazuje průběh operace, která nastane, když povolíte protokol HTTPS. Povolíte-li HTTPS, čtyři kroky operace se zobrazí v okně vlastní domény HTTPS. Každý krok stane aktivní, další podrobnosti se zobrazí v kroku průběhu. Po úspěšném dokončení krok, se zobrazí zelená značka zaškrtnutí vedle sebe. 

| Operace krok | Podrobnosti operace krok | 
| --- | --- |
| Požadavků na odeslání: 1 | Odesílá se požadavek |
| | Váš požadavek HTTPS je odeslání. |
| | Váš požadavek HTTPS byl úspěšně odeslán. |
| 2 ověření domény | Odeslali jsme vám e-mail s žádostí o ověření vlastnictví domény. Čeká na potvrzení. |
| | Vlastnictví domény byla úspěšně schválena. |
| | Platnost žádosti o ověření vlastnictví domény (zákazníků pravděpodobně neodpověděl v 6 dny). Ve vaší doméně nebude povolen protokol HTTPS. * |
| | Požadavek na ověření vlastnictví domény byl odmítnut zákazníka. Ve vaší doméně nebude povolen protokol HTTPS. * |
| 3 zřizování certifikátů | Certifikační autority je aktuálně vydání certifikátu potřebnými k povolení HTTPS ve vaší doméně. |
| | Certifikát byl vydán a je aktuálně nasazená do sítě CDN. To může trvat až 6 hodin. |
| | Certifikát byly úspěšně nasazeny do sítě CDN. |
| 4 dokončeno | Protokol HTTPS se úspěšně zapnul ve vaší doméně. |

\*Tato zpráva se nezobrazí, pokud došlo k chybě. 

Pokud dojde k chybě před odeslání žádosti, zobrazí se následující chybová zpráva:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="disabling-https"></a>Zakázání protokolu HTTPS

Po povolení protokolu HTTPS, můžete ji později zakázat. Zakázat protokol HTTPS, postupujte takto:

### <a name="step-1-disable-the-feature"></a>Krok 1: Zákaz funkce 

1. V [portál Azure](https://portal.azure.com), přejděte na svůj profil Verizon standard nebo premium CDN.

2. V seznamu koncových bodů klikněte na koncový bod, který obsahuje vaše vlastní doména.

3. Klikněte na tlačítko vlastní doménu, pro který chcete zakázat protokol HTTPS.

    ![Okno koncový bod](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Klikněte na tlačítko **vypnout** zakázat protokol HTTPS, pak klikněte na tlačítko **použít**.

    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="step-2-wait-for-propagation"></a>Krok 2: Počkejte šíření

Po HTTPS vlastní doména je zakázaná, může trvat až 6 až 8 hodin, než se projeví. Po dokončení procesu stav "Vlastní HTTPS" na webu Azure portal je nastaven na "disabled" a kroky tři operace v okně vlastní domény HTTPS jsou označeny jako dokončené. Vaše vlastní doména už může používat protokol HTTPS.

![Zakázat protokol HTTPS dialogové okno](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Průběh operace

Následující tabulka zobrazuje průběh operace, která nastane, když zakážete HTTPS. Po zakázání protokolu HTTPS se zobrazí v okně vlastní domény HTTPS tři kroky operace. Každý krok stane aktivní, se zobrazí v kroku na další podrobnosti. Po úspěšném dokončení krok, se zobrazí zelená značka zaškrtnutí vedle sebe. 

| Průběh operace | Podrobnosti o operaci | 
| --- | --- |
| Požadavků na odeslání: 1 | Odesílá se žádost |
| 2 zrušení zřízení certifikátu | Odstranění certifikátu |
| 3 dokončeno | Odstranit certifikát |

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

1. *Kdo je poskytovatel certifikátu a jaký typ certifikátu se použije?*

    Používáme alternativní názvy předmětu (SAN) certifikátu od DigiCert. Certifikát SAN můžete zabezpečit více plně kvalifikované názvy domény s jedním certifikátem.

2. *Můžete používat vyhrazené certifikát?*
    
    Aktuálně ale je plánovaná.

3. *Co když neobdrželi ověření domény e-mailu z DigiCert?*

    Pokud jste neobdrželi e-mailu do 24 hodin, obraťte se na podporu společnosti Microsoft.

4. *Používá certifikát SAN méně bezpečné než vyhrazené certifikát?*
    
    Certifikát SAN standardům stejné šifrování a zabezpečení jako vyhrazené certifikátu. Všechny vystavené certifikáty SSL používají algoritmus SHA-256 pro server rozšířené zabezpečení.

5. *Můžete použít vlastní doménu HTTPS s Azure CDN společnosti Akamai?*

    Tato funkce je v současné době pouze s Azure CDN společnosti Verizon k dispozici. Pracujeme na podporu této funkce s Azure CDN společnosti Akamai v nadcházejících měsících.


## <a name="next-steps"></a>Další kroky

- Zjistěte, jak nastavit [vlastní doménu na koncový bod Azure CDN](./cdn-map-content-to-custom-domain.md)


