---
title: "Rychlý start - privátní registru Docker v Azure vytvořit pomocí portálu Azure"
description: "Rychle se Naučte se vytvořit privátní registru kontejner Docker pomocí portálu Azure."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 514fa3490e480647f0923c99bd9606a3726d4d30
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Vytvoření registru kontejnerů pomocí webu Azure Portal

Registru kontejner Azure je privátní registru Docker v Azure, kam můžete ukládat a spravovat privátní kontejner imagí Dockeru. V tento rychlý start můžete vytvořit kontejner registru pomocí portálu Azure.

Abyste mohli dokončit tento rychlý start, musí mít Docker nainstalovány místně. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k portálu Azure na https://portal.azure.com.

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

Vyberte **nové** > **kontejnery** > **kontejner Azure registru**.

![Vytváření kontejneru registru na portálu Azure][qs-portal-01]

Zadejte hodnoty pro **název registru** a **skupiny prostředků**. Název registru musí být jedinečný v rámci Azure a obsahovat alfanumerické znaky 5 – 50. Vytvořit novou skupinu prostředků s názvem `myResourceGroup`a pro **SKU**, vyberte "Basic". Vyberte **vytvořit** nasazení ACR instance.

![Vytváření kontejneru registru na portálu Azure][qs-portal-03]

V tento rychlý start, vytvoříme *základní* registru. Azure kontejneru registru je k dispozici v několika různých SKU stručně popsány v následující tabulce. Rozšířené informace o jednotlivých v [kontejneru registru SKU](container-registry-skus.md).

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Když **nasazení bylo úspěšné** se zobrazí zpráva, vyberte kontejner registru na portálu a pak vyberte **přístupové klíče**.

![Vytváření kontejneru registru na portálu Azure][qs-portal-05]

V části **uživatel s oprávněními správce**, vyberte **povolit**. Poznamenejte si následující hodnoty:

* Přihlášení na server
* Uživatelské jméno
* heslo

Tyto hodnoty se použít v následujících krocích při práci s registrem pomocí rozhraní příkazového řádku Dockeru.

![Vytváření kontejneru registru na portálu Azure][qs-portal-06]

## <a name="log-in-to-acr"></a>Přihlaste se k ACR

Před odesíláním a vyžadováním imagí kontejnerů se musíte přihlásit k instanci služby ACR. Chcete-li to provést, použijte [docker přihlášení](https://docs.docker.com/engine/reference/commandline/login/) příkaz. Nahraďte *uživatelské jméno*, *heslo*, a *přihlášení na server* hodnoty s těmi, která jste si poznamenali v předchozím kroku.

```bash
docker login --username <username> --password <password> <login server>
```

Příkaz vrátí `Login Succeeded` po dokončení. Můžete si také prohlédnout upozornění zabezpečení doporučujeme použít `--password-stdin` parametr. Při jeho použití je mimo rámec tohoto článku, doporučujeme následující tento osvědčený postup. Najdete v článku [docker přihlášení](https://docs.docker.com/engine/reference/commandline/login/) příkaz pro další informace.

## <a name="push-image-to-acr"></a>Nabízená ACR bitové kopie

Tak, aby nabízel bitovou kopii do registru kontejner Azure, musíte nejprve mít bitovou kopii. V případě potřeby spusťte následující příkaz načítat stávající image z úložiště Docker Hub.

```bash
docker pull microsoft/aci-helloworld
```

Před nabízené bitovou kopii do registru, musíte označit bitová kopie se název ACR přihlášení serveru. Značka pomocí bitové kopie [docker značky](https://docs.docker.com/engine/reference/commandline/tag/) příkaz. Nahraďte *přihlášení na server* s přihlašovací jméno serveru jste si poznamenali dříve.

```
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Nakonec použijte [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) tak, aby nabízel bitovou kopii k instanci ACR. Nahraďte *přihlášení na server* s přihlašovacím jménem serveru vaší instance ACR.

```
docker push <login server>/aci-helloworld:v1
```

Výstup z úspěšně `docker push` je podobná příkazu:

```
The push refers to a repository [uniqueregistryname.azurecr.io/aci-helloworld]
7c701b1aeecd: Pushed
c4332f071aa2: Pushed
0607e25cc175: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:f2867748615cc327d31c68b1172cc03c0544432717c4d2ba2c1c2d34b18c62ba size: 1577
```

## <a name="list-container-images"></a>Výpis imagí kontejnerů

Pro zobrazení seznamu obrázků v instanci ACR, přejděte do registru v portálu a vyberte **úložiště**, pak vyberte úložiště, které jste vytvořili pomocí `docker push`.

V tomto příkladu jsme vyberte **aci helloworld** úložiště a jsme viděli `v1`– s příznakem bitové kopie v rámci **značky**.

![Vytváření kontejneru registru na portálu Azure][qs-portal-09]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, odstraňte **myResourceGroup** skupinu prostředků. Díky tomu bude odstranit skupinu prostředků, ACR instance a všechny Image kontejneru.

![Vytváření kontejneru registru na portálu Azure][qs-portal-08]

## <a name="next-steps"></a>Další kroky

V tento rychlý start vytvořili kontejner registru Azure pomocí Azure CLI. Pokud chcete používat registru kontejner Azure s Azure kontejner instancí, nadále kurzu instancí kontejnerů Azure.

> [!div class="nextstepaction"]
> [Kurzy služby Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-04]: ./media/container-registry-get-started-portal/qs-portal-04.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-06]: ./media/container-registry-get-started-portal/qs-portal-06.png
[qs-portal-07]: ./media/container-registry-get-started-portal/qs-portal-07.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png