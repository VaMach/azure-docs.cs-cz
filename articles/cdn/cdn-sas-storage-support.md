---
title: "Pomocí Azure CDN SAS | Microsoft Docs"
description: 
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: rli v-deasim
ms.openlocfilehash: de30f4319be75362131f8c8ad71aad57b0528f05
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="using-azure-cdn-with-sas"></a>Pomocí SAS Azure CDN

Když je poskytovat obsah z úložiště kontejneru účtu úložiště, můžete zabezpečit, jak uživatelům přístup k souborům tak, že udělíte soukromý přístup ke kontejneru úložiště. Kontejner úložiště, pro který byla udělena veřejný přístup, jinak hodnota přístupná libovolný uživatel, který zná jeho adresa URL. K ochraně účtu úložiště, který jste povolili síti pro doručování obsahu (CDN) pro přístup, můžete použít funkci sdíleného přístupového podpisu (SAS) z úložiště Azure udělit omezený přístup k privátním úložných kontejnerů.

SAS je identifikátor URI, udělí omezený přístupová práva ke svým prostředkům úložiště Azure bez vystavení klíč účtu. Klienti, kteří si nejste jisti, nahraďte svým klíčem účtu úložiště, ale kterým chcete delegovat přístup k určitým prostředkům účet úložiště, můžete zadat SAS. Distribucí sdílený přístupový podpis identifikátor URI pro tyto klienty jim udělíte přístup k prostředku pro zadaném časovém období.
 
SAS můžete zadat různé parametry přístupu do objektu blob, jako je například spuštění a vypršení platnosti, oprávnění (čtení a zápis) a rozsahy IP adres. Tento článek popisuje postup použití SAS ve spojení s Azure CDN. Další informace o tokenu SAS, včetně toho, jak vytvořit ji a její parametr možnosti najdete v části [pomocí sdílené přístupové podpisy (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Nastavení Azure CDN pro práci s úložiště SAS
Následující tři možnosti – se doporučují pro použití SAS s Azure CDN. Všechny možnosti předpokládají, že jste již vytvořili funkční SAS (viz požadavky). 
 
### <a name="prerequisites"></a>Požadavky
Pokud chcete spustit, vytvoření účtu úložiště a pak vytvořte SAS pro asset. Můžete vygenerovat dva typy uložené přístupových podpisů: SAS služby nebo SAS účtu. Další informace najdete v tématu [druhy sdílených přístupových podpisů](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Po vygenerování SAS, můžete přístup k souboru úložiště objektů blob s adresou URL, která má následující formát:`https://<account>.blob.core.windows.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
Příklad:
 ```
https://democdnstorage1.blob.core.windows.net/container1/sasblob.txt?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Další informace o nastavení parametrů najdete v tématu [SAS parametr aspekty](#sas-parameter-considerations) a [parametry podpis sdíleného přístupu](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![Nastavení CDN SAS](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-the-cdn"></a>Možnost 1: Použití SAS s průchozí do objektu blob úložiště od CDN

Tato možnost je nejjednodušší a používá jenom jeden token SAS, která se předá od CDN na zdrojový server. Je podporován **Azure CDN společnosti Verizon**, Standard a Premium profilů, a **Azure CDN společnosti Akamai**. 
 
1. Vyberte koncový bod, klikněte na **ukládání do mezipaměti pravidla**, pak vyberte **do mezipaměti každou jedinečnou adresu URL** z **ukládání do mezipaměti řetězce dotazu** seznamu.

    ![Ukládání do mezipaměti pravidla CDN](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Po nastavení SAS na vašem účtu úložiště, přístup k souboru pomocí tokenu SAS s adresou URL CDN. 
   
   Výsledná adresa URL má následující formát:`https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`

   Příklad:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Upřesnit dobu uložení do mezipaměti pomocí pravidel pro ukládání do mezipaměti nebo přidáním `Cache-Control` hlavičky v původu. Protože CDN považuje za řetězec dotazu prostý tokenu SAS, jako osvědčený postup měli byste nastavit ukládání do mezipaměti Doba trvání, kdy vyprší platnost času vypršení platnosti SAS nebo před ním. Jinak soubor je uložen v mezipaměti delší dobu, než je aktivní SAS, soubor může být přístupný ze serveru původu CDN po uplynutí doby vypršení platnosti SAS. Pokud k tomu dochází, a chcete si být nedostupné vaší souborů uložených v mezipaměti, je nutné provést operaci vyprázdnění souboru vymazat z mezipaměti. Informace o nastavení dobu uložení do mezipaměti na CDN najdete v tématu [řízení Azure Content Delivery Network s ukládáním do mezipaměti pravidla chování ukládání do mezipaměti](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-rewrite-rule"></a>Možnost 2: Skryté token zabezpečení CDN pomocí přepsání pravidla
 
Pomocí této možnosti můžete zabezpečit úložiště objektů blob počátek bez nutnosti SAS token pro uživatele CDN. Chcete tuto možnost použijte, pokud nepotřebujete omezení přístupu konkrétním souboru, ale chcete zabránit uživatelům v přístupu k počátku úložiště přímo na vylepšovat dobu snižování zátěže CDN. Tato možnost je dostupná pouze pro **Azure CDN Premium od společnosti Verizon** profily. 
 
1. Použití [stroj pravidel](cdn-rules-engine.md) k vytvoření pravidla přepisování adres URL. Nová pravidla trvat asi 90 minut rozšíření.

   ![Tlačítko Spravovat CDN](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Tlačítko modul pravidla CDN](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Tato ukázka přepisování adres URL pravidla nemá následující vzorce:
   
   Zdroj:   
   `/test/demo.jpg`
   
   Cíl:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![Přepisování adres URL CDN pravidlo](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
 
2. Přístup k souboru na vašem CDN bez tokenu SAS, v následujícím formátu:`https://<endpoint>.azureedge.net/<folder>/<file>`
 
   Příklad:   
   `https://demoendpoint.azureedge.net/test/demo.jpg`
       
   Všimněte si, že všichni uživatelé, bez ohledu na to, jestli jsou pomocí tokenu SAS, přístup k koncový bod CDN. 

3. Upřesnit dobu uložení do mezipaměti pomocí pravidel pro ukládání do mezipaměti nebo přidáním `Cache-Control` hlavičky v původu. Protože CDN považuje za řetězec dotazu prostý tokenu SAS, jako osvědčený postup měli byste nastavit ukládání do mezipaměti Doba trvání, kdy vyprší platnost času vypršení platnosti SAS nebo před ním. Jinak soubor je uložen v mezipaměti delší dobu, než je aktivní SAS, soubor může být přístupný ze serveru původu CDN po uplynutí doby vypršení platnosti SAS. Pokud k tomu dochází, a chcete si být nedostupné vaší souborů uložených v mezipaměti, je nutné provést operaci vyprázdnění souboru vymazat z mezipaměti. Informace o nastavení dobu uložení do mezipaměti na CDN najdete v tématu [řízení Azure Content Delivery Network s ukládáním do mezipaměti pravidla chování ukládání do mezipaměti](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Možnost 3: Pomocí pravidel přepisování ověření tokenu zabezpečení CDN

Tato možnost je zabezpečení a přizpůsobit. Chcete-li použít ověření tokenu zabezpečení CDN, musíte mít **Azure CDN Premium od společnosti Verizon** profilu. Klientský přístup je založenou na parametrech zabezpečení nastaven na token zabezpečení CDN. Ale pokud SAS stává neplatným, CDN nebudou moci znovu ověřit obsah ze zdrojového serveru.

1. [Vytvořit token zabezpečení CDN](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) a aktivujte ji pomocí stroj pravidel pro koncový bod CDN a cestu, kde uživatelé přístup k souboru.

   Adresa URL typu SAS má následující formát:   
   `https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
   Příklad:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Parametr možnosti pro ověření tokenu zabezpečení CDN jsou jiné než parametr možnosti pro SAS token. Pokud chcete použít při vytváření tokenu zabezpečení CDN čas vypršení platnosti, nastavte ji na stejnou hodnotu jako dobu vypršení platnosti tokenu SAS. Tím zajistíte, že doba vypršení platnosti je předvídatelný. 
 
2. Použití [stroj pravidel](cdn-rules-engine.md) k vytvoření pravidla přepisování adres URL pro tokenu přístupu pro všechny objekty BLOB v kontejneru. Nová pravidla trvat asi 90 minut rozšíření.

   Tato ukázka přepisování adres URL pravidla nemá následující vzorce:
   
   Zdroj:   
   `/test/demo.jpg`
   
   Cíl:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![Přepisování adres URL CDN pravidlo](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)

3. Když jste obnovovali SAS, aktualizujte pravidlo přepisování adres Url pro použití nového tokenu SAS. 

## <a name="sas-parameter-considerations"></a>Aspekty parametr SAS

Protože SAS parametry nejsou viditelné pro CDN, CDN nelze změnit její chování doručování na jejich základě. Definovaný parametr omezení se vztahují pouze na požadavky, které umožňuje CDN na zdrojový server, ne pro požadavky od klienta k CDN. Tento rozdíl je důležité vzít v úvahu při nastavení parametrů SAS. Pokud toto rozšířené možnosti jsou požadované a používáte [možnost 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), nastavte příslušná omezení na token zabezpečení CDN.

| Název parametru SAS | Popis |
| --- | --- |
| Start | Čas, můžete začít přistupovat k objektu blob souboru CDN. Z důvodu hodin zkreslit (Pokud hodiny signál dorazí na různé časy pro různé součásti), vyberte dobu 15 minut dříve, pokud chcete asset být okamžitě k dispozici. |
| Konec | Doba, po jejímž uplynutí CDN už přístup k objektu blob souboru. Dříve soubory uložené v mezipaměti na CDN jsou stále dostupné. Chcete-li řídit čas vypršení platnosti souboru, nastavte čas vypršení platnosti příslušné na token zabezpečení CDN nebo vymazat asset. |
| Povolené IP adresy | Volitelné. Pokud používáte **Azure CDN společnosti Verizon**, tento parametr můžete nastavit na rozsahy definované v [Azure CDN společnosti Verizon hraniční Server rozsahy IP adres](https://msdn.microsoft.com/library/mt757330.aspx). Pokud používáte **Azure CDN společnosti Akamai**, nelze nastavit parametr rozsahy IP, protože nejsou statické IP adresy.|
| Povolené protokoly | Protokol povolenou pro požadavek pomocí SAS účtu. Nastavení protokolu HTTPS se doporučuje.|

## <a name="see-also"></a>Další informace najdete v tématech
- [Použití sdílených přístupových podpisů (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Sdílené přístupové podpisy, část 2: Vytvoření a použití SAS s úložištěm Blob](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Zabezpečení prostředků Azure Content Delivery Network pomocí tokenu ověřování](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
