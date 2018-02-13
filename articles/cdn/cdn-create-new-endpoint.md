---
title: "Začínáme s Azure CDN | Dokumentace Microsoftu"
description: "Toto téma ukazuje, jak povolit Azure Content Delivery Network (CDN). Kurz vás provede vytvořením nového profilu a koncového bodu CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2018
ms.author: mazha
ms.openlocfilehash: f1681b0796885cd6dc880303485edb97c15cda71
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="getting-started-with-azure-cdn"></a>Začínáme s Azure CDN
Tento článek popisuje povolení služby Azure [Content Delivery Network (CDN)](cdn-overview.md) prostřednictvím vytvoření nového profilu a koncového bodu CDN.

## <a name="create-a-new-cdn-profile"></a>Vytvoření nového profilu CDN
Profil CDN je kolekcí koncových bodů CDN. Jednotlivé profily mohou obsahovat jeden nebo víc koncových bodů CDN. K uspořádání koncových bodů CDN podle internetové domény, webové aplikace nebo jiných kritérií můžete použít několik profilů.

> [!NOTE]
> Předplatné Azure má výchozí omezení pro následující prostředky:
> - Počet profilů CDN, které lze vytvořit
> - Počet koncových bodů, které lze vytvořit v profilu CDN 
> - Počet vlastních domén, které lze namapovat na koncový bod
>
> Informace o omezeních předplatného CDN najdete v tématu věnovaném [omezením CDN](https://docs.microsoft.com/azure/azure-subscription-service-limits#cdn-limits).
>
> Ceny CDN se uplatní na úrovni profilu CDN. Proto pokud chcete použít kombinaci cenových úrovní Azure CDN, musíte vytvořit víc profilů CDN.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Vytvoření nového koncového bodu CDN
**Postup vytvoření nového koncového bodu CDN**

1. Na webu [Azure Portal](https://portal.azure.com) přejděte na svůj profil CDN. Je možné, že jste si ho v předchozím kroku připnuli k řídicímu panelu. Pokud ne, najdete ho tak, že vyberete **Všechny služby** a potom vyberete **Profily CDN**. V podokně **Profily CDN** vyberte profil, ke kterému plánujete přidat koncový bod. 
   
    Otevře se podokno Profil CDN.
   
    ![Profil CDN][cdn-profile-settings]

2. Vyberte **Koncový bod**.
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    Otevře se podokno **Přidat koncový bod**.
   
    ![Přidat koncový bod][cdn-add-endpoint]

3. Do pole **Název** zadejte jedinečný název pro nový koncový bod CDN. Tento název se používá pro přístup k prostředkům v mezipaměti v doméně `<endpointname>.azureedge.net`.

4. V poli **Typ zdroje** vyberte typ zdroje. V případě účtu Azure Storage vyberte položku **Storage**, v případě cloudové služby Azure vyberte položku **Cloudová služba**, v případě webové aplikace Azure vyberte položku **Webová aplikace** a v případě jakéhokoli jiného veřejně přístupného původu webového serveru (hostovaného v Azure nebo jinde) vyberte položku **Vlastní původ**.
   
    ![Typ původu CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)

5. V poli **Název počátečního hostitele** vyberte nebo zadejte zdrojovou doménu. Tento rozevírací seznam obsahuje všechny dostupné zdroje typu, který jste zadali v kroku 4. Pokud jste jako typ zdroje vybrali **Vlastní zdroj**, zadejte doménu vlastního zdroje.
    
6. Do pole **Cesta ke zdroji** zadejte cestu k prostředkům, které chcete uložit do mezipaměti, nebo ponechte pole prázdné, čímž povolíte, aby se do mezipaměti uložil libovolný prostředek v doméně zadané v kroku 5.
    
7. Do pole **Hlavička počátečního hostitele** zadejte hlavičku hostitele, kterou má Azure CDN odeslat spolu s každou žádostí, nebo ponechte výchozí nastavení.
   
   > [!WARNING]
   > Některé typy původu (například Azure Storage a Web Apps) vyžadují, aby se hlavička hostitele shodovala s doménou původu. Pokud nemáte původ, který vyžaduje hlavičku hostitele odlišnou od své domény, je vhodné ponechat výchozí hodnotu.
   > 
    
8. V polích **Protokol** a **Port původu** určete protokoly a porty sloužící k přístupu k prostředkům v původu. Je nutné vybrat alespoň jeden protokol (HTTP nebo HTTPS). Pro přístup k obsahu HTTPS použijte doménu poskytnutou sítí CDN (`<endpointname>.azureedge.net`). 
   
   > [!NOTE]
   > Hodnota **Počáteční port** určuje jenom port použitý koncovým bodem k načtení informací ze zdroje. Koncový bod jako takový je dostupný jenom koncovým klientům na výchozích portech HTTP a HTTPS (80 a 443), a to bez ohledu na nastavení **Počáteční port**.  
   > 
   > Koncové body v profilech **Azure CDN od Akamai** pro počáteční porty neumožňují použití plného rozsahu portů. Seznam nepovolených portů původu najdete v tématu [Povolené porty původu Azure CDN společnosti Akamai](https://msdn.microsoft.com/library/mt757337.aspx).  
   > 
   > Pokud k obsahu CDN přistupujete s využitím HTTPS, platí tato omezení:
   > 
   > * Použijte certifikát SSL poskytnutý službou CDN. Certifikáty třetích stran nejsou podporovány.
   > * Podpora protokolu HTTPS pro vlastní domény Azure CDN je dostupná pouze v produktech **Azure CDN od Verizonu** (Standard a Premium). V produktech **Azure CDN od Akamai** se nepodporuje. Další informace najdete v tématu [Konfigurace HTTPS pro vlastní doménu Azure CDN](cdn-custom-ssl.md).
    
9. Pokud chcete vytvořit nový koncový bod, vyberte **Přidat**.
   
   Koncový bod se po vytvoření zobrazí v seznamu koncových bodů daného profilu.
    
   ![Koncový bod CDN][cdn-endpoint-success]
    
   > [!IMPORTANT]
   > Vzhledem k tomu, že rozšíření registrace nějakou dobu trvá, koncový bod není okamžitě dostupný pro použití. V případě profilů **Azure CDN od Akamai** je šíření obvykle hotové během jedné minuty. V případě profilů **Azure CDN od společnosti Verizon** je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.
    > 
    > Pokud se pokusíte použít název domény CDN dřív, než se konfigurace koncového bodu rozšíří do bodů POP, může se zobrazit stav odpovědi HTTP 404. Pokud jste koncový bod vytvořili před několika hodinami, a přesto se stále zobrazuje stav odpovědi 404, projděte si téma [Řešení problémů s koncovými body CDN, které vracejí stav 404](cdn-troubleshoot-endpoint.md).
    > 
    > 

## <a name="see-also"></a>Viz také
* [Řízení chování ukládání do mezipaměti u žádostí s řetězci dotazu](cdn-query-string.md)
* [Postup mapování obsahu CDN do vlastní domény](cdn-map-content-to-custom-domain.md)
* [Předběžné načtení prostředků v koncovém bodu Azure CDN](cdn-preload-endpoint.md)
* [Vyprázdnění koncového bodu Azure CDN](cdn-purge-endpoint.md)
* [Řešení problémů s koncovými body CDN, které vracejí stav 404](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
