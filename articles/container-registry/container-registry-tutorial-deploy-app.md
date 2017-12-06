---
title: "Kurz pro Azure kontejneru registru – nasazení webové aplikace z registru kontejner Azure"
description: "Nasazení založené na systému Linux webové aplikace pomocí bitové kopie kontejneru z registru geograficky replikované kontejner Azure. Dva součástí série, třemi částmi."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: d775a17cb8069a7521788d850d7d52b92cc67526
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-web-app-from-azure-container-registry"></a>Nasazení webové aplikace z registru kontejner Azure

Toto je část dva v řadě kurz třemi částmi. V [první část](container-registry-tutorial-prepare-registry.md), byl vytvořen privátní, geograficky replikované kontejneru registru a bitovou kopii kontejner byl vytvořen ze zdroje a instaluje do registru. V tomto článku nasadíte do dvě instance webové aplikace ve dvou různých oblastech Azure využívat výhod sítě zavřít aspektů registru geograficky replikované kontejneru.

V tomto kurzu součástí dvě řady:

> [!div class="checklist"]
> * Nasazení bitové kopie kontejneru do dvou *webové aplikace pro kontejnery* instancí
> * Ověřte nasazené aplikace

Pokud jste ještě nevytvořili geograficky replikované registru a instaluje bitovou kopii kontejnerizované ukázkové aplikace v registru vrátit k předchozí kurz v této sérii [Příprava geograficky replikované kontejner Azure registru](container-registry-tutorial-prepare-registry.md).

V další části řady aktualizaci aplikace potom odešlete novou bitovou kopii kontejneru do registru. Nakonec přejdete na každou spuštěnou instanci webové aplikace, abyste zobrazili změny automaticky projeví v obou zobrazující geografická replikace registru kontejner Azure a pomocí webhooků v akci.

## <a name="automatic-deployment-to-web-apps-for-containers"></a>Automatické nasazení do webové aplikace pro kontejnery

Azure registru kontejneru poskytuje podporu pro nasazování kontejnerizovaných aplikací přímo na [webové aplikace pro kontejnery](../app-service/containers/index.yml). V tomto kurzu použijete portál Azure k nasazení kontejneru bitové kopie vytvořené v předchozí kurzu dva plány webové aplikace umístěné v různých oblastech Azure.

Při nasazení webové aplikace z image kontejneru v registru, a máte registru geograficky replikované ve stejné oblasti, vytvoří kontejner registru Azure nasazení bitové kopie [webhooku](container-registry-webhook.md) za vás. Po stisknutí novou bitovou kopii do kontejneru úložiště, webhooku převezme změn a nová bitová kopie kontejneru automaticky nasadí do vaší webové aplikace.

## <a name="deploy-a-web-app-for-containers-instance"></a>Nasazení webové aplikace pro instanci kontejnery

V tomto kroku vytvoření webové aplikace pro instanci kontejnery v *západní USA* oblast.

Přihlaste se k [portál Azure](https://portal.azure.com) a přejděte do registru, kterou jste vytvořili v předchozí kurzu.

Vyberte **úložiště** > **acr helloworld**, klikněte pravým tlačítkem na **v1** značky pod **značky** a vyberte **Nasadit do webové aplikace**.

![Nasazení do služby app service na portálu Azure][deploy-app-portal-01]

V části **webovou aplikaci pro kontejnery** , se zobrazí, zadejte následující hodnoty pro každé nastavení:

| Nastavení | Hodnota |
|---|---|
| **Název lokality** | Globálně jedinečného názvu pro webovou aplikaci. V tomto příkladu používáme formát `<acrName>-westus` snadno identifikovat registru a webové aplikace je nasazená z oblasti. |
| **Skupina prostředků** | **Použít existující** > `myResourceGroup` |
| **Umístění plánu služby aplikace** | Vytvořit nový plán s názvem `plan-westus` v **západní USA** oblast. |
| **Bitové kopie** | `acr-helloworld:v1`

Vyberte **vytvořit** ke zřízení webové aplikace na *západní USA* oblast.

![Webové aplikace na konfiguraci systému Linux na portálu Azure][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>Zobrazení nasazené webové aplikace

Po dokončení nasazení můžete zobrazit běžící aplikaci tak, že přejdete na jeho adresu URL v prohlížeči.

Na portálu, vyberte **App Services**, pak webové aplikace, které jste zřídili v předchozím kroku. V tomto příkladu je název webové aplikace *uniqueregistryname westus*.

Vyberte s hypertextovým odkazem adresa URL webové aplikace v pravé horní části **služby App Service** Přehled zobrazíte běžící aplikaci v prohlížeči.

![Webové aplikace na konfiguraci systému Linux na portálu Azure][deploy-app-portal-04]

Po nasazení bitové kopie Docker z registru geograficky replikované kontejneru lokality zobrazí obrázek představující oblast Azure, který je hostitelem kontejneru registru.

![Zobrazit v prohlížeči nasazené webové aplikace][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>Nasazení druhý webové aplikace pro instanci kontejnery

Podle postupu uvedeného v předchozí části druhý webovou aplikaci k nasazení *východní USA* oblast. V části **webovou aplikaci pro kontejnery**, zadejte následující hodnoty:

| Nastavení | Hodnota |
|---|---|
| **Název lokality** | Globálně jedinečného názvu pro webovou aplikaci. V tomto příkladu používáme formát `<acrName>-eastus` snadno identifikovat registru a webové aplikace je nasazená z oblasti. |
| **Skupina prostředků** | **Použít existující** > `myResourceGroup` |
| **Umístění plánu služby aplikace** | Vytvořit nový plán s názvem `plan-eastus` v **východní USA** oblast. |
| **Bitové kopie** | `acr-helloworld:v1`

Vyberte **vytvořit** ke zřízení webové aplikace na *východní USA* oblast.

![Webové aplikace na konfiguraci systému Linux na portálu Azure][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>Zobrazení nasazené webové aplikace

Jako dříve, můžete zobrazit běžící aplikaci tak, že přejdete na jeho adresu URL v prohlížeči.

Na portálu, vyberte **App Services**, pak webové aplikace, které jste zřídili v předchozím kroku. V tomto příkladu je název webové aplikace *uniqueregistryname eastus*.

Vyberte s hypertextovým odkazem adresa URL webové aplikace v pravé horní části **Přehled služby App Service** zobrazíte běžící aplikaci v prohlížeči.

![Webové aplikace na konfiguraci systému Linux na portálu Azure][deploy-app-portal-07]

Po nasazení bitové kopie Docker z registru geograficky replikované kontejneru lokality zobrazí obrázek představující oblast Azure, který je hostitelem kontejneru registru.

![Zobrazit v prohlížeči nasazené webové aplikace][deployed-app-eastus]

## <a name="next-steps"></a>Další kroky

V tomto kurzu nasadíte dva webové aplikace pro kontejnery instancí z registru geograficky replikované kontejner Azure. Podle kroků v tomto kurzu jste:

> [!div class="checklist"]
> * Nasazení bitové kopie kontejner ke dvěma *webové aplikace pro kontejnery* instancí
> * Ověřit nasazené aplikace

Přechodu na další kurz k aktualizaci a pak nasadit novou bitovou kopii kontejneru registru kontejneru a ověřte, že se automaticky aktualizovaly webové aplikace spuštěné v obou oblastí.

> [!div class="nextstepaction"]
> [Nasaďte aktualizaci do bitové kopie geograficky replikované kontejneru](./container-registry-tutorial-deploy-update.md)

<!-- IMAGES -->
[deploy-app-portal-01]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-01.png
[deploy-app-portal-02]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-02.png
[deploy-app-portal-03]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-03.png
[deploy-app-portal-04]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-04.png
[deploy-app-portal-05]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-05.png
[deploy-app-portal-06]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-06.png
[deploy-app-portal-07]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-07.png
[deployed-app-westus]: ./media/container-registry-tutorial-deploy-app/deployed-app-westus.png
[deployed-app-eastus]: ./media/container-registry-tutorial-deploy-app/deployed-app-eastus.png