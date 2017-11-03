---
title: "Ověřování pomocí služby Azure kontejneru registru | Microsoft Docs"
description: "Jak se přihlásit Azure kontejneru registru, pomocí účtu správce nebo objektu služby Azure Active Directory"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d7d2ae0e9b1f7850332d151d78a4a5fdb013777
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Ověření pomocí privátní registru kontejner Docker
Pro práci s obrázky kontejneru v registru kontejner Azure, je přihlásit pomocí `docker login` příkaz. Můžete se přihlásit pomocí  **[objektu služby Azure Active Directory](../active-directory/active-directory-application-objects.md)**  nebo konkrétního registru **účet správce**. Tento článek obsahuje více podrobností o těchto identit.

## <a name="service-principal"></a>Instanční objekt

Můžete přiřadit hlavní název služby do registru a použijte ho pro základní ověřování Docker. Při používání objektu služby se doporučuje pro většinu scénářů. Zadejte ID aplikace a heslo k objektu zabezpečení služby `docker login` příkaz, jak je znázorněno v následujícím příkladu:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Po přihlášení, Docker ukládá do mezipaměti přihlašovací údaje, takže nemusíte pamatovat ID aplikace.

> [!TIP]
> Pokud chcete, můžete obnovit heslo objekt služby spuštěním `az ad sp reset-credentials` příkaz.
>

Objekty služby povolit [přístupu podle rolí](../active-directory/role-based-access-control-configure.md) do registru. Dostupné role jsou:
  * Čtečka (pouze přístup k vyžádání obsahu).
  * Přispěvatel (pull a push).
  * Vlastník (pull, nabízená oznámení a přiřadit role jiným uživatelům).

Anonymní přístup není k dispozici na registrech kontejner Azure. Pro veřejné bitové kopie můžete použít [úložiště Docker Hub](https://docs.docker.com/docker-hub/).

Několik hlavních objektů služby lze přiřadit registru, který umožňuje definovat přístup pro různé uživatele nebo aplikace. Objekty služby také povolit "bezobslužných" připojení k registru v vývojáře nebo DevOps scénářů, jako jsou následující příklady:

  * Nasazení kontejnerů z registru pro systémy orchestration včetně DC/OS, Docker Swarm a Kubernetes. Můžete také pull kontejneru registrů související služby Azure, jako [Container Service](../container-service/index.yml), [služby App Service](../app-service/index.yml), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/)a další.

  * Průběžnou integraci a nasazení řešení (například Visual Studio Team Services nebo volaných) které vytvářet bitové kopie kontejneru a vložit je registru.


## <a name="admin-account"></a>Účet správce
S každou registru, které vytvoříte se automaticky vytvoří účet správce. Ve výchozím nastavení je účet zakázán, ale můžete ji povolit a spravovat přihlašovací údaje, například prostřednictvím [portál](container-registry-get-started-portal.md#create-a-container-registry) nebo pomocí [příkazy Azure CLI 2.0](container-registry-get-started-azure-cli.md#create-a-container-registry). Každý účet správce se s dvě hesla, které mohou vytvořit znovu. Zadaná dvě hesla umožňují udržování připojení k registru pomocí jedno heslo, zatímco si znovu vygenerujete jiné heslo. Pokud je účet povolený, můžete předat uživatelské jméno a buď heslo k `docker login` příkazu pro základní ověřování do registru. Například:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> Účet správce je určená pro jednoho uživatele pro přístup k registru, především pro účely testování. Není doporučeno sdílet mezi ostatní uživatelé přihlašovací údaje účtu správce. Všichni uživatelé se zobrazí jako jednoho uživatele do registru. Změna nebo zakázání účtu zakáže přístup k registru pro všechny uživatele, kteří používají přihlašovací údaje.
>

### <a name="next-steps"></a>Další kroky
* [Push vaší první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md).
* Další informace o ověřování v kontejneru registru ve verzi preview, najdete v článku [příspěvku na blogu](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).
