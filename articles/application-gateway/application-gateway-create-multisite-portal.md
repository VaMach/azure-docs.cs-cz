---
title: "Hostitelem více webů s Azure Application Gateway | Microsoft Docs"
description: "Tato stránka obsahuje pokyny ke konfiguraci služby Azure application gateway existující pro hostování několika webových aplikací na stejnou bránu pomocí portálu Azure."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 28a7fcb3e08a9c4b6a27e9fbc8d3ebae309adc62
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfigurace existující aplikační brány pro hostování několika webových aplikací

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

Hostování více lokalitu umožňuje nasadit více než jednu webovou aplikaci ve stejném application gateway. Přitom spoléhá na přítomnost Hlavička hostitele v příchozím požadavku HTTP, chcete-li zjistit, který naslouchací proces by přijímat přenosy. Naslouchací proces pak přesměruje přenosy na příslušné back-end fondu podle konfigurace v definici pravidla brány. V protokolu SSL povoleno webových aplikací Aplikační brána spoléhá na toto rozšíření indikace názvu serveru (SNI) vyberte správné naslouchací proces pro webový provoz. Běžně používá pro více hostování lokality je načíst vyrovnávat požadavky na jiných webových domén na jiný server back endové fondy. Více subdomény stejné kořenové domény podobně také může být hostovaný na stejné aplikační brány.

## <a name="scenario"></a>Scénář

V následujícím příkladu se Aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com s dvěma fondy back-end serverů: contoso fondu serverů a fond serverů fabrikam. Podobně jako instalační program může subdomény hostitele jako app.contoso.com a blog.contoso.com.

![scénář nasazení ve více lokalitách][multisite]

## <a name="before-you-begin"></a>Než začnete

Tento scénář přidává podporu více lokalit existující aplikační brány. Abyste mohli dokončit tento scénář, musí být k dispozici ke konfiguraci existující aplikační brány. Navštivte [vytvoření služby application gateway pomocí portálu](application-gateway-create-gateway-portal.md) se dozvíte, jak vytvořit základní aplikační brána v portálu.

Tady jsou kroky potřebné k aktualizaci služby application gateway:

1. Vytvoření fondů back-end, které chcete použít pro každou lokalitu.
2. Vytvořte naslouchací proces pro každou lokalitu Aplikační brána podporuje.
3. Vytvoření pravidel pro mapování jednotlivých naslouchací proces s příslušnou back-end.

## <a name="requirements"></a>Požadavky

* **Fond back-end serverů:** Seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami. Můžete také použít plně kvalifikovaný název domény.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL). Pro aplikace s povolenou více servery brány název hostitele a indikátory SNI jsou také přidat.
* **Pravidlo:** pravidlo váže naslouchací proces fondu back-end serverů a definuje, kterému fondu back-end serverů provoz směrovat při volání příslušného naslouchacího procesu. Pravidla se zpracovávají v pořadí, ve kterém jsou uvedeny a provoz se přesměruje přes první pravidlo, které odpovídá bez ohledu na specifické podobě. Například, pokud máte pravidlo pomocí základní naslouchací proces a pravidla pomocí více lokalit naslouchací proces obou na stejném portu, musí být uvedené pravidlo s několika lokalitami naslouchací proces před pravidlo základní naslouchací proces, aby pravidlo více lokalit a fungovat podle očekávání. 
* **Certifikáty:** každý naslouchací proces vyžaduje jedinečný certifikát, v tomto příkladu jsou vytvořeny 2 naslouchací procesy pro více lokalit. Dva certifikáty PFX a hesla je potřeba vytvořit.

## <a name="create-back-end-pools-for-each-site"></a>Vytvoření fondu back-end pro každou lokalitu

Back-end fondu pro každou lokalitu, že je potřeba brána podporuje aplikace, v takovém případě 2 se vytvářejí, jeden pro contoso11.com a jeden pro fabrikam11.com.

### <a name="step-1"></a>Krok 1

Přejděte do existující aplikační brány na portálu Azure (https://portal.azure.com). Vyberte **back-endové fondy** a klikněte na tlačítko **přidat**

![Přidat back-endové fondy][7]

### <a name="step-2"></a>Krok 2

Zadejte informace pro fond back-end **pool1**, přidávání ip adresy nebo plně kvalifikované názvy domén pro back-end serverů a klikněte na tlačítko **OK**

![nastavení pool1 fondu back-end][8]

### <a name="step-3"></a>Krok 3

V okně back-endové fondy klikněte na **přidat** přidat další back-end fondu **pool2**, přidávání ip adresy nebo plně kvalifikované názvy DOMÉN pro back-end serverů a klikněte na tlačítko **OK**

![nastavení pool2 fondu back-end][9]

## <a name="create-listeners-for-each-back-end"></a>Vytvořte naslouchací procesy pro každý back-end

Služba Application Gateway se při hostování více než jednoho webu na stejné veřejné IP adrese a portu spoléhá na hlavičky hostitele HTTP 1.1. Základní naslouchací proces vytvořený na portálu tuto vlastnost neobsahuje.

### <a name="step-1"></a>Krok 1

Klikněte na tlačítko **naslouchací procesy** na existující aplikační brány a klikněte na **Multi-Site** přidat první naslouchací proces.

![okno Přehled – moduly naslouchání][1]

### <a name="step-2"></a>Krok 2

Zadejte informace pro naslouchací proces. V tomto příkladu SSL je nakonfigurované ukončení, vytvořte nový port front-endu. Nahrajte certifikát .pfx, který chcete použít pro ukončení protokolu SSL. Jediným rozdílem v tomto okně ve srovnání s okně Standardní základní naslouchací proces je název hostitele.

![naslouchací proces vlastnosti okna][2]

### <a name="step-3"></a>Krok 3

Klikněte na tlačítko **Multi-Site** a vytvořit naslouchací proces jiný, jak je popsáno v předchozím kroku pro druhou lokalitu. Nezapomeňte použít jiný certifikát pro druhý naslouchací proces. Jediným rozdílem v tomto okně ve srovnání s okně Standardní základní naslouchací proces je název hostitele. Zadejte informace pro naslouchací proces a klikněte na tlačítko **OK**.

![naslouchací proces vlastnosti okna][3]

> [!NOTE]
> Vytvoření naslouchacího procesu na portálu Azure pro službu application gateway je dlouhotrvající úlohy, může trvat nějakou dobu vytvořit dva naslouchací procesy v tomto scénáři. Po dokončení naslouchací procesy zobrazit na portálu, jak je vidět na následujícím obrázku:

![Přehled naslouchací proces][4]

## <a name="create-rules-to-map-listeners-to-backend-pools"></a>Vytvoření pravidel pro mapování moduly pro naslouchání na back-endové fondy

### <a name="step-1"></a>Krok 1

Přejděte do existující aplikační brány na portálu Azure (https://portal.azure.com). Vyberte **pravidla** a zvolte existující výchozí pravidlo **rule1 New** a klikněte na tlačítko **upravit**.

### <a name="step-2"></a>Krok 2

Vyplňte v okně pravidla, jak je vidět na následujícím obrázku. Výběr prvního naslouchací proces a první fondu a kliknutím na **Uložit** při dokončení.

![upravit existující pravidlo][6]

### <a name="step-3"></a>Krok 3

Klikněte na tlačítko **základní pravidlo** vytvoření druhého pravidla. Vyplňte formulář s druhý naslouchací proces a sekundu back-end fondu a klikněte na tlačítko **OK** uložit.

![přidat základní pravidlo okno][10]

Tento scénář se dokončí konfigurace aplikační brány s podporou více lokalit prostřednictvím portálu Azure.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak chránit své weby s [Application Gateway - brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
