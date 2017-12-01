---
title: "Vytvoření bitové kopie kontejner pro Azure Service Fabric | Microsoft Docs"
description: "Naučte se vytvořit kontejner Image pro aplikace Service Fabric více kontejneru."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker kontejnery, Mikroslužeb, Service Fabric, Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: ecb70b88f6548e4730bcc1578de2f748cda33b0a
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="create-container-images-for-service-fabric"></a>Vytvoření bitové kopie kontejner pro Service Fabric

V tomto kurzu je součástí, jeden kurzu série, která demonstruje použití kontejnery v clusteru s podporou Linux Service Fabric. V tomto kurzu je více kontejnerové aplikace připravené pro použití s nástrojem Service Fabric. V následujících kurzech tato Image slouží jako součást aplikace Service Fabric. V tomto kurzu se naučíte: 

> [!div class="checklist"]
> * Zdroj klonování aplikace z webu GitHub  
> * Vytvořit bitovou kopii kontejneru z aplikace zdroje.
> * Nasaďte instanci Azure Container registru (ACR)
> * Značka obrázku kontejner pro ACR
> * Nahrajte image na ACR

V této série kurzu zjistíte, jak: 

> [!div class="checklist"]
> * Vytvoření bitové kopie kontejner pro Service Fabric
> * [Sestavení a spuštění aplikace služby infrastruktury s kontejnery](service-fabric-tutorial-package-containers.md)
> * [Zpracování převzetí služeb při selhání a škálování v Service Fabric](service-fabric-tutorial-containers-failover.md)

## <a name="prerequisites"></a>Požadavky

- Vývojové prostředí Linux nastavení pro Service Fabric. Postupujte podle pokynů [sem](service-fabric-get-started-linux.md) nastavit prostředí Linux. 
- Tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 
- Navíc vyžaduje, abyste měli předplatné Azure k dispozici. Další informace o bezplatnou zkušební verzi, přejděte [zde](https://azure.microsoft.com/free/).

## <a name="get-application-code"></a>Získání kódu aplikace

Ukázková aplikace používá v tomto kurzu se hlasovací aplikaci. Aplikace se skládá z front-endové webové součásti a instanci Redis back-end. Součásti jsou sbaleny do kontejneru bitové kopie. 

Pomocí git stáhnout kopii aplikace na svoje vývojové prostředí.

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/
```

Adresář, kontejner tutorial' obsahuje složku s názvem 'azure hlas'. Tato složka 'azure hlas' obsahuje front-end zdrojový kód a soubor Docker k vytváření front-endu. Adresář, kontejner tutorial' také obsahuje adresáři 'redis', který má soubor Docker vytvořit bitovou kopii redis. Tyto adresáře obsahovat potřebné prostředky pro tento kurz sadu. 

## <a name="create-container-images"></a>Vytvořit kontejner bitové kopie

V adresáři, azure hlas, spusťte následující příkaz k vytvoření bitové kopie pro komponentu front-endové webové. Tento příkaz používá soubor Docker v tomto adresáři vytvořit bitovou kopii. 

```bash
docker build -t azure-vote-front .
```

Uvnitř, adresáři 'redis, spusťte následující příkaz k vytvoření bitové kopie pro back-end redis. Tento příkaz používá soubor Docker v adresáři vytvořit bitovou kopii. 

```bash
docker build -t azure-vote-back .
```

Po dokončení použít [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkazu zobrazte vytvořené bitové kopie.

```bash
docker images
```

Všimněte si, že čtyři bitové kopie byly staženy nebo vytvořeny. *Azure hlas front* image obsahuje aplikace. Odvozený z *python* bitovou kopii z úložiště Docker Hub. Bitovou kopii Redis byl stažen z úložiště Docker Hub.

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB

```

## <a name="deploy-azure-container-registry"></a>Nasadit kontejner Azure registru

Nejprve spuštěním [az přihlášení](/cli/azure/login) příkaz k přihlášení k účtu Azure. 

Pak pomocí [az účet](/cli/azure/account#set) příkazu vyberte předplatné, vytvoření kontejneru Azure registru. 

```bash
az account set --subscription <subscription_id>
```

Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v *westus* oblast. V oblasti okolo vás, vyberte skupinu prostředků. 

```bash
az group create --name myResourceGroup --location westus
```

Vytvoření kontejneru Azure registr s využitím [az acr vytvořit](/cli/azure/acr#create) příkaz. Název kontejneru registru **musí být jedinečné**.

```bash
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

Po celý zbytek v tomto kurzu používáme "acrname" jako zástupný symbol pro název kontejneru registru, který jste si zvolili.

## <a name="log-in-to-your-container-registry"></a>Přihlaste se k vaší registru kontejneru

Přihlaste se k instanci ACR před odesláním bitové kopie do ní. Použití [az acr přihlášení](/cli/azure/acr?view=azure-cli-latest#az_acr_login) příkaz k dokončení operace. Zadejte jedinečný název zadané registru kontejneru v okamžiku vytvoření.

```bash
az acr login --name <acrName>
```

Příkaz vrátí zprávu, byla úspěšná přihlášení po dokončení.

## <a name="tag-container-images"></a>Značka kontejneru obrázků

Každé bitové kopie kontejneru musí být označené loginServer název registru. Tato značka se používá pro směrování při nabízení kontejneru bitové kopie do registru bitovou kopii.

Chcete-li zobrazit seznam aktuální bitové kopie, použijte [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkaz.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB
```

Získat název loginServer, spusťte následující příkaz:

```bash
az acr show --name <acrName> --query loginServer --output table
```

Nyní, značky *azure hlas front* bitovou kopii s loginServer registru kontejneru. Navíc přidat `:v1` na konec název bitové kopie. Tato značka označuje verzi bitové kopie.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

V dalším kroku značky *azure. hlas zpětný* bitovou kopii s loginServer registru kontejneru. Navíc přidat `:v1` na konec název bitové kopie. Tato značka označuje verzi bitové kopie.

```bash
docker tag azure-vote-back <acrLoginServer>/azure-vote-back:v1
```

'Imagí dockeru' spustit po označené, abyste ověřili operaci.


Výstup:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
azure-vote-back                        latest              bf9a858a9269        22 minutes ago      107MB
<acrName>.azurecr.io/azure-vote-back    v1                  bf9a858a9269        22 minutes ago      107MB
azure-vote-front                       latest              052c549a75bf        23 minutes ago      708MB
<acrName>.azurecr.io/azure-vote-front   v1                  052c549a75bf        23 minutes ago      708MB
redis                                  latest              9813a7e8fcc0        2 days ago          107MB
tiangolo/uwsgi-nginx-flask             python3.6           590e17342131        5 days ago          707MB

```

## <a name="push-images-to-registry"></a>Push bitové kopie do registru.

Push *azure hlas front* bitovou kopii do registru. 

Pomocí následujícího příkladu, nahraďte název ACR loginServer loginServer ze svého prostředí.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

Push *azure. hlas zpětný* bitovou kopii do registru. 

Pomocí následujícího příkladu, nahraďte název ACR loginServer loginServer ze svého prostředí.

```bash
docker push <acrLoginServer>/azure-vote-back:v1
```

Příkazy nabízené docker trvat několik minut.

## <a name="list-images-in-registry"></a>Seznam obrázků v registru

Chcete-li vrátit seznam bitové kopie, které se nabídne do registru kontejneru Azure, použijte [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz. Aktualizujte příkaz s názvem instance ACR.

```bash
az acr repository list --name <acrName> --output table
```

Výstup:

```bash
Result
----------------
azure-vote-back
azure-vote-front
```

V kurzu dokončení bitovou kopii kontejneru byla uložena v privátní instanci Azure Container registru. V následujících kurzech je nasadit tuto bitovou kopii z ACR do clusteru Service Fabric.

## <a name="next-steps"></a>Další kroky

V tomto kurzu se vyžádat aplikace z webu Github a bitové kopie kontejneru byly vytvořeny a instaluje do registru. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Zdroj klonování aplikace z webu GitHub  
> * Vytvořit bitovou kopii kontejneru z aplikace zdroje.
> * Nasaďte instanci Azure Container registru (ACR)
> * Značka obrázku kontejner pro ACR
> * Nahrajte image na ACR

Přechodu na v dalším kurzu se dozvíte o balení kontejnery do aplikace Service Fabric pomocí Yeoman. 

> [!div class="nextstepaction"]
> [Zabalení a nasazení kontejnerů jako aplikace Service Fabric](service-fabric-tutorial-package-containers.md)
