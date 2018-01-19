---
title: "Vytvoření aplikace Azure Service Fabric typu kontejner v Linuxu | Dokumentace Microsoftu"
description: "Vytvoříte svou první aplikaci typu kontejner pro Linux na platformě Azure Service Fabric.  Sestavíte image Dockeru s vaší aplikací, nahrajete image do registru kontejneru a sestavíte a nasadíte aplikaci Service Fabric typu kontejner."
services: service-fabric
documentationcenter: linux
author: suhuruli
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 23cc9ce855eeba9e9a365e42beeee01b09f0fee3
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>Nasazení aplikace Azure Service Fabric typu kontejner pro Linux v Azure
Azure Service Fabric je platforma distribuovaných systémů pro nasazování a správu škálovatelných a spolehlivých mikroslužeb a kontejnerů. 

Tento rychlý start ukazuje, jak nasadit kontejnery Linuxu do clusteru Service Fabric. Jakmile budete hotovi, budete mít hlasovací aplikaci skládající se z webového front-endu v Pythonu a back-endu Redis spuštěného v clusteru Service Fabric. 

![quickstartpic][quickstartpic]

V tomto rychlém startu se naučíte:
> [!div class="checklist"]
> * Nasazení kontejnerů do clusteru Service Fabric s Linuxem v Azure
> * Škálování a převzetí služeb při selhání kontejnerů v Service Fabric

## <a name="prerequisite"></a>Požadavek
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.
  
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, ujistěte se, že používáte Azure CLI verze 2.0.4 nebo novější. Pokud chcete zjistit verzi, spusťte příkaz az --version. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="get-application-package"></a>Získání balíčku aplikace
K nasazení kontejnerů do Service Fabric potřebujete sadu souborů manifestu (definice aplikace), které popisují jednotlivé kontejnery a aplikaci.

Ve službě Cloud Shell naklonujte kopii definice aplikace pomocí příkazu git.

```azurecli-interactive
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/Voting
```

## <a name="deploy-the-containers-to-a-service-fabric-cluster-in-azure"></a>Nasazení kontejnerů do clusteru Service Fabric v Azure
Pokud chcete nasadit aplikaci do clusteru v Azure, použijte vlastní cluster nebo cluster Party.

> [!Note]
> Aplikace se musí nasadit do clusteru v Azure, a ne do clusteru Service Fabric na místním počítači pro vývoj. 
>

Party Clustery jsou bezplatné, časově omezené clustery Service Fabric hostované v Azure. Jsou udržované týmem Service Fabric a kdokoli na nich může nasazovat aplikace a seznamovat se s platformou. Pokud chcete získat přístup k clusteru Party, [postupujte podle těchto pokynů](http://aka.ms/tryservicefabric). 

Informace o vytvoření vlastního clusteru najdete v tématu věnovaném [vytvoření clusteru Service Fabric v Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

> [!Note]
> Webová front-end služba je nakonfigurovaná k naslouchání příchozímu provozu na portu 80. Ujistěte se, že je ve vašem clusteru tento port otevřený. Pokud používáte cluster Party, je tento port otevřený.
>

### <a name="install-service-fabric-command-line-interface-and-connect-to-your-cluster"></a>Install rozhraní příkazového řádku služby Service Fabric a připojení ke clusteru
Nainstalujte ve svém prostředí rozhraní příkazového řádku [Service Fabric CLI (sfctl)](service-fabric-cli.md).

```azurecli-interactive
pip3 install --user sfctl 
export PATH=$PATH:~/.local/bin
```

Připojte se pomocí Azure CLI ke clusteru Service Fabric v Azure. Koncový bod je koncový bod správy vašeho clusteru, například `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.

```azurecli-interactive
sfctl cluster select --endpoint http://linh1x87d1d.westus.cloudapp.azure.com:19080
```

### <a name="deploy-the-service-fabric-application"></a>Nasazení aplikace Service Fabric 
Kontejnerové aplikace Service Fabric je možné nasadit pomocí popsaného balíčku aplikace Service Fabric nebo nástroje Docker Compose. 

#### <a name="deploy-using-service-fabric-application-package"></a>Nasazení pomocí balíčku aplikace Service Fabric
Pomocí instalačního skriptu, poskytnutého ke zkopírování definice hlasovací aplikace do clusteru, zaregistrujte typ aplikace a vytvořte její instanci.

```azurecli-interactive
./install.sh
```

#### <a name="deploy-the-application-using-docker-compose"></a>Nasazení aplikace pomocí Docker Compose
K nasazení a instalaci aplikace v clusteru Service Fabric s využitím nástroje Docker Compose použijte následující příkaz.
```azurecli-interactive
sfctl compose create --deployment-name TestApp --file-path docker-compose.yml
```

Otevřete prohlížeč a přejděte na Service Fabric Explorer na adrese http://\<adresa_url_vašeho_clusteru_service_fabric>:19080/Explorer – například `http://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`. Rozbalte uzel Aplikace a všimněte si, že už obsahuje položku pro typ hlasovací aplikace a instanci, kterou jste vytvořili.

![Service Fabric Explorer][sfx]

Připojte se ke spuštěnému kontejneru.  Otevřete webový prohlížeč a přejděte na adresu URL vašeho clusteru, například `http://linh1x87d1d.westus.cloudapp.azure.com:80`. V prohlížeči by se měla zobrazit hlasovací aplikace.

![quickstartpic][quickstartpic]

## <a name="fail-over-a-container-in-a-cluster"></a>Převzetí služeb při selhání kontejneru v clusteru
Service Fabric zajišťuje v případě selhání automatický přesun instancí kontejneru do jiných uzlů clusteru. Kontejnery můžete z uzlu také ručně vyprázdnit a řádně je přesunout do jiných uzlů v clusteru. Služby můžete škálovat několika způsoby, v tomto příkladu používáme Service Fabric Explorer.

Pokud chcete převzít služby při selhání front-end kontejneru, proveďte následující kroky:

1. Otevřete ve vašem clusteru Service Fabric Explorer – například `http://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`.
2. Ve stromovém zobrazení klikněte na uzel **fabric:/Voting/azurevotefront** a rozbalte uzel oddílu (reprezentovaný identifikátorem GUID). Všimněte si názvu uzlu ve stromovém zobrazení, které zobrazuje uzly, na kterých je kontejner právě spuštěný – například `_nodetype_4`.
3. Ve stromovém zobrazení rozbalte uzel **Uzly**. Klikněte na tři tečky vedle uzlu, na kterém je kontejner spuštěný.
4. Pokud chcete tento uzel restartovat, zvolte **Restartovat** a potvrďte akci restartování. Restartování způsobí převzetí služeb při selhání kontejneru do jiného uzlu v clusteru.

![sfxquickstartshownodetype][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Škálování aplikací a služeb v clusteru
Služby Service Fabric je možné snadno škálovat napříč clusterem a vyřešit tak jejich zatížení. Služby se škálují změnou počtu instancí spuštěných v clusteru.

Pokud chcete škálovat webovou front-end službu, proveďte následující kroky:

1. Otevřete ve vašem clusteru Service Fabric Explorer – například `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Ve stromovém zobrazení klikněte na tři tečky vedle uzlu **fabric:/Voting/azurevotefront** a zvolte **Škálovat službu**.

    ![containersquickstartscale][containersquickstartscale]

  Nyní můžete škálovat počet instancí webové front-end služby.

3. Změňte počet na **2** a klikněte na **Škálovat službu**.
4. Ve stromovém zobrazení klikněte na uzel **fabric:/Voting/azurevotefront** a rozbalte uzel oddílu (reprezentovaný identifikátorem GUID).

    ![containersquickstartscaledone][containersquickstartscaledone]

    Nyní je vidět, že má služba dvě instance. Ve stromovém zobrazení vidíte, na kterých uzlech instance spuštěné.

Touto jednoduchou úlohou správy jsme zdvojnásobili prostředky, které má naše front-end služba k dispozici pro zpracování uživatelské zátěže. Je důležité si uvědomit, že pro spolehlivý provoz služby nepotřebujete více jejích instancí. Pokud služba selže, Service Fabric zajistí v clusteru spuštění nové instance služby.

## <a name="clean-up"></a>Vyčištění
Pomocí odinstalačního skriptu, který je součástí šablony, odstraňte instanci aplikace z clusteru a zrušte registraci typu aplikace. Vyčištění instance tímto příkazem nějakou dobu trvá a příkaz install'sh by se neměl spouštět ihned po tomto skriptu. 

```bash
./uninstall.sh
```

## <a name="next-steps"></a>Další kroky
V tomto rychlém startu jste se naučili:
> [!div class="checklist"]
> * Nasazení aplikace typu kontejner pro Linux v Azure
> * Převzetí služeb při selhání kontejneru v clusteru Service Fabric
> * Škálování kontejneru v clusteru Service Fabric

* Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-containers-overview.md).
* Informace o [životním cyklu aplikace](service-fabric-application-lifecycle.md) Service Fabric.
* Prohlédněte si [vzorové kódy kontejneru Service Fabric](https://github.com/Azure-Samples/service-fabric-containers) na GitHubu.

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png
