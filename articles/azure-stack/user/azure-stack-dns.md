---
title: "Služba DNS v Azure zásobníku | Microsoft Docs"
description: DNS v Azure Stacku
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: ac93b5eb4228cef373428b7b69932d5993d54fa0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="dns-in-azure-stack"></a>DNS v Azure Stacku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Azure Stack zahrnuje následující funkce DNS:
* Podpora pro překlad DNS názvu hostitele
* Vytváření a Správa zón DNS a záznamů pomocí rozhraní API

## <a name="support-for-dns-hostname-resolution"></a>Podpora pro překlad DNS názvu hostitele
Můžete zadat název název domény DNS pro prostředek veřejné IP, která vytvoří mapování pro *domainnamelabel.location*. cloudapp.azurestack.external veřejné IP adresy v zásobníku Azure spravované servery DNS.  

Například pokud vytvoříte prostředek veřejné IP s **contoso** jako popisek názvu domény v umístění místní zásobník Azure, plně kvalifikovaný název domény (FQDN) **contoso.local.cloudapp.azurestack.external**přeloží na veřejnou IP adresu prostředku. Chcete-li vytvořit vlastní doménu záznam CNAME, který odkazuje na veřejnou IP adresu v zásobníku Azure můžete použít tento plně kvalifikovaný název domény.

> [!IMPORTANT]
> Každý popisek názvu domény vytvořili musí být jedinečný v rámci svého umístění Azure zásobníku.

Pokud vytvoříte veřejnou IP adresu pomocí portálu, vypadá takto:

![Vytvoření veřejné IP adresy](media/azure-stack-whats-new-dns/image01.png)

Tato konfigurace je užitečná, pokud chcete spojit veřejnou IP adresu s vyrovnáváním zatížení prostředku. Například můžete mít Vyrovnávání zatížení zpracování žádostí z webové aplikace. Nástroje pro vyrovnávání za zatížení je webový server umístěný na jeden nebo více virtuálních počítačů. Nyní můžete zobrazit na webu Vyrovnávání zatížení sítě podle názvu DNS, a nikoli IP adresu.

## <a name="create-and-manage-dns-zones-and-records-using-api"></a>Vytváření a Správa zón DNS a záznamů pomocí rozhraní API
Můžete vytvořit a spravovat zóny DNS a záznamy v zásobníku Azure.  

Zásobník Azure poskytuje službu DNS jako Azure, pomocí rozhraní API, které jsou konzistentní s rozhraními API Azure DNS.  Hostování domény do Azure DNS zásobníku, můžete spravovat svoje záznamy DNS pomocí stejných přihlašovacích údajů, rozhraní API, nástroje, fakturaci a podporu jako jinými službami Azure. 

Infrastruktura DNS zásobník Azure je zřejmé z důvodů, kompaktnější než Azure. Obor, rozsah a výkon, proto závisí na škále nasazení zásobník Azure a prostředí, kde je nasazen.  Ano třeba výkon, dostupnost, globální distribuce a vysokou dostupnost (HA) se může lišit od nasazení k nasazení.

## <a name="comparison-with-azure-dns"></a>Porovnání s Azure DNS
Služba DNS v zásobníku Azure je podobná DNS v Azure, s dvě hlavní výjimky:
* **Nepodporuje záznamů AAAA**

    Azure zásobník nepodporuje záznamů AAAA, protože zásobník Azure nepodporuje adresy IPv6.  Toto je zásadní rozdíl mezi DNS v Azure a Azure zásobníku.
* **Není více klientů**

    Na rozdíl od Azure služba DNS v zásobníku Azure není více klientů. Každý klient proto nelze vytvořit stejné zóny DNS. Pouze první předplatné, které se pokusí o vytvoření zóny úspěšné a neúspěšné následných žádostí.  Jde o známý problém a klíče rozdíl mezi Azure a Azure DNS zásobníku. Tento problém bude vyřešen v budoucí verzi.

Kromě toho jsou některé malé rozdíly v tom, jak Azure DNS zásobníku implementuje značky, Metadata, značky etag binárním rozsáhlým a omezení.

Následující informace platí konkrétně pro Azure DNS zásobníku a se může mírně lišit od Azure DNS. Další informace o službě Azure DNS najdete v tématu [DNS zóny a zaznamenává](../../dns/dns-zones-records.md) v dokumentaci k webu Microsoft Azure.

### <a name="tags-metadata-and-etags"></a>Značky, metadat a značky etag binárním rozsáhlým

**Značky**

Azure DNS zásobníku podporuje pomocí Azure Resource Manager značek na prostředky zóny DNS. Nepodporuje značky na sady záznamů DNS, i když jako alternativu, metadata, je podporovaný na záznam DNS nastaví, jak je popsáno dále.

**Metadata**

Jako alternativu k značky sady záznamů DNS zásobník Azure podporuje, zadávání poznámek k sad záznamů pomocí 'metadat'. Podobně jako u značky, metadata vám umožní přidružit dvojice název hodnota s každou sadu záznamů. Například to může být užitečné k zaznamenání účel každá sada záznamů. Na rozdíl od značky metadata nelze použít k poskytují filtrované zobrazení faktury Azure a nelze zadat v zásadách Azure Resource Manager.

**Značky etag binárním rozsáhlým**

Předpokládejme, že dvě osoby nebo dva procesy zkuste upravit záznam DNS ve stejnou dobu. Které z nich wins? A Vítěz vědět, že jste se přepsat změny vytvořený jiným uživatelem?

Azure DNS zásobníku používá značky etag binárním rozsáhlým bezpečně zpracování souběžných změny do stejného zdroje. Značky etag binárním rozsáhlým jsou oddělené od Azure Resource Manager, značky'. Všechny prostředky DNS (zóny nebo sady záznamů) má Etag s ním spojená. Vždy, když se načte prostředek, je také načíst jeho Značka Etag. Při aktualizaci prostředek, můžete předat zpět značku Etag, můžete Azure zásobníku DNS ověřte, že značku Etag na serveru odpovídá. Vzhledem k tomu, že každá aktualizace na prostředek, výsledkem Značka Etag se znovu vygeneruje, neshoda značek Etag označuje, že došlo ke změně souběžných. Značky etag binárním rozsáhlým mohou sloužit také při vytváření nového prostředku zajistit, že prostředek již neexistuje.

Ve výchozím nastavení používá Azure PowerShell DNS zásobníku značky etag binárním rozsáhlým blokovat souběžných změny zón a sady záznamů. Volitelné *-přepsat* přepínač můžete použít k potlačení kontroly Značka Etag, v takovém případě všechny souběžných budou přepsána změny, které mají došlo k chybě.

Na úrovni rozhraní API REST služby Azure zásobníku DNS jsou značky etag binárním rozsáhlým zadán pomocí hlavičky protokolu HTTP. V následující tabulce je uveden jejich chování:

| Záhlaví | Chování|
|--------|---------|
| Žádný   | PUT vždy úspěšné (žádná značka Etag kontrola)|
| If-match| PUT pouze úspěšná, pokud prostředek existuje a Značka Etag odpovídá|
| If-match *| PUT pouze úspěšná, pokud existuje prostředek|
| If-none-match *| PUT pouze úspěšná, pokud prostředek neexistuje.|

### <a name="limits"></a>Omezení

Následující výchozí omezení platí při použití Azure DNS zásobníku:

| Prostředek| Výchozí omezení|
|---------|--------------|
| Zóny na předplatné| 100|
| Sady záznamů na zóny| 5000|
| Záznamů na sady záznamů| 20|

## <a name="next-steps"></a>Další kroky
[Představení IDN pro Azure zásobníku](azure-stack-understanding-dns.md)
