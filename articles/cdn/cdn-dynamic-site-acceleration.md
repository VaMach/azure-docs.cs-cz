---
title: "Akcelerace dynamických webů prostřednictvím Azure CDN"
description: "Podrobné informace akcelerace dynamických webů"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: v-semcev
ms.openlocfilehash: be2719e0e02c8bc69800ef4a3e7da3c3164cb9dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Akcelerace dynamických webů prostřednictvím Azure CDN

S prudký nárůst počtu sociálních médií, elektronické obchodování a webové technologie hyper přizpůsobené rychle roste procento obsah obsluhuje koncovým uživatelům se generuje v reálném čase. Uživatelé očekávají, že rychlé, spolehlivé a přizpůsobené webových stránek, nezávisle na jejich prohlížeče, umístění, zařízení nebo sítě. Však velmi inovací, které tyto činnosti, takže zapojení také zpomalit stránce stahování a ohrozit kvalitu prostředí pro uživatele. 

Standardní možnost CDN zahrnuje schopnost souborů z mezipaměti blíže koncovým uživatelům pro urychlení doručení statických souborů. Však s dynamické webové aplikace, ukládání do mezipaměti tohoto obsahu v hraniční umístění není možné, protože server vygeneruje obsah v reakci na chování uživatele. Urychlení doručování takový obsah je mnohem složitější než tradiční edge ukládání do mezipaměti a vyžaduje-komplexní řešení, která přesné vyladění každý prvek v cestě celého datového od zahájení k doručení. S Azure CDN dynamické lokality akcelerace (DSA), je měřitelný výkon lepší výkon webových stránek s dynamickým obsahem.

Azure CDN společnosti Akamai a Verizon nabízí DSA optimalizace prostřednictvím **optimalizované pro** nabídky během vytvoření koncového bodu.

## <a name="configuring-cdn-endpoint-to-accelerate-delivery-of-dynamic-files"></a>Konfigurace koncového bodu CDN pro urychlení doručování dynamické soubory

Můžete nakonfigurovat koncový bod CDN k optimalizaci doručování dynamické soubory prostřednictvím portálu Azure tak, že vyberete **dynamických webů akcelerace** možnost pod **optimalizované pro** vlastnost výběr během vytvoření koncového bodu. Můžete také použít rozhraní REST API nebo některou z klienta sady SDK na stejnou věc udělat prostřednictvím kódu programu. 

### <a name="probe-path"></a>Cesta k testu
Test cesta je funkce specifická pro dynamické akcelerace lokality a platný je vyžadována pro vytvoření. DSA používá malá *testu cesta* souboru umístit na počátku za účelem optimalizace směrování konfigurace sítě pro CDN. Můžete stáhnout a odesílání naše ukázkový soubor do vaší lokality nebo použijte existující prostředek na vaše původ, který se zhruba 10 KB pro cestu testu místo toho pokud existuje asset.

> [!Note]
> DSA budou vám účtovány další poplatky. Další informace najdete v tématu [stránce s cenami](https://azure.microsoft.com/pricing/details/cdn/) Další informace.

Na následujících snímcích obrazovky ilustraci proces prostřednictvím portálu Azure.
 
![Přidání nového koncového bodu CDN](./media/cdn-dynamic-site-acceleration/01_Endpoint_Profile.png) 

*Obrázek 1: Přidání nového koncového bodu CDN z profilu CDN*
 
![Vytvoření nového koncového bodu CDN pomocí agenta DSA](./media/cdn-dynamic-site-acceleration/02_Optimized_DSA.png)  

*Obrázek 2: Vytvoření koncového bodu CDN pomocí dynamických webů akcelerace optimalizace vybrané*

Po vytvoření koncového bodu CDN, bude se vztahovat DSA optimalizace pro všechny soubory, které splňují určitá kritéria. Následující část popisuje optimalizace DSA podrobně.

## <a name="dsa-optimization-using-azure-cdn"></a>Optimalizace DSA pomocí Azure CDN

Dynamické akcelerace lokality na Azure CDN urychluje doručení dynamické prostředků pomocí následujících metod:

-   Optimalizace směrování
-   Optimalizace TCP
-   Předběžné načtení objektu (pouze Akamai)
-   Komprese mobilní bitové kopie (pouze Akamai)

### <a name="route-optimization"></a>Optimalizace směrování

Optimalizace směrování je důležité, protože Internetu je dynamické místo, kde provozu a dočasně výpadků se neustále mění síťové topologie. Protokol BGP (Border Gateway) je protokol směrování v Internetu, ale může být rychlejší trasy prostřednictvím zprostředkující serverů bodů přítomnosti (PoP). 

Optimalizace směrování rozhodne, že optimální cesta k počátku tak, aby lokalita se nepřetržitě dostupné a dynamický obsah doručuje koncovým uživatelům prostřednictvím nejrychlejší a nejspolehlivější trasy možné. 

Akamai síť používá techniky ke shromažďování dat v reálném čase a porovnání různé cesty prostřednictvím různých uzlů v Akamai serveru, jakož i výchozí trasa protokolu BGP přes otevřete Internet k určení nejrychlejší trasy mezi zdroji a hraniční CDN. Tyto postupy vyhnout internetové body zahlcení a dlouho trasy. 

Podobně sítě Verizon se používá v kombinaci libovolného vysílání DNS, vysoké kapacity podpora bodů POP a kontroly stavu určit nejlepší brány a nejlepší data trasy od klienta k počátku.
 
V důsledku toho plně dynamické a transakční obsah doručuje rychleji a spolehlivěji koncovým uživatelům, i když se uncacheable. 

### <a name="tcp-optimizations"></a>Optimalizace TCP

Protokol TCP (Transmission Control) je standardní sada protokolů Internet slouží k poskytování informací mezi aplikací v síti IP.  Ve výchozím nastavení nejsou některé a zpět požadavky nutné k nastavení připojení TCP, stejně jako omezení předejdete congestions sítě, které mít za následek umožňuje zvýšit efektivitu ve velkém měřítku. Azure CDN společnosti Akamai přistupuje k tomuto problému pomocí optimalizace do tří oblastí: 

 - Odstranění pomalé spuštění
 - Využívání trvalé připojení
 - ladění parametry paketu protokolu TCP (pouze Akamai)

#### <a name="eliminating-slow-start"></a>Odstranění pomalé spuštění

*Zpomalit spuštění* je součástí protokolu TCP, která zabraňuje zahlcení sítě omezením množství dat posílaných prostřednictvím sítě. Začíná velikosti okna malé zahlcení mezi odesílatele a příjemce až do dosažení maximální nebo je zjištěna ztráta paketů.

Azure CDN společnosti Akamai a Verizon eliminuje pomalé začne v tři kroky:

1.  Akamai a společnosti Verizon sítě pomocí šířky pásma sledování stavu a měřit šířku pásma připojení mezi servery edge PoP.
2. Podle metrik, které jsou sdílené mezi servery PoP edge tak, aby každý server si je vědoma síťových podmínkách a stavu serveru dalších bodů POP je obcházet.  
3. Servery edge CDN jsou teď moct provádět předpoklady o některé parametry přenosu, například co optimální velikost okna musí být při komunikaci s ostatními servery edge CDN v jeho blízkosti. Tento krok znamená, že velikost okna počáteční zahlcení může být zvýšena, pokud stav připojení mezi servery edge CDN umožňuje přenosech souborů vyšší paketů.  

#### <a name="leveraging-persistent-connections"></a>Využívání trvalé připojení

Pomocí název CDN, menší počet jedinečných počítačů připojení k serveru počátek přímo ve srovnání s uživateli připojovat přímo k do zdrojového umístění. Azure CDN společnosti Akamai a Verizon fondu také požadavky uživatelů společně ke zřízení méně spojení s počátek.

Jak už bylo zmíněno dříve, připojení TCP několik požadavků a zpět provádět v provádění metody handshake pro vytvoření nového připojení. Trvalé připojení, také známé jako "HTTP Keep-Alive," opakovaně používat existující připojení TCP pro víc požadavků HTTP uložte časy odezvy a urychlit doručení. 

Verizon síti přes připojení TCP, aby se zabránilo otevřené připojení z dochází k uzavření rovněž odesílá pravidelné udržovací pakety.

#### <a name="tuning-tcp-packet-parameters"></a>Ladění parametry paketu protokolu TCP

Azure CDN společnosti Akamai také vložených melodie parametry, které řídí připojení k serveru a snižuje množství dlouhé vzdálenosti zaokrouhlit služebních cest potřebnou k načtení obsahu v lokalitě pomocí následujících postupů:

1.  Zvýšit počáteční zahlcení okna tak, aby pakety další můžete odeslat bez čekání na potvrzení.
2.  Časový limit opakování počáteční přenosu snížení tak, že se detekuje ztrátu a opakování přenosu dojde rychleji.
3.  Snížení opakovaného přenosu minimální a maximální časový limit zkrátit čas čekání před za předpokladu, že byly ztrátě paketů při přenosu.

### <a name="object-prefetch-akamai-only"></a>Předběžné načtení objektu (pouze Akamai)

Většina webů obsahovat stránku HTML, který odkazuje na různé prostředky, jako obrázků a skriptů. Obvykle když klient požádá o webovou stránku, prohlížeč nejprve stáhne a analyzuje objekt HTML a potom provede další žádosti o propojené prostředky, které jsou nutné k plně načtení stránky. 

*Předběžné načtení* je technika načíst obrázků a skriptů vložených na stránce HTML během obsluhovaného kódu HTML do prohlížeče, a před i odesílá požadavky těchto objektů. 

Pomocí **předběžné načtení** zapnuta možnost v době, kdy CDN slouží základní HTML stránky do prohlížeče klienta, CDN analyzuje soubor HTML a zkontrolujte další požadavky pro všechny propojené prostředky a ukládá je do své mezipaměti. Když klient zadává požadavky pro propojené prostředky, CDN edge server už má požadovaný objekty a můžete je sloužit bez odezvy na počátek. Tato optimalizace výhody lze uložit do mezipaměti a neurčené obsahu.

### <a name="adaptive-image-compression-akamai-only"></a>Komprese Adaptivní bitové kopie (pouze Akamai)

Některá zařízení, zejména mobilní ty prostředí nižší rychlost sítě čas od času. V těchto scénářích platí je více vhodné pro rychleji přijímat menší Image jejich webové stránce místo čekání na dlouhou dobu pro bitové kopie úplné řešení.

Tato funkce automaticky monitoruje sítě kvality a využívá standardní metody komprese JPEG při rychlost sítě jsou pomalejší ke zlepšení čas doručení.

Komprese Adaptivní bitové kopie | Přípony souborů  
--- | ---  
Komprese JPEG | JPG, JPEG, JPE, .jig, .jgig, .jgi

## <a name="caching"></a>Ukládání do mezipaměti

S DSA ukládání do mezipaměti je vypnutý ve výchozím nastavení v CDN, i když počátek zahrnuje mezipaměti – ovládací prvek nebo vyprší hlavičky v odpovědi. Toto výchozí nastavení je vypnutá, protože DSA se obvykle používá pro dynamické prostředky, které by neměly uložené v mezipaměti, protože jsou jedinečné pro každého klienta a zapnutí ukládání do mezipaměti ve výchozím nastavení může dojít k narušení toto chování.

Pokud máte web se smíšenými statické a dynamické prostředky, je nejlepší postup hybridní získat nejlepší výkon. 

Pokud používáte ADN s Verizon Premium, chcete-li ukládání do mezipaměti zpět na pro určité případy použití modulu pravidla.  

Další možností je použít dva koncové body CDN. Jeden s DSA k poskytování dynamické prostředky a jiným koncovým bodem, jako je například doručení obecné webové k doručení Uložitelný prostředky statické optimalizace typu. Aby bylo možné tento alternativní, upraví adresám URL webové stránky na přímý odkaz na prostředek na koncového bodu CDN, které chcete používat. 

Příklad: `mydynamic.azureedge.net/index.html` dynamické stránky a je načíst z DSA koncového bodu.  Stránky html odkazuje na více statických prostředků, například knihoven jazyka JavaScript nebo bitové kopie, které jsou načítány ze statické koncového bodu CDN, jako například `mystatic.azureedge.net/banner.jpg` a `mystatic.azureedge.net/scripts.js`. 

Příklad můžete nalézt [sem](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller) o tom, jak použít řadiče ve webové aplikaci ASP.NET, která bude sloužit obsah prostřednictvím konkrétní adresy URL CDN.




