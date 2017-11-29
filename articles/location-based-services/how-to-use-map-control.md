---
title: "Jak používat Azure umístění na základě služby mapový ovládací prvek | Microsoft Docs"
description: "Další informace o použití knihovny Javascript klienta řízení mapy služeb na základě umístění Azure."
services: location-based-services
keywords: "Nemáte přidat nebo upravit klíčová slova bez konzultace ohledně specialistou na SEO."
author: philmea
ms.author: philmea
ms.date: 11/22/2017
ms.topic: how-to
ms.service: location-based-services
manager: timlt
ms.openlocfilehash: 9ddd11806accddb41c02c0c6681aac07bba25356
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="how-to-use-the-azure-location-based-services-map-control"></a>Jak používat Azure umístění na základě služby mapový ovládací prvek
Knihovna Javascript na straně klienta mapový ovládací prvek umožňuje vykreslení mapy a vložené funkce služeb na základě umístění Azure do webové nebo mobilních aplikací. 

## <a name="prerequisites"></a>Požadavky
Na základě Azure polohy účet a předplatné klíčem. Informace o vytvoření účtu a načítání klíč předplatného najdete v tématu [Správa vašeho účtu služeb na základě umístění Azure a klíče](how-to-manage-account-keys.md). 

## <a name="create-a-new-map-in-a-web-page-using-the-map-control-api"></a>Vytvořit nové mapování na webové stránce pomocí rozhraní API mapy ovládacího prvku
Mapu můžete vložit na webové stránce pomocí knihovny Javascript klienta mapový ovládací prvek.

1. Vytvořte nový soubor s názvem MapSearch.html.

2. Přidejte odkazy na základě Azure polohy šablony stylů a skript zdroje do `<head>` element souboru:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. K vykreslení nové mapování v prohlížeči, přidejte **#map** odkaz v `<style>` elementu.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Chcete-li inicializovat mapový ovládací prvek, zadejte novou část v těle html a vytvořit skript. Použijte svůj vlastní klíč předplatného z vašeho účtu služeb na základě umístění Azure. 

    ```html
    <div id="map">
        <script>
            var subscriptionKey = "<_subscriptionKey_>";
            var map = new atlas.Map("map", {
                "subscription-key": subscriptionKey,
                center: [47.59093,-122.33263],
                zoom: 12
            });
        <script>
    <div>
    ```
    
5. Otevřete soubor ve webovém prohlížeči a zobrazit vykreslené mapy.