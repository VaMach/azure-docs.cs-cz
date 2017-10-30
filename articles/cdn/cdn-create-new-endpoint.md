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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: b211c2076840b6eff7c21cb481da569ca6bc49a4
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="getting-started-with-azure-cdn"></a>Začínáme s Azure CDN
Tento článek popisuje povolení Azure CDN prostřednictvím vytvoření nového profilu a koncového bodu CDN.

> [!IMPORTANT]
> Úvod do sítě CDN a seznam funkcí najdete v tématu [Přehled sítě CDN](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Vytvoření nového profilu CDN
Profil CDN je kolekcí koncových bodů CDN.  Jednotlivé profily obsahují jeden nebo víc koncových bodů CDN.  Můžete použít více profilů a uspořádat koncové body CDN podle internetové domény, webové aplikace nebo jiných kritérií.

> [!NOTE]
> Předplatné Azure má výchozí omezení pro následující prostředky:
> - Počet profilů CDN, které lze vytvořit
> - Počet koncových bodů, které lze vytvořit v profilu CDN 
> - Počet vlastních domén, které lze namapovat na koncový bod
>
> Informace o omezeních předplatného CDN najdete v tématu věnovaném [omezením CDN](https://docs.microsoft.com/azure/azure-subscription-service-limits#cdn-limits).
>
> Ceny CDN se uplatní na úrovni profilu CDN. Pokud chcete použít kombinaci cenových úrovní Azure CDN, budete potřebovat víc profilů CDN.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Vytvoření nového koncového bodu CDN
**Postup vytvoření nového koncového bodu CDN**

1. Na webu [Azure Portal](https://portal.azure.com) přejděte na svůj profil CDN.  Je možné, že jste si ho v předchozím kroku připnuli k řídicímu panelu.  Pokud ne, najdete ho kliknutím na položku **Procházet**, poté položku **Profily CDN** a nakonec kliknutím na profil, ke kterému plánujete přidat koncový bod.
   
    Otevře se okno Profil CDN.
   
    ![Profil CDN][cdn-profile-settings]
2. Klikněte na tlačítko **Přidat koncový bod**.
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    Otevře se okno **Přidání koncového bodu**.
   
    ![Okno Přidání koncového bodu][cdn-add-endpoint]
3. Zadejte **Název** tohoto koncového bodu CDN.  Tento název se používá pro přístup k prostředkům v mezipaměti v doméně `<endpointname>.azureedge.net`.
4. V rozevíracím seznamu **Typ původu** vyberte typ původu.  V případě účtu Azure Storage vyberte položku **Storage**, v případě cloudové služby Azure vyberte položku **Cloudová služba**, v případě webové aplikace Azure vyberte položku **Webová aplikace** a v případě jakéhokoli jiného veřejně přístupného původu webového serveru (hostovaného v Azure nebo jinde) vyberte položku **Vlastní původ**.
   
    ![Typ původu CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. V rozevíracím seznamu **Název hostitele původu** vyberte nebo zadejte doménu původu.  Tento rozevírací seznam obsahuje všechny dostupné původy typu, který jste zadali v kroku 4.  Pokud jako **Typ zdroje** vyberete *Vlastní zdroj*, zadejte doménu vlastního zdroje.
6. Do textového pole **Cesta původu** zadejte cestu k prostředkům, které chcete uložit do mezipaměti, nebo ponechte pole prázdné, čímž povolíte, aby byl do mezipaměti uložen libovolný prostředek v doméně zadané v kroku 5.
7. Do pole **Hlavička hostitele původu** zadejte hlavičku hostitele, kterou má CDN odeslat spolu s každou žádostí, nebo ponechte výchozí nastavení.
   
   > [!WARNING]
   > Některé typy původu (například Azure Storage a Web Apps) vyžadují, aby se hlavička hostitele shodovala s doménou původu. Pokud nemáte původ, který vyžaduje hlavičku hostitele odlišnou od své domény, je vhodné ponechat výchozí hodnotu.
   > 
   > 
8. V polích **Protokol** a **Port původu** určete protokoly a porty sloužící k přístupu k prostředkům v původu. Je nutné vybrat alespoň jeden protokol (HTTP nebo HTTPS). Pro přístup k obsahu HTTPS použijte doménu poskytnutou sítí CDN (`<endpointname>.azureedge.net`). 
   
   > [!NOTE]
   > **Port původu** má vliv pouze na port použitý koncovým bodem k načtení informací z původu.  Koncový bod jako takový je dostupný pouze koncovým klientům na výchozích portech HTTP a HTTPS (80 a 443), a to bez ohledu na **Port původu**.  
   > 
   > Koncové body **Azure CDN společnosti Akamai** neumožňují použít u původu plný rozsah portů.  Seznam nepovolených portů původu najdete v tématu [Povolené porty původu Azure CDN společnosti Akamai](https://msdn.microsoft.com/library/mt757337.aspx).  
   > 
   > Přístup k obsahu CDN pomocí protokolu HTTPS má tato omezení:
   > 
   > * Je nutné použít certifikát SSL poskytnutý systémem CDN. Certifikáty třetích stran nejsou podporovány.
   > * Podpora protokolu HTTPS pro vlastní domény Azure CDN je dostupná pouze v produktech **Azure CDN od Verizonu** (Standard a Premium). V produktech **Azure CDN od Akamai** se nepodporuje. Další informace najdete v tématu [Povolení nebo zakázání protokolu HTTPS pro vlastní doménu Azure CDN](cdn-custom-ssl.md).
  
9. Kliknutím na tlačítko **Přidat** vytvořte nový koncový bod.
   
   Koncový bod se po vytvoření zobrazí v seznamu koncových bodů daného profilu.
    
   ![Koncový bod CDN][cdn-endpoint-success]
    
   > [!IMPORTANT]
   > Vzhledem k tomu, že rozšíření registrace nějakou dobu trvá, koncový bod nebude hned dostupný pro použití.  V případě profilů <b>Azure CDN od Akamai</b> je šíření obvykle hotové během jedné minuty. V případě profilů <b>Azure CDN od společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.
    > 
    > Pokud se uživatel pokusí použít název domény CDN dříve, než se konfigurace koncového bodu rozšíří do bodů POP, obdrží kódy odpovědí protokolu HTTP 404.  Pokud jste vytvořili koncový bod před několika hodinami, a přesto obdržíte kód odpovědi 404, prostudujte téma [Řešení problémů s koncovými body CDN, které vracejí stav 404](cdn-troubleshoot-endpoint.md).
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
