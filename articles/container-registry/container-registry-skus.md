---
title: Kontejner Azure registru SKU
description: "Porovnání mezi různých úrovních služby dostupné v registru kontejner Azure"
services: container-registry
author: stevelas
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 10/16/2017
ms.author: stevelas
ms.openlocfilehash: cf7724dd9e3e870cfd7ec0b5d2e4ea1d0694e9de
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-registry-skus"></a>Kontejner Azure registru SKU

Azure kontejneru registru (ACR) je k dispozici ve více úrovní služeb, označuje jako SKU. Tyto identifikátory SKU zadejte předvídatelná cena a několik možností, jak chcete použít vaší privátní registru Docker v Azure. Výběr vyšší úrovně SKU poskytuje další výkonu a možností škálování. Ale všech skladových položek poskytovat stejné programové funkce povolení vývojář začít pracovat s Basic a převedeno na Standard a Premium jako zvýšení využití registru.

## <a name="basic"></a>Basic
Náklady na optimalizované vstupní bod pro vývojáře, získávání informací o registru kontejner Azure. Základní registrech mít stejné programové funkce jako Standard a Premium (Integrace ověřování služby Azure Active Directory, odstranění bitové kopie a webové háky), ale existují omezení velikosti a využití.

## <a name="standard"></a>Standard
Standardní registrech nabízí stejné funkce jako základní s limity vyšší úložiště a propustnost bitové kopie. Standardní registrech by měl vyhovovat potřebám většině produkčních scénářích.

## <a name="premium"></a>Premium
Premium registrech poskytují vyšší limity na omezení, jako je například úložiště a souběžných operací, povolení vysoký počet scénáře. Kromě vyšší kapacity propustnosti bitové kopie, Premium přidává funkce, jako jsou [geografická replikace](container-registry-geo-replication.md) pro správu jednoho registru v několika oblastech, zachování registru zavřít sítě pro jednotlivá nasazení.

## <a name="classic"></a>Classic
Skladová položka registru Classic povoleno počáteční verzi služby Azure kontejneru registru v Azure. Účet úložiště, který vytvoří Azure v rámci vašeho předplatného, což omezí možnosti pro ACR zajistit vyšší úrovně funkcí, jako je vyšší propustnost a geografická replikace jsou zajišťované Classic registrech. Z důvodu jeho omezené možnosti plánujeme Classic SKU přestat používat v budoucnosti.

> [!NOTE]
> Z důvodu plánované vyřazení registru Classic SKU doporučujeme používat Basic, Standard nebo Premium pro všechny nové registrech. Informace o převodu vaší existující klasické registru najdete v tématu [změna SKU](#changing-skus).
>

## <a name="registry-sku-feature-matrix"></a>Matice funkce skladová položka registru

V následující tabulce jsou funkcí a omezení úrovně služeb Basic, Standard a Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="manage-registry-size"></a>Spravovat velikost registru
Omezení úložiště každý SKU jsou určené k přizpůsobení s Typický scénář: Začínáme Basic, Standard pro většinu výrobní aplikace a Premium velkého rozsahu výkonu a [geografická replikace](container-registry-geo-replication.md). Po celou dobu životnosti registru systému Windows by měla spravovat jeho velikost pravidelně odstraněním nepoužitý obsah.

Aktuální využití registru můžete najít v kontejneru registru **přehled** na portálu Azure:

![Informace o využití registru na portálu Azure](media/container-registry-skus/registry-overview-quotas.png)

Velikost registru systému Windows můžete spravovat odstraňování úložiště na portálu Azure.

V části **služby**, vyberte **úložiště**, klikněte pravým tlačítkem úložiště, které chcete odstranit a pak vyberte **odstranit**.

![Odstranit úložiště na portálu Azure](media/container-registry-skus/delete-repository-portal.png)

## <a name="changing-skus"></a>Změna SKU

SKU registru na portálu Azure, můžete změnit.

V registru **přehled** na portálu Azure vyberte **aktualizace**, pak vyberte nový **SKU** z rozevíracího seznamu SKU.

![Aktualizace registru kontejneru SKU na portálu Azure](media/container-registry-skus/update-registry-sku.png)

## <a name="changing-from-classic"></a>Změna z Classic
Pokud změníte Classic registru na edici Basic, Standard nebo Premium, Azure kopie existující kontejner bitové kopie z přidruženého účtu úložiště v rámci vašeho předplatného na účet úložiště, který spravuje Azure. Tento proces může trvat delší dobu.

Při převodu `docker pull` i nadále fungovat, ale `docker push` je blokován, dokud dokončení převodu.

Po dokončení je účet úložiště předplatné již používán ACR.

### <a name="why-change-from-classic-to-basic-standard-or-premium"></a>Proč změnit Classic na Basic, Standard nebo Premium?

Z důvodu omezené možnosti registrech Classic doporučujeme změnit vaše registrech Classic na úrovně Basic, Standard nebo Premium. Tyto vyšší úrovně SKU hlubšímu integrovat registru do služby Azure. Mezi tyto funkce patří:

* Azure integrace služby Active Directory pro jednotlivé ověřování (viz [az acr přihlášení](/cli/azure/acr?view=azure-cli-latest#az_acr_login))
* Podpora odstranění bitové kopie a značka
* [Geografická replikace](container-registry-geo-replication.md)
* [Webhooky](container-registry-webhook.md)

Většina všech Classic registru závisí na účet úložiště tuto Azure automaticky zřizovat ve vašem předplatném Azure, když jste vytvořili registru. Naopak Basic, Standard a Premium SKU využít výhod *spravované úložiště*. To znamená, Azure transparentně spravuje úložiště obrázků pro vás – účet samostatného úložiště není vytvořená ve svého vlastního předplatného.

Některé z výhod úložiště spravovaný poskytované registrech Basic, Standard a Premium:

* Kontejner Image jsou [zašifrovaná přinejmenším](../storage/common/storage-service-encryption.md).
* Image jsou uložené pomocí [geograficky redundantní úložiště](../storage/common/storage-redundancy.md#geo-redundant-storage), zajišťuje zálohování obrázků s více oblast replikace.
* Schopnost [přesunout mezi SKU](#changing-skus), povolení vyšší propustnost, když zvolíte vyšší úrovně SKU. S každou SKU můžete ACR splňují požadavky propustnost zvýšit vašim potřebám. Základní implementace jak ACR nedosáhne požadované propustnost je vyjádřen jako *záměr* (podle výběru vyšších identifikátorů SKU), aniž byste je museli spravovat podrobnosti implementace.

## <a name="pricing"></a>Ceny

Informace o cenách ve všech SKU registru kontejner Azure, najdete v části [ceny kontejneru registru](https://azure.microsoft.com/pricing/details/container-registry/).

## <a name="next-steps"></a>Další kroky

**Kontejner Azure registru plán**

Přejděte [ACR plán](https://aka.ms/acr/roadmap) na Githubu se najít informace o chystaných funkcí ve službě.

**Kontejner Azure registru UserVoice**

Odeslání a hlasovat o nové funkce návrhů v [ACR UserVoice](https://feedback.azure.com/forums/903958-azure-container-registry).