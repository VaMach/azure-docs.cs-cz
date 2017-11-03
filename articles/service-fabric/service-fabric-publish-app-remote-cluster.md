---
title: "Publikování aplikace do vzdáleného clusteru pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak publikovat aplikaci, do clusteru s podporou vzdálené služby prostředků infrastruktury pomocí sady Visual Studio."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: faecd892-eb54-4d9c-8023-c67442afb8e8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/29/2016
ms.author: cawa
ms.openlocfilehash: c440c520d84fc503ff9e705555449e92555d4721
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-and-remove-applications-using-visual-studio"></a>Nasazení a odebírat aplikace pomocí sady Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [Rozhraní API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Azure Service Fabric rozšíření pro Visual Studio poskytuje snadný, opakovatelné a Skriptovatelná způsob k publikování aplikace do clusteru Service Fabric.

## <a name="the-artifacts-required-for-publishing"></a>Artefakty potřebné pro publikování
### <a name="deploy-fabricapplicationps1"></a>Nasazení FabricApplication.ps1
Toto je skript prostředí PowerShell, který používá cestu k profilu publikování jako parametr pro publikování aplikací Service Fabric. Vzhledem k tomu, že tento skript je součástí aplikace, jste Vítejte upravit podle potřeby pro vaši aplikaci.

### <a name="publish-profiles"></a>Publikační profily
Složka v projektu aplikace Service Fabric název **PublishProfiles** obsahuje soubory XML, které obsahují informace nezbytné pro publikování aplikace, jako například:

* Parametry připojení clusteru Service Fabric
* Cesta k souboru aplikace parametr
* Nastavení upgradu

Ve výchozím nastavení, bude vaše aplikace obsahovat tři publikační profily: Local.1Node.xml, Local.5Node.xml a Cloud.xml. Můžete přidat další profily pomocí kopírování a vkládání mezi výchozí soubory.

### <a name="application-parameter-files"></a>Soubory parametrů aplikace
Složka v projektu aplikace Service Fabric název **ApplicationParameters** obsahuje soubory XML hodnoty parametrů manifestu aplikace definované uživatelem. Soubory manifestu aplikace lze nastavit parametry, takže můžete použít jiné hodnoty pro nastavení nasazení. Další informace o parametrizaci aplikace naleznete v tématu [spravovat prostředí s více v Service Fabric](service-fabric-manage-multiple-environment-app-configuration.md).

> [!NOTE]
> Pro služby objektu actor by měl sestavení projektu nejprve před pokusu o úpravu souboru v editoru nebo prostřednictvím dialogovém okně Publikovat. To je proto součástí souborů manifestu se budou generovat během sestavení.

## <a name="to-publish-an-application-using-the-publish-service-fabric-application-dialog-box"></a>K publikování aplikace pomocí dialogového okna publikovat aplikace Service Fabric
Následující kroky ukazují, jak publikovat aplikace pomocí **publikovat aplikace Service Fabric** poskytované služby prostředků infrastruktury nástroje sady Visual Studio dialogové okno.

1. V místní nabídce projektu aplikace Service Fabric, zvolte **publikování...** Chcete-li zobrazit **publikovat aplikace Service Fabric** dialogové okno.
   
    ![** Publikování Service Fabric aplikace ** dialogové okno][0]
   
    Souboru vybraného v **cíle profil** rozevíracím seznamu je tam, kde všechna nastavení, s výjimkou **Manifest verze**, se uloží. Můžete znovu použít stávající profil nebo vytvořit novou výběrem **< spravovat profily... >** v **cíle profil** rozevíracím seznamu. Pokud vyberete profil publikování, její obsah se zobrazí v odpovídající polích dialogového okna. Pokud chcete uložit provedené změny kdykoli, zvolte **uložit profil** odkaz.    
2. V **koncového bodu připojení** , určete místního nebo vzdáleného clusteru Service Fabric na publikování koncový bod. Chcete-li přidat nebo změnit koncového bodu připojení, klikněte na **koncového bodu připojení** rozevíracího seznamu. V seznamu jsou uvedeny dostupné cluster Service Fabric koncové body připojení na které můžete publikovat podle vašich předplatných Azure. Všimněte si, že pokud už nejste přihlášeni sadě Visual Studio, vás vyzve k tomu.
   
    Vybrat ze sady dostupných předplatných a clusterů použijte dialogové okno Výběr clusteru.
   
    ![** Vyberte Service Fabric clusteru ** dialogové okno][1]
   
   > [!NOTE]
   > Pokud chcete publikovat na libovolný koncový bod (například strany clusteru), najdete v článku **publikování pro koncový bod libovolný clusteru** části níže.
   > 
   > 
   
    Jakmile vyberete koncový bod, Visual Studio ověří připojení k vybraný cluster Service Fabric. Pokud cluster není zabezpečený, Visual Studio k nim mohla připojit okamžitě. Ale pokud clusteru je bezpečné, musíte nainstalovat certifikát v místním počítači, než budete pokračovat. V tématu [jak nakonfigurovat zabezpečené připojení](service-fabric-visualstudio-configure-secure-connections.md) Další informace. Až budete hotoví, vyberte **OK** tlačítko. Vybraný cluster se zobrazí v **publikovat aplikace Service Fabric** dialogové okno.
3. V **soubor parametrů aplikace** rozevíracího seznamu pole, přejděte na soubor parametr aplikace. Parametr souboru aplikace obsahuje zadán uživatel hodnoty pro parametry v souboru manifestu aplikace. Chcete-li přidat nebo změnit parametr, zvolte **upravit** tlačítko. Zadejte nebo změňte hodnotu parametru v **parametry** mřížky. Až budete hotoví, vyberte **Uložit** tlačítko.
   
    ![** Upravit parametry ** dialogové okno][2]
4. Použití **upgradu aplikace** zaškrtávacího políčka určete, zda tuto publikování akce je upgrade. Upgrade publikování akce se liší od normální publikování akce. V tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md) seznam rozdíly. Chcete-li nakonfigurovat nastavení upgradu, zvolte **nakonfigurovat nastavení upgradu** odkaz. Zobrazí se editor parametr upgradu. V tématu [konfigurace upgradu aplikace Service Fabric](service-fabric-visualstudio-configure-upgrade.md) Další informace o upgradu parametry.
5. Vyberte **verze manifestu...** tlačítko Zobrazit **upravit verze** dialogové okno. Je potřeba aktualizovat aplikace a služby verze pro upgrade provést. V tématu [kurz upgradu aplikace Service Fabric](service-fabric-application-upgrade-tutorial.md) se dozvíte, jak aplikace a verze manifestu služby vliv upgradu.
   
    ![** Upravit verze ** dialogové okno][3]
   
    Pokud aplikace a verze aktualizace service použít sémantické verze jako je například 1.0.0 nebo číselné hodnoty ve formátu 1.0.0.0, vyberte **automatické aktualizaci aplikace a verze aktualizace service** možnost. Když zvolíte tuto možnost, služby a čísel verzí aplikace se automaticky aktualizují vždy, když verze balíčku kódu, konfigurace nebo data se aktualizuje. Pokud chcete upravit verze ručně, zrušte zaškrtnutí tohoto políčka tuto funkci zakázat.
   
   > [!NOTE]
   > Pro všechny položky balíčku pro projekt objektu actor zobrazení nejprve sestavení projektu pro generování položky v souborech Service Manifest.
   > 
   > 
6. Po dokončení zadání všechna potřebná nastavení, vyberte **publikovat** tlačítko pro publikování aplikace pro vybraný cluster Service Fabric. Nastavení, která jste zadali, se použijí pro proces publikování.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Publikovat do libovolné clusteru koncového bodu (včetně clusterů strany)
Publikování prostředí sady Visual Studio je optimalizovaná pro publikování vzdálené clustery přiřazený k některé z vašich předplatných Azure. Nicméně je možné publikovat na libovolný koncových bodů (například clusterů Service Fabric strany) přímou úpravou profil publikování XML. Jak je popsáno výše, tři publikační profily jsou dostupné ve výchozím nastavení--**Local.1Node.xml**, **Local.5Node.xml**, a **Cloud.xml**– ale Vítejte vytvořit Další profily pro různá prostředí. Například můžete chtít vytvořit profil pro publikování do clusterů strany, případně s názvem **Party.xml**.

Pokud se připojujete k zabezpečená clusteru, všechny, které je nutné koncového bodu připojení clusteru, je třeba `partycluster1.eastus.cloudapp.azure.com:19000`. V takovém případě koncového bodu připojení v profilu publikování by vypadat přibližně takto:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Pokud se připojujete k zabezpečeným clusteru, bude také muset zadejte podrobnosti o klientský certifikát z místního úložiště, který se má použít pro ověřování. Další podrobnosti najdete v tématu [konfigurace zabezpečení připojení ke clusteru Service Fabric](service-fabric-visualstudio-configure-secure-connections.md).

  Až nastavíte svůj profil publikování, můžete odkazovat v dialogovém okně Publikovat jak je uvedeno níže.

  ![Nový profil publikování v dialogové okno publikování][4]

  Všimněte si, že v tomto případě se nový profil publikování odkazuje na jeden ze souborů parametr výchozí aplikace. To je vhodné, pokud chcete publikovat stejnou konfiguraci aplikace na počet prostředí. Naopak v případech, ve které chcete mít různé konfigurace pro každé prostředí, který chcete publikovat, by mít smysl vytvořit odpovídající soubor parametr aplikace.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak automatizovat proces publikování v prostředí průběžnou integraci, najdete v tématu [nastavte průběžnou integraci Service Fabric](service-fabric-set-up-continuous-integration.md).

[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
