---
title: "Vytvoření pravidla na základě cesty pro službu application gateway - portálu Azure | Microsoft Docs"
description: "Naučte se vytvářet pravidla na základě cesty pro aplikační bránu pomocí portálu Azure."
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.openlocfilehash: b207e7e7bd83e56db68288190c7bedafa8b5b7fa
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-azure-portal"></a>Vytvoření pravidla na základě cesty pro aplikační bránu pomocí portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Pomocí adresy URL na základě cestu směrování, přidružíte tras na základě cesty adresy URL požadavků HTTP. Zkontroluje, zda existuje trasa do fondu back-end serverů pro adresu URL uvedené v bráně aplikace nakonfigurovaná, a pak pošle síťový provoz do fondu definované. Běžně používá pro směrování adres URL na základě cesty je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.

Application Gateway mít dva typy pravidel: basic a pravidla na základě cesty adresy URL. Typ základní pravidlo poskytuje kruhového dotazování služby pro back endové fondy. Pravidla na základě cesty, kromě distribučních kruhového dotazování vzorek cesty adresy URL žádosti také použít při výběru vhodné fond back-end.

## <a name="scenario"></a>Scénář

V následujícím scénáři vytvoří pravidlo založené na cestu v existující aplikační brány.
Tento scénář předpokládá, že jste již provedli kroky v [vytvoření služby application gateway pomocí portálu](application-gateway-create-gateway-portal.md).

![Adresa URL trasy][scenario]

## <a name="createrule"></a>Vytvoření pravidla na základě cesty

Pravidla na základě cesty vyžaduje vlastní naslouchací proces. Než vytvoříte pravidlo, ujistěte se, že jste ověřte, zda jsou k dispozici naslouchací proces používat.

### <a name="step-1"></a>Krok 1

Přejděte na [portál Azure](http://portal.azure.com) a vyberte existující aplikační brány. Klikněte na tlačítko **pravidla**.

![Přehled služby Application Gateway][1]

### <a name="step-2"></a>Krok 2

Klikněte **na základě cesty** tlačítko Přidat nové pravidlo na základě cesty.

### <a name="step-3"></a>Krok 3

**Přidat na základě cesty pravidlo** okno obsahuje dvě části. V první části je, kde jste definovali naslouchací proces, název pravidla a nastavení výchozí cesty. Výchozí cesta nastavení jsou pro trasy, které nejsou spadají pod vlastní trasy na základě cesty. Druhá část **přidat na základě cesty pravidlo** je okno, kde můžete definovat pravidla na základě cesty sami.

**Základní nastavení**

* **Název**: popisný název pravidla, která je přístupná na portálu.
* **Naslouchací proces**: naslouchací proces, který se používá pro pravidlo.
* **Výchozí fond back-end**: back-end má být použit pro výchozí pravidlo.
* **Výchozí nastavení HTTP**: nastavení protokolu HTTP, který se má použít pro výchozí pravidlo.

**Nastavení pravidla na základě cesty**

* **Název**: popisný název pravidla na základě cesty.
* **Cesty**: cesta toto pravidlo vyhledá při předávání provozu.
* **Fond back-end**: back-end má být použit pro pravidlo.
* **Nastavení HTTP**: nastavení protokolu HTTP, který se má použít pro pravidlo.

> [!IMPORTANT]
> **Cesty** nastavení je seznam vzorů cesta tak, aby odpovídaly. Každý vzor musí začínat lomítkem a hvězdičkou je povoleno pouze na konci. Platný příklady: /xyz, /xyz*a /xyz/*.  

![Přidat pravidlo na základě cesty okno s informacemi o doplnit][2]

Přidávání pravidla na základě cestu k existující aplikační brány je jednoduchý proces prostřednictvím portálu Azure. Po vytvoření pravidla na základě cesty, můžete upravit tak, aby obsahovala dalších pravidlech. 

![Přidat další pravidla, na základě cesty][3]

Tento krok nakonfiguruje trasu na základě cesty. Je důležité si uvědomit, že požadavky nejsou přepsán. Jako požadavky mohou, aplikační bránu zkontroluje žádosti a, na základě vzoru pro adresu URL, odešle požadavek na příslušný fond back-end.

## <a name="next-steps"></a>Další kroky

Další postup konfigurace s Azure Application Gateway snižování zátěže protokolu SSL najdete v tématu [konfigurace aplikační brány pro přesměrování zpracování SSL pomocí portálu Azure](application-gateway-ssl-portal.md).

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
