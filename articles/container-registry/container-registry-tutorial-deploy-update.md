---
title: "Kurz pro Azure kontejneru registru – nabízené aktualizovanou bitovou kopii pro místní nasazení"
description: "Nabízená upravené bitové kopie Docker vaše geograficky replikované Azure přidat registru, a poté automaticky nasadí do webové aplikace běžící v několika oblastech změny nezobrazí. Tři součástí série, třemi částmi."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 359fdcabd579d277e40f02eba2d4603ebd9f5f1f
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="push-an-updated-image-to-regional-deployments"></a>Push aktualizovanou bitovou kopii pro místní nasazení

Toto je část tři v řadě kurz třemi částmi. V [předchozí kurzu](container-registry-tutorial-deploy-app.md), geografická replikace byl nakonfigurován na dvou různých místní nasazení webové aplikace. V tomto kurzu jste nejprve upravit aplikaci, pak vytvořit novou bitovou kopii kontejneru a poslat ho přímo vaší geograficky replikované registru. Nakonec můžete zobrazit změnu, automaticky nasadit pomocí webhooků registru kontejner Azure, v obou případech webové aplikace.

V tomto kurzu poslední část v řadě:

> [!div class="checklist"]
> * Úprava webové aplikace HTML
> * Sestavení a označit bitovou kopii Docker
> * Oznámení změn registru kontejner Azure
> * Zobrazení aktualizované aplikace ve dvou různých oblastech

Pokud jste nakonfigurovali ještě dva *webovou aplikaci pro kontejnery* místní nasazení vrátit k předchozí kurz v této sérii [nasadit webovou aplikaci z registru kontejner Azure](container-registry-tutorial-deploy-app.md).

## <a name="modify-the-web-application"></a>Úprava webové aplikace

V tomto kroku změňte webové aplikace, které se bude po push bitovou kopii aktualizované kontejneru do registru kontejner Azure vysoce viditelné.

Najít `AcrHelloworld/Views/Home/Index.cshtml` souboru ve zdroji aplikace [klonovat z Githubu](container-registry-tutorial-prepare-registry.md#get-application-code) v předchozí kurzu a otevřete ji ve svém oblíbeném textovém editoru. Přidejte následující řádek pod existující `<h1>` řádku:

```html
<h1>MODIFIED</h1>
```

Vaše změny `Index.cshtml` by měl vypadat podobně jako:

```html
@{
    ViewData["Title"] = "Azure Container Registry :: Geo-replication";
}
<style>
    body {
        background-image: url('images/azure-regions.png');
        background-size: cover;
    }
    .footer {
        position: fixed;
        bottom: 0px;
        width: 100%;
    }
</style>

<h1 style="text-align:center;color:blue">Hello World from:  @ViewData["REGION"]</h1>
<h1>MODIFIED</h1>
<div class="footer">
    <ul>
        <li>Registry URL: @ViewData["REGISTRYURL"]</li>
        <li>Registry IP: @ViewData["REGISTRYIP"]</li>
        <li>Registry Region: @ViewData["REGION"]</li>
    </ul>
</div>
```

## <a name="rebuild-the-image"></a>Znovu sestavte bitovou kopii

Teď, když po aktualizaci webové aplikace, znovu sestavte jeho image kontejneru. Jako dříve použijte název plně kvalifikovaný bitové kopie, včetně adresu URL serveru přihlášení pro značku:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

## <a name="push-image-to-azure-container-registry"></a>Push bitové kopie do registru kontejner Azure

Nyní, push aktualizovaný *acr helloworld* kontejneru bitovou kopii do vaší geograficky replikované registru. Zde se provádění jedné `docker push` příkazu nasaďte aktualizovanou bitovou kopii do registru replik v obou *západní USA* a *východní USA* oblasti.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Výstup by měl vypadat přibližně takto:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
5b9454e91555: Pushed
d6803756744a: Layer already exists
b7b1f3a15779: Layer already exists
a89567dff12d: Layer already exists
59c7b561ff56: Layer already exists
9a2f9413d9e4: Layer already exists
a75caa09eb1f: Layer already exists
v1: digest: sha256:4c3f2211569346fbe2d1006c18cbea2a4a9dcc1eb3a078608cef70d3a186ec7a size: 1792
```

## <a name="view-the-webhook-logs"></a>Zobrazit protokoly webhooku

Když je právě replikován bitovou kopii, najdete v registru kontejner Azure webhooků, které se aktivuje.

Chcete-li zobrazit místní webhooků, které byly vytvořeny při nasazení kontejner, aby *webové aplikace pro kontejnery* předchozí kurzu, přejděte do kontejneru registr na portálu Azure a pak vyberte **Webhooky**pod **služby**.

![Kontejner registru Webhooky na portálu Azure][tutorial-portal-01]

Vyberte každý Webhooku zobrazíte historii jeho volání a odpovědi. Měli byste vidět řádek pro **nabízené** akce v protokolech obou Webhooky. Tady, se protokol pro Webhook umístěný v *západní USA* oblast ukazuje **nabízené** akce aktivuje `docker push` v předchozím kroku:

![Kontejner registru Webhooku protokolu na portálu Azure (západní USA)][tutorial-portal-02]

## <a name="view-the-updated-web-app"></a>Zobrazení aktualizované webové aplikace

Webhooků, které jsou upozornit webové aplikace, aby bylo posunuto novou bitovou kopii do registru, které automaticky nasadí aktualizované kontejneru na dva místní webové aplikace.

Ověřte, že aplikace se aktualizovalo v obou nasazení tak, že přejdete do obou místní nasazení webové aplikace ve webovém prohlížeči. Připomínáme můžete získat adresu URL nasazené webové aplikace v pravé horní části každé kartě Přehled služby App Service.

![Přehled služby App Service na portálu Azure][tutorial-portal-03]

Pokud chcete zobrazit aktualizovanou aplikaci, vyberte odkaz v přehledu služby App Service. Tady je příklad zobrazení aplikace spuštěné v *západní USA*:

![Zobrazit v prohlížeči upravené webové aplikace spuštěna v oblasti západní USA][deployed-app-westus-modified]

Ověřte, že bitovou kopii aktualizované kontejneru byla nasazená taky pro *východní USA* nasazení zobrazením v prohlížeči.

![Zobrazit v prohlížeči upravené webové aplikace spuštěna v oblasti Východ USA][deployed-app-eastus-modified]

S jedním `docker push`po aktualizaci obou místní nasazení webové aplikace a registru kontejner Azure zpracování obrázků kontejneru z úložiště sítě zavřít.

## <a name="next-steps"></a>Další kroky

V tomto kurzu aktualizovat a instaluje nové verze webového kontejneru aplikace do vaší geograficky replikované registru. Webhooky v registru kontejner Azure oznámení webové aplikace pro kontejnery aktualizace, která spustí místní vyžádání obsahu z registru replik.

V tomto posledním kurz v této sérii, můžete:

> [!div class="checklist"]
> * Aktualizovat webovou aplikaci HTML
> * Vytvořené a označí bitovou kopii Docker
> * Instaluje změny do registru kontejner Azure
> * Zobrazit aktualizovaná aplikace ve dvou různých oblastech

<!-- IMAGES -->
[deployed-app-eastus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-eastus-modified.png
[deployed-app-westus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-westus-modified.png
[local-container-01]: ./media/container-registry-tutorial-deploy-update/local-container-01.png
[tutorial-portal-01]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-01.png
[tutorial-portal-02]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-02.png
[tutorial-portal-03]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-03.png