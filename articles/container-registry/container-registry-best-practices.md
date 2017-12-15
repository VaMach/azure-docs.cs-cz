---
title: "Osvědčené postupy ve službě Azure Container Registry"
description: "Zjistěte, jak pomocí těchto osvědčených postupů efektivně používat službu Azure Container Registry."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 9ec5573082dbf9de1288e1511f5041f8c20b416e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="best-practices-for-azure-container-registry"></a>Osvědčené postupy pro službu Azure Container Registry

Když se budete řídit těmito osvědčenými postupy, můžete maximalizovat výkon a nákladově efektivní používání svého privátního registru Dockeru v Azure.

## <a name="network-close-deployment"></a>Nasazení blízko sítě

Vytvořte registr kontejnerů ve stejné oblasti Azure, do které nasazujete kontejnery. Umístění registru do oblasti blízko sítě hostitelů kontejnerů může pomoct snížit latenci i náklady.

Nasazení blízko sítě je jedním z hlavních důvodů pro použití privátního registru kontejnerů. Image Dockeru obsahují skvělý [koncept vrstvení](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/), který umožňuje přírůstkové nasazování. Nové uzly však musí přetahovat všechny vrstvy požadované pro danou image. Toto počáteční přetáhnutí příkazem `docker pull` se rychle může rozrůst až na několik gigabajtů. Umístění privátního registru blízko nasazení minimalizuje latenci sítě.
Všechny veřejné cloudy včetně Azure navíc zahrnují poplatky za odchozí přenosy v síti. Přetahování imagí z jednoho datacentra do jiného zvyšuje kromě latence i poplatky za odchozí přenosy v síti.

## <a name="geo-replicate-multi-region-deployments"></a>Geografická replikace nasazení ve více oblastech

Pokud nasazujete kontejnery do více oblastí, využijte funkci [geografické replikace](container-registry-geo-replication.md) ve službě Azure Container Registry. Ať už obsluhujete globální zákazníky z místních datacenter nebo je váš vývojový tým rozmístěný v různých oblastech, díky geografické replikaci registru můžete zjednodušit správu registru a minimalizovat latenci. Tato funkce, která je aktuálně ve verzi Preview, je dostupná pro registry úrovně [Premium](container-registry-skus.md#premium).

Informace o použití geografické replikace najdete v třídílném kurzu [Geografická replikace ve službě Azure Container Registry](container-registry-tutorial-prepare-registry.md).

## <a name="repository-namespaces"></a>Obory názvů úložiště

S využitím oborů názvů úložiště můžete umožnit sdílení jednoho registru napříč několika skupinami v rámci vaší organizace. Registry se můžou sdílet napříč nasazeními a týmy. Azure Container Registry podporuje vnořené obory názvů a díky tomu umožňuje izolaci skupin.

Představte si například následující značky image kontejneru. Image používané v rámci celého podniku, například `aspnetcore`, jsou umístěné v kořenovém oboru názvů, zatímco jednotlivé image kontejnerů vlastněné produkčními a marketingovými skupinami používají svůj vlastní obor názvů.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Vyhrazená skupina prostředků

Vzhledem k tomu, že registry kontejnerů jsou prostředky, které se používají napříč několika hostiteli kontejnerů, měl by se registr nacházet ve své vlastní skupině prostředků.

I když můžete experimentovat s konkrétním typem hostitele, jako je služba Azure Container Instances, pravděpodobně budete chtít instanci kontejneru odstranit, jakmile budete hotovi. Můžete však také chtít zachovat kolekci imagí, které jste nasdíleli do služby Azure Container Registry. Umístěním registru do vlastní skupiny prostředků minimalizujete riziko nechtěného odstranění kolekce imagí v registru při odstraňování skupiny prostředků instance kontejneru.

## <a name="authentication"></a>Authentication

Při ověřování ve službě Azure Container Registry existují dva primární scénáře: jednotlivé ověření a ověření služby (neboli bezobslužné ověření). Následující tabulka obsahuje stručný přehled těchto scénářů a doporučenou metodu ověřování pro každý z nich.

| Typ | Příklad scénáře | Doporučená metoda |
|---|---|---|
| Jednotlivá identita | Vývojář přetahující image do svého vývojového počítače nebo sdílející image ze svého vývojového počítače. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Bezobslužné ověření/identita služby | Kanály sestavení a nasazení bez přímého zapojení uživatele. | [Instanční objekt](container-registry-authentication.md#service-principal) |

Podrobné informace o ověřování ve službě Azure Container Registry najdete v tématu [Ověřování ve službě Azure Container Registry](container-registry-authentication.md).

## <a name="next-steps"></a>Další kroky

Služba Azure Container Registry je dostupná v několika úrovních, označovaných jako skladové položky, každá z nichž poskytuje různé možnosti. Podrobnosti o dostupných skladových položkách najdete v tématu [Skladové položky služby Azure Container Registry](container-registry-skus.md).