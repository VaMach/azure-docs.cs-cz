---
title: "Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service, která používá Traffic Manager pro vyrovnávání zatížení."
description: "Vlastní název domény pro použití webové aplikace ve službě Azure App Service, která zahrnuje Traffic Manager pro vyrovnávání zatížení."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 5f099201d9018a6f8577cb3daf127d09560fb94b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service pomocí Traffic Manager
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Tento článek obsahuje obecné pokyny pro používání vlastního názvu domény službou Azure App Service, které pomocí služby Traffic Manager pro vyrovnávání zatížení.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Principy záznamy DNS
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurace webových aplikacích pro standardní režim
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Přidejte záznam DNS pro vaši vlastní doménu.
> [!NOTE]
> Pokud jste zakoupili domény prostřednictvím Azure App Service Web Apps přeskočit následující kroky a odkazovat na posledním kroku [koupit domény pro webové aplikace](custom-dns-web-site-buydomains-web-app.md) článku.
> 
> 

Pokud chcete přiřadit vlastní domény webové aplikace ve službě Azure App Service, musí přidáte nový záznam v tabulce DNS pro vaši vlastní doménu pomocí nástrojů poskytovaných registrátora domény, které jste zakoupili název domény z. Použijte následující postup pro vyhledání a pomocí nástrojů DNS.

1. Přihlaste se ke svému účtu u registrátora domény a vyhledejte stránku pro správu záznamy DNS. Vyhledejte odkazy nebo oblasti lokality označený jako **název domény**, **DNS**, nebo **název serveru správy**. Často můžete najít odkaz na tuto stránku zobrazení informací o vašem účtu a podívat se na odkaz, jako **mé domény**.
2. Jakmile naleznete na stránce Správa pro název domény, vyhledejte odkaz, který slouží k úpravě záznamů DNS. To může být uveden jako **souboru zóny**, **záznamy DNS**, nebo jako **Upřesnit** odkazu na konfiguraci.
   
   * Stránky bude mít pravděpodobně několik záznamů, které jsou již vytvořeny, jako je například přidružení vstupního '**@**'nebo'\*' 'domény parkovací' stránky. Záznamy pro běžné dílčím doménám domény může obsahovat také jako **www**.
   * Bude zmínili stránce **záznamy CNAME**, nebo zadejte rozevíracího seznamu vyberte typ záznamu. Například je může další záznamy také zmínili **záznamy A** a **záznamů MX**. V některých případech záznamy CNAME bude volat jiné názvy, jako **záznam aliasu**.
   * Stránky bude mít i pole, které vám umožní **mapy** z **název hostitele** nebo **název domény** na jiný název domény.
3. Když jsou specifikace každý Registrátor liší, obecně namapujete *z* vlastního názvu domény (například **contoso.com**,) *k* název domény Traffic Manageru (**contoso.trafficmanager.net**) používané pro vaši webovou aplikaci.
   
   > [!NOTE]
   > Pokud záznam se už používá a je nutné ho preventivně svázat aplikace k němu, případně můžete vytvořit další záznam CNAME. Například ho preventivně vazby **www.contoso.com** do webové aplikace, vytvořte záznam CNAME z **awverify.www** k **contoso.trafficmanager.net**. Potom můžete přidat "www.contoso.com" do vaší webové aplikace beze změny záznam CNAME "www". Další informace najdete v tématu [záznamy DNS vytvořit pro webové aplikace ve vlastní doménu][CREATEDNS].
   > 
   > 
4. Po dokončení přidávání nebo úpravě záznamů DNS u registrátora uložte změny.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Povolit správce provozu
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Další kroky
Další informace najdete ve [Středisku pro vývojáře Node.js](/develop/nodejs/).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
