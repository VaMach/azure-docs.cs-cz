---
title: "Mapování obsahu Azure CDN k vlastní doméně | Microsoft Docs"
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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: cd6db44f7776859d1e6a893543cf0666182ca41a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="map-azure-cdn-content-to-a-custom-domain"></a>Mapování obsahu Azure CDN k vlastní doméně
Chcete-li použít vlastní název domény v adresách URL obsahu v mezipaměti, místo používání subdoména azureedge.net můžete namapovat vlastní doménu pro koncový bod CDN.

Pro mapování vaši vlastní doménu na koncový bod CDN dvěma způsoby:

1. [Vytvořte záznam CNAME u vašeho registrátora domény a mapování vlastních domén a subdomén na koncový bod CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint).
   
    Záznam CNAME je funkce DNS, která mapuje zdrojovou doménu, například `www.contosocdn.com` nebo `cdn.contoso.com`, na cílovou doménu. V takovém případě je ve zdrojové doméně vlastních domén a subdomén (subdoména, například **www** nebo **cdn** vždy je nutné). Cílová doména je koncový bod CDN.  
   
    Registrace domény na portálu Azure může, ale proces mapování vaši vlastní doménu na koncový bod CDN způsobit na krátkou dobu výpadku pro doménu.
2. [Přidat na krok zprostředkující registrace s **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)
   
    Pokud vaše vlastní doména je aktuálně podporuje aplikace s smlouvu úrovně služeb (SLA), který vyžaduje, aby existuje bez výpadku, pak můžete použít Azure **cdnverify** subdomény zajistit na krok zprostředkující registrace tak, aby uživatelé budou mít přístup k vaší doméně při mapování DNS probíhá.  

Po dokončení registrace vaši vlastní doménu pomocí jedné z výše uvedených postupů, budete chtít [ověřte, zda vlastní subdomény odkazuje váš koncový bod CDN](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [!NOTE]
> Musíte vytvořit záznam CNAME u vašeho registrátora domény chcete namapovat doménu k koncového bodu CDN. Záznamy CNAME mapování specifické subdomény, jako `www.contoso.com` nebo `cdn.contoso.com`. Není možné mapovat záznam CNAME do kořenové domény, jako například `contoso.com`.
> 
> Subdoména lze přidružit pouze jeden koncový bod CDN. Záznam CNAME, který vytvoříte bude směrovat veškerý provoz adresovaných subdoméně zadaný koncový bod.  Například Pokud přidružíte `www.contoso.com` s koncový bod CDN, pak nelze přidružíte k jiné Azure koncových bodů, jako je koncový bod účtu úložiště nebo koncového bodu služby cloud. Můžete však použít jiný subdomény ze stejné domény pro koncové body jinou službu. Různé subdomény můžete namapovat také stejné koncového bodu CDN.
> 
> Pro **Azure CDN společnosti Verizon** (Standard a Premium) koncových bodů, Všimněte si, že trvá až **90 minut** pro vlastní doménu změny rozšíří do CDN okraj uzly.
> 
> 

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Zaregistrovat vlastní doménu pro koncový bod Azure CDN
1. Přihlaste se [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **Procházet**, pak **profilů CDN**, potom profil CDN s koncovým bodem, kterou chcete mapovat k vlastní doméně.  
3. V **profil CDN** okně klikněte na koncový bod CDN, pro který chcete přidružit subdoméně.
4. V horní části okna koncový bod, klikněte na položku **přidat vlastní doménu** tlačítko.  V **přidat vlastní doménu** okně uvidíte názvu hostitele koncového bodu, odvozené od váš koncový bod CDN používat při vytváření nového záznamu CNAME. Formát adresy název hostitele se zobrazí jako  **&lt;EndpointName >. azureedge.net**.  Tento název hostitele pro použití v vytváření záznam CNAME, můžete zkopírovat.  
5. Přejděte na web registrátora vaší domény a vyhledejte část pro vytváření záznamů DNS. Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.
6. Najděte část pro správu záznamů CNAME. Možná bude nutné přejít na stránku rozšířeného nastavení a hledat slova jako CNAME, Alias nebo Subdomény.
7. Vytvořit nový záznam CNAME, který mapuje vaši zvolenou subdomény (například **www** nebo **cdn**) k názvu hostitele součástí **přidat vlastní doménu** okno. 
8. Vraťte se na **přidat vlastní doménu** okně a zadejte vlastní domény, včetně subdomény, v dialogovém okně. Například zadejte název domény ve formátu `www.contoso.com` nebo `cdn.contoso.com`.   
   
   Azure ověří, zda existuje záznam CNAME pro název domény, který jste zadali. Pokud je záznam CNAME správný, vaše vlastní doména se ověří.  Pro **Azure CDN společnosti Verizon** koncových bodů (Standard a Premium), může trvat až 90 minut rozšíření pro všechny uzly okraj CDN, ale nastavení vlastní domény.  
   
   Všimněte si, že v některých případech může trvat dobu záznam CNAME rozšíří do názvové servery na Internetu. Pokud vaše doména není ověřený okamžitě, a budete mít dojem, že záznam CNAME je správná, počkejte několik minut a zkuste to znovu.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Zaregistrovat vlastní doménu pro koncový bod Azure CDN pomocí subdoméně zprostředkující cdnverify
1. Přihlaste se [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **Procházet**, pak **profilů CDN**, potom profil CDN s koncovým bodem, kterou chcete mapovat k vlastní doméně.  
3. V **profil CDN** okně klikněte na koncový bod CDN, pro který chcete přidružit subdoméně.
4. V horní části okna koncový bod, klikněte na položku **přidat vlastní doménu** tlačítko.  V **přidat vlastní doménu** okně uvidíte názvu hostitele koncového bodu, odvozené od váš koncový bod CDN používat při vytváření nového záznamu CNAME. Formát adresy název hostitele se zobrazí jako  **&lt;EndpointName >. azureedge.net**.  Tento název hostitele pro použití v vytváření záznam CNAME, můžete zkopírovat.
5. Přejděte na web registrátora vaší domény a vyhledejte část pro vytváření záznamů DNS. Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.
6. Najděte část pro správu záznamů CNAME. Možná budete muset přejít na stránku Upřesnit nastavení a vyhledejte slova **CNAME**, **Alias**, nebo **subdomény**.
7. Vytvořit nový záznam CNAME a zadejte alias subdomény, která zahrnuje **cdnverify** subdomény. Například subdomény, který zadáte, bude ve formátu **cdnverify.www** nebo **cdnverify.cdn**. Potom zadejte název hostitele, který je koncový bod CDN, ve formátu **cdnverify.&lt; EndpointName >. azureedge.net**. Vaše mapování DNS by měl vypadat podobně jako:`cdnverify.www.consoto.com   CNAME   cdnverify.consoto.azureedge.net`  
8. Vraťte se na **přidat vlastní doménu** okně a zadejte vlastní domény, včetně subdomény, v dialogovém okně. Například zadejte název domény ve formátu `www.contoso.com` nebo `cdn.contoso.com`. Všimněte si, že v tomto kroku není nutné adresa subdoméně s **cdnverify**.  
   
    Azure ověří, zda existuje záznam CNAME pro cdnverify název domény, který jste zadali.
9. V tomto okamžiku se neověří vaše vlastní doména Azure, ale provoz k vaší doméně není ještě směrování koncový bod CDN. Po čekání dostatečně dlouho na povolit nastavení vlastní domény rozšíří do CDN okraj uzly (90 minut **Azure CDN společnosti Verizon**, 1 – 2 minuty pro **Azure CDN společnosti Akamai**), vraťte se na webu registrátora DNS a vytvořit další záznam CNAME, který se mapuje na koncový bod CDN vaší subdomény. Můžete například zadat subdoméně jako **www** nebo **cdn**a název hostitele jako  **&lt;EndpointName >. azureedge.net**. Pro tento krok je dokončena registrace vaši vlastní doménu.
10. Nakonec můžete odstranit záznam CNAME, který jste vytvořili pomocí **cdnverify**, stejně jako tomu bylo nezbytné pouze jako zprostředkující krok.  

## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Ověřte, zda vlastní subdomény odkazuje váš koncový bod CDN
* Po dokončení registrace vaši vlastní doménu, můžete mít přístup k obsahu, který je uložený v mezipaměti v koncový bod CDN pomocí vlastní domény.
  Nejprve ověřit, že máte veřejný obsah, který je uloží do mezipaměti na koncový bod. Například pokud je váš koncový bod CDN přidružený účet úložiště, CDN ukládá obsah do mezipaměti v kontejnerech veřejného objektu blob. Pokud chcete otestovat vlastní domény, ujistěte se, že vaše kontejneru je nastavená na veřejný přístup a obsahuje alespoň jeden objekt blob.
* V prohlížeči přejděte na adresu objektu blob pomocí vlastní domény. Například, pokud je vaše vlastní doména `cdn.contoso.com`, bude vypadat podobně jako následující adresu URL adresu URL v mezipaměti objektů blob: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Viz také
[Postup povolení sítě pro doručování obsahu (CDN) pro Azure.](cdn-create-new-endpoint.md)  

