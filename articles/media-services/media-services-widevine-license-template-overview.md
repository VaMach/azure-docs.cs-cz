---
title: "Přehled šablonu licence Widevine | Microsoft Docs"
description: "Toto téma poskytuje přehled o šablonu licence Widevine, který slouží ke konfiguraci licence na Widevine."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 68d519cd36d41728f57419cd6cecd2a79d65a4af
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="widevine-license-template-overview"></a>Přehled šablonu licence Widevine
Azure Media Services umožňuje nakonfigurovat a požadavků licence na Widevine. Když player koncový uživatel pokusí o přehrání obsahu Widevine chráněný, žádost o posílá službu doručování licencí jak licenci získat. Pokud licenční služby schvalovat žádosti, vydá licenci, která se může odeslat klientovi a slouží k dešifrování- and -play zadaný obsah.

Požadavek na licenční Widevine je naformátován jako zprávu JSON.  

>[!NOTE]
> Můžete vytvořit prázdnou zprávu s žádné hodnoty právě "{}" a vytvoří se šablona licence s všechny výchozí hodnoty. Výchozí hodnota se dá použít na většině případů. Například pro MS na základě scénáře doručení licence, které by měl vždycky být výchozí. Pokud potřebujete nastavit "zprostředkovatele" a "content_id" hodnoty, se musí shodovat zprostředkovatele Google Widevine pověření.

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>Zpráva JSON
| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| datová část |Řetězec s kódováním base64 |Licence požadavek odeslaný klientem. |
| content_id |Řetězec s kódováním base64 |Identifikátor použít k odvození KeyId(s) a obsahu klíče pro každý content_key_specs.track_type. |
| Zprostředkovatel |Řetězec |Použít k vyhledání obsahu klíče a zásady. Pokud MS doručení klíče se používá pro doručování licence Widevine, tento parametr je ignorován. |
| název_zásad |Řetězec |Název zásady dříve zaregistrovaný. Nepovinné |
| allowed_track_types |výčet |SD_ONLY nebo SD_HD. Ovládací prvky, které obsahu klíče by měl být součástí licenci |
| content_key_specs |pole JSON struktur, najdete v části **obsahu specifikace klíče** níže |Přesnější možnosti řízení na který obsah klíče vrátit. Podrobnosti najdete v obsahu specifikace klíče níže.  Lze zadat pouze jeden z allowed_track_types a content_key_specs. |
| use_policy_overrides_exclusively |Logická hodnota. hodnotu true nebo false |Pomocí určeného policy_overrides atributy zásad a vynechejte všechny dříve uložené zásad. |
| policy_overrides |Struktura JSON, najdete v části **zásada přepíše** níže |Nastavení zásad pro tuto licenci.  V případě, že tento prostředek má předem definované zásady, tyto zadané hodnoty se použije. |
| session_init |Struktura JSON, najdete v části **inicializace relace** níže |Volitelná data předána licence. |
| parse_only |Logická hodnota. hodnotu true nebo false |Požadavek na licenční je analyzována ale je vydán žádná licence. Ale hodnot formuláře žádosti o licenci jsou vráceny v odpovědi. |

## <a name="content-key-specs"></a>Obsahu specifikace klíče
Pokud existují předem existující zásady, není třeba chcete zadat některé z hodnot v obsahu specifikace klíče.  Existující zásady, které jsou přidružené k tomuto obsahu se použije k určení ochrany výstup například HDCP a CGMS.  Pokud se serverem licence Widevine není registrované existující zásady, poskytovateli obsahu můžete vložit hodnoty do žádosti o licenci.   

Každý content_key_specs je třeba zadat pro všechny sleduje, bez ohledu na to use_policy_overrides_exclusively možnost. 

| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| content_key_specs. track_type |Řetězec |Název typu sledování. Pokud content_key_specs je zadané v žádosti o licenci, nezapomeňte určit, že všechny typy sledovat explicitně. Uděláte to tak bude dojít k selhání k přehrávání posledních 10 sekund. |
| content_key_specs  <br/> security_level |UInt32 |Definuje požadavky na klienta odolnosti pro přehrávání. <br/> 1 - softwarový whitebox kryptografických je povinný. <br/> 2 – software kryptografických a zakódovaná dekodér je vyžadován. <br/> 3-materiál klíče a kryptografické operace, je nutné provést v prostředí zálohovány důvěryhodné spouštěcí hardwaru. <br/> 4-kryptografických a dekódování obsahu, je nutné provést v prostředí zálohovány důvěryhodné spouštěcí hardwaru.  <br/> 5 - na kryptografický, dekódování a všechny zpracování média (komprimovaným a nekomprimovaným) musí být zpracován zálohovány důvěryhodné spouštěcí prostředí hardwaru. |
| content_key_specs <br/> required_output_protection.hDC |String – jeden z: HDCP_V2 HDCP_NONE, HDCP_V1, |Označuje, zda je vyžadují HDCP |
| content_key_specs <br/>key |formátu Base64. <br/>řetězec s kódováním |Klíč obsahu používat tento stopy. -Li zadána, je zapotřebí track_type nebo key_id.  Tato možnost umožňuje poskytovateli obsahu se vložit klíč k obsahu pro toto sledování místo Widevine licenční server generovat nebo vyhledat klíč. |
| content_key_specs.key_id |Kódováním base64, pomocí řetězce binární, 16 bajtů |Jedinečný identifikátor pro klíč. |

## <a name="policy-overrides"></a>Přepsání zásad
| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| policy_overrides. can_play |Logická hodnota. hodnotu true nebo false |Určuje, že přehrávání obsahu je povolen. Výchozí hodnota je false. |
| policy_overrides. can_persist |Logická hodnota. hodnotu true nebo false |Označuje, že může uložit trvale licence na stálé úložiště pro použití v offline režimu. Výchozí hodnota je false. |
| policy_overrides. can_renew |Logická hodnota PRAVDA nebo NEPRAVDA |Označuje, zda je povoleno obnovení tuto licenci. V případě hodnoty true platnost licence, které lze rozšířit pomocí prezenčního signálu. Výchozí hodnota je false. |
| policy_overrides. license_duration_seconds |Int64 |Určuje časový interval pro tuto konkrétní licenci. Hodnota 0 značí, že neexistuje žádné omezení doba trvání. Výchozí hodnota je 0. |
| policy_overrides. rental_duration_seconds |Int64 |Určuje časový interval, při přehrávání je povoleno. Hodnota 0 značí, že neexistuje žádné omezení doba trvání. Výchozí hodnota je 0. |
| policy_overrides. playback_duration_seconds |Int64 |Okno zobrazení času po zahájení v rámci platnost licence. Hodnota 0 značí, že neexistuje žádné omezení doba trvání. Výchozí hodnota je 0. |
| policy_overrides. renewal_server_url |Řetězec |Všechny požadavky prezenčního signálu (obnovení) pro tuto licenci musí směřovat na zadanou adresu URL. Toto pole je použít jen v případě can_renew má hodnotu true. |
| policy_overrides. renewal_delay_seconds |Int64 |Kolik sekund po license_start_time, než je první pokus o obnovení. Toto pole je použít jen v případě can_renew má hodnotu true. Výchozí hodnota je 0. |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Určuje zpoždění v sekundách mezi požadavky na obnovení certifikátu další licence, v případě selhání. Toto pole je použít jen v případě can_renew má hodnotu true. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |Okno čas, ve kterém je povoleno pokračovat při obnovení přehrávání je pokus o, ještě neúspěšný kvůli problémům s back-end s licenční server. Hodnota 0 značí, že neexistuje žádné omezení doba trvání. Toto pole je použít jen v případě can_renew má hodnotu true. |
| policy_overrides. renew_with_usage |Logická hodnota PRAVDA nebo NEPRAVDA |Označuje, že licence se zasílá pro obnovení při spuštění využití. Toto pole je použít jen v případě can_renew má hodnotu true. |

## <a name="session-initialization"></a>Inicializace relace
| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| provider_session_token |Řetězec s kódováním base64 |Tento token relace je předán zpět v licenci a bude existovat v dalších prodloužení.  Relace tokenu neuchovává kromě relací. |
| provider_client_token |Řetězec s kódováním base64 |Token klienta odeslat zpět v reakci licence.  Pokud požadavek na licenční obsahuje token klienta, je tato hodnota ignorována. Token klienta se uchová nad rámec relace licence. |
| override_provider_client_token |Logická hodnota. hodnotu true nebo false |Obsahuje-li hodnotu false a požadavek na licenční token klienta, použití tokenu z požadavku i v případě, že token klienta byla zadána v této struktuře.  V případě hodnoty true vždy používejte uveden v této struktuře token. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>Nakonfigurujte své licence Widevine pomocí typy .NET
Služba Media Services poskytuje rozhraní API .NET, která umožňují nakonfigurujte své licence Widevine. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Třídy definované v .NET SDK služby Media Services
Níže jsou uvedeny definice z těchto typů.

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Příklad
Následující příklad ukazuje, jak konfigurovat jednoduché licenci Widevine pomocí rozhraní API technologie .NET.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Použití běžného šifrování PlayReady nebo Widevine dynamický](media-services-protect-with-playready-widevine.md)

