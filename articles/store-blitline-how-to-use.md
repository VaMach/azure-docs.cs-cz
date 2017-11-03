---
title: "Jak používat Blitline pro bitovou kopii zpracování - Azure funkce Průvodce"
description: "Zjistěte, jak používat službu Blitline zpracování obrázků v rámci aplikace Azure."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: support@blitline.com
ms.openlocfilehash: 1d90599e028b3407a513b04b878e3aefc39928a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Použití Blitline s Azure a Azure Storage
Tato příručka vysvětluje, jak získat přístup ke službám Blitline a jak k odesílání úloh do Blitline.

## <a name="what-is-blitline"></a>Co je Blitline?
Blitline je bitové kopie založené na cloudu zpracování služba, která poskytuje podnikové úrovni image zpracování za zlomek ceny, které by náklady vytvořit sami.

Skutečnost je, že zpracování obrázků bylo provedeno opakovaně, obvykle znovu sestavit od základů pro každý web. Uvědomujeme si to vzhledem k tomu, že jsme sestavili je mil. časy příliš. Jeden den, které jsme se rozhodli, možná je čas jsme právě provést pro všechny uživatele. Víme, jak to udělat, provést rychlé a efektivní a uložte všechny pracovní do té doby.

Další informace najdete v tématu [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Co Blitline není...
O vysvětlení, co je užitečné pro Blitline, je často usnadňují identifikaci co Blitline neprovádí než budete pokračovat dál.

* Blitline nemá pomůcky HTML pro odesílání obrázků. Bitové kopie, které musí mít veřejně nebo s omezenými oprávněními pro Blitline spojit.
* Blitline neprovádí zpracování, jako je Aviary.com za provozu obrázku
* Blitline nepřijímá odesílání obrázků, obrázků na Blitline nemůžete nabízet přímo. Musíte vložit je Azure Storage nebo z jiných míst podporuje Blitline a potom zadejte Blitline, kde je získat.
* Blitline je massively parallel a neprovádí žádné synchronní zpracování. Což znamená, že musí poskytnout nám postback_url a jsme vám řeknou jsme se po dokončení zpracování.

## <a name="create-a-blitline-account"></a>Vytvoření účtu Blitline
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Jak vytvořit úlohu Blitline
Blitline používá JSON definovat akce, které budete chtít využít na bitovou kopii. Tento formát JSON se skládá z několika jednoduchých polí.

V nejjednodušší příkladu vypadá takto:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Tady bychom měli formátu JSON, který bude trvat bitovou kopii "src" "... boys.jpeg" a potom změňte velikost této bitové kopie do 240 x 140.

ID aplikace se můžete najít v vaše **informace o připojení** nebo **SPRAVOVAT** karty v Azure. Je váš tajný identifikátor, který umožňuje spouštění úloh na Blitline.

Parametr "uložit" identifikuje informace o kam chcete umístit bitovou kopii po jsme ho mít zpracování. V tomto případě trivial jsme nebyly definovány jedna. Pokud je definována žádná umístění Blitline uloží ho místně (a dočasně) v umístění jedinečný cloudu. Bude moct získat z JSON vrácený Blitline při provádění Blitline tohoto umístění. Identifikátor "image" je vyžadován a je vrácen v případě k identifikaci této konkrétní uloženy bitové kopie.

Můžete najít další informace o *funkce* podporujeme zde: <http://www.blitline.com/docs/functions>

Můžete také vyhledat dokumentaci o možnosti úlohy: <http://www.blitline.com/docs/api>

Až budete mít vaše struktury JSON všechny musíte udělat je **POST** jej do`http://api.blitline.com/job`

Zobrazí se zpět JSON, která vypadá přibližně takto:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Znamená to, Blitline přijal žádost, se pozastavil ve frontě zpracování a po jeho dokončení bitovou kopii, budou k dispozici na: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_aplikace\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Jak uložit bitovou kopii do účtu úložiště Azure
Pokud máte účet úložiště Azure, budete moci snadno Blitline push zpracovaná bitové kopie do Azure container. Přidáním "azure_destination" Zadejte umístění a oprávnění pro Blitline k.

Zde naleznete příklad:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Vyplněním CAPITALIZED hodnoty vlastními odešlete tento formát JSON na http://api.blitline.com/job a bitovou kopii "src" budou zpracování pomocí filtru rozostření a pak instaluje do Azure cílové můžete.

### <a name="please-note"></a>Poznámka:
SAS musí obsahovat celou SAS adresa url, včetně názvu cílového souboru.

Příklad:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Můžete si také přečíst nejnovější verzi dokumentace Azure Storage na Blitline [zde](http://www.blitline.com/docs/azure_storage).

## <a name="next-steps"></a>Další kroky
Blitline.com najdete další informace o všech našich dalších funkcí:

* Dokumentace pro koncový bod rozhraní API Blitline <http://www.blitline.com/docs/api>
* Funkce Blitline rozhraní API <http://www.blitline.com/docs/functions>
* Příklady Blitline rozhraní API <http://www.blitline.com/docs/examples>
* Třetí součástí Nuget knihovny <http://nuget.org/packages/Blitline.Net>

