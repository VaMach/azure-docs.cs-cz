---
title: "Začínáme s Azure Media výřez | Microsoft Docs"
description: "Začínáme se z Azure výřez média, nástroj pro vytváření médií klipy z prostředků"
services: media-services
keywords: "klip; subclip; kódování; média"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8a4f2c79131664ca0d078fa58c6a75b54243e705
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="create-clips-with-azure-media-clipper"></a>Vytvoření klipů pomocí Azure Media výřez
Tato část uvádí základní kroky Začínáme se službou Azure Media výřez. Části obsahují podrobnosti o tom, jak konfigurovat Azure Media výřez.

- Nejprve přidejte následující odkazy pro přehrávač médií Azure a Azure Media výřez head vašeho dokumentu. Doporučujeme, abyste explicitně určit verzi výřez a Azure Media Player v adresách URL. Nepoužívejte nejnovější verzi těchto prostředků v produkčním prostředí, protože se jedná o mohou změnit na vyžádání.

```javascript
<!--Azure Media Player 2.1.4 or later is a prerequisite-->
<link href="//amp.azure.net/libs/amp/2.1.4/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amp/2.1.4/azuremediaplayer.min.js"></script>
<!--Azure Media Clipper script and stylesheet-->
<link href="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.min.js"></script>
```

- Dál přidejte následující třídy element div, kde chcete vytvořit instanci výřez.

```javascript
<div id="root" class="azure-subclipper" />
```

Volitelně můžete povolit tmavým motivem, přidejte třídu světlý vzhledu:

```javascript
<div id="root" class="azure-subclipper dark-skin" />
```

- V dalším kroku doložit výřez na následující volání rozhraní API:

```javascript
var subclipper = new subclipper({
    selector: '#root',
    logLevel: 'info',
    restVersion: '2.0',
    minimumMarkerGap: 6,
    submitSubclipCallback: onSubmitSubclip,
    singleBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with single H264Layers
                }, {
                    // Audio Codec
                }]
    },
    multiBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with multiple H264Layers
                }, {
                    // Audio Codec
                }]
    },
    keymap: {
      // See below for more info
    },
   // Enable the Video Assets panel in the widget to automatically load assets (input contract)
    assetsPanelLoaderCallback: onLoadAssets,
    height: 600,
    subclippingMode: 'all',
    filterAssetsTypes: true,
    speedLevels: [
        { name: '4x', value: 4.0 },
        { name: '3x', value: 3.0 },
        { name: '2x', value: 2.0 },
        { name: 'normal', value: 1.0 },
        { name: '0.75x', value: 0.75 },
        { name: '0.5x', value: 0.5 },
    ],
    resetOnJobDone: false,
    autoplayVideo: true,
    language: 'en'    
});
```

Parametry pro volání metody inicializace jsou:
- `selector`{POTŘEBY řetězec}: výběr CSS odpovídající elementu HTML, kde má být vykreslen widgetu.
- `restVersion`{POTŘEBY řetězec}: verze rozhraní API REST služby Azure Media k cíli. Verze REST definuje formát výstupu generované widgetu. V současné době se podporuje jenom 2.0.
- `submitSubclipCallback`{POVINNÉ, promise} Funkce zpětného volání, která je volána, když po kliknutí na tlačítko "Odeslat" widgetu. Funkce zpětného volání by měl očekávat výstup generovaný ve widgetu (konfigurace úlohu vykreslování nebo definici filtru). Další informace najdete v tématu Odeslání subclip zpětného volání.
- `logLevel`{VOLITELNÉ, {'informace o', "Upozornit", "Chyba"}}: úroveň protokolování, který se má zobrazit v konzole prohlížeče. Výchozí hodnota: Chyba
- `minimumMarkerGap`{VOLITELNÉ, int}: minimální velikost subclip (v sekundách). Poznámka: hodnota by měla být větší nebo rovna hodnotě 6, což je také výchozí.
- `singleBitrateMp4Profile`{Objekt volitelné, JSON} Profil mp4 jednou přenosovou rychlostí pro vykreslení úlohy konfigurace generované widgetu. Pokud není zadaná, použije [výchozí profil MP4 jednou přenosovou rychlostí](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-single-bitrate-1080p).
- `multiBitrateMp4Profile`{Objekt volitelné, JSON} Profil mp4 přenosovou rychlostí více pro vykreslení úlohy konfigurace generované widgetu. Pokud není zadaná, použije [výchozí profil MP4 více přenosovými rychlostmi](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-multiple-bitrate-1080p).
- `keymap`{Objekt volitelné, json} Umožňuje přizpůsobení klávesových zkratek widgetu. Další informace najdete v tématu [přizpůsobitelné klávesové zkratky](media-services-azure-media-clipper-keyboard-shortcuts.md).
- `assetsPanelLoaderCallback`{VOLITELNÉ, promise} Funkce zpětného volání, která je volána načíst (asynchronně) novou stránku prostředků do podokna prostředky pokaždé, když uživatel posune do dolní části podokna. Další informace najdete v tématu Asset podokně zavaděč zpětného volání.
- `height`{VOLITELNÉ, number} Celková výška widgetu (minimální výška je 600 px bez podokně prostředky a 850 px s podokně prostředky).
- `subclippingMode`(Volitelné tam, {"vše", 'vykreslení' barvy, 'filtr'}): režimy subclipping povoleny. Výchozí hodnota je všechny.
- `filterAssetsTypes`(Volitelné, logická hodnota): filterAssetsTypes umožňuje zobrazit nebo skrýt rozevíracího seznamu filtrů z podokna prostředky. Výchozí hodnota je true.
- `speedLevels`(Volitelná pole): speedLevels umožňuje nastavení úrovně jinou rychlost pro přehrávání videa, najdete v části [dokumentace Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/#amp.player.playbackspeedoptions) Další informace.
- `resetOnJobDone`(Volitelné, logická hodnota): resetOnJobDone umožňuje výřez resetovat subclipper počáteční stav, když je úloha odeslána úspěšně.
- `autoplayVideo`(Volitelné, logická hodnota): autoplayVideo umožňuje výřez automatické přehrávání videa na zatížení. Výchozí hodnota je true.
- `language`{VOLITELNÝ, řetězec}: jazyk nastaví jazyk widgetu. Pokud není zadaný, pokusí se widgetu lokalizaci zprávy podle jazyka prohlížeče. Pokud žádný jazyk v prohlížeči, widgetu výchozí angličtině. Další informace najdete v části podporované jazyky.
- `languages`{VOLITELNÉ, JSON}: Parametr jazyky nahradí výchozí slovník jazyky vlastní slovník definovaných uživatelem. Další informace najdete v části podporované jazyky.
- `extraLanguages`(Volitelné, JSON): Parametr extraLanaguages přidá do slovníku výchozí nové jazyky. Další informace najdete v části podporované jazyky.

## <a name="typescript-definition"></a>Definice typeScript
A [TypeScript](https://www.typescriptlang.org/) najdete soubor definice pro výřez [zde](http://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media výřez rozhraní API
Tato část popisuje plochy rozhraní API poskytované výřez.

- `load(assets)`: načte seznam prostředků do podokně prostředky (by se neměla používat společně s `assetsPanelLoaderCallback`). Toto [článku](media-services-azure-media-clipper-load-assets.md) podrobnosti o tom, jak načíst prostředky do výřez.
- `setLogLevel(level)`: Určuje úroveň protokolování, který se má zobrazit v konzole prohlížeče. Možné hodnoty jsou: `info`, `warn`, `error`.
- `setHeight(height)`: Nastaví celkový výšku widgetu v pixelech (minimální výška je 600 px bez podokně prostředky a 850 px s podokně prostředky).
- `version`: získá verzi pomůcky.

## <a name="configuring-azure-media-clipper"></a>Konfigurace Azure Media výřez
Najdete v části Další kroky pro konfiguraci Azure Media výřez:
- [Načítání prostředků do Azure Media výřez](media-services-azure-media-clipper-load-assets.md)
- [Konfigurace vlastní klávesové zkratky](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Odeslání úlohy výstřižek z výřez](media-services-azure-media-clipper-submit-job.md)

## <a name="supported-languages"></a>Podporované jazyky
Pomůcka výřez je k dispozici v 18 jazycích. Nastavení jazyka pomůcky, je nutné definovat `language` parametr během inicializace. Předejte v řetězci požadovaný jazyk kódu z následujícího seznamu:
- Čínština (zjednodušená): zh-hans
- Čínština (tradiční): zh-hant
- Čeština: cs
- Holandská, Flemish: nl
- Angličtina: en
- Francouzština: fr
- Němčina: de
- Maďarština: hu
- Italština: ho
- Japonština: Japonsko
- Korejština: ko
- Polština: pl
- Portugalština (Brazílie): pt-br
- Portugalština (Portugalsko): pt-pt
- Ruština: ru
- Španělština: es
- Švédština: sv
- Turečtina: tr

Nastavit slovník vlastní jazyk nebo rozšířit slovníku výchozí jazyk, je nutné definovat `languages` nebo `extraLanguages` parametr, v uvedeném pořadí. Předejte vlastní slovník pomocí následujícího formátu JSON:

```javascript
{
      "{language-code}":
          "{message-id}": "{message}"
          ...
      }
      ...
}
```

Například následující ukázka definuje lokalizované anglickou řetězce:

```javascript
export default {
  'VideoPlayer.noPreview': 'No video preview',
  'VideoPlayer.loadAsset': 'You must provide a valid asset',
  'AssetsPanel.name': 'Name',
  'AssetsPanel.type': 'Asset type',
  'AssetsPanel.actions': 'Actions',
  'AssetsPanel.loading': 'Loading...',
  'AssetsPanel.duration': 'Duration',
  'AssetsPanel.resolution': 'Resolution',
  'AssetsPanel.pluralFiles': '{0} assets',
  'AssetsPanel.searchFiles': 'Search assets',
  'AssetsPanel.showTypes': 'Show:',
  'AssetsPanel.typesInfo': 'Rendered assets are actual MP4 files. Dynamic manifest filters are filters applied to a parent asset\'s video segment playlist.',
  'AssetsPanel.filterTypes': 'Filters',
  'AssetsPanel.assetTypes': 'Assets',
  'AssetsPanel.assetsAll': 'All',
  'AssetsPanel.addAsset': 'Add asset to the end',
  'AssetsPanel.addFilter': 'Add filter to the timeline',
  'AssetsPanel.invalidAsset': 'The metadata of this asset is not compatible with the other assets in the timeline',
  'AssetsPanel.addAssetWarning': 'Subclipping on assets with different resolutions may cause resolution autoscaling.',
  'AssetsPanel.live': 'LIVE',
  'AssetsPanel.unknown': 'UNKNOWN',
  'AssetsPanel.minimGapNotMet': 'The asset duration must be greater than the minimum clip duration ({0} seconds)',
  'VideoPlayer.openAdvancedSettings': 'Advanced settings',
  'VideoPlayer.singleBitrate': 'Single-bitrate MP4 (rendered)',
  'VideoPlayer.multiBitrate': 'Multi-bitrate MP4 (rendered)',
  'VideoPlayer.dynamicManifest': 'Dynamic manifest filter',
  'VideoPlayer.ErrorWithMessage': 'There was an error in the video player, code {0}, message: {1}',
  'Common.cancel': 'Cancel',
  'Common.OK': 'OK',
  'AdvancedSettings.framerate': 'Frame rate',
  'Dropdown.select': 'Select an option...',
  'InputAsset.RemoveInput': 'Remove source',
  'Zoom.startTime': 'Start time',
  'Zoom.endTime': 'End time',
  'VideoPlayer.subclips': 'Subclips:',
  'VideoPlayer.length': 'Clip length:',
  'Accordion.scrollLeft': 'Scroll to the left',
  'Accordion.scrollRight': 'Scroll to the right',
  'AdvancedSettings.title': 'Advanced settings',
  'AdvancedSettings.subclipName': 'Subclip name',
  'AdvancedSettings.subclipType': 'Subclipping mode',
  'AdvancedSettings.includeAudioTracks': 'Include audio tracks',
  'AdvancedSettings.subclipTypeInfo': 'Single-bitrate and multi-bitrate MP4s are frame accurate rendered assets. Dynamic manifest filters are group-of-pictures (GOP) accurate filters applied to a parent asset. Creating filters does not create a new asset and does not require encoding. Subclipping jobs on live assets are valid as long as their mark times are within the archive window of the parent asset. Filters are valid as long as the parent asset exists and mark times are within its archive window.',
  'AdvancedSettings.frameRateInfo': 'We autodetect frame rate under most scenarios. however, If we cannot autodetect, choose a frame rate from the dropdown for the selected asset(s).',
  'AdvancedSettings.frameRateError': 'Unable to determine frame rate',
  'AdvancedSettings.subclipNameInfo': 'Choose a name for your subclip.',
  'AdvancedSettings.singleAudioTrack': '1 audio track selected',
  'AdvancedSettings.allAudioTracks': 'All audio tracks selected',
  'AdvancedSettings.someAudioTracks': '{0} audio tracks selected',
  'AdvancedSettings.includeAllAudioTracks': 'Include all audio tracks',
  'AssetsPanel.loadingError': 'Failed to retreive assets from server.',
  'AssetsPanel.retry': 'Retry?',
  'CommandBar.prevFrameTitle': 'Back up one frame',
  'CommandBar.prevKeyFrameTitle': 'Back up one GOP',
  'CommandBar.cleanJob': 'Remove all assets',
  'CommandBar.cleanJobTitle': 'Remove all assets from timeline',
  'CommandBar.cleanJobMessage': 'This will empty all video clips from your timeline.',
  'CommandBar.update': 'Update filter',
  'CommandBar.createFilter': 'Create filter',
  'CommandBar.submit': 'Submit subclipper job',
  'CommandBar.jobErrorTitle': 'Operation failed',
  'CommandBar.jobErrorMessage': 'Your subclip failed to submit. Please try again.',
  'CommandBar.markInTitle': 'Set in at playhead',
  'CommandBar.markInPosition': 'Mark in timecode',
  'CommandBar.markOutTitle': 'Set out at playhead',
  'CommandBar.markOutPosition': 'Mark out timecode',
  'CommandBar.nextFrameTitle': 'Advance one frame',
  'CommandBar.nextKeyFrameTitle': 'Advance one GOP',
  'CommandBar.play': 'Play video',
  'CommandBar.pause': 'Pause video',
  'CommandBar.playPreviewTitle': 'Play subclip preview',
  'CommandBar.pausePreviewTitle': 'Pause subclip preview',
  'CommandBar.redoTitle': 'Redo last action',
  'CommandBar.removeAsset': 'Remove current asset',
  'CommandBar.undoTitle': 'Undo last action',
  'VideoPlayer.errorTitle': 'Error',
  'VideoPlayer.errorMessage': 'There was an error loading the selected asset.',
  'Timeline.markIn': 'Mark in bracket',
  'Timeline.markOut': 'Mark out bracket',
  'Timeline.playHead': 'Play head',
};
```