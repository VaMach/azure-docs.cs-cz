---
title: "Kurz pro Azure kontejneru registru – Příprava registru geograficky replikované kontejner Azure"
description: "Vytvoření služby Azure kontejneru registru, nakonfigurujte geografická replikace, Příprava bitové kopie Docker a nasaďte ho do registru. Část, jednu z řady třemi částmi."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/26/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 9503dc4a1fa8bc7773677de1673fed3abe1c1b51
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="prepare-a-geo-replicated-azure-container-registry"></a>Příprava registru geograficky replikované kontejner Azure

Registru kontejner Azure je privátní registru Docker nasazené v Azure, které se vejdou zavřít sítě pro vaše nasazení. V této sadě tři kurz články zjistíte, jak nasadit webovou aplikaci ASP.NET Core běží Linux kontejner, dva pomocí geografická replikace [webové aplikace pro kontejnery](../app-service/containers/index.yml) instance. Uvidíte, jak Azure automaticky nasadí bitovou kopii k jednotlivým instancím webovou aplikaci z nejbližšího geograficky replikované úložiště.

V tomto kurzu první část v řadě tří částí:

> [!div class="checklist"]
> * Vytvořit kontejner Azure geograficky replikované registru
> * Klonování zdrojovému kódu aplikace z webu GitHub
> * Sestavení Docker kontejneru bitové kopie ze zdroje aplikace
> * Nabízená bitovou kopii kontejneru v registru

V následujících kurzech můžete nasadit kontejner z vaší privátní registru pro webové aplikace spuštěna v dvou oblastech Azure. Pak aktualizujte kód v aplikaci a aktualizovat obě instance webové aplikace s jedním `docker push` do registru.

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte Azure CLI verze 2.0.20 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).

V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru. V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem).

K dokončení tohoto kurzu potřebujete vývojové prostředí pro Docker. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Prostředí Azure Cloud neobsahuje součásti Docker nutné pro dokončení každý krok v tomto kurzu. Proto doporučujeme místní instalace rozhraní příkazového řádku Azure a nástrojem Docker vývojového prostředí.

> [!IMPORTANT]
> Geografická replikace funkce registru kontejner Azure je aktuálně v **preview**. Verze Preview jsou k dispozici pro vás, za předpokladu, že souhlasíte se [dodatečné podmínky použití](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Některé aspekty této funkce může změnit před obecné dostupnosti (GA).
>

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

Přihlaste se k webu [Azure Portal](http://portal.azure.com).

Vyberte **nové** > **kontejnery** > **kontejner Azure registru**.

![Vytváření kontejneru registru na portálu Azure][tut-portal-01]

Vaše nové registru nakonfigurujte následující nastavení:

* **Název registru**: Vytvořte název registru, který je globálně jedinečný v rámci Azure a obsahuje alfanumerické znaky 5 – 50
* **Skupina prostředků**: **vytvořit nové** > `myResourceGroup`
* **Umístění**:`West US`
* **Uživatel s oprávněními správce**: `Enable` (povinné pro webovou aplikaci pro kontejnery bitové kopie pro vyžádání obsahu)
* **Skladová položka**: `Premium` (požadováno pro geografická replikace)

Vyberte **vytvořit** nasazení ACR instance.

![Vytváření kontejneru registru na portálu Azure][tut-portal-02]

Po celý zbytek v tomto kurzu používáme `<acrName>` jako zástupný symbol pro kontejner **název registru** který jste si zvolili.

> [!TIP]
> Protože registrech kontejner Azure jsou obvykle dlohotrvající prostředky, které se používají ve víc hostitelích kontejneru, doporučujeme vytvořit v registru v vlastní skupině prostředků. Při konfiguraci geograficky replikované registrech a pomocí webhooků, tyto další prostředky jsou umístěny ve stejné skupině prostředků.
>

## <a name="configure-geo-replication"></a>Konfigurace geografické replikace

Teď, když máte registru Premium, můžete nakonfigurovat geografická replikace. Webové aplikace, které nakonfigurujete v dalším kurzu ke spuštění ve dvou oblastí, potom můžete načítat jeho kontejneru bitové kopie z nejbližší registru.

Přejděte do vaší nové registru kontejneru ve službě Azure portálu a vyberte možnost **replikace** pod **služby**:

![Replikace v registru kontejner Azure portálu uživatelského rozhraní][tut-portal-03]

Zobrazí mapu zobrazující zelená Šestiúhelníky představující oblastí Azure k dispozici pro geografická replikace:

 ![Oblasti map na portálu Azure][tut-map-01]

Replikovat vaše registru pro oblast východní USA výběrem jeho zelená šestiúhelník a pak vyberte **vytvořit** pod **vytvoření replikace**:

 ![Vytvoření replikace uživatelského rozhraní na portálu Azure][tut-portal-04]

Po dokončení replikace na portálu odráží *připraven* pro obě oblasti. Použití **aktualizovat** tlačítko Aktualizovat stav replikace; může trvat několik minut nebo tak pro replik vytvořena a synchronizována.

![Stav replikace uživatelského rozhraní na portálu Azure][tut-portal-05]

## <a name="container-registry-login"></a>Kontejner registru přihlášení

Nyní, když jste nakonfigurovali geografická replikace, vytvořit bitovou kopii kontejneru a poslat ho přímo v registru. Musíte nejprve se přihlásit k vaší instanci ACR před odesláním bitové kopie do ní. S [Basic, Standard a Premium SKU](container-registry-skus.md), můžete ověřovat pomocí Azure identity.

Použití [az acr přihlášení](https://docs.microsoft.com/en-us/cli/azure/acr#az_acr_login) příkaz k ověřování a přihlašovací údaje pro vaše registru do mezipaměti. Nahraďte `<acrName>` s názvem registru, který jste vytvořili v předchozích krocích.

```azurecli
az acr login --name <acrName>
```

Příkaz vrátí `Login Succeeded` při dokončení.

## <a name="get-application-code"></a>Získání kódu aplikace

Ukázka v tomto kurzu obsahuje malé webové aplikace vytvořené s [ASP.NET Core](http://dot.net). Aplikace slouží stránky HTML, která zobrazuje oblast, ze kterého byla nasazená bitovou kopii pomocí registru kontejner Azure.

![Ukázková aplikace zobrazená v prohlížeči][tut-app-01]

Stáhněte ukázku do místního adresáře pomocí git a `cd` do adresáře:

```bash
git clone https://github.com/Azure-Samples/acr-helloworld.git
cd acr-helloworld
```

## <a name="update-dockerfile"></a>Aktualizovat soubor Docker

Soubor Docker zahrnuté v ukázce ukazuje, jak je integrovaná kontejneru. Spuštění z oficiální [aspnetcore](https://store.docker.com/community/images/microsoft/aspnetcore) bitovou kopii, kopie aplikace soubory do kontejneru, nainstaluje závislosti, budou kompilovány výstup pomocí oficiální [aspnetcore sestavení](https://store.docker.com/community/images/microsoft/aspnetcore-build) bitovou kopii a v neposlední řadě Vytvoří bitovou kopii optimalizované aspnetcore.

Soubor Docker se nachází v `./AcrHelloworld/Dockerfile` ve zdroji klonovaný.

```dockerfile
FROM microsoft/aspnetcore:2.0 AS base
# Update <acrName> with the name of your registry
# Example: uniqueregistryname.azurecr.io
ENV DOCKER_REGISTRY <acrName>.azurecr.io
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.sln ./
COPY AcrHelloworld/AcrHelloworld.csproj AcrHelloworld/
RUN dotnet restore
COPY . .
WORKDIR /src/AcrHelloworld
RUN dotnet build -c Release -o /app

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS production
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "AcrHelloworld.dll"]
```

Aplikace *acr helloworld* bitové kopie se pokusí určit oblasti, ze kterého byla nasazená jejímu kontejneru pomocí dotazů DNS na informace o serveru přihlášení v registru. Musíte zadat adresu URL serveru přihlášení v registru v `DOCKER_REGISTRY` proměnné prostředí v soubor Docker.

Nejdřív získat v registru přihlašovací adresa URL serveru s `az acr show` příkaz. Nahraďte `<acrName>` s názvem registru, který jste vytvořili v předchozích krocích.

```azurecli
az acr show --name <acrName> --query "{acrLoginServer:loginServer}" --output table
```

Výstup:

```bash
AcrLoginServer
-----------------------------
uniqueregistryname.azurecr.io
```

Potom aktualizujte `DOCKER_REGISTRY` řádku s adresou URL serveru přihlášení v registru. V tomto příkladu budeme aktualizovat řádek tak, aby odrážela registru náš příklad názvu *uniqueregistryname*:

```dockerfile
ENV DOCKER_REGISTRY uniqueregistryname.azurecr.io
```

## <a name="build-container-image"></a>Vytvoření kontejneru image

Teď, když aktualizujete soubor Docker s adresou URL registru systému Windows můžete použít `docker build` vytvořit bitovou kopii kontejneru. Spusťte následující příkaz k vytvoření image a značky s adresou URL vaší privátní registru, znovu nahraďte `<acrName>` s názvem v registru:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

Víceřádkový výstupu se zobrazí jako bitovou kopii Docker vychází (tady zobrazené zkrácený):

```bash
Sending build context to Docker daemon  523.8kB
Step 1/18 : FROM microsoft/aspnetcore:2.0 AS base
2.0: Pulling from microsoft/aspnetcore
3e17c6eae66c: Pulling fs layer
...
Step 18/18 : ENTRYPOINT dotnet AcrHelloworld.dll
 ---> Running in 6906d98c47a1
 ---> c9ca1763cfb1
Removing intermediate container 6906d98c47a1
Successfully built c9ca1763cfb1
Successfully tagged uniqueregistryname.azurecr.io/acr-helloworld:v1
```

Použití `docker images` příkazu zobrazte vytvořené bitové kopie:

```bash
docker images
```

Výstup:

```bash
REPOSITORY                                      TAG    IMAGE ID        CREATED               SIZE
uniqueregistryname.azurecr.io/acr-helloworld    v1     01ac48d5c8cf    About a minute ago    284MB
...
```

## <a name="push-image-to-azure-container-registry"></a>Push bitové kopie do registru kontejner Azure

Nakonec použijte `docker push` příkaz tak, aby nabízel *acr helloworld* bitové kopie do registru. Nahraďte `<acrName>` s názvem registru systému Windows.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Vzhledem k tomu, že jste nakonfigurovali v registru pro geografická replikace, image se automaticky replikují do obou *západní USA* a *východní USA* oblasti tento jedné `docker push` příkaz.

Výstup:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
cd54739c444b: Pushed
d6803756744a: Pushed
b7b1f3a15779: Pushed
a89567dff12d: Pushed
59c7b561ff56: Pushed
9a2f9413d9e4: Pushed
a75caa09eb1f: Pushed
v1: digest: sha256:0799014f91384bda5b87591170b1242bcd719f07a03d1f9a1ddbae72b3543970 size: 1792
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili kontejner privátní, geograficky replikované registru, vytvořené bitové kopie kontejner a pak instaluje této bitové kopie do registru. Podle kroků v tomto kurzu jste:

> [!div class="checklist"]
> * Vytvořit kontejner Azure geograficky replikované registru
> * Klonovaná aplikace zdrojového kódu z Githubu
> * Vytvořené bitové kopie kontejner Docker ze zdroje aplikace
> * Nainstaluje bitovou kopii kontejneru do registru

Přechodu na další kurzu se dozvíte o nasazení více webových aplikací pro kontejnery instancí pomocí geografická replikace k obsluze místně bitové kopie vašeho kontejneru.

> [!div class="nextstepaction"]
> [Nasazení webové aplikace z registru kontejner Azure](container-registry-tutorial-deploy-app.md)

<!-- IMAGES -->
[tut-portal-01]: ./media/container-registry-tutorial-prepare-registry/tut-portal-01.png
[tut-portal-02]: ./media/container-registry-tutorial-prepare-registry/tut-portal-02.png
[tut-portal-03]: ./media/container-registry-tutorial-prepare-registry/tut-portal-03.png
[tut-portal-04]: ./media/container-registry-tutorial-prepare-registry/tut-portal-04.png
[tut-portal-05]: ./media/container-registry-tutorial-prepare-registry/tut-portal-05.png
[tut-app-01]: ./media/container-registry-tutorial-prepare-registry/tut-app-01.png
[tut-map-01]: ./media/container-registry-tutorial-prepare-registry/tut-map-01.png
