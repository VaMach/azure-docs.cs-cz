---
title: "Konfigurovat přesměrování zpracování SSL - Azure Application Gateway - portálu Azure | Microsoft Docs"
description: "Tento článek poskytuje pokyny pro vytvoření služby application gateway pomocí protokolu SSL, přesměrování zpracování úloh pomocí portálu Azure"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 2f7f5d4132e28c8c192d90d5f4bfb2a9034f8b8c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-azure-portal"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí portálu Azure

> [!div class="op_single_selector"]
> * [portál Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě. Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.

## <a name="scenario"></a>Scénář

Následující scénář vás provede konfigurací přesměrování zpracování SSL na existující aplikační brány. Tento scénář předpokládá, že jste již provedli postup [vytvoření služby application gateway](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Než začnete

Pokud chcete konfigurovat přesměrování zpracování SSL pomocí služby application gateway, je vyžadován certifikát. Tento certifikát je načteno ve službě application gateway a slouží k šifrování a dešifrování přenosů odeslána prostřednictvím protokolu SSL. Certifikát musí být ve formátu Personal Information Exchange (.pfx). Tento formát souboru umožňuje exportovat privátní klíč, který vyžaduje službu application gateway slouží k šifrování a dešifrování přenosů.

## <a name="add-an-https-listener"></a>Přidejte naslouchací proces HTTPS

Naslouchací proces HTTPS hledá provozu na základě jeho konfigurace a pomáhá směrovat přenosy back endové fondy. Pokud chcete přidat naslouchací proces protokolu HTTPS, postupujte takto:

   1. Přejděte na portál Azure a vyberte existující aplikační brány.

   2. Vyberte **naslouchací procesy**a pak vyberte **přidat** tlačítko přidejte naslouchací proces.

   ![Přehled brány podokno aplikace][1]


   3. Vyplňte následující požadované informace pro naslouchací proces a nahrání certifikátu .pfx:
      - **Název**: popisný název naslouchacího procesu.

      - **Konfigurace IP front-endu**: konfiguraci front-end IP adresy, který se používá pro naslouchací proces.

      - **Front-endový port (název/Port)**: popisný název portu, který používá front-endu služby application gateway a skutečný port, který se používá.

      - **Protokol**: přepínač k určení, pokud se používá protokol HTTPS nebo HTTP pro front-endu.

      - **Certifikát (jméno a heslo)**: přesměrování zpracování SSL Pokud se používá, je vyžadován pro toto nastavení certifikát .pfx. Vyžadují se i popisný název a heslo.

   4. Vyberte **OK**.

![Přidání podokna naslouchací proces][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Vytvoření pravidla a přidružit ho k naslouchacímu procesu

Byla vytvořena naslouchací proces. Dále vytvořte pravidlo pro zpracování provozu z naslouchací proces. Pravidla určují, jak se provoz se směruje na back endové fondy, na základě více nastavení konfigurace. Tato nastavení zahrnují protokol, port a sondy stavu, a jestli spřažení na základě souboru cookie relace je použití. Pokud chcete vytvořit a přidružit pravidlo pro naslouchací proces, postupujte takto:


   1. Vyberte **pravidla** aplikační bránu, a potom vyberte **přidat**.

   ![Podokno pravidel brány aplikace][3]


   2. V části **přidat základní pravidlo**, zadejte popisný název pro pravidlo v **název** pole a pak vyberte **naslouchací proces** vytvořili v předchozím kroku. Vyberte odpovídající **fond back-end** a **nastavení HTTP**a potom vyberte **OK**.

   ![Okno nastavení protokolu HTTPS][4]

Nastavení jsou nyní uloženy do služby application gateway. Proces ukládání pro toto nastavení může chvíli trvat, než jsou k dispozici k zobrazení prostřednictvím portálu nebo pomocí prostředí PowerShell. Po uložení, služby application gateway zpracovává šifrování a dešifrování přenosů. Prostřednictvím protokolu HTTP se budou zpracovávat všechny přenosy mezi aplikační bránu a webovými back-end servery. Veškeré komunikace zpět do klienta, pokud iniciované přes protokol HTTPS, bude vrácen do klienta zašifrovaná.

## <a name="next-steps"></a>Další kroky

Pokud chcete dozvědět, jak nakonfigurovat vlastní stav testu s Azure Application Gateway, přečtěte si téma [vytvořit sondu vlastní stavu](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png

