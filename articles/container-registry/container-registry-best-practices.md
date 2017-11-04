---
title: "Osvědčené postupy v registru kontejner Azure"
description: "Zjistěte, jak efektivně používat registr kontejner Azure pomocí následujících tyto osvědčené postupy."
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
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="best-practices-for-azure-container-registry"></a>Osvědčené postupy pro registru kontejner Azure

Pomocí následujících tyto doporučené postupy, můžete pomoct maximalizovat výkon a nákladově efektivní využití vaší privátní registru Docker v Azure.

## <a name="network-close-deployment"></a>Nasazení sítě zavřít

Ve stejné oblasti Azure, ve kterém nasazení kontejnerů vytvořte kontejner registr. Umístění v registru v oblasti, která je zavřít sítě do vašeho kontejneru může pomoci snížit latenci a náklady na hostitele.

Nasazení sítě Zavřít je jedním z hlavních důvodů pro pomocí registru privátní kontejneru. Imagí dockeru mít skvělá [rozvrstvení konstrukce](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) umožňuje pro přírůstkové nasazení. Nové uzly se však nutné všechny vrstvy, které pro danou bitovou kopii pro vyžádání obsahu. Tento počáteční `docker pull` můžete rychle přidat až několika gigabajtů. S privátní registru blízko nasazení minimalizuje latenci sítě.
Všechny veřejné cloudy, Azure zahrnuty, navíc implementují sítě odchozí poplatky. Stahování bitové kopie z jednoho datového centra na jiný přidá poplatků za odchozí sítě, kromě latence.

## <a name="geo-replicate-multi-region-deployments"></a>Nasazení s více oblast geograficky replikovat

Použití Azure kontejneru registru [geografická replikace](container-registry-geo-replication.md) funkci, pokud nasazujete kontejnerů do několika oblastí. Ať už jste obsluhující globální odběratelé z místních datových center nebo váš vývojový tým je v různých umístěních, můžete zjednodušit správu registru a zajištění minimální latence geografickou replikací v registru. Aktuálně ve verzi preview, tato funkce je k dispozici [Premium](container-registry-skus.md#premium) registrech.

Další informace o použití geografická replikace naleznete v části kurzu třemi částmi [geografická replikace v registru kontejner Azure](container-registry-tutorial-prepare-registry.md).

## <a name="repository-namespaces"></a>Obory názvů úložiště

S využitím úložiště obory názvů, můžete povolit sdílení jednoho registru v několika skupinách v rámci vaší organizace. Registry můžete sdílet mezi nasazení a týmy. Azure registru kontejner podporuje vnořené obory názvů, povolení skupiny izolace.

Představte si třeba následující kontejneru obrázek značky. Bitové kopie, které se používají podnikové úrovni, jako je `aspnetcore`, jsou umístěny v kořenového oboru názvů, zatímco kontejneru Image vlastníkem skupiny produkční a uvádění na trh každém použití vlastní obory názvů.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Skupina prostředků vyhrazené

Protože kontejner registrech jsou prostředky, které se používají ve víc hostitelích kontejneru, registru by měl být umístěn vlastní skupině prostředků.

I když může experimentovat s typem konkrétního hostitele, jako je například instancí kontejnerů Azure, budete pravděpodobně chtít odstranit kontejner instance, když jste hotovi. Ale můžete také chtít zachovat kolekci imagí, které se instaluje do registru kontejner Azure. Tím, že vaše registru vlastní skupině prostředků, můžete minimalizovat riziko omylem odstraněním kolekce obrázků v registru při odstranění skupiny prostředků instance kontejneru.

## <a name="authentication"></a>Authentication

Při ověřování pomocí služby Azure kontejneru registru, existují dva základní scénáře: jednotlivých ověřování a ověřování služby (nebo "bezobslužných"). Následující tabulka obsahuje stručný přehled těchto scénářů a doporučenou metodou ověřování pro každou.

| Typ | Příklad scénáře | Doporučené metody |
|---|---|---|
| Jednotlivé identity | Vývojář bitové kopie na vyžádání nebo vkládání bitové kopie z jejich vývojovém počítači. | [AZ acr přihlášení](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Identita bezobslužných/služby | Sestavení a nasazení kanály, kde uživatel není zahrnut přímo. | [Instanční objekt](container-registry-authentication.md#service-principal) |

Podrobné informace o ověřování Azure kontejneru registru najdete v tématu [ověřit přístup registru kontejner Azure](container-registry-authentication.md).

## <a name="next-steps"></a>Další kroky

Azure kontejneru registru je k dispozici v několika vrstvami, názvem SKU, že každý poskytovat různé možnosti. Podrobnosti na dostupné edice najdete v tématu [SKU registru kontejner Azure](container-registry-skus.md).