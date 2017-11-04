---
title: "Přidat vlastní doménu pro koncový bod CDN | Microsoft Docs"
description: "Postup mapování obsahu Azure CDN vlastní doménu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: mazha
ms.openlocfilehash: 98d4900e28f1850050dc4fbe1f97435e52afaf08
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="add-a-custom-domain-to-your-cdn-endpoint"></a>Přidat vlastní doménu pro koncový bod CDN
Po vytvoření profilu, je obvykle také vytvořit jeden nebo více CDN [koncové body](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint) (subdoména `azureedge.net`) k doručení obsahu pomocí protokolu HTTP a HTTPS. Ve výchozím nastavení je tento koncový bod součástí všechny adresy URL (například `https://contoso.azureedge.net/photo.png`). Pro usnadnění vaší práce Azure CDN vám umožní přidružit k vlastní doméně (například `www.contoso.com`) s váš koncový bod. Pomocí této možnosti používat vlastní doménu pro doručování obsahu místo váš koncový bod. Tato možnost je užitečná, pokud například chcete vlastní název domény, který se má uvidí vaši zákazníci pro branding pro účely.

Pokud jste již nemají vlastní doménu, je nutné nejprve zakoupit s zprostředkovatele domény. Po získání vlastní doménu, postupujte takto:
1. [Přístup k záznamy DNS poskytovatel domény](#step-1-access-dns-records-by-using-your-domain-provider)
2. [Vytvořit záznamy CNAME DNS](#step-2-create-the-cname-dns-records)
    - Možnost 1: Přímé mapování vaši vlastní doménu pro koncový bod CDN
    - Možnost 2: Mapování vaši vlastní doménu pro koncový bod CDN pomocí cdnverify 
3. [Povolit mapování záznam CNAME ve službě Azure](#step-3-enable-the-cname-record-mapping-in-azure)
4. [Ověřte, zda vlastní subdomény odkazuje váš koncový bod CDN](#step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint)
5. [(Závislé krok) Mapování trvalé vlastní doménu na koncový bod CDN](#step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint)

## <a name="step-1-access-dns-records-by-using-your-domain-provider"></a>Krok 1: Zaznamenává přístup DNS pomocí poskytovatele domény

Pokud používáte Azure na hostitele vaše [domén DNS](https://docs.microsoft.com/en-us/azure/dns/dns-overview), musí delegovat zprostředkovatele domény DNS do Azure DNS. Další informace najdete v tématu [delegování domény do Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)

Jinak hodnota Pokud používáte vaší domény zprostředkovatele pro zpracování vaše doména DNS, přihlaste se k webu poskytovatel domény. Najděte stránku pro správu záznamy DNS konzultace ohledně poskytovatele dokumentaci, nebo hledáním v oblastech, webové stránky s názvem bez přípony **název domény**, **DNS**, nebo **název serveru správy**. Často můžete najít stránce záznamy DNS, a zobrazení informací o vašem účtu a hledá odkaz, jako **mé domény**. Někteří poskytovatelé obsahovat jiné odkazy přidat různé typy záznamů.

> [!NOTE]
> U určitých poskytovatelů, jako je například GoDaddy, se změny záznamů DNS neprojeví, dokud nevyberete samostatný odkaz **Save Changes** (Uložit změny). 


## <a name="step-2-create-the-cname-dns-records"></a>Krok 2: Vytvoření záznamy CNAME DNS

Předtím, než se koncový bod Azure CDN můžete použít vlastní doménu, musíte nejdřív vytvořit záznam kanonický název (CNAME) s poskytovatelem domény. Záznam CNAME je typ záznamu v v systému DNS (Domain Name) mapující cílová doména zdrojovou doménu tak, že zadáte název domény aliasu pro název domény "kanonický" nebo hodnotu true. Pro Azure CDN zdrojovou doménu je vaše vlastní domény (a subdomény) a cílová doména je koncový bod CDN. Azure CDN ověřuje záznam CNAME DNS při přidání vlastní doménu na koncový bod z portálu nebo rozhraní API. 

Záznam CNAME mapuje konkrétní domény a subdomény, jako například `www.contoso.com` nebo `cdn.contoso.com`; není možné mapovat záznam CNAME do kořenové domény, jako například `contoso.com`. Subdoména může být přidružen pouze jeden koncový bod CDN a záznam CNAME, který vytvoříte bude směrovat veškerý provoz adresovaných subdoméně zadaný koncový bod. Například Pokud přidružíte `www.contoso.com` s váš koncový bod CDN nelze přidružit ho s jiným Azure koncovým bodem, jako je koncový bod účtu úložiště nebo koncového bodu služby cloud. Můžete však použít jiný subdomény ze stejné domény pro koncové body jinou službu. Různé subdomény můžete namapovat také stejné koncového bodu CDN.

K mapování vaši vlastní doménu na koncový bod CDN, použijte jednu z následujících možností:

- Možnost 1: Přímé mapování. Pokud žádný provoz produkční běží na vlastní domény, můžete namapovat vlastní doménu pro koncový bod CDN přímo. Proces mapování vaši vlastní doménu na koncový bod CDN může dojít po krátkou dobu výpadku pro doménu registrace domény na portálu Azure. Zadání mapování CNAME musí být v tomto formátu: 
 
  | JMÉNO             | TYP  | HODNOTA                  |
  |------------------|-------|------------------------|
  | Webová\.consoto.com | CNAME | consoto\.azureedge.net |


- Možnost 2: Mapování s **cdnverify** subdomény. Pokud produkční provoz, který nelze přerušit běží na vlastní domény, můžete vytvořit dočasný mapování CNAME pro koncový bod CDN. Pomocí této možnosti můžete použít Azure **cdnverify** subdomény zajistit na krok zprostředkující registrace tak, aby uživatelé mají přístup k vaší doméně bez přerušení při mapování DNS probíhá.

   1. Vytvořit nový záznam CNAME a zadejte alias subdomény, která zahrnuje **cdnverify** subdomény. Například **cdnverify.www** nebo **cdnverify.cdn**. 
   2. Zadejte název hostitele, který je koncový bod CDN, v následujícím formátu: `cdnverify.<EndpointName>.azureedge.net`. Zadání mapování CNAME musí být v tomto formátu: 

   | JMÉNO                       | TYP  | HODNOTA                            |
   |----------------------------|-------|----------------------------------|
   | cdnverify.WWW\.consoto.com | CNAME | cdnverify.consoto\.azureedge.net | 


## <a name="step-3-enable-the-cname-record-mapping-in-azure"></a>Krok 3: Povolení mapování záznam CNAME ve službě Azure

Po registraci vaši vlastní doménu pomocí jedné z předchozích kroků můžete pak povolit funkci vlastní doménu v Azure CDN. 

1. Přihlaste se k [portál Azure](https://portal.azure.com/) a vyhledejte profil CDN ke koncovému bodu, kterou chcete mapovat k vlastní doméně.  
2. V **profil CDN** okně Vybrat koncový bod CDN, pro který chcete přidružit subdoméně.
3. V levém horním rohu okna koncový bod, klikněte na **vlastní domény**. 

   ![Tlačítko vlastní doménu.](./media/cdn-map-content-to-custom-domain/cdn-custom-domain-button.png)

4. V **vlastní název hostitele** textové pole, zadejte vlastní domény, včetně subdoméně. Například `www.contoso.com` nebo `cdn.contoso.com`.

   ![Přidat dialogové okno vlastní doménu.](./media/cdn-map-content-to-custom-domain/cdn-add-custom-domain-dialog.png)

5. Klikněte na tlačítko **Přidat**.

   Azure ověří, že pro zadaný název domény existuje záznam CNAME. Pokud je záznam CNAME správný, vaše vlastní doména se ověří. Může trvat nějakou dobu záznam CNAME rozšíří do názvové servery. Pokud vaše doména není ověřen okamžitě, ověřte správnost záznam CNAME, počkejte několik minut a zkuste to znovu. Pro **Azure CDN společnosti Verizon** koncových bodů (Standard a Premium), může trvat až 90 minut nastavení vlastní domény na všechny uzly okraj CDN.  


## <a name="step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Krok 4: Ověření, zda vlastní subdomény odkazuje váš koncový bod CDN

Po dokončení registrace vaši vlastní doménu, ověřte, zda vlastní subdomény odkazuje váš koncový bod CDN.
 
1. Ujistěte se, že máte veřejný obsah, který je uloží do mezipaměti na koncový bod. Například pokud je váš koncový bod CDN přidružený účet úložiště, CDN ukládá obsah do mezipaměti v kontejnerech veřejného objektu blob. Pokud chcete otestovat vlastní doménu, ověřte, zda vaše kontejneru je nastavená na veřejný přístup a obsahuje alespoň jeden objekt blob.

2. V prohlížeči přejděte na adresu objektu blob pomocí vlastní doménu. Například, pokud je vaše vlastní doména `cdn.contoso.com`, by mělo být podobné na následující adresu URL adresu URL v mezipaměti objektů blob: `http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg`.


## <a name="step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint"></a>Krok 5 (závislé krok): mapování trvalé vlastní doménu na koncový bod CDN

Tento krok je závislá na krok 2, možnost 2 (mapování s **cdnverify** subdomény). Pokud používáte dočasný **cdnverify** subdomény a ověříte, že funguje, poté můžete namapovat trvalé vlastní domény ke koncovému bodu CDN.

1. Na webu svého poskytovatele domény vytvořte záznam CNAME DNS pro mapování trvalé vlastní domény ke koncovému bodu CDN. Zadání mapování CNAME musí být v tomto formátu: 
 
   | JMÉNO             | TYP  | HODNOTA                  |
   |------------------|-------|------------------------|
   | Webová\.consoto.com | CNAME | consoto\.azureedge.net |
2. Odstranit záznam CNAME s **cdnverify** subdomény, kterou jste vytvořili.

## <a name="see-also"></a>Viz také
[Postup povolení sítě pro doručování obsahu (CDN) pro Azure.](cdn-create-new-endpoint.md)  
[Delegování domény Azure DNS](../dns/dns-domain-delegation.md)