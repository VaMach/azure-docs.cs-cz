---
title: "Připojení svazku gitRepo instancí kontejnerů Azure"
description: "Zjistěte, jak připojit svazek gitRepo klonovat úložiště Git do vaší instance kontejneru"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 9acde9259fcb392458e7b2fa7d3369776978285e
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2018
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Připojení svazku gitRepo v Azure kontejner instancí

Zjistěte, jak se připojit *gitRepo* svazku klonovat úložiště Git do vaší instancí kontejnerů.

> [!NOTE]
> Připojení *gitRepo* svazek je aktuálně omezeno na kontejnery Linux. Pracujeme na tom, aby všechny funkce byly dostupné i pro kontejnery Windows. Aktuální rozdíly pro tyto platformy najdete v tématu věnovaném [kvótám a dostupnosti oblastí pro Azure Container Instances](container-instances-quotas.md).

## <a name="gitrepo-volume"></a>gitRepo svazku

*GitRepo* svazek připojí adresáře a provede klonování zadaný úložiště Git do ní při spuštění kontejneru. Pomocí *gitRepo* svazku v vaše instance kontejneru, se můžete vyhnout přidání kódu, jak to udělat ve svých aplikacích.

Když připojíte *gitRepo* svazku, můžete nastavit tři vlastnosti pro konfiguraci svazku:

| Vlastnost | Požaduje se | Popis |
| -------- | -------- | ----------- |
| `repository` | Ano | Úplnou adresu URL, včetně `http://` nebo `https://`, klonovat úložiště Git.|
| `directory` | Ne | Adresář, do kterého by měl být klonovat úložiště. Cesta nesmí obsahovat ani začínat "`..`".  Pokud zadáte "`.`", je do adresáře svazku klonovat úložiště. Jinak je do podadresáři se zadaným názvem v adresáři svazku klonovat úložiště Git. |
| `revision` | Ne | Hodnota hash potvrzení revize ke klonování. Pokud tento parametr zadán, `HEAD` naklonována revize. |

## <a name="mount-a-gitrepo-volume"></a>Připojení svazku gitRepo

Připojit *gitRepo* svazku v instanci kontejneru, je nutné nasadit pomocí [šablony Azure Resource Manageru](/azure/templates/microsoft.containerinstance/containergroups).

Nejprve naplnit `volumes` pole ve skupině kontejneru `properties` část šablony. Dále pro každý kontejner ve skupině kontejner, ve kterém byste chtěli připojit *gitRepo* svazku, naplnit `volumeMounts` pole v `properties` části definici kontejneru.

Například následující šablony Resource Manageru vytvoří skupinu kontejneru, který se skládá z jednoho kontejneru. Kontejner provede klonování dvě úložišť GitHub určeného *gitRepo* blocích svazku. Druhý svazek zahrnuje určení adresáře klonovat na další vlastnosti a hodnotu hash potvrzení konkrétní revizi klonovat.

[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

Výsledná struktura adresářů dvě klonovaný úložiště, které jsou definované v šabloně předchozí je:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Příklad nasazení kontejneru instance pomocí šablony Azure Resource Manager, najdete v sekci [nasazení skupiny více kontejnerů v Azure kontejner instancí](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Další postup

Zjistěte, jak připojit jiné typy svazku v Azure kontejner instancí:

* [Připojit sdílenou složku Azure v Azure kontejner instancí](container-instances-volume-azure-files.md)
* [Připojit emptyDir svazek v Azure kontejner instancí](container-instances-volume-emptydir.md)
* [Připojení tajný svazku v Azure kontejner instancí](container-instances-volume-secret.md)
