---
title: "Přizpůsobení řešení připojených factory - Azure | Microsoft Docs"
description: "Popis toho, jak přizpůsobit chování připojeného objektu pro vytváření předkonfigurovaného řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: d9dfd856a95d0b1f925487f4ca9d27e617093405
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Přizpůsobit způsob, jakým připojené vytváření řešení zobrazí data z vašich serverů OPC UA

Řešení připojených factory agreguje a zobrazí data ze serverů OPC UA připojení k řešení. Můžete procházet a odesílat příkazy na OPC UA servery ve vašem řešení. Další informace o OPC UA najdete v tématu [Propojená továrna – Nejčastější dotazy](iot-suite-faq-cf.md).

Agregovaná data v řešení příkladem efektivitu celkové zařízení (OEE) a klíčových ukazatelů výkonu (KPI), můžete zobrazit v řídicím panelu v objektu pro vytváření, řádku a úrovně stanice. Následující snímek obrazovky ukazuje hodnoty OEE a klíčového ukazatele výkonu **sestavení** na stanici **produkční řádku 1**v **Mnichov** objekt pro vytváření:

![Příklad hodnoty OEE a klíčových ukazatelů výkonu v řešení][img-oee-kpi]

Toto řešení umožňuje zobrazit podrobné informace z konkrétní datové položky z OPC UA servery, nazývané *stanice*. Následující snímek obrazovky ukazuje pozemků počet vyrobenou položek z konkrétní stanice:

![Pozemků počet vyrobenou položek][img-manufactured-items]

Pokud kliknete na některý z grafů, můžete si prostudovat data další pomocí časové řady přehledy (TSI):

![Prozkoumejte data pomocí statistiky časové řady][img-tsi]

Tento článek popisuje:

- Data jak je k dispozici různé zobrazení v řešení.
- Jak můžete přizpůsobit způsob řešení zobrazí data.

## <a name="data-sources"></a>Zdroje dat

Řešení připojených factory zobrazí data ze serverů OPC UA připojení k řešení. Výchozí instalace zahrnuje několik OPC UA servery se systémem simulace objekt pro vytváření. Můžete přidat vlastní servery OPC UA, [připojení přes bránu] [ lnk-connect-cf] do řešení.

Můžete procházet datových položek, které připojený server OPC UA může odesílat do řešení v řídicím panelu:

1. Zvolte **prohlížeče** přejděte na **vyberte server OPC UA** zobrazení:

    ![Přejděte do vyberte zobrazení serverů OPC UA][img-select-server]

1. Vyberte server a klikněte na tlačítko **Connect**. Klikněte na tlačítko **pokračovat** Jakmile se zobrazí upozornění zabezpečení.

    > [!NOTE]
    > Toto upozornění pouze vyskytuje jednou pro každý server a vytvoří vztah důvěryhodnosti mezi řídicí panel řešení a serverem.

1. Nyní je možné procházet datových položek, které může server odeslat do řešení. Položky, které jsou odesílány do řešení mají zaškrtnuto:

    ![Publikované položky][img-published]

1. Pokud jste *správce* v řešení, můžete publikovat položku dat, aby byl k dispozici v připojených objekt pro vytváření řešení. Jako správce můžete také změnit hodnotu datové položky a volání metody v serveru OPC UA.

## <a name="map-the-data"></a>Mapování dat.

Řešení připojených factory mapuje a agreguje položky publikovaných dat ze serveru OPC UA u různých zobrazení v řešení. Řešení připojených factory nasadí do účtu Azure při zřizování řešení. Soubor JSON v objektu pro vytváření řešení Visual Studio připojené ukládá tyto informace o mapování. Můžete zobrazit a upravit tento konfigurační soubor JSON v připojených vytváření řešení sady Visual Studio. Můžete znovu nasadit řešení po provedení změny.

Můžete použít konfiguračního souboru na:

- Upravte existující simulované objekty Factory, řádky výroby a stanice.
- Mapování dat ze skutečné OPC UA serverů, které se připojují k řešení.

Klonovat kopie připojeného objektu pro vytváření řešení sady Visual Studio, použijte následující příkaz git:

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

Soubor **ContosoTopologyDescription.json** definuje mapování z OPC UA serveru datové položky k zobrazení v řídicím panelu řešení připojených objekt pro vytváření. Tento konfigurační soubor můžete najít **Contoso\Topology** složku **WebApp** projekt v sadě Visual Studio řešení.

Obsah souboru JSON jsou uspořádána jako hierarchie objekt pro vytváření, řádek výrobní a uzly stanice. Tato hierarchie definuje hierarchii navigace v řídicím panelu připojené objekt pro vytváření. Hodnoty v každém uzlu hierarchie určují informace zobrazené na řídicím panelu. Například soubor JSON obsahuje následující hodnoty pro objekt pro vytváření Mnichov:

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

Název, popis a umístění se zobrazují v tomto zobrazení v řídicím panelu:

![Mnichov data v řídicím panelu][img-munich]

Každý objekt pro vytváření, řádek výrobní a stanice mít vlastnost bitové kopie. Můžete najít v těchto JPEG – soubory **Content\img** složku **WebApp** projektu. Tyto soubory bitové kopie se zobrazí v řídicím panelu připojené objekt pro vytváření.

Každé stanici zahrnuje několik podrobné vlastnosti, které definují mapování z OPC UA datové položky. Tyto vlastnosti jsou popsány v následujících částech:

### <a name="opcuri"></a>OpcUri

**OpcUri** hodnota je identifikátor URI aplikace OPC UA, který jednoznačně identifikuje OPC UA serveru. Například **OpcUri** hodnota sestavení stanice na řádku výrobní 1 v Mnichově vypadá takto: **urn: scada2194:ua:munich:productionline0:assemblystation**.

Identifikátory URI propojených serverů OPC UA můžete zobrazit v řídicím panelu řešení:

![Zobrazit OPC UA server identifikátory URI][img-server-uris]

### <a name="simulation"></a>Simulace

Informace v **simulace** uzel je specifické pro simulaci OPC UA, který je spuštěn v OPC UA servery, které jsou zřízené ve výchozím nastavení. Pro skutečné serveru OPC UA se nepoužívá.

### <a name="kpi1-and-kpi2"></a>Kpi1 a Kpi2

Tyto uzly popisují, jak se data z stanice přispívá k tyto dvě hodnoty klíčového ukazatele výkonu v řídicím panelu. Ve výchozím nasazení jsou tyto hodnoty klíčového ukazatele výkonu jednotky za hodinu a kWh za hodinu. Řešení vypočítá vales klíčového ukazatele výkonu na úrovni stanice a agreguje je na úrovních objekt pro vytváření a produkční řádku.

Každý ukazatel KPI má minimální, maximální a cílovou hodnotu. Každá hodnota klíčového ukazatele výkonu můžete také definovat akce výstrah pro připojené vytváření řešení k provedení. Následující fragment kódu ukazuje definice klíčového ukazatele výkonu pro sestavení stanice na řádku výrobní 1 v Mnichově:

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

Následující snímek obrazovky ukazuje dat klíčového ukazatele výkonu v řídicím panelu.

![Informace o klíčových ukazatelů výkonu v řídicím panelu][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

**OpcNodes** uzly identifikaci položek publikovaných dat ze serveru OPC UA a určete, jak zpracovat data.

**NodeId** hodnota identifikuje konkrétní NodeID OPC UA ze serveru OPC UA. První uzel v sestavení stanice pro produkční řádku 1 v Mnichově má hodnotu **ns = 2; i = 385**. A **NodeId** hodnota určuje datové položce číst ze serveru OPC UA a **symbolickou** poskytuje a uživatelské jméno, které se má používat v řídicím panelu pro tato data.

Ostatní hodnoty přidružené k každý uzel jsou shrnuty v následující tabulce:

| Hodnota | Popis |
| ----- | ----------- |
| Důležitost  | Klíčový ukazatel výkonu a OEE hodnoty, které tato data přispívá k. |
| Operační kód     | Jak agregovaná data. |
| Jednotky      | Jednotky pro použití v řídicím panelu.  |
| Viditelné    | Jestli se má zobrazit tuto hodnotu na řídicím panelu. Některé hodnoty se používá ve výpočtech ale nezobrazí.  |
| Maximální počet    | Maximální hodnota, která aktivuje výstrahu v řídicím panelu. |
| MaximumAlertActions | Akce provést v reakci na výstrahy. Například odeslat příkaz ke stanici. |
| ConstValue | Konstantní hodnota používá výpočtu. |

## <a name="deploy-the-changes"></a>Nasazení změny

Když dokončíte provádění změn **ContosoTopologyDescription.json** souboru, je nutné znovu nasadit řešení připojených factory ke svému účtu Azure.

**Azure-iot připojené factory** zahrnuje úložiště **build.ps1** skript prostředí PowerShell můžete znovu vytvořit a nasadit řešení.

## <a name="next-steps"></a>Další kroky

Další informace o připojené objekt pro vytváření předkonfigurovaného řešení přečíst v následujících článcích:

* [Průvodce předkonfigurovaným řešením propojené továrny][lnk-rm-walkthrough]
* [Nasazení brány pro připojené vytváření][lnk-connect-cf]
* [Oprávnění na webu azureiotsuite.com][lnk-permissions]
* [Propojená továrna – nejčastější dotazy](iot-suite-faq-cf.md)
* [NEJČASTĚJŠÍ DOTAZY][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-faq]: iot-suite-v1-faq.md