---
title: "Používání Azure CDN | Dokumentace Microsoftu"
description: "Toto téma ukazuje, jak povolit Síť pro doručování obsahu (CDN) pro Azure. Kurz vás provede vytvořením nového profilu a koncového bodu CDN."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e5568503541e773fa11bcde530da09befc51b2ad


---
# <a name="using-azure-cdn"></a>Používání Azure CDN
Toto téma vás provede povolením Azure CDN prostřednictvím vytvoření nového profilu a koncového bodu CDN.

> [!IMPORTANT]
> Úvod do fungování CDN a seznam funkcí najdete v tématu [Přehled CDN](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Vytvoření nového profilu CDN
Profil CDN je kolekcí koncových bodů CDN.  Jednotlivé profily obsahují jeden nebo víc koncových bodů CDN.  Můžete použít více profilů a uspořádat koncové body CDN podle internetové domény, webové aplikace nebo jiných kritérií.

> [!NOTE]
> Ve výchozím nastavení je jedno předplatné Azure omezeno na osm profilů CDN. Jednotlivé profily CDN jsou omezené na deset koncových bodů CDN.
> 
> Ceny CDN se uplatní na úrovni profilu CDN. Pokud chcete použít kombinaci cenových úrovní Azure CDN, budete potřebovat víc profilů CDN.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Vytvoření nového koncového bodu CDN
**Postup vytvoření nového koncového bodu CDN**

1. Na [webu Azure Portal](https://portal.azure.com) přejděte na svůj profil CDN.  Je možné, že jste si ho v předchozím kroku připnuli k řídicímu panelu.  Pokud ne, najdete ho kliknutím na položku **Procházet**, poté položku **Profily CDN** a nakonec kliknutím na profil, ke kterému plánujete přidat koncový bod.
   
    Otevře se okno Profil CDN.
   
    ![Profil CDN][cdn-profile-settings]
2. Klikněte na tlačítko **Přidat koncový bod**.
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    Otevře se okno **Přidání koncového bodu**.
   
    ![Okno Přidání koncového bodu][cdn-add-endpoint]
3. Zadejte **Název** tohoto koncového bodu CDN.  Tento název se použije pro přístup k prostředkům v mezipaměti v doméně `<endpointname>.azureedge.net`.
4. V rozevíracím seznamu **Typ původu** vyberte typ původu.  V případě účtu Azure Storage vyberte položku **Storage**, v případě cloudové služby Azure vyberte položku **Cloudová služba**, v případě webové aplikace Azure vyberte položku **Webová aplikace** a v případě jakéhokoli jiného veřejně přístupného původu webového serveru (hostovaného v Azure nebo jinde) vyberte položku **Vlastní původ**.
   
    ![Typ původu CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. V rozevíracím seznamu **Název hostitele původu** vyberte nebo zadejte doménu původu.  Tento rozevírací seznam bude obsahovat všechny dostupné původy typu, který jste zadali v kroku 4.  Pokud jste jako **Typ původu** vybrali možnost *Vlastní původ*, zadáte doménu vlastního původu.
6. Do textového pole **Cesta původu** zadejte cestu k prostředkům, které chcete uložit do mezipaměti, nebo ponechte pole prázdné, čímž povolíte, aby byl do mezipaměti uložen libovolný prostředek v doméně zadané v kroku 5.
7. Do pole **Hlavička hostitele původu** zadejte hlavičku hostitele, kterou má CDN odeslat spolu s každou žádostí, nebo ponechte výchozí nastavení.
   
   > [!WARNING]
   > Některé typy původu (například Azure Storage a Web Apps) vyžadují, aby se hlavička hostitele shodovala s doménou původu. Pokud nemáte původ, který vyžaduje hlavičku hostitele odlišnou od své domény, je vhodné ponechat výchozí hodnotu.
   > 
   > 
8. V polích **Protokol** a **Port původu** určete protokoly a porty sloužící k přístupu k prostředkům v původu.  Je nutné vybrat alespoň jeden protokol (HTTP nebo HTTPS).
   
   > [!NOTE]
   >  **Port původu** má vliv pouze na port použitý koncovým bodem k načtení informací z původu.  Koncový bod jako takový bude dostupný pouze koncovým klientům na výchozích portech HTTP a HTTPS (80 a 443), a to bez ohledu na **Port původu**.  
   > 
   > Koncové body **Azure CDN společnosti Akamai** neumožňují použít u původu plný rozsah portů.  Seznam nepovolených portů původu najdete v tématu [Povolené porty původu Azure CDN společnosti Akamai](https://msdn.microsoft.com/library/mt757337.aspx).  
   > 
   > Přístup k obsahu CDN pomocí protokolu HTTPS má tato omezení:
   > 
   > * Je nutné použít certifikát SSL poskytnutý systémem CDN. Certifikáty třetích stran nejsou podporovány.
   > * Pro přístup k obsahu HTTPS je nutné použít doménu poskytnutou systémem CDN (`<endpointname>.azureedge.net`). U vlastních názvů domén (záznamů CNAME) není dostupná podpora protokolu HTTPS, protože CDN aktuálně nepodporuje vlastní certifikáty.
   > 
   > 
9. Kliknutím na tlačítko **Přidat** vytvořte nový koncový bod.
10. Jakmile je koncový bod vytvořený, zobrazí se v seznamu koncových bodů daného profilu. Zobrazení seznamu zobrazuje adresu URL, kterou je nutné použít k přístupu k obsahu v mezipaměti, a také doménu původu.
    
    ![Koncový bod CDN][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > Koncový bod nebude hned dostupný pro použití, protože chvíli trvá, než se registrace rozšíří v rámci CDN.  V případě profilů <b>Azure CDN společnosti Akamai</b> je šíření obvykle hotové během jedné minuty.  V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.
    > 
    > Pokud se uživatel pokusí použít název domény CDN dřív, než se konfigurace koncového bodu rozšíří do bodů POP, obdrží kódy odpovědí protokolu HTTP 404.  Pokud jste vytvořili koncový bod před několika hodinami, a přesto obdržíte kód odpovědi 404, prostudujte prosím téma [Řešení problémů s koncovými body CDN, které vracejí stav 404](cdn-troubleshoot-endpoint.md).
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



<!--HONumber=Nov16_HO2-->


