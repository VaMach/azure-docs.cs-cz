# [Přehled](media-services-overview.md)
## [Koncepty](media-services-concepts.md)


# Začínáme
## [Vytvoření a správa účtu](media-services-portal-create-account.md)
## [Vytvoření a nastavení vývojového prostředí](media-services-set-up-computer.md)
## Doručování videa na vyžádání
### [Azure Portal](media-services-portal-vod-get-started.md)
### [.NET SDK](media-services-dotnet-get-started.md)
### [Java](media-services-java-how-to-use.md)
### [REST](media-services-rest-get-started.md)
## Zajištění živého streamování
### [Azure Portal](media-services-portal-live-passthrough-get-started.md)
### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)

# Postup
## Spravovat
### [Správa koncových bodů streamování pomocí portálu](media-services-portal-manage-streaming-endpoints.md)
### Správa entit
#### [.NET](media-services-dotnet-manage-entities.md)
#### [REST](media-services-rest-manage-entities.md)
### [Správa účtů pomocí prostředí PowerShell](media-services-manage-with-powershell.md)
### [Oříznutí videa pomocí kodéru Media Encoder Standard](media-services-crop-video.md)
### [Postup aktualizace Media Services po postupném zavedení přístupových klíčů k úložišti](media-services-roll-storage-access-keys.md)
### [Kvóty a omezení](media-services-quotas-and-limitations.md)
### Filtry
#### [Vytváření filtrů pomocí sady Azure Media Services .NET SDK](media-services-dotnet-dynamic-manifest.md)
#### [Kódování prostředku pomocí kodéru Media Encoder Standard](media-services-rest-encode-asset.md)
### Připojení prostřednictvím kódu programu
#### [.NET](media-services-dotnet-connect-programmatically.md)
#### [REST](media-services-rest-connect-programmatically.md)

## Nahrání obsahu
### Nahrání souborů do účtu
#### [Portál](media-services-portal-upload-files.md)
#### [.NET](media-services-dotnet-upload-files.md)
#### [REST](media-services-rest-upload-files.md)
### [Kopírování existujících objektů blob](media-services-copying-existing-blob.md)

## Kódování
### [Obsah](media-services-encode-asset.md)
#### Kódování prostředku pomocí kodéru Media Encoder Standard
##### [Azure Portal](media-services-portal-encode.md)
##### [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
#### [Postup generování miniatur pomocí kodéru Media Encoder Standard a .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
#### [Pokročilé kódování](media-services-advanced-encoding-with-mes.md)
##### [Pracovní postup kodéru Media Encoder Premium](media-services-encode-with-premium-workflow.md)
##### [Výukové kurzy k pracovním postupům kodéru Media Encoder Premium](media-services-media-encoder-premium-workflow-tutorials.md)
##### [Vytváření pokročilých pracovních postupů kódování pomocí Návrháře postupu provádění](media-services-workflow-designer.md)
##### [Pracovní postup Premium s více vstupy](media-services-media-encoder-premium-workflow-multiplefilesinput.md)

#### Schémata 
#####[Media Encoder Standard](media-services-mes-schema.md)
#####[Vstupní metadata](media-services-input-metadata-schema.md)
#####[Výstupní metadata](media-services-output-metadata-schema.md)

#### Starší kodéry
##### [Použití služby Azure Media Packager](media-services-static-packaging.md)

### [Živé streamování](media-services-manage-channels-overview.md)
#### [Místní kodéry](media-services-live-streaming-with-onprem-encoders.md)
#### Kurz použití místních kodérů
##### [Azure Portal](media-services-portal-live-passthrough-get-started.md)
##### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
#### [Živé streamování s použitím cloudového kodéru](media-services-manage-live-encoder-enabled-channels.md)
#### Kurzy použití cloudových kodérů
##### [Azure Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
##### [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
#### [Konfigurace místních kodérů pro použití s cloudovým kodérem](media-services-live-encoders-overview.md)
#### [Zpracování dlouhotrvajících operací](media-services-dotnet-long-operations.md)
#### [Specifikace živého ingestování fragmentovaného MP4](media-services-fmp4-live-ingest-overview.md)
#### [Dynamické balení](media-services-dynamic-packaging-overview.md)

### Zpracování médií
#### [.NET](media-services-get-media-processor.md)
#### [REST](media-services-rest-get-media-processor.md)

### Konfigurace kodérů pro živý stream s jednou přenosovou rychlostí
#### [Kodér Elemental Live](media-services-configure-elemental-live-encoder.md)
#### [Kodér FMLE](media-services-configure-fmle-live-encoder.md)
#### [Kodér NewTek TriCaster](media-services-configure-tricaster-live-encoder.md)
#### [Kodér Wirecast](media-services-configure-wirecast-live-encoder.md)

## [Ochrana](media-services-content-protection-overview.md)
### [Konfigurace ochrany obsahu pomocí portálu](media-services-portal-protect-content.md)
### [Konfigurace nezašifrovaného klíče AES-128 pro váš stream](media-services-protect-with-aes128.md)
### [Šifrování obsahu pomocí šifrování úložiště s použitím rozhraní AMS REST API](media-services-rest-storage-encryption.md)
### [Přehled šablon licencování Media Services PlayReady](media-services-playready-license-template-overview.md)
### [Doručování licencí DRM](media-services-deliver-keys-and-licenses.md)
### [Využití služeb partnerů k distribuci licencí Widevine pro Azure Media Services](media-services-licenses-partner-integration.md)
### [Použití běžného dynamického šifrování PlayReady nebo Widevine](media-services-protect-with-drm.md)
### [Použití Azure Media Services ke streamování obsahu HLS chráněného technologií Apple FairPlay](media-services-protect-hls-with-fairplay.md)
### [CENC využívající Multi-DRM a Access Control: Referenční návrh a implementace v prostředí Azure a Azure Media Services](media-services-cenc-with-multidrm-access-control.md)

### Doručování prostředků
#### Konfigurace zásad doručování prostředků
##### [.NET](media-services-dotnet-configure-asset-delivery-policy.md)
##### [REST](media-services-rest-configure-asset-delivery-policy.md)
### Vytvoření klíčů ContentKeys
#### [.NET](media-services-dotnet-create-contentkey.md)
#### [REST](media-services-rest-create-contentkey.md)
### Konfigurace zásad autorizace klíčů obsahu
#### [Azure Portal](media-services-portal-configure-content-key-auth-policy.md)
#### [.NET](media-services-dotnet-configure-content-key-auth-policy.md)
#### [REST](media-services-rest-configure-content-key-auth-policy.md)

## [Analýza](media-services-analytics-overview.md)
### [Zpracování pomocí Indexeru 2](media-services-process-content-with-indexer2.md)
### [Zpracování pomocí Indexeru](media-services-index-content.md)
### [Zpracování pomocí technologie Hyperlapse](media-services-hyperlapse-content.md)
### [Zpracování pomocí detektoru obličejů](media-services-face-and-emotion-detection.md)
### [Zpracování pomocí detektoru pohybu](media-services-motion-detection.md)
### [Zpracování pomocí Face Redaction](media-services-face-redaction.md)
### [Zpracování pomocí miniatur videí](media-services-video-summarization.md)
### [Zpracování pomocí technologie OCR](media-services-video-optical-character-recognition.md)

## Měřítko
### [Zpracování médií](media-services-scale-media-processing-overview.md)
#### [Azure Portal](media-services-portal-scale-media-processing.md)
#### [.NET](media-services-dotnet-encoding-units.md)
#### [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
### Koncové body streamování
#### [Azure Portal](media-services-portal-scale-streaming-endpoints.md)

## [Doručování obsahu](media-services-deliver-content-overview.md)
### [Přehled filtrů a dynamických manifestů](media-services-dynamic-manifest-overview.md)
### Vytváření filtrů
#### [.NET](media-services-dotnet-dynamic-manifest.md)
#### [REST](media-services-rest-dynamic-manifest.md)
### Publikování obsahu
#### [Azure Portal](media-services-portal-publish.md)
#### [.NET](media-services-deliver-streaming-content.md)
#### [REST](media-services-rest-deliver-streaming-content.md)
### [Doručení materiálu stažením](media-services-deliver-asset-download.md)
### [Scénář streamování při selhání](media-services-implement-failover.md)

## Konzumace
### [Přehrávání multimédií pomocí stávajících přehrávačů](media-services-playback-content-with-existing-players.md)
### [Přehrávání multimédií pomocí Media Playeru](media-services-develop-video-players.md)
### Další možnosti přehrávání
#### [Aplikace pro Windows Store využívající Smooth Streaming](media-services-build-smooth-streaming-apps.md)
#### [Aplikace HTML5 využívající DASH.js](media-services-embed-mpeg-dash-in-html5.md)
#### [Přehrávače Adobe Open Source Media Framework](media-services-use-osmf-smooth-streaming-client-plugin.md)
### [Vkládání reklam na straně klienta](media-services-inserting-ads-on-client-side.md)

## Integrace
### [Zásady ukládání do mezipaměti CDN v rozšíření Media Services Extension](../cdn/cdn-caching-policy.md?toc=%2fazure%2fmedia-services%2ftoc.json)
### [Licencování sady pro portování klienta Microsoft†" Smooth Streaming](media-services-sspk.md)
### [Správa prostředků ve více účtech úložiště](meda-services-managing-multiple-storage-accounts.md)
### [Distribuce licencí Widevine pro Azure Media Services pomocí Axinomu](media-services-axinom-integration.md)
### [Distribuce licencí Widevine pro Azure Media Services pomocí castLabs](media-services-castlabs-integration.md)
### [Přehled šablon licencování Widevine](media-services-widevine-license-template-overview.md)

## Monitorování
### Kontrola průběhu úlohy
#### [REST](media-services-rest-check-job-progress.md)
#### [Azure Portal](media-services-portal-check-job-progress.md)
#### [.NET](media-services-check-job-progress.md)
### [Služba Queue Storage pro monitorování oznámení úloh](media-services-dotnet-check-job-progress-with-queues.md)

## Řešení potíží
### [Nejčastější dotazy](media-services-frequently-asked-questions.md)
### [Průvodce řešením problémů s živým streamováním](media-services-troubleshooting-live-streaming.md)
###[Kódy chyb](media-services-error-codes.md)
###[Logika opakování](media-services-retry-logic-in-dotnet-sdk.md)

# Referenční informace
## [Poznámky k verzi](media-services-release-notes.md)
## [.NET](media-services-dotnet-how-to-use.md)
## [REST](media-services-rest-how-to-use.md)
## [Formáty a kodeky pracovního postupu Media Encoderu Premium](media-services-premium-workflow-encoder-formats.md)
## [Kodeky a formáty Media Encoderu Standard](media-services-media-encoder-standard-formats.md)

# Zdroje
## [Ceny](https://azure.microsoft.com/pricing/details/media-services/)
## [Komunita Azure Media Services](media-services-community.md)











<!--HONumber=Nov16_HO2-->


