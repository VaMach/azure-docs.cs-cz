---
title: "Konfigurace oznámení a e-mailových šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak konfigurace oznámení a e-mailových šablon ve službě Azure API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: ec560bbab3caf4cde090ed3c9a47ccc0afcb2492
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Konfigurace oznámení a e-mailových šablon ve službě Azure API Management
API Management nabízí možnost konfigurace oznámení pro konkrétní události a nakonfigurovat e-mailových šablon, které se používají ke komunikaci s správci a vývojáři instance služby API Management. Toto téma ukazuje, jak nakonfigurovat oznámení události, k dispozici a obsahuje základní informace o konfiguraci e-mailových šablon pro tyto události.

## <a name="publisher-notifications"></a>Konfigurace vydavatele oznámení
Chcete-li konfigurovat oznámení, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure. Tím přejdete na portál vydavatele služby API Management.

![Portál vydavatele][api-management-management-console]

> [!NOTE] 
> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management][Create an API Management service instance].

Klikněte na tlačítko **oznámení** z **API Management** nabídky na levé straně, chcete-li zobrazit dostupné oznámení.

![Vydavatele oznámení][api-management-publisher-notifications]

Následující seznam událostí může být nakonfigurována pro oznámení.

* **Žádostí o odběr (které vyžadují schválení)** -příjemců zadané e-mailu a uživatelé dostanou e-mailová oznámení o odběru požadavky pro rozhraní API produkty, které vyžadují schválení.
* **Nová předplatná** -příjemců zadané e-mailu a uživatelé dostanou e-mailová oznámení o nových předplatných rozhraní API produktu.
* **Žádosti o aplikace Galerie** -příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení při nové aplikace se odešlou do Galerie aplikací.
* **SKRYTÁ** -příjemců zadané e-mailu a uživatelé budou obdrží e-mailu skrytá kopii všechny e-maily odesílané pro vývojáře.
* **Nový problém nebo komentář** – příjemců zadané e-mailu a uživatelé dostanou e-mailová oznámení, když nový problém nebo komentář je odesíláno na portál pro vývojáře.
* **Zprávu zavřete účtu** -příjemců zadané e-mailu a uživatelé dostanou e-mailová oznámení po uzavření účtu.
* **Blížící limit kvóty předplatného** -následující příjemců e-mailu a uživatelé obdrží e-mailová oznámení při použití předplatné získá brzy bude dosaženo kvóty využití.

U každé události lze zadat příjemců e-mailu pomocí e-mailovou adresu textového pole nebo můžete vybrat uživatele ze seznamu.

Pokud chcete zadat e-mailové adresy, které mají být informování, zadejte je do textového pole e-mailovou adresu. Pokud máte více e-mailové adresy, oddělte je čárkami.

![Příjemců oznámení][api-management-email-addresses]

K určení uživatelů, aby se zobrazilo oznámení, klikněte na tlačítko **přidat příjemce**, zaškrtněte políčko vedle položky uživatelé upozorněni, a klikněte na **OK**.

> [!NOTE] 
> V seznamu se zobrazí pouze správci.


Po dokončení konfigurace příjemců oznámení, klikněte na tlačítko **Uložit** použít aktualizované oznámení příjemci.

> [!NOTE] 
> Pokud přejdete směrem od **vydavatele oznámení** kartě portálu vydavatele upozorní vás, pokud existují neuložené změny.


## <a name="email-templates"></a>Konfigurovat e-mailových šablon
API Management poskytuje e-mailových šablon pro e-mailové zprávy, které se odesílají při správě a používání služby. Následující e-mailové šablony jsou k dispozici.

* Odeslání aplikace Galerie schválení
* Písmeno farewell vývojáře
* Limit kvóty vývojáře blíží oznámení
* Pozvat uživatele
* Nový komentář přidat problému
* Nový problém přijat
* Nové předplatné aktivované
* Potvrzení odběru obnovit
* Odmítne požadavek na odběr
* Přišel požadavek na předplatné

Tyto šablony se dá změnit podle potřeby.

Chcete-li zobrazit a konfigurovat e-mailových šablon pro vaše instance služby API Management, klikněte na tlačítko **oznámení** z **API Management** nabídky na levé straně a vyberte **e-mailových šablon**kartě.

![Šablony e-mailů][api-management-email-templates]

Chcete-li zobrazit nebo upravit šablonu, vyberte ho z **šablony** rozevíracího seznamu.

![Seznam šablon e-mailů][api-management-email-templates-list]

Každý e-mailové šablony má předmět ve formátu prostého textu a definice text ve formátu HTML. Každá položka se dají přizpůsobit podle potřeby.

![Editor šablony e-mailu][api-management-email-template]

**Parametry** seznam obsahuje seznam parametrů, které v případě vložit do předmětu nebo textu, bude nahrazen určenou hodnotu, při odeslání e-mailu. Pokud chcete vložit parametr, umístěte kurzor, kde chcete parametru přejděte a klikněte na šipku nalevo od názvu parametru.

Klikněte na tlačítko **Preview** nebo **odeslat testovací** zobrazíte jak e-mailu bude vypadat nebo odeslat testovací e-mail.

> [!NOTE] 
> Parametry nejsou nahrazené skutečnými hodnotami při zobrazení náhledu nebo odeslání testu.

Chcete-li uložit změny do šablony e-mailu, klikněte na tlačítko **Uložit**, nebo zrušit změny, klikněte na tlačítko **zrušit**.
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
