---
title: "Hledání se umístění Azure na základě služby | Microsoft Docs"
description: "Hledání nedaleko bodu zájmu pomocí služeb na základě umístění Azure"
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
ms.openlocfilehash: e033b1005902a9639fc352ffb9af91cb20875bee
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="search-nearby-point-of-interest-using-azure-location-based-services"></a>Hledání nedaleko bodu zájmu pomocí služeb na základě umístění Azure

Tento kurz ukazuje, jak nastavit účet s služeb na základě umístění Azure, a pak použít zadaný rozhraní API pro vyhledávání bodů zájmu. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořte účet s služeb na základě umístění Azure
> * Získat klíč předplatného pro váš účet
> * Vytvořit novou webovou stránku pomocí rozhraní API mapy ovládacího prvku
> * Použít službu vyhledávání k vyhledání blízkých bodu zájmu

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

# <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Přihlaste se k portálu [Azure Portal](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-location-based-services"></a>Vytvořte účet s služeb na základě umístění Azure

Postupujte podle těchto kroků můžete vytvořit nový účet na základě polohy.

1. V levém horním rohu webu [Azure Portal](https://portal.azure.com) klikněte na **Vytvořit prostředek**.
2. Do pole *Hledat na Marketplace* zadejte **location based services**.
3. Ve *výsledcích* klikněte na **Location Based Services (Preview)**. Klikněte na tlačítko **Vytvořit**, které se zobrazí pod mapou. 
4. Na **vytvořit účet služby služby umístění na základě** stránky, zadejte následující hodnoty:
    - *Název* svého nového účtu. 
    - *Předplatné* , kterou chcete použít pro tento účet.
    - *Skupiny prostředků* název pro tento účet. Můžete zvolit *vytvořit nový* nebo *použít existující* skupinu prostředků.
    - Vyberte *umístění skupiny prostředků*.
    - Pro čtení *podmínky* a zaškrtnutím políčka přijmout podmínky. 
    - Nakonec klikněte na **vytvořit** tlačítko.
   
    ![Vytvoření účtu Location Based Services na portálu](./media/tutorial-search-location/create-lbs-account.png)


<a id="getkey"></a>

## <a name="get-the-subscription-key-for-your-account"></a>Získat klíč předplatného pro váš účet

Po úspěšném vytvoření účtu na základě polohy, postupujte podle kroků jej připojit k jeho vyhledávání map rozhraní API:

1. Otevřete svůj účet služeb na základě umístění na portálu.
2. Přejděte na svůj účet **nastavení**a potom vyberte **klíče**.
3. Kopírování **primární klíč** do schránky. Uložte místně pro použití v krocích budete pokračovat. 

    ![Získání primární klíč v portálu](./media/tutorial-search-location/lbs-get-key.png)


<a id="createmap"></a>

## <a name="create-new-web-page-using-azure-map-control-api"></a>Vytvořit novou webovou stránku pomocí rozhraní API Azure mapy ovládacího prvku
Azure mapy ovládacího prvku rozhraní API je vhodné klientské knihovny, která umožňuje snadno integrovat služeb na základě umístění Azure do webové aplikace. Se skrývá složitosti úplné volání služby REST a zvyšuje produktivitu s styleable a přizpůsobitelných součásti. Následující kroky ukazují, jak vytvořit statické stránky HTML vložených s rozhraním API řízení na základě polohy mapy. 

1. Na místním počítači, vytvořte nový soubor s názvem **MapSearch.html**. 
2. Přidejte následující součásti HTML k souboru:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

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
    Všimněte si, že hlavičku HTML obsahuje hostované Azure mapový ovládací prvek knihovna prostředků souborů CSS a JavaScript. Poznámka: *skriptu* přidat do segmentu *textu* souboru HTML. Vložený kód jazyka JavaScript pro přístup k rozhraní API Azure umístění na základě služby budou obsahovat tento segment.
 
3.  Přidejte následující kód JavaScript, který *skriptu* bloku souboru HTML. Nahraďte zástupný symbol *< klávesy insert >* s primární klíč účtu na základě polohy. 

    ```JavaScript
    // Instantiate map to the div with id "map"
    var subscriptionKey = "<insert-key>";
    var map = new atlas.Map("map", {
        "subscription-key": subscriptionKey
    });
    ```
    Tento segment zahájí rozhraní API ovládacího prvku mapy pro svůj klíč předplatného. **Atlas** je obor názvů, který obsahuje rozhraní API služby Azure mapy ovládacího prvku a související vizuální součásti. **Atlas. Mapa** poskytuje ovládací prvek pro mapu visual a interaktivní web. Může sledovat, jak mapy vypadá tak, že otevřete stránku HTML v prohlížeči. 

4. Přidejte následující kód JavaScript, který *skriptu* bloku pro přidání vrstvy vyhledávání kódů PIN pro mapový ovládací prvek:

    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    map.addPins([], {
        name: searchLayerName,
        cluster: false,
        icon: "pin-round-darkblue"
    });
    ```

5. Uložte soubor na počítači. 


<a id="usesearch"></a>

## <a name="use-search-service-to-find-nearby-point-of-interest"></a>Použít službu vyhledávání k vyhledání blízkých bodu zájmu

Tato část ukazuje způsob použití rozhraní API služby Search Azure na základě polohy při hledání bodu zájmu na mapě. Je určený pro vývojáře k vyhledání adresy, vás zajímá a jiné zeměpisné údaje rozhraní RESTful API. Službu vyhledávání informací o zeměpisné šířky a délky přiřadí zadaná adresa. 

1. Otevřete **MapSearch.html** soubor vytvořili v předchozí části a přidejte následující kód JavaScript, který *skriptu* bloku pro ilustraci službu vyhledávání. 
    ```JavaScript
    // Perform a request to the search service and create a pin on the map for each result
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        var searchPins = [];

        if (this.readyState === 4 && this.status === 200) {
            var response = JSON.parse(this.responseText);

            var poiResults = response.results.filter((result) => { return result.type === "POI" }) || [];

            searchPins = poiResults.map((poiResult) => {
                var poiPosition = [poiResult.position.lon, poiResult.position.lat];
                return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                    name: poiResult.poi.name,
                    address: poiResult.address.freeformAddress,
                    position: poiResult.position.lat + ", " + poiResult.position.lon
                });
            });

            map.addPins(searchPins, {
                name: searchLayerName
            });

            var lons = searchPins.map((pin) => { return pin.geometry.coordinates[0] });
            var lats = searchPins.map((pin) => { return pin.geometry.coordinates[1] });

            var swLon = Math.min.apply(null, lons);
            var swLat = Math.min.apply(null, lats);
            var neLon = Math.max.apply(null, lons);
            var neLat = Math.max.apply(null, lats);

            map.setCameraBounds({
                bounds: [swLon, swLat, neLon, neLat],
                padding: 50
            });
        }
    };
    ```
    Vytvoří tento fragment kódu [XMLHttpRequest](https://xhr.spec.whatwg.org/), a přidá obslužnou rutinu události analyzovat příchozí odpověď. Pro úspěšné odpovědi, shromažďuje adresy, názvy, zeměpisnou šířku a logitude informace pro každé umístění, vrátila ve `searchPins` proměnné. Nakonec přidá umístění odkazuje na tato kolekce `map` ovládací prvek, kódy PIN. 

2. Přidejte následující kód, který *skriptu* blok, k odeslání XMLHttpRequest službě Search Azure na základě polohy.:

    ```JavaScript
    var url = "https://atlas.microsoft.com/search/fuzzy/json?";
    url += "&api-version=1.0";
    url += "&query=gasoline%20station";
    url += "&subscription-key=" + subscriptionKey;
    url += "&lat=47.6292";
    url += "&lon=-122.2337";
    url += "&radius=100000";

    xhttp.open("GET", url, true);
    xhttp.send();
    ``` 
    Tento fragment kódu používá základní hledání rozhraní API služby vyhledávání, volá se **přibližné vyhledávání**. Zpracovává nejvíce přibližné vstupy zpracování libovolnou kombinaci adresy nebo *bodů zájmu* tokeny. Vyhledá blízkou **benzinového stanice**, pro danou adresu v zeměpisné šířky a délky a v rámci zadaného protokolu radius. Používá klíč předplatného váš účet dříve součástí ukázkový soubor pro volání do služeb na základě umístění. Vrátí výsledky jako zeměpisnou šířku a délku páry pro umístění nalezena. Kódy PIN vyhledávání může sledovat tak, že otevřete stránku HTML v prohlížeči. 

3. Přidejte následující řádky, které se *skriptu* bloku, vytvořit automaticky otevíraná okna pro body zájmu vrácený službu vyhledávání:

    ```JavaScript
    // Add a popup to the map which will display some basic information about a search result on hover over a pin
    var popup = new atlas.Popup();
    map.addEventListener("mouseover", searchLayerName, (e) => {
        var popupContentElement = document.createElement("div");
        popupContentElement.style.padding = "5px";

        var popupNameElement = document.createElement("div");
        popupNameElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupNameElement);

        var popupAddressElement = document.createElement("div");
        popupAddressElement.innerText = e.features[0].properties.address;
        popupContentElement.appendChild(popupAddressElement);

        var popupPositionElement = document.createElement("div");
        popupPositionElement.innerText = e.features[0].properties.position;
        popupContentElement.appendChild(popupPositionElement);

        popup.setPopupOptions({
            position: e.features[0].geometry.coordinates,
            content: popupContentElement
        });

        popup.open(map);
    });
    ```
    Rozhraní API **atlas. Místní nabídka** poskytuje informace okno ukotvené umístění na mapě. Tento fragment kódu nastaví obsah a pozici pro místní nabídce, a také přidá naslouchací proces událostí k `map` řízení, čeká _myši_ rozšíří automaticky otevřeném okně. 

4. Uložte soubor a pak otevřete **MapSearch.html** souborů ve webovém prohlížeči podle svého výběru a sledovat výsledek. V tomto okamžiku mapy v prohlížeči zobrazí automaticky otevíraná okna informace po přesunutí ukazatele myši žádné PIN hledání ukazuje, podobný následujícímu. 

    ![Azure mapový ovládací prvek a vyhledávací služba](./media/tutorial-search-location/lbs-map-search.png)


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvořte účet s služeb na základě umístění Azure
> * Získat klíč předplatného pro váš účet
> * Vytvořit novou webovou stránku pomocí rozhraní API mapy ovládacího prvku
> * Použít službu vyhledávání k vyhledání blízkých bodu zájmu

Pokračujte v kurzu [trasy, která má bod týkající se použití služeb na základě umístění Azure](./tutorial-route-location.md) se dozvíte, jak používat službu na základě umístění Azure směrovat na body. 
