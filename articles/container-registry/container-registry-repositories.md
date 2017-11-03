---
title: "Kontejner Azure registru úložiště | Microsoft Docs"
description: "Jak používat Azure registru kontejner úložiště pro imagí Dockeru"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: d737df98fee955b57166cb44f45f61dff906514e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
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
