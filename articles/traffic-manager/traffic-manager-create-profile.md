---
title: "Vytvořte profil Traffic Manageru v Azure | Microsoft Docs"
description: "Tento článek popisuje postup vytvoření profilu Traffic Manageru"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: kumud
ms.openlocfilehash: e9ff7947e7801a9f352a7a947b09893b8f615d88
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Traffic Manageru

Tento článek popisuje, jak profil s **s prioritou** typ směrování je možné vytvořit trasu uživatelům dva koncové body Azure Web Apps. Pomocí **s prioritou** směrování typu, veškerý provoz se směruje na první koncový bod při druhý se ukládají jako záložní. Uživatelé mohou být v důsledku toho směrovány na druhý koncový bod, pokud první koncový bod se změní na není v pořádku.

V tomto článku jsou přidružené k tomuto nově vytvořený profil služby Traffic Manager dva koncové body vytvořené webové aplikace Azure. Další informace o tom, jak vytvořit webovou aplikaci Azure koncové body, najdete [stránce dokumentace Azure Web Apps](https://docs.microsoft.com/azure/app-service/). Můžete přidat libovolný koncový bod, který má název DNS a je dostupný prostřednictvím veřejného Internetu a že jako příklad používáme koncové body Azure Web Apps.

### <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Traffic Manageru
1. V prohlížeči se přihlaste k webu [Azure Portal](http://portal.azure.com). Pokud účet nemáte, můžete pro registraci [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/free/). 
2. V nabídce **Centrum** klikněte na **Nový** >  **Sítě** > **Zobrazit vše** a pak na profil **Traffic Manager**, otevřete okno **Vytvořit profil Traffic** Manageru, kde klikněte na **Vytvořit**.
3. Okno **Vytvořit profil Traffic Manageru** vyplňte následovně:
    1. Do pole **Název** zadejte název profilu. Tento název musí být jedinečný v rámci zóny trafficmanager.net a výsledkem název DNS <name>, trafficmanager.net, který se používá pro přístup k vašeho profilu Traffic Manageru.
    2. Jakou **Metodu směrování** vyberte **Priorita**.
    3. V poli **Předplatné** vyberte předplatné, v rámci kterého chcete profil vytvořit.
    4. V části **Skupina prostředků** vytvořte novou skupinu prostředků, do které chcete profil umístit.
    5. V poli **Umístění skupiny prostředků** vyberte umístění skupiny prostředků. Toto nastavení se týká umístění skupiny prostředků a nemá žádný vliv na profil Traffic Manageru, který se nasadí globálně.
    6. Klikněte na možnost **Vytvořit**.
    7. Po dokončení globálního nasazení profilu Traffic Manageru bude uveden jako jeden z prostředků v příslušné skupině prostředků.

    ![Vytvoření profilu Traffic Manageru](./media/traffic-manager-create-profile/Create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Přidat koncové body Traffic Manager

1. V panelu vyhledávání na portálu, vyhledejte **profil služby Traffic Manager** název, který jste vytvořili v předchozí části a klikněte na profil správce provozu ve výsledcích, zobrazené.
2. V okně **Profil služby Traffic Manager** v části **Nastavení** klikněte na **Koncové body**.
3. V zobrazeném okně **Koncové body** klikněte na **Přidat**.
4. Okno **Přidat koncový bod** vyplňte následujícím způsobem:
    1. V části **Typ** klikněte na **Koncový bod Azure**.
    2. Zadejte **Název**, podle kterého tento koncový bod rozpoznáte.
    3. Pro **cíle typ prostředku**, klikněte na tlačítko **služby App Service**.
    4. Pro **cíle prostředků**, klikněte na tlačítko **vybrat aplikační službu** zobrazíte seznam webových aplikací v rámci stejného předplatného. V **prostředků** okno, které se zobrazí, vyberte, kterou chcete přidat jako první koncový bod služby App service.
    5. V části **Priorita** vyberte **1**. Výsledkem bude, že veškerý provoz bude přicházet do tohoto koncového bodu, pokud bude v pořádku.
    6. Políčko **Přidat jako zakázaný** ponechte nezaškrtnuté.
    7. Klikněte na tlačítko **OK**.
5.  Zopakujte kroky 3 a 4 pro další koncový bod Azure Web Apps. Nezapomeňte ho přidat s hodnotou **Priorita** nastavenou na **2**.
6.  Po přidání se oba koncové body zobrazí v okně **Profil služby Traffic Manager** a jejich stav monitorování bude **Online**.

    ![Přidání koncového bodu Traffic Manager](./media/traffic-manager-create-profile/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Použít profil služby Traffic Manager
1.  V panelu vyhledávání na portálu, vyhledejte **profil služby Traffic Manager** název, který jste vytvořili v předchozí části. V výsledky, které se zobrazí klikněte na profil správce provozu.
2. V **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**.
3. **Profil služby Traffic Manager** zobrazuje název DNS vašeho nově vytvořený profil služby Traffic Manager. Tímto lze všechny klienty (například tak, že přejdete do ní pomocí webového prohlížeče) k získání směruje na pravý koncový bod jako Určuje typ směrování. V takovém případě všechny požadavky jsou směrovány do první koncový bod a Traffic Manager zjistí, je-li být není v pořádku, provoz automaticky přejde na další koncový bod.

## <a name="delete-the-traffic-manager-profile"></a>Odstranit profil služby Traffic Manager
Pokud již nepotřebujete, odstraňte skupinu prostředků a profil služby Traffic Manager, který jste vytvořili. To pokud chcete udělat, vyberte skupinu prostředků z **profil služby Traffic Manager** a klikněte na **odstranit**.

## <a name="next-steps"></a>Další kroky

- Další informace o [směrování typy](traffic-manager-routing-methods.md).
- Další informace o typy koncových bodů [typy koncových bodů](traffic-manager-endpoint-types.md).
- Další informace o [monitorování koncového bodu](traffic-manager-monitoring.md).



