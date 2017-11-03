---
title: "Vytvoření služby application gateway pomocí pravidel směrování adres URL - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka obsahuje pokyny o tom, jak vytvořit a konfigurovat služby application gateway pomocí pravidel směrování adres URL."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 10d01d5d80e2d111d6b39598eed3612f80162b23
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-by-using-path-based-routing-with-azure-cli-20"></a>Vytvoření služby application gateway pomocí na základě cestu směrování s Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Pomocí adresy URL na základě cestu směrování, přidružíte tras na základě cesty adresy URL požadavků HTTP. Zkontroluje, zda existuje trasa do fondu back-end serverů pro adresu URL uvedené v bráně aplikace nakonfigurovaná, a pak pošle síťový provoz do fondu definované. Běžně používá pro směrování adres URL na základě cesty je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.

Služba Azure Application Gateway používá dva typy pravidel: basic a pravidla na základě cesty adresy URL. Typ základní pravidlo poskytuje kruhového dotazování služby pro back endové fondy. Pravidla na základě cesty, kromě distribučních kruhového dotazování, také pomocí vzorek cesty adresy URL žádosti vyberte odpovídající fond back-end.

## <a name="scenario"></a>Scénář

V následujícím příkladu, služby application gateway slouží provoz pro doménu contoso.com s dvěma fondy back-end serverů: výchozí fond serverů a fond bitové kopie serveru.

Požadavky pro http://contoso.com/image * jsou směrovány do fondu serverů bitové kopie (**imagesBackendPool**). Pokud se neshoduje se vzorek cesty, služby application gateway vybere výchozí fond serveru (**appGatewayBackendPool**).

![Adresa URL trasy](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Otevřete **Microsoft Azure příkazového řádku** a přihlaste se:

```azurecli
az login -u "username"
```

> [!NOTE]
> Můžete také použít `az login` bez přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.

Po zadání předchozí příkaz obdržíte kód. Přejděte na https://aka.ms/devicelogin v prohlížeči pokračovat v procesu přihlášení.

![cmd zobrazující zařízení přihlášení][1]

V prohlížeči zadejte kód, který jste dostali. Přesměruje na přihlašovací stránku.

![prohlížeče k zadání kódu][2]

Zadejte kód pro přihlášení a pak zavřete prohlížeč pokračovat.

![Úspěšné přihlášení][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Přidat pravidlo založené na cestu k existující aplikační brány

Následující kroky ukazují, jak přidat pravidlo založené na cestu k existující aplikační brány.
### <a name="create-a-new-back-end-pool"></a>Vytvoření nového fondu back-end

Konfigurace nastavení brány aplikace **imagesBackendPool** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end. V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro nový fond back-end. Každý fond back-end může mít svůj vlastní nastavení. Na základě cesty pravidla používají nastavení HTTP back-end přesměrovat provoz na členy správné fond back-end. Určuje protokol a port, který se používá při odesílání provozu na členy fondu back-end. Nastavení HTTP back-end taky určit, na základě souboru cookie relace.  Pokud je povoleno, spřažení relace na základě souborů cookie zasílá provozu jako předchozí požadavky jednotlivých paketů na stejnou back-end.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port-for-an-application-gateway"></a>Vytvořit nový front-end port pro službu application gateway

Objekt konfigurace front-end port se používá ve naslouchací proces k definování jaké portu brány aplikace nenaslouchala komunikaci na naslouchací proces.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Vytvořte nový

V tomto kroku je nakonfigurován naslouchací proces pro veřejnou IP adresu a port používaný pro příjem příchozího síťového provozu. Následující příklad trvá dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https, které jsou velká a malá písmena) a nakonfiguruje naslouchací proces. V tomto příkladu naslouchá naslouchací proces HTTP přenosy na portu 82 na veřejnou IP adresu v tomto scénáři vytvořili.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>Vytvořit mapování cesty adresy URL

Tento krok nakonfiguruje relativní cesty URL používá k definování mapování mezi cestu a fond back-end přiřazen zpracovávat příchozí provoz služby application gateway.

> [!IMPORTANT]
> Každá cesta musí začínat znakem "/", a bude jediným místem je povoleno hvězdičku je na konci. Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *. Řetězec dodáni do objekt přiřazení vzorce cesta nezahrnuje jakýkoli text po první "?" nebo "#" a tyto znaky nejsou povoleny. 

Následující příklad vytvoří jedno pravidlo pro/Image / * cesty, směrování provozu do back-end **imagesBackendPool**. Toto pravidlo zajišťuje, že přenosy dat pro každou sadu adresy URL se směruje na back-end. Například http://adatum.com/images/figure1.jpg přejde k **imagesBackendPool**. Pokud cesta neodpovídá žádné z pravidel předem definovaná cesta, nakonfiguruje konfiguraci pravidla cesty mapy taky výchozího fondu adres back-end. Například http://adatum.com/shoppingcart/test.html přejde k **pool1** protože je definována jako výchozí fond pro neodpovídající provoz.

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a>Další kroky

Pokud chcete další informace o přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
