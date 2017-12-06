---
title: "Kontejner Azure registru úložiště"
description: "Jak používat Azure registru kontejner úložiště pro imagí Dockeru"
services: container-registry
author: cristy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 3321dd1d8bbd1b8232c26491edd8c374df16b813
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-registry-repositories"></a>Kontejner Azure registru úložiště

Kontejner Azure registru umožňuje ukládání imagí kontejneru v úložiště. A to uložením do úložiště bitové kopie, může mít skupin bitové kopie (nebo verzi bitové kopie) v izolovaném prostředí. Tyto úložiště můžete zadat, když push bitové kopie do registru.


## <a name="prerequisites"></a>Požadavky
* **Registr kontejnerů Azure** – Vytvořte registr kontejnerů ve svém předplatném Azure. Použijte k tomu například [Azure Portal](container-registry-get-started-portal.md) nebo [Azure CLI 2.0 ](container-registry-get-started-azure-cli.md).
* **Rozhraní příkazového řádku Dockeru** – Chcete-li nastavit místní počítač jako hostitele Dockeru a získat přístup k příkazům rozhraní příkazového řádku Dockeru, nainstalujte si modul runtime [Docker Engine](https://docs.docker.com/engine/installation/).
* **Obrázek pro vyžádání obsahu** – bitovou kopii pro vyžádání obsahu z veřejného úložiště Docker Hub registru, se značkami a poslat ho přímo v registru. Návod push a bitové kopie pro vyžádání obsahu, najdete v části [Docker nabízené bitovou kopii do Azure privátní registru](container-registry-get-started-docker-cli.md).


## <a name="viewing-repositories-in-the-portal"></a>Zobrazení úložiště na portálu

Jakmile bitové kopie mít nabídnutých do kontejneru registr, uvidíte seznam úložiště hostování bitové kopie na portálu Azure.

Pokud jste postupovali podle kroků v [Push Docker bitovou kopii do Azure privátní registru](container-registry-get-started-docker-cli.md) článku teď byste měli mít bitovou kopii Nginx v registru kontejneru. Jako součást pokynů by měl mít zadat obor názvů pro bitovou kopii. V následujícím příkladu příkaz sami bitovou kopii NGinx do úložiště "samples":

```
docker push myregistry.azurecr.io/samples/nginx
```
 Azure Container Registry podporuje víceúrovňové obory názvů úložiště. Tato funkce umožňuje seskupovat kolekce imagí souvisejících s konkrétní aplikací nebo kolekcí aplikací pro konkrétní vývojové nebo provozní týmy. Další informace o úložiště v registrech kontejneru, najdete v části [registrech kontejner Docker privátní v Azure](container-registry-intro.md).

Chcete-li zobrazit úložiště registru kontejneru:

1. Přihlášení k portálu Azure Portal
2. Na **registru kontejner Azure** okně vyberte registru, které chcete zkontrolovat
3. V okně registru klikněte na tlačítko **úložiště** zobrazíte seznam všech úložiště a jejich obrázků
4. (Volitelné) Vyberte konkrétní image zobrazíte značky

![Úložiště na portálu](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a>Další kroky
Nyní již znáte základy a jste připraveni používat svůj registr! Můžete například začít nasazovat image kontejnerů do clusteru [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/).
