---
title: "Řízení síťového provozu webové aplikace Azure nástroje Azure Traffic Manager"
description: "Tento článek obsahuje souhrnné informace pro Azure Traffic Manager, protože se týká webových aplikacích Azure."
services: app-service\web
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: fb7d391e3118a9dccde5501c3f30c6f580932a30
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Řízení síťového provozu webové aplikace Azure nástroje Azure Traffic Manager
> [!NOTE]
> Tento článek obsahuje souhrnné informace pro Microsoft Azure Traffic Manager, protože se týká Azure App Service Web Apps. Další informace o Azure Traffic Manager, sám, může navštívením odkazy na konci tohoto článku.
> 
> 

## <a name="introduction"></a>Úvod
Azure Traffic Manageru můžete použít k řízení, jak jsou žádosti klientů webové distribuovány do webové aplikace v Azure App Service. Když koncových bodů webové aplikace jsou přidány do profilu Azure Traffic Manager, Azure Traffic Manager uchovává informace o stavu vašich webových aplikací (spuštěna, zastavena nebo odstraněné) tak, aby se můžete rozhodnout, která z těchto koncových bodů musí přijímat přenosy.

## <a name="load-balancing-methods"></a>Metody vyrovnávání zatížení
Azure Traffic Manager používá tři metody vyrovnávání zatížení různé. Tyto možnosti jsou popsány v následujícím seznamu podle kterých se týkají webové aplikace Azure.

* **Převzetí služeb při selhání**: Pokud máte webové aplikace klony v různých oblastech, můžete pomocí této metody můžete nakonfigurovat jednu webovou aplikaci do veškerá komunikace s klienty webové služby a nakonfigurovat jiné webové aplikace v jiné oblasti pro případ, že nedostupný první webové aplikace služby tento přenos.
* **Kruhové dotazování**: Pokud máte webové aplikace klony v různých oblastech, tuto metodu můžete použít pro provoz se rovnoměrně distribuuje mezi webové aplikace v různých oblastech.
* **Výkon**: metoda The výkonu distribuuje provoz podle nejkratší doba odezvy pro klienty. Výkon metodu můžete použít pro webové aplikace v rámci stejné oblasti nebo v různých oblastech.

## <a name="web-apps-and-traffic-manager-profiles"></a>Webové aplikace a profily Traffic Manageru
Konfigurace řízení provozu webové aplikace, vytvořte profil v, používá, jeden ze tří zatížení vyrovnávání metody popsané programem Azure Traffic Manager a poté přidejte koncových bodů (v tomto případě webové aplikace) pro které chcete k řízení provozu do profilu. Stav vaší webové aplikace (spuštěna, zastavena nebo odstranit) je pravidelně oznamovat v profilu tak, aby Azure Traffic Manager může směrovat přenosy odpovídajícím způsobem.

Pokud používáte Azure Traffic Manageru službou Azure, mějte na paměti následující body:

* Pro nasazení pouze webových aplikací v rámci stejné oblasti webové aplikace již poskytuje převzetí služeb při selhání a funkce kruhového dotazování bez ohledu na režim webové aplikace.
* Pro nasazení ve stejné oblasti, která používají webové aplikace ve spojení s jiné cloudové služby Azure můžete kombinovat oba typy koncových bodů povolit hybridní scénáře.
* V profilu můžete určit pouze jeden koncový bod webové aplikace podle oblastí. Když vyberete jako koncový bod pro jednu oblast webové aplikace, zbývající webové aplikace v této oblasti k dispozici pro výběr pro tento profil.
* Koncových bodů webové aplikace, které zadáte v profilu Azure Traffic Manager se zobrazí v části **názvy domén** části na stránce konfigurace pro webovou aplikaci v profilu, ale nebude konfigurovat existuje.
* Po přidání webové aplikace do profilu, **adresa URL webu** na řídicím panelu na webu zobrazí stránku portálu aplikace vlastní domény adresa URL webové aplikace, pokud jste nastavili jednu. Jinak, zobrazí adresu URL profilu Traffic Manageru (například `contoso.trafficmgr.com`). Název domény přímé webovou aplikaci a adresu URL správce provozu se nebude zobrazovat na stránce konfigurace webové aplikace v rámci **názvy domén** části.
* Názvy vlastních domén budou fungovat podle očekávání, ale kromě jejich přidáním do webové aplikace, musíte také nakonfigurovat mapu DNS a přejděte na adresu URL správce provozu. Informace o tom, jak nastavit vlastní doménu pro Azure webové aplikace najdete v tématu [konfigurace vlastního názvu domény pro webovou stránku Azure](app-service-web-tutorial-custom-domain.md).
* Můžete přidat pouze webových aplikací, které jsou v režimu standard nebo premium do profilu Azure Traffic Manager.

## <a name="next-steps"></a>Další kroky
Koncepční a technický přehled o Azure Traffic Manager, najdete v části [Traffic Manager Přehled](../traffic-manager/traffic-manager-overview.md).

Další informace o používání správce provozu s webovými aplikacemi, najdete v příspěvcích na blogu [pomocí Azure Traffic Manager s weby Azure](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) a [Azure Traffic Manager teď můžete integrovat s weby Azure](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

