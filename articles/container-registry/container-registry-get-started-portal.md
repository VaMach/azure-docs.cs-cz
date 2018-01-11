---
title: "Rychlý start – Vytvoření privátního registru Dockeru v Azure pomocí webu Azure Portal"
description: "Rychle se naučíte, jak vytvořit privátní registr Dockeru pomocí webu Azure Portal."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 12/06/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: c6ce1c16a3f7d5b52a3fcb47213618eb75183149
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Vytvoření registru kontejnerů pomocí webu Azure Portal

Registr kontejnerů Azure je privátním registrem Dockeru v Azure, kde můžete ukládat a spravovat privátní image kontejnerů Dockeru. V tomto rychlém startu vytvoříte registr kontejnerů pomocí webu Azure Portal.

K dokončení tohoto rychlého startu je potřeba místně nainstalovaný Docker. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli [Macu][docker-mac] nebo systému [Windows][docker-windows] či [Linux][docker-linux].

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k webu Azure Portal na adrese https://portal.azure.com.

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

Vyberte **Nový** > **Kontejnery** > **Azure Container Registry**.

![Vytvoření registru kontejnerů na webu Azure Portal][qs-portal-01]

Zadejte odpovídající hodnoty pro **Název registru** a **Skupina prostředků**. Název registru musí být jedinečný v rámci Azure a musí obsahovat 5 až 50 alfanumerických znaků. Vytvořte novou skupinu prostředků `myResourceGroup` a jako **Skladová položka** vyberte Basic. Vyberte **Vytvořit** a nasaďte instanci služby ACR.

![Vytvoření registru kontejnerů na webu Azure Portal][qs-portal-03]

V tomto rychlém startu vytvoříme registr úrovně *Basic*. Služba Azure Container Registry je dostupná v několika různých skladových položkách, které stručně popisuje následující tabulka. Další podrobnosti o každé z nich najdete v tématu [Skladové položky služby Container Registry][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Po zobrazení zprávy **Nasazení bylo úspěšné** vyberte na portálu registr kontejnerů a pak vyberte **Přístupové klíče**.

![Vytvoření registru kontejnerů na webu Azure Portal][qs-portal-05]

V části **Uživatel s rolí správce** vyberte **Povolit**. Poznamenejte si následující hodnoty:

* Přihlašovací server
* Uživatelské jméno
* heslo

Tyto hodnoty použijete v následujících krocích při práci s registrem pomocí rozhraní příkazového řádku Dockeru.

![Vytvoření registru kontejnerů na webu Azure Portal][qs-portal-06]

## <a name="log-in-to-acr"></a>Přihlášení ke službě ACR

Před odesíláním a vyžadováním imagí kontejnerů se musíte přihlásit k instanci služby ACR. K tomu použijte příkaz [docker login][docker-login]. Nahraďte hodnoty *username* (uživatelské jméno), *password* (heslo) a *login server* (přihlašovací server) hodnotami, které jste si poznamenali v předchozím kroku.

```bash
docker login --username <username> --password <password> <login server>
```

Příkaz po dokončení vrátí zprávu `Login Succeeded` (Přihlášení bylo úspěšné). Může se zobrazit také upozornění zabezpečení doporučující použití parametru `--password-stdin`. I když je jeho použití nad rámec tohoto článku, doporučujeme řídit se osvědčeným postupem. Další informace najdete v referenčních informacích k příkazu [docker login][docker-login].

## <a name="push-image-to-acr"></a>Nasdílení image do služby ACR

Pokud chcete nasdílet image do služby Azure Container Registry, musíte nejprve mít nějakou image. V případě potřeby si přetáhněte existující image z Docker Hubu spuštěním následujícího příkazu.

```bash
docker pull microsoft/aci-helloworld
```

Před nasdílením image do registru k ní musíte přidat značku s názvem přihlašovacího serveru služby ACR. Označte image pomocí příkazu [docker tag][docker-tag]. Nahraďte *login server* názvem přihlašovacího serveru, který jste si poznamenali dříve.

```
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Nakonec pomocí příkazu [docker push][docker-push] nasdílejte image do instance služby ACR. Nahraďte *login server* názvem přihlašovacího serveru vaší instance služby ACR.

```
docker push <login server>/aci-helloworld:v1
```

Výstup z úspěšného příkazu `docker push` vypadá nějak takto:

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

Pokud chcete zobrazit výpis všech imagí ve vaší instanci služby ACR, přejděte na portálu do vašeho registru, vyberte **Úložiště** a pak vyberte úložiště, které jste vytvořili pomocí příkazu `docker push`.

V tomto příkladu vybereme úložiště **aci-helloworld** a v části **ZNAČKY** se zobrazí image se značkou `v1`.

![Vytvoření registru kontejnerů na webu Azure Portal][qs-portal-09]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už ji nepotřebujete, odstraňte skupinu prostředků **myResourceGroup**. Tím se odstraní skupina prostředků, instance služby ACR a všechny image kontejneru.

![Vytvoření registru kontejnerů na webu Azure Portal][qs-portal-08]

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili službu Azure Container Registry pomocí Azure CLI. Pokud chcete používat službu Azure Container Registry se službou Azure Container Instances, přejděte na kurz služby Azure Container Instances.

> [!div class="nextstepaction"]
> [Kurzy služby Azure Container Instances][container-instances-tutorial-prepare-app]

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

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
