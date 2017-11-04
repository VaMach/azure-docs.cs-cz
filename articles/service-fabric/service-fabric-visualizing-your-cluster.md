---
title: "Vizualizace vašeho clusteru pomocí Service Fabric Explorer | Microsoft Docs"
description: "Service Fabric Explorer je webový nástroj pro kontrolu a správa cloudových aplikací a uzly v clusteru s podporou Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: ryanwi
ms.openlocfilehash: 965ffc0f8cec26cccbe6e6459731afc234111f4d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Vizualizujte cluster pomocí Service Fabric Exploreru
Service Fabric Explorer je webový nástroj pro kontrolu a Správa aplikací a uzly v clusteru služby Azure Service Fabric. Service Fabric Explorer je hostován přímo v rámci clusteru, tak, aby byl vždy k dispozici, bez ohledu na to, kde běží vaše clusteru.

## <a name="video-tutorial"></a>Videokurz

Informace o použití Service Fabric Explorer najdete v následujícím videu Microsoft Virtual Academy:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a>Připojení k Service Fabric Exploreru
Pokud jste postupovali podle pokynů [Příprava vývojového prostředí](service-fabric-get-started.md), Service Fabric Explorer můžete spustit v místním clusteru přechodem na http://localhost: 19080/Explorer.

## <a name="understand-the-service-fabric-explorer-layout"></a>Pochopení rozložení Service Fabric Exploreru
Pomocí stromové struktury nalevo můžete přejít pomocí Service Fabric Exploreru. Řídicí panel clusteru v kořenu stromu, obsahuje přehled clusteru, včetně shrnutí stavu uzlu a aplikace.

![Řídicí panel clusteru Service Fabric Exploreru][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Zobrazit rozložení clusteru
Uzly v clusteru Service Fabric se umístí napříč dvourozměrná mřížku domén selhání a upgradu domény. Toto umístění zaručuje, že vaše aplikace zachovány k dispozici v případě selhání hardwaru a upgrady aplikací. Můžete zobrazit, jak aktuální cluster rozložená pomocí mapy clusteru.

![Mapa clusteru Service Fabric Exploreru][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Zobrazení aplikace a služby
Cluster obsahuje dva podstromy: jeden pro aplikace a druhý pro uzly.

Zobrazení aplikací můžete procházet logické hierarchie Service Fabric: aplikace, služby, oddíly a repliky.

V příkladu níže aplikace **Moje aplikace** se skládá ze dvou služeb **MyStatefulService** a **WebService**. Vzhledem k tomu **MyStatefulService** je stavová, obsahuje oddíl s jeden primární a dva sekundární repliky. Naopak WebSvcService je bezstavové a obsahuje jednu instanci.

![Zobrazení aplikace Service Fabric Exploreru][sfx-application-tree]

Na každé úrovni stromu hlavním podokně zobrazí příslušné informace o položce. Například se zobrazí stav a verze pro konkrétní službu.

![Podokno essentials Service Fabric Exploreru][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Zobrazení uzlů clusteru
Zobrazení uzlu obsahuje fyzické rozložení clusteru. Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód. Přesněji řečeno uvidíte, které repliky jsou aktuálně spuštěné existuje.

## <a name="actions"></a>Akce
Service Fabric Explorer nabízí rychlý způsob, jak vyvolání akce na uzly, aplikace a služby v rámci clusteru.

Chcete-li odstranit instanci aplikace, například ze stromu na levé straně zvolte aplikaci a poté zvolte **akce** > **odstranit aplikaci**.

![Odstranění aplikace v Service Fabric Exploreru][sfx-delete-application]

> [!TIP]
> Kliknutím na tlačítko se třemi tečkami vedle jednotlivých prvků můžete provádět stejné akce.
>
>

Následující tabulka uvádí akce, které jsou dostupné u jednotlivých entit:

| **Entity** | **Akce** | **Popis** |
| --- | --- | --- |
| Typ aplikace |Zrušit zřízení typu |Odebere balíček aplikace z úložiště bitových kopií clusteru. Vyžaduje nejdřív odstranit všechny aplikacím daného typu. |
| Aplikace |Odstranění aplikace |Odstraňte aplikaci, včetně všech jejích služeb a jejich stavu (pokud existuje). |
| Služba |Odstranění služby |Odstraňte službu a její stav (pokud existuje). |
| Node |Aktivovat |Aktivujte uzlu. |
| Node | Deaktivovat (Pozastavit) | Pozastavení uzlu v jejím aktuálním stavu. Služby pokračovat ke spuštění, ale Service Fabric nepřesouvá proaktivně nic na nebo vypněte ho Pokud je třeba, aby se zabránilo nekonzistenci výpadku nebo data. Tato akce se obvykle používá k povolení ladění služeb v konkrétním uzlu zajistit, že není přesunout během kontroly. | |
| Node | Deaktivovat (restartovat) | Bezpečně přesunete všechny služby v paměti vypnout uzlu a trvalé services zavřete. Obvykle nepoužívá, pokud hostitelské procesy nebo počítač musí restartovat. | |
| Node | Deaktivovat (odebrat data) | Bezpečně zavřete všechny služby spuštěné na uzlu po sestavení dostatečná k výměně za chodu repliky. Typicky používá při uzlu (nebo aspoň jeho úložiště) jsou mimo Komise trvale přijata. | |
| Node | Odeberte stav uzlu | Odebrání znalosti repliky uzlu z clusteru. Obvykle používá, když už selhání uzlu se považuje neopravitelné. | |
| Node | Restartování | Simulace selhání uzlu restartováním uzlu. Další informace [sem](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps) | |

Vzhledem k tomu, že mnoho akce jsou destruktivní, můžete být vyzváni k potvrzení vašich představ, před dokončením akce.

> [!TIP]
> Každou akci, která lze provádět pomocí Service Fabric Explorer můžete provést i pomocí prostředí PowerShell nebo rozhraní REST API, jak povolit automatizaci.
>
>

Můžete také Service Fabric Explorer pro vytvoření instancí aplikace daný typ a verze aplikace. Vyberte typ aplikace, v zobrazení stromu a pak klikněte na **vytvořit instanci aplikace** odkaz vedle verze byste chtěli v pravém podokně.

![Vytvoření instance aplikace v Service Fabric Exploreru][sfx-create-app-instance]

> [!NOTE]
> Instance aplikace vytvořené pomocí Service Fabric Explorer nemůže být aktuálně parametry. Jsou vytvořeny pomocí výchozí hodnoty parametrů.
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a>Připojení ke vzdálené clusteru Service Fabric
Pokud znáte koncový bod clusteru a že máte dostatečná oprávnění k Service Fabric Explorer máte přístup z libovolného prohlížeče. Je to proto, že je právě jiné službě, která běží v clusteru Service Fabric Exploreru.

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Zjistit koncový bod Service Fabric Explorer pro vzdálený cluster
K dosažení pro daný cluster Service Fabric Exploreru, přejděte v prohlížeči na:

http://&lt;vašeho clusteru endpoint&gt;: 19080/Explorer

Azure v rámci clusterů úplnou adresu URL je také k dispozici v podokně clusteru essentials na portálu Azure.

### <a name="connect-to-a-secure-cluster"></a>Připojení k zabezpečenému clusteru
Ke svému clusteru Service Fabric pomocí certifikátů nebo pomocí Azure Active Directory (AAD) můžete řídit přístup klienta.

Pokud se pokusíte připojit k Service Fabric Explorer na clusteru s podporou zabezpečení, pak v závislosti na konfiguraci clusteru budete se budete muset certifikát klienta k dispozici nebo se přihlaste pomocí AAD.

## <a name="next-steps"></a>Další kroky
* [Přehled testovatelnosti](service-fabric-testability-overview.md)
* [Správu aplikací Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Nasazení aplikace Service Fabric pomocí prostředí PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
