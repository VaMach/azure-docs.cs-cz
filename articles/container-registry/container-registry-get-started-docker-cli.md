---
title: "Nahrání image Dockeru do soukromého registru Azure | Dokumentace Microsoftu"
description: "Nahrání a stažení imagí Dockeru do soukromého registru kontejnerů v Azure pomocí rozhraní příkazového řádku Dockeru"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 10f01e4e8c86bbbfa17cf2559caca645ff13bdcc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Nahrání první image do soukromého registru kontejnerů Dockeru pomocí rozhraní příkazového řádku Dockeru
Registr kontejnerů Azure uchovává a spravuje privátní image kontejnerů [Dockeru](http://hub.docker.com) podobným způsobem, jakým [Docker Hub](https://hub.docker.com/) uchovává veřejné image Dockeru. K [přihlášení](https://docs.docker.com/engine/reference/commandline/login/), [nahrávání](https://docs.docker.com/engine/reference/commandline/push/), [stahování](https://docs.docker.com/engine/reference/commandline/pull/) a dalším operacím nad svým registrem kontejnerů použijete [rozhraní příkazového řádku Dockeru](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI).

Další související informace a koncepty najdete v tématu [s přehledem](container-registry-intro.md).



## <a name="prerequisites"></a>Požadavky
* **Registr kontejnerů Azure** – Vytvořte registr kontejnerů ve svém předplatném Azure. Použijte k tomu například [Azure Portal](container-registry-get-started-portal.md) nebo [Azure CLI 2.0 ](container-registry-get-started-azure-cli.md).
* **Rozhraní příkazového řádku Dockeru** – Chcete-li nastavit místní počítač jako hostitele Dockeru a získat přístup k příkazům rozhraní příkazového řádku Dockeru, nainstalujte si modul runtime [Docker Engine](https://docs.docker.com/engine/installation/).

## <a name="log-in-to-a-registry"></a>Přihlášení k registru
Spusťte příkaz `docker login` a přihlaste se ke svému registru kontejnerů pomocí [přihlašovacích údajů registru](container-registry-authentication.md).

Následující příklad předá ID a heslo [instančního objektu](../active-directory/active-directory-application-objects.md) Azure Active Directory. Instanční objekt jste k registru mohli přiřadit například pro účely scénáře automatizace.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> Nezapomeňte zadat plně kvalifikovaný název registru (malými písmeny). V tomto příkladu je to `myregistry.azurecr.io`.

## <a name="steps-to-pull-and-push-an-image"></a>Postup nahrání a stažení image
Následující příklad stáhne image serveru Nginx z veřejného registru Docker Hub, označí ji pro váš privátní registr kontejnerů Azure, nahraje ji do vašeho registru a znovu ji stáhne.

**1. Stažení oficiální image Dockeru pro server Nginx**

Nejprve si stáhněte veřejnou image serveru Nginx do místního počítače.

```
docker pull nginx
```
**2. Spuštění kontejneru Nginx**

Následující příkaz spustí místní kontejner Nginx v interaktivním režimu na portu 8080, což umožní zobrazit výstup serveru Nginx. Jakmile se příkaz zastaví, odebere spuštěný kontejner.

```
docker run -it --rm -p 8080:80 nginx
```

Přejděte na [http://localhost:8080](http://localhost:8080) a zobrazte spuštěný kontejner. Uvidíte obrazovku, která se podobá následující.

![Server Nginx na místním počítači](./media/container-registry-get-started-docker-cli/nginx.png)

Chcete-li spuštěný kontejner zastavit, stiskněte Ctrl+C.

**3. Vytvoření aliasu image ve vlastním registru**

Následující příkaz vytvoří alias image s plně kvalifikovanou cestou k vašemu registru. V tomto příkladu se určí obor názvů `samples`, aby se zabránilo nepořádku v kořenovém adresáři registru.

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```  

**4. Nahrání image do vlastního registru**

```
docker push myregistry.azurecr.io/samples/nginx
```

**5. Stažení image z vlastního registru**

```
docker pull myregistry.azurecr.io/samples/nginx
```

**6. Spuštění kontejneru Nginx z vlastního registru**

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Přejděte na [http://localhost:8080](http://localhost:8080) a zobrazte spuštěný kontejner.

Chcete-li spuštěný kontejner zastavit, stiskněte Ctrl+C.

**7. (Volitelné) Odebrání image**

```
docker rmi myregistry.azurecr.io/samples/nginx
```

##<a name="concurrent-limits"></a>Omezení souběžnosti
V některých scénářích provádění volání současně může vést k chybám. Následující tabulka obsahuje omezení souběžných volání s operacemi Push a Pull po registr kontejnerů Azure:

| Operace  | Omezení                                  |
| ---------- | -------------------------------------- |
| PULL       | Až 10 souběžných volání Pull na registr |
| PUSH       | Až 5 souběžných volání Push na registr |

## <a name="next-steps"></a>Další kroky
Nyní již znáte základy a jste připraveni používat svůj registr! Můžete například začít nasazovat image kontejnerů do clusteru [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/).
