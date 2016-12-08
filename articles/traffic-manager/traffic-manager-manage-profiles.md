---
title: "Správa profilů Azure Traffic Manageru | Dokumentace Microsoftu"
description: "Tento článek vám pomůže vytvořit, zakázat, povolit nebo odstranit profil Azure Traffic Manageru, případně zobrazit jeho historii."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 7e7de7dc1eca6903403afef03fdd6afb98ff16c9

---

# <a name="manage-an-azure-traffic-manager-profile"></a>Správa profilu Azure Traffic Manageru

Profily Traffic Manageru využívají metody směrování provozu k řízení provozu do vašich cloudových služeb nebo na koncové body webů. Tento článek vysvětluje, jak tyto profily vytvořit a spravovat.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Vytvoření profilu Traffic Manageru pomocí funkce Rychle vytvořit

Profil Traffic Manageru můžete rychle vytvořit pomocí funkce Rychle vytvořit na portálu Azure Classic. Prostřednictvím funkce Rychle vytvořit můžete vytvářet profily se základními nastaveními konfigurace. Funkci Rychle vytvořit však nelze použít pro nastavení, jako je sada koncových bodů (pro služby Cloud Services a weby), pořadí převzetí služeb při selhání pro metodu směrování provozu s převzetím služeb při selhání nebo nastavení monitorování. Po vytvoření profilu, můžete tato nastavení konfigurovat na portálu Azure Classic. Traffic Manager podporuje až 200 koncových bodů na jeden profil. Většina scénářů použití však vyžaduje jen několik koncových bodů.

### <a name="to-create-a-traffic-manager-profile"></a>Vytvoření profilu Traffic Manageru

1. **Nasaďte služby Cloud Services a weby do produkčního prostředí.** Další informace o službách Cloud Services najdete v tématu [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074). Další informace o webech najdete v tématu [Weby](http://go.microsoft.com/fwlink/p/?LinkId=393327).
2. **Přihlaste se na portál Azure Classic.** Klikněte na **Nový** v levém dolním rohu portálu, na **Síťové služby > Traffic Manager** a pak na **Rychle vytvořit** a začněte profil konfigurovat.
3. **Nakonfigurujte předponu DNS.** Zadejte pro profil Traffic Manageru jedinečný název předpony DNS. Pro název domény Traffic Manageru můžete určit pouze předponu.
4. **Vyberte předplatné.** Vyberte příslušné předplatné Azure. Každý profil je přidružen k jednomu předplatnému. Pokud máte pouze jedno předplatné, tato možnost se nezobrazí.
5. **Vyberte metodu směrování provozu.** V části **Zásady směrování provozu** vyberte metodu směrování provozu. Další informace o metodách směrování provozu najdete v tématu [Informace o metodách směrování provozu Traffic Manageru](traffic-manager-routing-methods.md).
6. **Kliknutím na Vytvořit vytvořte profil.** Po dokončení konfigurace profilu můžete příslušný profil vyhledat v podokně Traffic Manageru na portálu Azure Classic.
7. **Na portálu Azure Classic nakonfigurujte koncové body, monitorování a další nastavení.** Funkce pro rychlé vytvoření nakonfiguruje jen základní nastavení. Je nutné nakonfigurovat další nastavení, například seznam koncových bodů a pořadí přebírání služeb koncových bodů při selhání.

## <a name="disable-enable-or-delete-a-profile"></a>Zakázání, povolení nebo odstranění profilu

Existující profil můžete zakázat, aby Traffic Manager neodkazoval požadavky uživatelů na nakonfigurované koncové body. Pokud profil Traffic Manageru zakážete, zůstane zachován beze změny včetně informací, které obsahuje, a je možné ho upravovat v rámci rozhraní Traffic Manageru.  Odkazování se obnoví, až profil znovu povolíte. Když vytvoříte profil Traffic Manageru na portálu Azure Classic, je automaticky povolen. Pokud se rozhodnete, že profil již nebude potřebný, můžete ho odstranit.

### <a name="to-disable-a-profile"></a>Zakázání profilu

1. Pokud používáte vlastní název domény, změňte si na internetovém serveru DNS záznam CNAME tak, aby už neodkazoval na váš profil Traffic Manageru.
2. Nasměrování provozu do koncových bodů prostřednictvím nastavení profilu Traffic Manageru se zastaví.
3. Vyberte profil, který chcete zakázat. Na stránce Traffic Manageru zvýrazněte profil kliknutím do sloupce vedle jeho názvu. Nezapomeňte, že kliknutím na jeho název nebo na šipku u něh byste otevřeli stránku nastavení pro příslušný profil.
4. Po výběru profilu klikněte na **Zakázat** v dolní části stránky.

### <a name="to-enable-a-profile"></a>Povolení profilu

1. Vyberte profil, který chcete zakázat. Na stránce Traffic Manageru zvýrazněte profil kliknutím do sloupce vedle jeho názvu. Nezapomeňte, že kliknutím na jeho název nebo na šipku u něh byste otevřeli stránku nastavení pro příslušný profil.
2. Po výběru profilu klikněte na **Povolit** v dolní části stránky.
3. Pokud používáte vlastní název domény, vytvořte si na internetovém serveru DNS záznam prostředku DNS, který bude odkazovat na název domény ve vašem profilu Traffic Manageru.
4. Provoz se směruje do příslušných koncových bodů.

### <a name="to-delete-a-profile"></a>Odstranění profilu

1. Ověřte, že záznam prostředku DNS na vašem serveru DNS pro internet již nepoužívá záznam prostředku CNAME, který odkazuje na název domény pro váš profil Traffic Manageru.
2. Vyberte profil, který chcete zakázat. Na stránce Traffic Manageru zvýrazněte profil kliknutím do sloupce vedle jeho názvu. Nezapomeňte, že kliknutím na jeho název nebo na šipku u něh byste otevřeli stránku nastavení pro příslušný profil.
3. Po výběru profilu klikněte na **Odstranit** v dolní části stránky.

## <a name="view-traffic-manager-profile-change-history"></a>Zobrazení historie změn profilu Traffic Manageru

Můžete zobrazit historii změn pro svůj profil Traffic Manageru na portálu Azure Classic v části Služby správy.

### <a name="to-view-your-traffic-manager-change-history"></a>Zobrazení historie změn v Traffic Manageru

1. V levém podokně portálu Azure Classic klikněte na **Služby správy**.
2. Na stránce Služby správy klikněte na **Protokoly operací**.
3. Na stránce Protokoly operací můžete filtrovat údaje a zobrazit historii změn pro svůj profil Traffic Manageru. Po výběru možností filtrování klikněte na symbol zaškrtnutí, aby se zobrazily výsledky.

   * Chcete-li zobrazit změny všech svých profilů, vyberte své předplatné a časový rozsah a pak **Traffic Manager** z místní nabídky **Typ**.
   * Chcete-li filtrovat údaje podle názvu profilu, zadejte název profilu do pole **Název služby** pole nebo ho vyberte v místní nabídce.
   * Chcete-li zobrazit podrobnosti o každé jednotlivé změně, vyberte řádek se změnou, kterou chcete zobrazit, a pak klikněte na **Podrobnosti** v dolní části stránky. V okně **Detaily operace** můžete zobrazit reprezentaci XML pro objekt rozhraní API, který byl vytvořen nebo aktualizován v rámci operace.

## <a name="next-steps"></a>Další kroky

* [Přidání koncového bodu](traffic-manager-endpoints.md)
* [Konfigurace metody směrování s převzetím služeb při selhání](traffic-manager-configure-failover-routing-method.md)
* [Konfigurace metody směrování s kruhovým dotazováním](traffic-manager-configure-round-robin-routing-method.md)
* [Konfigurace metody směrování podle výkonu](traffic-manager-configure-performance-routing-method.md)
* [Nasměrování internetové domény společnosti na název domény Traffic Manageru](traffic-manager-point-internet-domain.md)
* [Řešení potíží při sníženém výkonu Traffic Manageru](traffic-manager-troubleshooting-degraded.md)



<!--HONumber=Nov16_HO5-->


