---
title: "Najít trasa služeb na základě umístění Azure | Microsoft Docs"
description: "Směrovat do bodu zájmu pomocí služeb na základě umístění Azure"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 0f784e8ecd8fc94c12df1a819055718e06547b6b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Směrovat do bodu zájmu pomocí služeb na základě umístění Azure

Tento kurz ukazuje způsob použití vašeho účtu služeb na základě umístění Azure a sadu SDK služby trasy k vyhledání trasy, která má body. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Získat adresu souřadnice
> * Dotaz na službu trasy pro pokynů k bodu zájmu

## <a name="prerequisites"></a>Požadavky

Než budete pokračovat, nezapomeňte [vytvoření účtu Azure zjišťování na základě polohy](./tutorial-search-location.md#createaccount), a [získat klíč předplatného pro váš účet](./tutorial-search-location.md#getkey). Může také sledovat, jak používat mapový ovládací prvek a rozhraní API pro vyhledávání služby, jak je popsáno v tomto kurzu [vyhledávání nedaleko bodu zájmu pomocí služeb na základě umístění Azure](./tutorial-search-location.md).


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Získat adresu souřadnice

Pomocí následujících kroků můžete vytvořit statické stránky HTML vložených s rozhraním API řízení na základě polohy mapy. 

1. Na místním počítači, vytvořte nový soubor s názvem **MapRoute.html**. 
2. Přidejte následující součásti HTML k souboru:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #map {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body>
        <div id="map"></div>
        <script>
        // Embed Map Control JavaScript code here
        </script>

    </body>

    </html>
    ```
    Všimněte si, jak hlavičku HTML vloží umístění prostředků pro souborů CSS a JavaScript pro knihovnu služeb na základě umístění Azure. Všimněte si také *skriptu* segmentu v těle souboru HTML, která bude obsahovat vložený kód jazyka JavaScript pro přístup k rozhraní API Azure umístění na základě služby.

3. Přidejte následující kód JavaScript, který *skriptu* bloku souboru HTML. Nahraďte zástupný symbol *< klávesy insert >* s primární klíč účtu na základě polohy.

    ```HTML
            // Instantiate map to the div with id "map"
            var subscriptionKey = "<insert-key>";
            var map = new atlas.Map("map", {
                "subscription-key": subscriptionKey
            });

    ```
    **Atlas. Mapa** poskytuje ovládací prvek pro mapu visual a interaktivní webové, a je součástí rozhraní API služby Azure mapy ovládacího prvku.

4. Přidejte následující kód JavaScript, který *skriptu* bloku. To přidává další vrstvu *linestrings* pro mapový ovládací prvek zobrazíte trasy:

    ```HTML
            // Initialize the linestring layer for routes on the map
            var routeLinesLayerName = "routes";
            map.addLinestrings([], {
                name: routeLinesLayerName,
                color: "#2272B9",
                width: 5,
                cap: "round",
                join: "round",
                before: "labels"
            });
    ```

5. Přidejte následující kód v JavaScriptu vytvořit počáteční a koncové body pro trasy:

    ```HTML
            // Create the GeoJSON objects which represent the start and end point of the route
            var startPoint = new atlas.data.Point([-122.130137, 47.644702]);
            var startPin = new atlas.data.Feature(startPoint, {
                title: "Microsoft",
                icon: "pin-round-blue"
            });

            var destinationPoint = new atlas.data.Point([-122.3352, 47.61397]);
            var destinationPin = new atlas.data.Feature(destinationPoint, {
                title: "Contoso Oil & Gas",
                icon: "pin-blue"
            });
    ```
    Tento kód vytvoří dvě [GeoJSON objekty](https://en.wikipedia.org/wiki/GeoJSON) představují počáteční a koncový bod trasy. Koncový bod je kombinací zeměpisnou šířku a délku pro jednu ze *benzinového stanice* vyhledávat v předchozí kurzu [vyhledávání nedaleko bodu zájmu pomocí služeb na základě umístění Azure](./tutorial-search-location.md).

6. Přidejte následující kód v JavaScriptu pro přidání do mapy PIN kódy pro počáteční a koncové body:

    ```HTML
            // Fit the map window to the bounding box defined by the start and destination points
            var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
            var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
            var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
            var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
            map.setCameraBounds({
                bounds: [swLon, swLat, neLon, neLat],
                padding: 50
            });

            // Add pins to the map for the start and end point of the route
            map.addPins([startPin, destinationPin], {
                name: "route-pins",
                textFont: "SegoeUi-Regular",
                textOffset: [0, -20]
            });
    ``` 
    Rozhraní API **map.setCameraBounds** upraví okna mapa podle souřadnice počátečního a koncového bodu. Rozhraní API **map.addPins** přidá body mapový ovládací prvek jako vizuální součásti.

7. Uložit **MapRoute.html** soubor na vašem počítači. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>Dotaz na službu trasy pro pokynů k bodu zájmu

V této části ukazuje, jak používat rozhraní API služby Azure na základě polohy trasy k vyhledání trasy z dané počáteční bod do cílového umístění. Služba směrování poskytuje rozhraní API pro plánování nejrychlejších, nejkratší nebo úsporném trasy mezi dvěma umístěními, vzhledem k tomu, podmínky přenos v reálném čase. Umožňuje také uživatelům plánování trasy v budoucnu pomocí databáze rozsáhlé historické provozu Azure a odhad doby trvání trasy pro žádné datum a čas. 

1. Otevřete **MapRoute.html** soubor vytvořili v předchozí části a přidejte následující kód JavaScript, který *skriptu* bloku pro ilustraci službu směrování.

    ```HTML
            // Perform a request to the route service and draw the resulting route on the map
            var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function () {
                if (this.readyState == 4 && this.status == 200) {
                    var response = JSON.parse(xhttp.responseText);

                    var route = response.routes[0];
                    var routeCoordinates = [];
                    for (var leg of route.legs) {
                        var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                        routeCoordinates = routeCoordinates.concat(legCoordinates);
                    }

                    var routeLinestring = new atlas.data.LineString(routeCoordinates);
                    map.addLinestrings([new atlas.data.Feature(routeLinestring)], { name: routeLinesLayerName });
                }
            };
    ```
    Vytvoří tento fragment kódu [XMLHttpRequest](https://xhr.spec.whatwg.org/), a přidá obslužnou rutinu události analyzovat příchozí odpověď. Pro úspěšné odpovědi vytvoří se pole souřadnice úsečky první trasy, která vrátila. Potom přidá této sady souřadnic pro tuto trasu na mapu *linestrings* vrstvy.

2. Přidejte následující kód, který *skriptu* blok, k odeslání XMLHttpRequest službě trasy Azure na základě polohy.:

    ```HTML
            var url = "https://atlas.microsoft.com/route/directions/json?";
            url += "&api-version=1.0";
            url += "&subscription-key=" + subscriptionKey;
            url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
                destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];
    
            xhttp.open("GET", url, true);
            xhttp.send();
    ```
    Výše uvedený požadavek ukazuje požadované parametry, které jsou klíč předplatného vašeho účtu a souřadnic pro počáteční a koncový bod, v uvedeném pořadí. 

3. Uložit **MapRoute.html** soubor místně, potom otevřete ve webovém prohlížeči podle svého výběru a sledovat výsledek. Pro úspěšné připojení na základě polohy rozhraní API měli byste vidět mapu podobný následujícímu. 

    ![Azure mapový ovládací prvek a služba Směrování](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Získat adresu souřadnice
> * Dotaz na službu trasy pro pokynů k bodu zájmu

Pokračujte v kurzu [najít trasy pro různé režimy cesta pomocí služeb na základě umístění Azure](./tutorial-prioritized-routes.md) Další informace o použití služeb na základě umístění Azure k určení priority trasy k bodu zájmu, aby na základě režim přenosu. 
