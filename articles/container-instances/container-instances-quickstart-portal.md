---
title: "Rychlý start - vytvoření vaší první kontejner instancí kontejnerů Azure pomocí portálu Azure"
description: "Nasazení služby Azure Container Instances a zahájení práce"
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 0179107ece1e150246ab40836783d810425be3ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Vytvoření prvního kontejneru ve službě Azure Container Instances

Azure Container Instances zjednodušuje vytváření a správu kontejnerů v Azure. V tento rychlý start vytvořit kontejner ve službě Azure a umístěte ji do internet s veřejnou IP adresu. Tuto operaci je dokončit pomocí portálu Azure. Pomocí několika kliknutí zobrazí se to v prohlížeči:

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="create-a-container-instance"></a>Vytvoření instance kontejneru

Vyberte **nový** > **kontejnery** > **instancí kontejnerů Azure (preview)**.

![Zahájit vytvoření nové instance kontejneru na portálu Azure][aci-portal-01]

Zadejte následující hodnoty v **název kontejneru**, **kontejneru image**, a **skupiny prostředků** textových polí. Nechte ostatní hodnoty výchozích hodnot a pak klikněte na **OK**.

* Název kontejneru:`mycontainer`
* Obrázek kontejneru:`microsoft/aci-helloworld`
* Skupina prostředků:`myResourceGroup`

![Konfigurace základního nastavení pro novou instanci kontejneru na portálu Azure][aci-portal-03]

Kontejnery Windows a Linux můžete vytvořit v Azure kontejner instancí. V tento rychlý start necháme výchozí nastavení **Linux** vzhledem k tomu, že jsme zadali kontejner založených na Linuxu (`microsoft/aci-helloworld`) v předchozím kroku.

Ponechejte v nastavení **konfigurace** výchozích hodnot, pak klikněte na tlačítko **OK** ověření konfigurace.

![Konfigurace nové instance kontejneru na portálu Azure][aci-portal-04]

Po dokončení ověření se zobrazí souhrn nastavení kontejneru. Vyberte **OK** odešlete žádost nasazení kontejneru.

![Souhrn nastavení pro novou instanci kontejneru na portálu Azure][aci-portal-05]

Při spuštění nasazení je umístěn na dlaždici na řídicím panelu portálu označující průběh nasazení. Po dokončení nasazení dlaždici aktualizována na nové **můj_kontejner myc1** skupina kontejneru.

![Průběh vytváření nové instance kontejneru na portálu Azure][aci-portal-08]

Vyberte **můj_kontejner myc1** skupina kontejneru zobrazíte vlastnosti skupiny kontejneru. Poznamenejte si **Ip adresu** kontejner skupiny, a taky **stavu** vašeho kontejneru.

![Přehled skupinových kontejneru na portálu Azure][aci-portal-06]

Jakmile kontejneru přesune **systémem** stavu, přejděte na adresu IP, který jste si poznamenali v předchozím kroku k zobrazení aplikace hostované na nový kontejner.

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-app-browser]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png

## <a name="next-steps"></a>Další kroky

V tento rychlý start vytvořili kontejner Instance Azure z bitové kopie v veřejného úložiště Docker Hub. Pokud chcete zkuste jej sestavit kontejner sami a nasadíte ho do Azure kontejner instancí pomocí registru kontejner Azure, přejděte k kurzu instancí kontejnerů Azure.

> [!div class="nextstepaction"]
> [Kurzy služby Azure Container Instances](./container-instances-tutorial-prepare-app.md)