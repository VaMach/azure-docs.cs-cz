---
title: "Více než jedna služeb na základě umístění Azure | Microsoft Docs"
description: "Najít trasy pro různé režimy cesta pomocí služeb na základě umístění Azure"
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
ms.openlocfilehash: 19cf9da839d9d3a1ec78c8d1f6994628684f4e31
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Najít trasy pro různé režimy cesta pomocí služeb na základě umístění Azure

Tento kurz ukazuje, jak najít trasy, která má body, nastavovat ve vašem režimu cesta pomocí vašeho účtu služeb na základě umístění Azure a sadu SDK služby směrování. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Konfigurace služby postupu dotazu
> * Vykreslení trasy nastavovat pomocí režimu cesta

## <a name="prerequisites"></a>Požadavky

Než budete pokračovat, nezapomeňte [vytvoření účtu Azure zjišťování na základě polohy](./tutorial-search-location.md#createaccount), a [získat klíč předplatného pro váš účet](./tutorial-search-location.md#getkey). Může také sledovat, jak používat mapový ovládací prvek a rozhraní API pro vyhledávání služby, jak je popsáno v tomto kurzu [vyhledávání nedaleko bodu zájmu pomocí služeb na základě umístění Azure](./tutorial-search-location.md), stejně jako další základní použití rozhraní API služby trasy jako popsané v tomto kurzu [trasy, která má bod týkající se použití služeb na základě umístění Azure](./tutorial-route-location.md).


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Konfigurace služby postupu dotazu

Pomocí následujících kroků můžete vytvořit statické stránky HTML vložených s rozhraním API řízení na základě polohy mapy. 

1. Na místním počítači, vytvořte nový soubor s názvem **MapTruckRoute.html**. 
2. Přidejte následující součásti HTML k souboru:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
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
    Všimněte si, že hlavičku HTML vloží umístění prostředků pro souborů CSS a JavaScript pro knihovnu služeb na základě umístění Azure. Všimněte si také *skriptu* segmentu přidat k tělu HTML, tak, aby obsahovala vloženého kódu jazyka JavaScript pro přístup k rozhraní API služby Azure mapy ovládacího prvku.
3. Přidejte následující kód JavaScript, který *skriptu* bloku souboru HTML. Nahraďte zástupný symbol *< klávesy insert >* s primární klíč účtu na základě polohy.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var subscriptionKey = "<insert-key>";
    var map = new atlas.Map("map", {
        "subscription-key": subscriptionKey
    });
    ```
    **Atlas. Mapa** poskytuje ovládací prvek pro mapu visual a interaktivní webové, a je součástí rozhraní API služby Azure mapy ovládacího prvku.

4. Přidejte následující kód JavaScript, který *skriptu* bloku, přidat zobrazení tok přenosů do mapy:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Tento kód nastaví tok přenosů na `relative`, což je rychlosti silniční relativně k bezplatným toku. Můžete ho také nastavit na `absolute` rychlosti silniční, nebo `relative-delay` zobrazuje relativní rychlosti Pokud se liší od volného toku. 

5. Přidejte následující kód JavaScript, k vytvoření kódů PIN pro počáteční a koncový bod trasy:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.356099, 47.580045]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Fabrikam, Inc.",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Microsoft",
        icon: "pin-blue"
    });
    ```
    Tento kód vytvoří dvě [GeoJSON objekty](https://en.wikipedia.org/wiki/GeoJSON) představují počáteční a koncový bod trasy. 

6. Přidejte následující kód v JavaScriptu přidat vrstev *linestrings* pro mapový ovládací prvek zobrazíte tras na základě režimu přenosu, například _car_ a _vůz_.

    ```JavaScript
    // Place route layers on the map
    var carRouteLayerName = "car-route";
    map.addLinestrings([], {
        name: carRouteLayerName,
        color: "#B76DAB",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });

    var truckRouteLayerName = "truck-route";
    map.addLinestrings([], {
        name: truckRouteLayerName,
        color: "#2272B9",
        width: 9,
        cap: "round",
        join: "round",
        before: carRouteLayerName
    });
    ```

7. Přidejte následující kód v JavaScriptu přidat k mapy počáteční a koncové body:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 100
    });

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    Rozhraní API **map.setCameraBounds** upraví okna mapa podle souřadnice počátečního a koncového bodu. Rozhraní API **map.addPins** přidá body mapový ovládací prvek jako vizuální součásti.

8. Uložit **MapTruckRoute.html** soubor na vašem počítači. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Vykreslení trasy nastavovat pomocí režimu cesta

Tato část ukazuje způsob použití rozhraní API služby trasy Azure na základě polohy najít víc tras z dané počáteční přejděte na cíl, v závislosti na vašem režimu přenosu. Služba směrování poskytuje rozhraní API pro plánování nejrychlejších, nejkratší nebo úsporném trasy mezi dvěma umístěními, vzhledem k tomu, podmínky přenos v reálném čase. Umožňuje také uživatelům plánování trasy v budoucnu pomocí databáze rozsáhlé historické provozu Azure a odhad doby trvání trasy pro žádné datum a čas. 

1. Otevřete **MapTruckRoute.html** soubor vytvořili v předchozí části a přidejte následující kód JavaScript, který *skriptu* bloku, získejte trasy pro vůz, pomocí služby postupu.

    ```JavaScript
    // Perform a request to the route service and draw the resulting truck route on the map
    var xhttpTruck = new XMLHttpRequest();
    xhttpTruck.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: truckRouteLayerName
            });
        }
    };

    var truckRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    truckRouteUrl += "&api-version=1.0";
    truckRouteUrl += "&subscription-key=" + subscriptionKey;
    truckRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];
    truckRouteUrl += "&travelMode=truck";
    truckRouteUrl += "&vehicleWidth=2";
    truckRouteUrl += "&vehicleHeight=2";
    truckRouteUrl += "&vehicleLength=5";
    truckRouteUrl += "&vehicleLoadType=USHazmatClass2";

    xhttpTruck.open("GET", truckRouteUrl, true);
    xhttpTruck.send();
    ```
    Vytvoří tento fragment kódu [XMLHttpRequest](https://xhr.spec.whatwg.org/), a přidá obslužnou rutinu události analyzovat příchozí odpověď. Pro úspěšné odpovědi, vytvoří pole souřadnice trasy, která vrátí a přidá ho mapu `truckRouteLayerName` vrstvy. 
    
    Tento fragment kódu rovněž odesílá dotaz službě trasy, k získání trasy pro zadaný počáteční a koncový bod pro váš účet klíč předplatného. Následující volitelné parametry, které se používají k označení trasy pro velkou vůz:-Parametr `travelMode=truck` Určuje režim cesta jako *vůz*. Další režimy cesta podporována jsou *taxíkem*, *sběrnice*, *van*, *motorky*a ve výchozím nastavení *car* .  
        -Parametry `vehicleWidth`, `vehicleHeight`, a `vehicleLength` zadejte dimenze nástroj v měřidla a jsou považovány za pouze pokud je režim cesta *vůz*.  
        -Na `vehicleLoadType` klasifikuje nákladní jako nebezpečných a na některých cestách s omezeným přístupem. Je také aktuálně to jenom pro *vůz* režimu.  

2. Přidejte následující kód v JavaScriptu získat trasy pro automobilu pomocí služby trasy:

    ```JavaScript
    // Perform a request to the route service and draw the resulting car route on the map
    var xhttpCar = new XMLHttpRequest();
    xhttpCar.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: carRouteLayerName
            });
        }
    };

    var carRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    carRouteUrl += "&api-version=1.0";
    carRouteUrl += "&subscription-key=" + subscriptionKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Tento fragment kódu vytvoří jiné [XMLHttpRequest](https://xhr.spec.whatwg.org/), a přidá obslužnou rutinu události analyzovat příchozí odpověď. Pro úspěšné odpovědi, vytvoří pole souřadnice trasy, která vrátí a přidá ho mapu `carRouteLayerName` vrstvy. 
    
    Tento fragment kódu rovněž odesílá dotaz službě trasy, k získání trasy pro zadaný počáteční a koncový bod pro váš účet klíč předplatného. Vzhledem k tomu, že nejsou použity žádné další parametry, trasu pro výchozí režim cesta *car* je vrácen. 

3. Uložit **MapTruckRoute.html** soubor místně, potom otevřete ve webovém prohlížeči podle svého výběru a sledovat výsledek. Pro úspěšné připojení na základě polohy rozhraní API měli byste vidět mapu podobný následujícímu. 

    ![Seřazený podle priority trasy trasy službou Azure](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Všimněte si, že vůz trasy, která je v modrou barvu, zatímco trasy car je fialové.

## <a name="next-steps"></a>Další postup
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Konfigurace služby postupu dotazu
> * Vykreslení trasy nastavovat pomocí režimu cesta

Pokračujte **koncepty** a **postupy** články další SDK služby Azure umístění na základě služby podrobněji. 
