---
title: "Nasazení aplikace .NET v kontejneru do Azure Service Fabric | Microsoft Docs"
description: "Se naučíte, jak zabalit aplikace .NET v sadě Visual Studio v kontejner Docker. Tuto novou aplikaci \"kontejner\" se pak nasadí do clusteru Service Fabric."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 021c695a91ff46274b2a5174918711d04bcff239
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a>Nasazení aplikace .NET v kontejneru systému Windows do Azure Service Fabric

V tomto kurzu se dozvíte, jak nasadit stávající aplikaci ASP.NET v kontejneru systému Windows v Azure.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření projektu Docker v sadě Visual Studio
> * Containerize stávající aplikaci
> * Instalační program průběžnou integraci s Visual Studio a služby VSTS

## <a name="prerequisites"></a>Požadavky

1. Nainstalujte [CE Docker pro systém Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) tak, aby kontejnery můžete spustit na Windows 10.
2. Seznamte se s [rychlý start pro Windows 10 kontejnery][link-container-quickstart].
3. Stažení [Fabrikam Fiber CallCenter] [ link-fabrikam-github] ukázkové aplikace.
4. Nainstalujte [prostředí Azure PowerShell][link-azure-powershell-install]
5. Nainstalujte [rozšíření průběžné doručování nástrojů pro Visual Studio 2017][link-visualstudio-cd-extension]
6. Vytvoření [předplatného Azure] [ link-azure-subscription] a [účet Visual Studio Team Services][link-vsts-account]. 
7. [Vytvoření clusteru s podporou v Azure](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-the-application"></a>Containerize aplikace

Teď, když máte [cluster Service Fabric běží v Azure](service-fabric-tutorial-create-cluster-azure-ps.md) budete chtít vytvořit a nasadit kontejnerizované aplikaci. Spustit aplikaci v kontejneru, je potřeba přidat **Docker podporu** na projekt v sadě Visual Studio. Když přidáte **Docker podporu** do aplikace, dvě věci dojít. První, _soubor Docker_ se přidá do projektu. Tento nový soubor popisuje, jak bitovou kopii kontejneru má být sestaven. Potom druhé, nový _docker compose_ projekt je přidán do řešení. Nový projekt obsahuje několik docker compose soubory. Docker compose soubory můžete použít k popisu, jak spouštět kontejneru.

Další informace o práci s [kontejner nástroje sady Visual Studio][link-visualstudio-container-tools].

>[!NOTE]
>Pokud je prvním používáte bitových kopií kontejneru systému Windows v počítači, musí Docker CE stahují základní Image pro kontejnerů. Obrázků použitých v tomto kurzu jsou 14 GB. Pokračujte a spusťte následující příkaz terminálu vyžádat základní bitové kopie:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Přidání podpory Docker

Otevřete [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] souborů v sadě Visual Studio.

Klikněte pravým tlačítkem myši **FabrikamFiber.Web** Projekt > **přidat** > **Docker podporu**.

### <a name="add-support-for-sql"></a>Přidání podpory pro SQL

Tato aplikace používá SQL jako poskytovatel dat, SQL server je vyžadován ke spuštění aplikace. Referenční bitovou kopii systému SQL Server kontejneru v našem docker compose.override.yml souboru.

V sadě Visual Studio otevřete **Průzkumníku řešení**, Najít **docker compose**a otevřete soubor **docker compose.override.yml**.

Přejděte na `services:` uzlu přidat uzel s názvem `db:` položce SQL Server pro kontejner, který definuje.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>Jakýkoli Server SQL dáváte přednost pro místní ladění, můžete použít, dokud je dostupný z vašeho hostitele. Ale **localdb** nepodporuje `container -> host` komunikace.

>[!WARNING]
>Zachování dat není podporován systémem SQL Server v kontejneru. Když se zastaví kontejner, data budou vymazána. Nepoužívejte tuto konfiguraci pro produkční prostředí.

Přejděte na `fabrikamfiber.web:` uzel a přidat podřízený uzel s názvem `depends_on:`. To zajistí, že `db` (kontejneru systému SQL Server) bude spuštěn před naše webové aplikace (fabrikamfiber.web).

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a>Aktualizace webové konfigurace

Zpět v **FabrikamFiber.Web** projektu, aktualizovat připojovací řetězec **web.config** souboru, do bodu k systému SQL Server v kontejneru.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Pokud chcete použít jinou konzolu systému SQL Server při vytváření verze sestavení webové aplikace, přidejte další připojovací řetězec do souboru web.release.config.

### <a name="test-your-container"></a>Vaše kontejner testů

Stiskněte klávesu **F5** ke spuštění a ladění aplikace v vašeho kontejneru.

Hraniční otevře se stránka definované spuštění vaší aplikace pomocí IP adresy kontejneru v interní síti NAT (obvykle 172.x.x.x). Další informace o ladění aplikací v kontejnerech pomocí Visual Studio 2017 najdete v tématu [v tomto článku][link-debug-container].

![třeba společnost fabrikam v kontejneru][image-web-preview]

Kontejner je nyní připraven k vytvořené a zabalené aplikace Service Fabric. Až budete mít bitovou kopii kontejneru založený na váš počítač, můžete poslat ho přímo žádné registru kontejneru a načítat na každém hostiteli spustit.

## <a name="get-the-application-ready-for-the-cloud"></a>Příprava aplikace pro cloud

Příprava aplikace pro spuštění v Service Fabric v Azure, je potřeba provést dva kroky:

1. Vystavení portu, kde Chceme mít možnost připojení naše webové aplikace v clusteru Service Fabric.
2. Zadejte databázi SQL připravené pro produkční pro naši aplikaci.

### <a name="expose-the-port-for-the-app"></a>Vystavení port pro aplikace
Cluster Service Fabric jsme nakonfigurovali, má port *80* otevřete ve výchozím nastavení nástroje pro vyrovnávání zatížení Azure, který vyrovnává příchozí provoz do clusteru. Naše kontejneru na port jsme můžou zpřístupnit prostřednictvím našich soubor docker-compose.yml.

V sadě Visual Studio otevřete **Průzkumníku řešení**, Najít **docker compose**a otevřete soubor **docker-compose.yml**.

Změnit `fabrikamfiber.web:` uzlu přidat podřízený uzel s názvem `ports:`.

Přidat položku řetězec `- "80:80"`. Toto je, jak by měla vypadat váš soubor docker-compose.yml:

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Použít provozní databáze SQL
Při spuštění v produkčním prostředí, potřebujeme naše data v našem databáze. Aktuálně neexistuje žádný způsob, jak zajistit trvalou data v kontejneru, proto nelze uložit provozních dat v systému SQL Server v kontejneru.

Doporučujeme, abyste že využívat Azure SQL Database. Chcete-li nastavit a spustit na spravovaný Server SQL v Azure, přejděte [Azure SQL Database – elementy QuickStart] [ link-azure-sql] článku.

>[!NOTE]
>Mějte na paměti, chcete-li změnit připojovací řetězce k systému SQL server v **web.release.config** v soubor **FabrikamFiber.Web** projektu.
>
>Tato aplikace se elegantně selže, pokud žádné databáze SQL je dostupný. Můžete pokračovat a nasadit aplikace s žádné systému SQL server.

## <a name="deploy-with-visual-studio-team-services"></a>Nasazení s Visual Studio Team Services

Chcete-li nastavit nasazení pomocí sady Visual Studio Team Services, je potřeba nainstalovat [rozšíření průběžné doručování nástrojů pro Visual Studio 2017][link-visualstudio-cd-extension]. Toto rozšíření umožňuje snadno nasadit do Azure tak, že nakonfigurujete Visual Studio Team Services a získat aplikace nasazené do clusteru Service Fabric.

Abyste mohli začít, musí být váš kód hostované ve správě zdrojového kódu. Zbývající část tohoto oddílu předpokládá **git** je používán.

### <a name="set-up-a-vsts-repo"></a>Nastavení služby VSTS úložišti
V pravém dolním rohu sady Visual Studio, klikněte na položku **přidat do správy zdrojového kódu** > **Git** (nebo obě tyto možnosti dáváte přednost).

![Klikněte na tlačítko Zdroj ovládacího prvku][image-source-control]

V _Team Explorer_ podokně, stiskněte klávesu **publikování úložiště Git**.

Vyberte název služby VSTS úložiště a stiskněte klávesu **úložiště**.

![publikování do služby VSTS úložišti][image-publish-repo]

Teď, když kód synchronizována s zdrojové úložiště služby VSTS, můžete nakonfigurovat průběžnou integraci a nastavené průběžné doručování.

### <a name="setup-continuous-delivery"></a>Instalační program nastavené průběžné doručování

V _Průzkumníku řešení_, klikněte pravým tlačítkem myši **řešení** > **konfigurace nastavené průběžné doručování**.

Vyberte předplatné Azure.

Nastavit **hostitele typ** k **služby Fabric Cluster**.

Nastavit **cílového hostitele** na cluster service fabric jste vytvořili v předchozí části.

Vyberte **kontejneru registru** publikovat kontejner tak, aby.

>[!TIP]
>Použití **upravit** tlačítko vytvořte kontejner registru.

Stiskněte **OK**.

![instalace služby fabric průběžnou integraci][image-setup-ci]
   
   Po dokončení konfigurace vašeho kontejneru se nasadí do Service Fabric. Vždy, když jste nabízené aktualizace do úložiště nového sestavení a verze je provedena.
   
   >[!NOTE]
   >Vytváření proveďte bitové kopie kontejneru přibližně 15 minut.
   >První nasazení tak, aby cluster Service Fabric způsobí, že základní Image kontejneru jádro systému Windows Server ke stažení. Stahování bude dokončeno další 5 až 10 minut.

Přejděte do aplikace společnosti Fabrikam Call Center pomocí adresy url clusteru: například *http://mycluster.westeurope.cloudapp.azure.com*

Teď, když máte kontejnerizované a nasadit řešení Fabrikam Call Center, můžete otevřít [portál Azure] [ link-azure-portal] a aplikace běžící v Service Fabric. Pokud chcete vyzkoušet aplikace, otevřete webový prohlížeč a přejděte na adresu URL clusteru Service Fabric.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření projektu Docker v sadě Visual Studio
> * Containerize stávající aplikaci
> * Instalační program průběžnou integraci s Visual Studio a služby VSTS

V další části kurzu, zjistěte, jak nastavit [monitorování pro váš kontejner](service-fabric-tutorial-monitoring-wincontainers.md).

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance to the next tutorial to learn how to bind a custom SSL certificate to it.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
