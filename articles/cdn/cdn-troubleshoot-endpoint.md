---
title: "Řešení potíží s koncovými body Azure CDN, které vracejí stav 404 | Microsoft Docs"
description: "Řešení potíží s 404 kódy odpovědí s koncovými body Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f59fbd18413fb44026d8c92b7f6940ed2f8a00a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Poradce při potížích s koncovými body sítě CDN, které vrací stav 404
Tento článek vám pomůže vyřešit problémy s [koncové body CDN](cdn-create-new-endpoint.md) vrácení chyb 404.

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [MSDN Azure a fóra Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na **získat podporu**.

## <a name="symptom"></a>Příznaky
Vytvoření profilu CDN a koncového bodu, ale asi není k dispozici na CDN svůj obsah.  Uživatelé, kteří se pokusí o přístup k vašemu obsahu prostřednictvím adresy URL CDN zobrazí stavové kódy HTTP 404. 

## <a name="cause"></a>Příčina
Existuje několik možných příčin, včetně:

* Původ souboru není viditelná pro CDN
* Koncový bod není správně nakonfigurovaný, způsobuje od CDN k podívejte se na nesprávné místo
* Hostitel zamítá hlavičku hostitele od CDN
* Koncový bod neproběhla čas potřebný k šíření v rámci CDN

## <a name="troubleshooting-steps"></a>Řešení potíží
> [!IMPORTANT]
> Po vytvoření koncového bodu CDN, nebude okamžitě k dispozici pro použití, jak trvá, než se registrace rozšíří v rámci CDN.  Pro <b>Azure CDN společnosti Akamai</b> profily, šíření obvykle dokončení během jedné minuty.  V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.  Pokud je provést kroky v tomto dokumentu a stále vám kód odpovědi 404, zvažte čekání na pár hodin zkontrolovat znovu před otevřením lístku podpory.
> 
> 

### <a name="check-the-origin-file"></a>Zkontrolujte k původnímu souboru
Nejdřív jsme měli ověřte, zda se, že soubor chceme, aby v mezipaměti k dispozici na naše původ a je veřejně přístupná.  Nejrychlejší způsob, jak to udělat, je v privátní In nebo Incognito relace otevřete prohlížeč a přejděte na soubor.  Právě zadejte nebo vložte adresu URL do pole Adresa a li zobrazit, výsledky v souboru, které očekáváte.  V tomto příkladu kliknete na použít soubor je nutné v účtu Azure Storage, přístupná na `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Jak vidíte, úspěšně projde testem.

![Úspěšné!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Přestože se nejrychlejší a nejjednodušší způsob, jak ověřit, jestli že je váš soubor veřejně dostupné, některé konfigurace sítě ve vaší organizaci může poskytnout dojem, že tento soubor je veřejně dostupné, pokud je ve skutečnosti jenom viditelné uživatelům sítě (i když je hostovaná v Azure).  Pokud máte externího prohlížeče, ze kterého můžete otestovat, jako je mobilní zařízení, který není připojen k síti vaší organizace, nebo virtuálního počítače v Azure, která je nejvhodnější.
> 
> 

### <a name="check-the-origin-settings"></a>Zkontrolujte nastavení pro počátek
Teď, když jsme si ověřit, že je soubor veřejně dostupný na Internetu, jsme měli ověřit naše nastavení počátek.  V [portálu Azure](https://portal.azure.com), přejděte na svůj profil CDN a klikněte na koncový bod se řešení potíží.  V výsledná **koncový bod** okně klikněte na počátku.  

![Okno koncový bod se zvýrazněnou počátek](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

**Původu** otevře se okno. 

![Okno počátek](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Typ původu a název hostitele
Ověřte **typ původu** je odstraňte a ověřte **název počátečního hostitele**.  V tomto příkladu `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, část názvu hostitele adresy URL je `cdndocdemo.blob.core.windows.net`.  Jak je vidět na snímku obrazovky je to v pořádku.  Pro úložiště Azure, webové aplikace a původu cloudové služby **název počátečního hostitele** pole je rozevíracího seznamu, takže potřebujeme nemusíte si dělat starosti kontrola pravopisu správně.  Pokud používáte vlastní původ, je však *absolutně nezbytné* vašeho názvu hostitele je napsán správně!

#### <a name="http-and-https-ports"></a>Porty HTTP a HTTPS
Dalším krokem, zkontrolujte, zde je vaše **HTTP** a **HTTPS porty**.  Ve většině případů 80 a 443 jsou správné, a bude vyžadovat žádné změny.  Ale pokud je zdrojový server naslouchá na jiný port, který potřebovat a nelze je zde.  Pokud si nejste jistí, podívejte se jen na adresu URL pro zdrojový soubor.  Specifikace protokolu HTTP a HTTPS zadejte porty 80 a 443 jako výchozí hodnoty. V mé URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, není port určen, tak, aby se předpokládá výchozí hodnota 443 a nastavení jsou správné.  

Však můžete adresu URL pro váš zdrojový soubor, který jste dříve neotestovali je `http://www.contoso.com:8080/file.txt`.  Poznámka: `:8080` na konci tohoto segmentu název hostitele.  Který sděluje prohlížeči, aby používal port `8080` pro připojení k webovému serveru na `www.contoso.com`, takže budete muset zadat 8080 v **HTTP port** pole.  Je důležité si uvědomit, že tato nastavení portů ovlivňují jenom jaké port koncového bodu se používá k načtení informací z tohoto počátku.

> [!NOTE]
> Koncové body **Azure CDN společnosti Akamai** neumožňují použít u původu plný rozsah portů.  Seznam nepovolených portů původu najdete v tématu [Povolené porty původu Azure CDN společnosti Akamai](https://msdn.microsoft.com/library/mt757337.aspx).  
> 
> 

### <a name="check-the-endpoint-settings"></a>Zkontrolujte nastavení koncového bodu
Zpět na **koncový bod** okně klikněte na tlačítko **konfigurace** tlačítko.

![Koncový bod okno s tlačítkem konfigurace](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Pro koncový bod **konfigurace** otevře se okno.

![Konfigurace okna](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoly
Pro **protokoly**, ověřte, zda je vybrán protokol používá klienty.  Stejný protokol, používá klient bude používaný pro přístup k počátek, takže je důležité mít portů původu správně nakonfigurovaná v předchozí části.  Koncový bod naslouchá jenom na výchozí porty HTTP a HTTPS (80 a 443), bez ohledu na původ porty.

Umožňuje vrátit do našich hypotetický příklad s `http://www.contoso.com:8080/file.txt`.  Jak budete si pamatovat, zadaný Contoso `8080` jako jejich HTTP port, ale také Předpokládejme, že zadaný `44300` jako jejich port HTTPS.  Pokud vytvořené koncový bod s názvem `contoso`, jejich hostitele koncového bodu CDN by `contoso.azureedge.net`.  Žádost o `http://contoso.azureedge.net/file.txt` je požadavek HTTP, takže koncový bod by prostřednictvím protokolu HTTP na portu 8080 načíst z tohoto počátku.  Žádost o zabezpečené pomocí protokolu HTTPS, `https://contoso.azureedge.net/file.txt`, by způsobilo koncový bod pro použití protokolu HTTPS na portu 44300 při načíst soubor z tohoto počátku.

#### <a name="origin-host-header"></a>Hlavičku hostitele zdroje
**Hlavičky hostitele počátku** je hodnota hlavičky hostitele odesílaná na počátek s každou žádostí.  Ve většině případů to by měl být stejný jako **název počátečního hostitele** jsme ověřit dříve.  Nesprávná hodnota v tomto poli obvykle nezpůsobí stav 404, ale může způsobit další 4xx stavy, v závislosti na tom, co počátek očekává.

#### <a name="origin-path"></a>Cesty ke zdroji
A konečně, bylo by měl ověřit naše **cesty ke zdroji**.  Ve výchozím nastavení je toto pole prázdné.  Toto pole byste měli používat jenom, pokud chcete zúžit rozsah hostované počátek prostředky, které chcete zpřístupnit v CDN.  

Například v mé koncový bod, měla být všechny prostředky na svůj účet úložiště k dispozici, tak I zbývajících **cesty ke zdroji** prázdné.  To znamená, že žádost o `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` výsledkem připojení z mé koncového bodu `cdndocdemo.core.windows.net` který požadavky `/publicblob/lorem.txt`.  Podobně žádost o `https://cdndocdemo.azureedge.net/donotcache/status.png` výsledkem požadavku koncového bodu `/donotcache/status.png` z tohoto počátku.

Ale co když nechcete používat CDN pro každou cestu na můj počátek?  Řekněme I chtěli vystavit `publicblob` cesta.  Pokud lze zadávat */publicblob* v mé **cesty ke zdroji** pole, které způsobí, že koncový bod vložení */publicblob* před každým požadavkem určené k počátku.  To znamená, že žádost o `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` bude nyní ve skutečnosti trvat žádost část adresy URL, `/publicblob/lorem.txt`a připojte `/publicblob` na začátku. Výsledkem je žádost o `/publicblob/publicblob/lorem.txt` z tohoto počátku.  Pokud tato cesta se nepřekládá na skutečný soubor, počátek vrátí stav 404.  Ve skutečnosti správnou adresu URL pro načtení lorem.txt v tomto příkladu by `https://cdndocdemo.azureedge.net/lorem.txt`.  Všimněte si, že jsme neobsahují */publicblob* cestu na všechny, protože požadavku část adresy URL je `/lorem.txt` a přidá koncový bod `/publicblob`, což `/publicblob/lorem.txt` předávány požadavek na počátek.

