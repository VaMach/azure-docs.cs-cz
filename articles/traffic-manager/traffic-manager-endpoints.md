---
title: "Správa koncových bodů v Azure Traffic Manageru | Dokumentace Microsoftu"
description: "Tento článek vám pomůže při přidávání, odebírání, povolování a zakazování koncových bodů v Azure Traffic Manageru."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 038270d1-28ba-4078-9c5d-37fc5d683be6
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/17/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 22729a7164b8998e92147e418ce96ff9b09896b9

---

# <a name="add-disable-enable-or-delete-endpoints"></a>Přidávání, zakazování, povolování nebo odstraňování koncových bodů

Funkce Web Apps v rámci služby Azure App Service již poskytuje funkce směrování provozu s převzetím služeb při selhání a s kruhovým dotazováním pro weby v datovém centru bez ohledu na režim příslušného webu. Azure Traffic Manager umožňuje určit směrování provozu s převzetím služeb při selhání a s kruhovým dotazováním pro weby a služby Cloud Services v různých datových centrech. Jako první krok pro poskytnutí těchto funkcí je třeba přidat příslušný koncový bod služby Cloud Services nebo webu do Traffic Manageru.

> [!NOTE]
> Externí umístění ani profily Traffic Manageru nelze přidávat jako koncové body prostřednictvím portálu Azure Classic. Je nutné použít funkci [Vytvořit definici](http://go.microsoft.com/fwlink/p/?LinkId=400772) rozhraní API REST nebo funkci [Add-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774) prostředí Windows PowerShell.

Můžete také zakázat jednotlivé koncové body, které jsou součástí profilu Traffic Manageru. Koncovými body můžou být služby Cloud Services nebo weby. Zakázaný koncový bod zůstává součástí profilu, profil však pracuje, jako by příslušný koncový bod neobsahoval. Tato akce je velmi užitečná pro dočasné odebrání koncového bodu, který se nachází v režimu údržby nebo který je znovu nasazován. Jakmile je koncový bod znovu spuštěn a pracuje, může být povolen.

> [!NOTE]
> Zakázání koncového bodu nijak nesouvisí s jeho stavem nasazení v Azure. Funkční koncový bod zůstane v provozu a může přijímat provoz i poté, co je v Traffic Manageru zakázán. Zakázání koncový bod v jednom profilu navíc nemá vliv na jeho stav v jiném profilu.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Přidání koncového bodu služby Cloud Services nebo webu

1. V podokně Traffic Manager na portálu Azure Classic vyhledejte profil Traffic Manageru obsahující nastavení koncového bodu, která chcete upravit, a pak klikněte na šipku vpravo od názvu profilu. Otevře se stránka nastavení pro příslušný profil.
2. V horní části stránky klikněte na **Koncové body**. Zobrazí se koncové body, které již jsou součástí konfigurace.
3. V dolní části stránky, klikněte na **Přidat**. Přejdete na stránku **Přidat koncové body služby**. Ve výchozím nastavení jsou na stránce uvedeny služby Cloud Services v části **Koncové body služby**.
4. Chcete-li služby Cloud Services povolit jako koncové body pro tento profil, vyberte je v seznamu. Vymazáním názvu služby Cloud Services odeberete příslušnou službu Cloud Services ze seznamu koncových bodů.
5. V případě webů klikněte na **Typ služby** a pak vyberte možnost **Webová aplikace**.
6. Výběrem webů v seznamu je přidáte jako koncové body pro tento profil. Vymazáním názvu webu odeberete příslušný web ze seznamu koncových bodů. Všimněte si, že pro jedno datové centrum Azure (označováno také jako oblast) můžete vybrat jen jeden web. Když vyberete web v datovém centru, které je hostitelem více webů, po výběru prvního webu přestanou být ostatní weby ve stejném datovém centru k dispozici pro výběr. Všimněte si také, že jsou uvedeny pouze standardní weby.
7. Po výběru koncových bodů pro tento profil uložte změny kliknutím na symbol zaškrtnutí vpravo dole.

> [!NOTE]
> Pokud používáte metodu směrování provozu *Převzetí služeb při selhání*, po přidání nebo odebrání koncového bodu nezapomeňte upravit seznam priorit převzetí služeb při selhání na stránce Konfigurace tak, aby odpovídal požadovanému pořadí převzetí služeb při selhání pro příslušnou konfiguraci. Další informace najdete v části [Konfigurování směrování provozu s převzetím služeb při selhání](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Zakázání koncového bodu

1. V podokně Traffic Manager na portálu Azure Classic vyhledejte profil Traffic Manageru obsahující nastavení koncového bodu, která chcete upravit, a pak klikněte na šipku vpravo od názvu profilu. Otevře se stránka nastavení pro příslušný profil.
2. V horní části stránky klikněte na **Koncové body**. Zobrazí se koncové body, které jsou zahrnuty v konfiguraci.
3. Klikněte na koncový bod, který chcete zakázat, a pak klikněte na **Zakázat** v dolní části stránky.
4. Provoz směrem do příslušného koncového bodu se s ohledem na hodnotu TTL (Time to Live) systému DNS nakonfigurovanou pro název domény Traffic Manageru zastaví. Hodnotu TTL můžete změnit na stránce Konfigurace profilu Traffic Manageru.

## <a name="to-enable-an-endpoint"></a>Povolení koncového bodu

1. V podokně Traffic Manager na portálu Azure Classic vyhledejte profil Traffic Manageru obsahující nastavení koncového bodu, která chcete upravit, a pak klikněte na šipku vpravo od názvu profilu. Otevře se stránka nastavení pro příslušný profil.
2. V horní části stránky klikněte na **Koncové body**. Zobrazí se koncové body, které jsou zahrnuty v konfiguraci.
3. Klikněte na koncový bod, který chcete povolit, a pak klikněte na **Povolit** v dolní části stránky.
4. Obnoví se provoz do služby podle nastavení profilu.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Odstranění koncového bodu služby Cloud Services nebo webu

1. V podokně Traffic Manager na portálu Azure Classic vyhledejte profil Traffic Manageru obsahující nastavení koncového bodu, která chcete upravit, a pak klikněte na šipku vpravo od názvu profilu. Otevře se stránka nastavení pro příslušný profil.
2. V horní části stránky klikněte na **Koncové body**. Zobrazí se koncové body, které již jsou součástí konfigurace.
3. Na stránce Koncové body klikněte na název koncového bodu, který chcete z profilu odstranit.
4. Na konci stránky klikněte na **Odstranit**.

> [!NOTE]
> Externí umístění ani profily Traffic Manageru nelze odstraňovat jako koncové body pomocí portálu Azure Classic. Je nutné použít prostředí Windows PowerShell. Další informace najdete v tématu [Remove-AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Další kroky

* [Konfigurace metody směrování s převzetím služeb při selhání](traffic-manager-configure-failover-routing-method.md)
* [Konfigurace metody směrování s kruhovým dotazováním](traffic-manager-configure-round-robin-routing-method.md)
* [Konfigurace metody směrování podle výkonu](traffic-manager-configure-performance-routing-method.md)
* [Řešení potíží při sníženém výkonu Traffic Manageru](traffic-manager-troubleshooting-degraded.md)
* [Operace v Traffic Manageru (referenční informace k rozhraní API REST)](http://go.microsoft.com/fwlink/p/?LinkID=313584)




<!--HONumber=Nov16_HO5-->


