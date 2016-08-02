<properties
   pageTitle="Správa profilů Azure Traffic Manageru | Microsoft Azure"
   description="Tento článek vám pomůže vytvořit, zakázat, povolit, odstranit nebo zobrazit historii profilu Azure Traffic Manageru."
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="joaoma" />

# Správa profilu Azure Traffic Manageru

Prostřednictvím profilu Traffic Manageru můžete určovat koncové body služeb Cloud Services nebo webů, které budou monitorovány Traffic Managerem, a metodu směrování provozu, kterou chcete používat pro distribuci připojení do těchto koncových bodů.

## Vytvoření profilu Traffic Manageru pomocí funkce Rychle vytvořit

Profil Traffic Manageru můžete rychle vytvořit pomocí funkce Rychle vytvořit na portálu Azure Classic. Prostřednictvím funkce Rychle vytvořit můžete vytvářet profily se základními nastaveními konfigurace. Funkci Rychle vytvořit však nelze použít pro nastavení, jako je sada koncových bodů (pro služby Cloud Services a weby), pořadí převzetí služeb při selhání pro metodu směrování provozu s převzetím služeb při selhání nebo nastavení monitorování. Po vytvoření profilu, můžete tato nastavení konfigurovat na portálu Azure Classic. Traffic Manager podporuje až 200 koncových bodů na jeden profil. Většina scénářů použití však vyžaduje pouze malý počet koncových bodů. 

### Vytvoření nového profilu Traffic Manageru

1. **Nasaďte služby Cloud Services a weby do produkčního prostředí.** Další informace o službách Cloud Services najdete v tématu [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074). Informace o službách Cloud Services najdete v tématu [Osvědčené postupy](https://msdn.microsoft.com/library/azure/5229dd1c-5a91-4869-8522-bed8597d9cf5#bkmk_TrafficManagerBestPracticesProfile). Další informace o webech najdete v tématu [Weby](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Přihlaste se k portálu Azure Classic** Chcete-li vytvořit nový profil Traffic Manageru, klikněte na **Nový** v levém dolním rohu portálu, klikněte na **Síťové služby > Traffic Manager** a pak klikněte na **Rychle vytvořit** a začněte profil konfigurovat.
3. **Nakonfigurujte předponu DNS.** Zadejte pro profil Traffic Manageru jedinečný název předpony DNS. Pro název domény Traffic Manageru můžete určit pouze předponu.
4. **Vyberte předplatné.** Vyberte příslušné předplatné Azure. Každý profil je přidružen k jednomu předplatnému. Pokud máte pouze jedno předplatné, tato možnost se nezobrazí.
5. **Vyberte metodu směrování provozu.** V části **Zásady směrování provozu** vyberte metodu směrování provozu. Další informace o metodách směrování provozu najdete v tématu [Informace o metodách směrování provozu Traffic Manageru](traffic-manager-routing-methods.md).
6. **Klikněte na Vytvořit a vytvořte nový profil**. Po dokončení konfigurace profilu můžete příslušný profil vyhledat v podokně Traffic Manageru na portálu Azure Classic.
7. **Na portálu Azure Classic nakonfigurujte koncové body, monitorování a další nastavení.** Jelikož prostřednictvím funkce Rychle vytvořit můžete konfigurovat jen základní nastavení, je třeba konfigurovat další nastavení, jako je seznam koncových bodů a pořadí převzetí služeb při selhání koncových bodů, aby bylo možné dokončit požadovanou konfiguraci. 


## Zakázání, povolení nebo odstranění profilu

Existující profil Traffic Manageru můžete zakázat, aby neodkazoval požadavky uživatelů na příslušné nakonfigurované koncové body. Pokud profil Traffic Manageru zakážete, příslušný profil včetně informací, které obsahuje, zůstane zachován beze změny a lze ho upravovat v rámci rozhraní Traffic Manageru. Chcete-li profil znovu povolit, můžete tak snadno učinit na portálu Azure Classic. Odkazování se obnoví. Když vytvoříte profil Traffic Manageru na portálu Azure Classic, je automaticky povolen. Pokud se rozhodnete, že profil již nebude potřebný, můžete ho odstranit.

### Zakázání profilu

1. Upravte záznam prostředku DNS na serveru DNS pro internet tak, aby používal odpovídající typ záznamu a odkaz buď na jiný název, nebo na IP adresu konkrétního umístění v internetu. Jinými slovy: změňte záznam prostředku DNS na serveru DNS pro internet tak, aby již nepoužíval záznam prostředku CNAME, který odkazuje na název domény pro váš profil Traffic Manageru.
2. Nasměrování provozu do koncových bodů prostřednictvím nastavení profilu Traffic Manageru se zastaví.
3. Vyberte profil, který chcete zakázat. Profil můžete vybrat tak, že ho na stránce Traffic Manageru zvýrazníte kliknutím do sloupce vedle názvu profilu. Neklikejte na název profilu ani na šipku u názvu, protože byste přešli na stránku nastavení pro příslušný profil.
4. Po výběru profilu klikněte na **Zakázat** v dolní části stránky.

### Povolení profilu

1. Vyberte profil, který chcete povolit. Profil můžete vybrat tak, že ho na stránce Traffic Manageru zvýrazníte kliknutím do sloupce vedle názvu profilu. Neklikejte na název profilu ani na šipku u názvu, protože byste přešli na stránku nastavení pro příslušný profil.
2. Po výběru profilu klikněte na **Povolit** v dolní části stránky.
3. Upravte záznamu prostředku DNS na serveru DNS pro internet tak, aby používal typ záznamu CNAME, který mapuje název domény vaší společnosti na název domény vašeho profilu Traffic Manageru. Další informace najdete v tématu [Nasměrování internetové domény společnosti na doménu Traffic Manageru](traffic-manager-point-internet-domain.md).
4. Provoz se začne znovu směrovat do příslušných koncových bodů.

### Odstranění profilu

1. Ověřte, že záznam prostředku DNS na vašem serveru DNS pro internet již nepoužívá záznam prostředku CNAME, který odkazuje na název domény pro váš profil Traffic Manageru.
2. Vyberte profil, který chcete odstranit. Profil můžete vybrat tak, že ho na stránce Traffic Manageru zvýrazníte kliknutím do sloupce vedle profilu. Neklikejte na název profilu ani na šipku u názvu, protože byste přešli na stránku nastavení pro příslušný profil.
4. Po výběru profilu klikněte na **Odstranit** v dolní části stránky.

## Zobrazení historie změn profilu Traffic Manageru

Můžete zobrazit historii změn pro svůj profil Traffic Manageru na portálu Azure Classic v části Služby správy.

### Zobrazení historie změn v Traffic Manageru

1. V levém podokně portálu Azure Classic klikněte na **Služby správy**.
2. Na stránce Služby správy klikněte na **Protokoly operací**.
3. Na stránce Protokoly operací můžete filtrovat údaje a zobrazit historii změn pro svůj profil Traffic Manageru. Po výběru možností filtrování klikněte na symbol zaškrtnutí, aby se zobrazily výsledky.
   - Chcete-li zobrazit změny všech svých profilů, vyberte své předplatné a časový rozsah a pak vyberte **Traffic Manager** z místní nabídky **Typ**.
   - Chcete-li filtrovat údaje podle názvu profilu, zadejte název profilu do pole **Název služby** pole nebo ho vyberte v místní nabídce.
   - Chcete-li zobrazit podrobnosti o každé jednotlivé změně, vyberte řádek se změnou, kterou chcete zobrazit, a pak klikněte na **Podrobnosti** v dolní části stránky. V okně **Detaily operace** můžete zobrazit reprezentaci XML pro objekt rozhraní API, který byl vytvořen nebo aktualizován v rámci operace, a zkopírovat příslušný kód XML do schránky.


## Další kroky

[Přidání koncového bodu](traffic-manager-endpoints.md)

[Konfigurace metody směrování s převzetím služeb při selhání](traffic-manager-configure-failover-routing-method.md)

[Konfigurace metody směrování s kruhovým dotazováním](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurace metody směrování podle výkonu](traffic-manager-configure-performance-routing-method.md)

[Řešení potíží při sníženém výkonu Traffic Manageru](traffic-manager-troubleshooting-degraded.md)


<!--HONumber=Jun16_HO2-->


