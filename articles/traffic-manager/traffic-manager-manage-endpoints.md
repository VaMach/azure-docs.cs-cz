---
title: "Správa koncových bodů v Azure Traffic Manageru | Dokumentace Microsoftu"
description: "Tento článek vám pomůže při přidávání, odebírání, povolování a zakazování koncových bodů v Azure Traffic Manageru."
services: traffic-manager
documentationcenter: 
author: sdwheeler
manager: carmonm
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e6af6652d39fad5812c15e19fa29c757595a78b6

---

# <a name="add-disable-enable-or-delete-endpoints"></a>Přidávání, zakazování, povolování nebo odstraňování koncových bodů

Funkce Web Apps v rámci služby Azure App Service již poskytuje funkce směrování provozu s převzetím služeb při selhání a s kruhovým dotazováním pro weby v datovém centru bez ohledu na režim příslušného webu. Azure Traffic Manager umožňuje určit směrování provozu s převzetím služeb při selhání a s kruhovým dotazováním pro weby a služby Cloud Services v různých datových centrech. Jako první krok pro poskytnutí těchto funkcí je třeba přidat příslušný koncový bod služby Cloud Services nebo webu do Traffic Manageru.

> [!NOTE]
> Tento článek vysvětluje, jak používat klasický portál. Portál Azure Classic podporuje jen vytváření a přiřazování cloudových služeb a webových aplikací jako koncových bodů. Preferovaným rozhraním je nový [Azure Portal](https://portal.azure.com).

Můžete také zakázat jednotlivé koncové body, které jsou součástí profilu Traffic Manageru. Zakázaný koncový bod zůstává součástí profilu, profil však pracuje, jako by příslušný koncový bod neobsahoval. Tato akce je užitečná pro dočasné odebrání koncového bodu, který se nachází v režimu údržby nebo který se znovu nasazuje. Jakmile je koncový bod znovu spuštěn a pracuje, může být povolen.

> [!NOTE]
> Zakázání koncového bodu nijak nesouvisí s jeho stavem nasazení v Azure. Funkční koncový bod zůstane v provozu a může přijímat provoz i poté, co je v Traffic Manageru zakázán. Zakázání koncový bod v jednom profilu navíc nemá vliv na jeho stav v jiném profilu.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Přidání koncového bodu služby Cloud Services nebo webu

1. V podokně Traffic Manager na portálu Azure Classic vyhledejte profil Traffic Manageru obsahující nastavení koncového bodu, která chcete upravit. Stránku nastavení otevřete kliknutím na šipku vpravo od názvu profilu.
2. V horní části stránky klikněte na **Koncové body**. Zobrazí se koncové body, které již jsou součástí konfigurace.
3. V dolní části stránky, klikněte na **Přidat**. Přejdete na stránku **Přidat koncové body služby**. Ve výchozím nastavení jsou na stránce uvedeny služby Cloud Services v části **Koncové body služby**.
4. Pokud chcete jako koncové body pro tento profil přidat cloudové služby, vyberte je v seznamu. Vymazáním názvu služby Cloud Services odeberete příslušnou službu Cloud Services ze seznamu koncových bodů.
5. V případě webů klikněte na **Typ služby** a pak vyberte možnost **Webová aplikace**.
6. Výběrem webů v seznamu je přidáte jako koncové body pro tento profil. Vymazáním názvu webu odeberete příslušný web ze seznamu koncových bodů. Pro jedno datacentrum Azure (označované také jako oblast) můžete vybrat jen jeden web. Když vyberete první web, znemožní se výběr dalších webů ve stejném datacentru. Všimněte si také, že jsou uvedeny pouze standardní weby.
7. Po výběru koncových bodů pro tento profil uložte změny kliknutím na symbol zaškrtnutí vpravo dole.

> [!NOTE]
> Po přidání nebo odebrání koncového bodu z profilu metodou směrování provozu *Převzetí služeb při selhání* nemusí být seznam priorit přebírání služeb seřazen tak, jak si představujete. Pořadí tohoto seznamu priorit můžete upravit na stránce Konfigurace. Další informace najdete v části [Konfigurování směrování provozu s převzetím služeb při selhání](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Zakázání koncového bodu

1. V podokně Traffic Manager na portálu Azure Classic vyhledejte profil Traffic Manageru obsahující nastavení koncového bodu, která chcete upravit. Stránku nastavení otevřete kliknutím na šipku vpravo od názvu profilu.
2. V horní části stránky klikněte na **Koncové body**. Zobrazí se koncové body, které jsou zahrnuty v konfiguraci.
3. Klikněte na koncový bod, který chcete zakázat, a pak klikněte na **Zakázat** v dolní části stránky.
4. Klienti budou dál posílat data na koncový bod po dobu odpovídající hodnotě Time-to-Live (TTL). Hodnotu TTL můžete změnit na stránce Konfigurace profilu Traffic Manageru.

## <a name="to-enable-an-endpoint"></a>Povolení koncového bodu

1. V podokně Traffic Manager na portálu Azure Classic vyhledejte profil Traffic Manageru obsahující nastavení koncového bodu, která chcete upravit. Stránku nastavení otevřete kliknutím na šipku vpravo od názvu profilu.
2. V horní části stránky klikněte na **Koncové body**. Zobrazí se koncové body, které jsou zahrnuty v konfiguraci.
3. Klikněte na koncový bod, který chcete povolit, a pak klikněte na **Povolit** v dolní části stránky.
4. Klienti se budou směrovat na povolený koncový bod určený profilem.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Odstranění koncového bodu služby Cloud Services nebo webu

1. V podokně Traffic Manager na portálu Azure Classic vyhledejte profil Traffic Manageru obsahující nastavení koncového bodu, která chcete upravit. Stránku nastavení otevřete kliknutím na šipku vpravo od názvu profilu.
2. V horní části stránky klikněte na **Koncové body**. Zobrazí se koncové body, které již jsou součástí konfigurace.
3. Na stránce Koncové body klikněte na název koncového bodu, který chcete z profilu odstranit.
4. Na konci stránky klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

* [Správa profilů Traffic Manager](traffic-manager-manage-profiles.md)
* [Konfigurace metod směrování](traffic-manager-configure-routing-method.md)
* [Řešení potíží při sníženém výkonu Traffic Manageru](traffic-manager-troubleshooting-degraded.md)
* [Důležité informace o výkonu nástroje Traffic Manager](traffic-manager-performance-considerations.md)
* [Operace v Traffic Manageru (referenční informace k rozhraní API REST)](http://go.microsoft.com/fwlink/p/?LinkID=313584)




<!--HONumber=Nov16_HO2-->


