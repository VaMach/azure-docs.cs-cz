---
title: Co je Azure RemoteApp? | Dokumentace Microsoftu
description: "Přečtěte si, jak můžete prostřednictvím Azure RemoteAppu sdílet aplikace a prostředky se všemi zařízeními."
services: remoteapp
documentationcenter: 
author: lizap
manager: mbaldwin
editor: 
ms.assetid: d7a8a311-e70a-4463-ac85-c7f62c500921
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: elizapo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 95b4e6914de787bb2c25527ce9635f872b4746a6


---
# <a name="what-is-azure-remoteapp"></a>Co je Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp přináší funkce místního programu Microsoft RemoteApp založeného na vzdálené ploše do Azure. Azure RemoteApp pomáhá poskytovat zabezpečený vzdálený přístup k aplikacím z mnoha různých uživatelských zařízení. Azure RemoteApp v podstatě hostuje dočasné relace terminálového serveru v cloudu a umožňuje vám jejich použití a sdílení s uživateli.

S Azure RemoteAppem můžete sdílet aplikace a prostředky s uživateli na téměř jakémkoliv zařízení. Vaše aplikace hostujeme v cloudu, což znamená, že se staráme o hardware a škálování, abychom splnili nároky uživatelů. Stačí, abyste nahráli aplikace, které chcete sdílet, a vaši uživatelé je hned mohou začít používat. [Uživatelé mohou používat svá vlastní zařízení](remoteapp-clients.md), zatímco vy vše spravujete prostřednictvím portálu Azure. K zabezpečení svých aplikací a dat můžete dokonce používat své podnikové přihlašovací údaje.

Přečtěte si další informace o Azure RemoteAppu, nebo, pokud jsme vás už přesvědčili, si ho rovnou [vyzkoušejte](https://azure.microsoft.com/services/remoteapp/).

Máte k Azure RemoteAppu otázky? Podívejte se na odpovědi na [časté otázky](remoteapp-faq.md).

Azure RemoteApp je součástí [infrastruktury virtuálních klientských počítačů Microsoft](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Nové!** Chcete se o Azure RemoteAppu dozvědět více? Nebo chcete ověřit funkci Azure RemoteAppu ve větším měřítku? Připojte se k našemu týdennímu webináři [zeptejte se odborníka](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Kolekce Azure RemoteAppu
Existují dva typy [kolekcí Azure RemoteAppu](remoteapp-collections.md):

* **Cloudová kolekce** je hostována v cloudu a ukládá data programů v cloudu. Uživatelé mohou získat přístup k aplikacím přihlášením pomocí svého účtu Microsoft nebo podnikových přihlašovacích údajů synchronizovaných nebo federovaných se službou Azure Active Directory.
  
    Cloudovou kolekci zvolte, když aplikace, kterou chcete sdílet, nevyžaduje připojení k žádnému prostředku v privátní síti vaší společnosti (například prostřednictvím zařízení VPN). Pokud bude aplikace používat prostředky dostupné na internetu, ve OneDrivu nebo v Azure, cloudová kolekce splní všechna vaše očekávání. Současně se také nejrychleji vytváří.
* **Hybridní kolekce** je hostována v cloudu Azure a také do něj ukládá data, ale kromě toho umožňuje uživatelům přistupovat k datům a prostředkům uloženým v místní síti. Uživatelé mohou získat přístup k aplikacím přihlášením pomocí svých podnikových přihlašovacích údajů synchronizovaných nebo sdružených se službou Azure Active Directory.
  
    Hybridní kolekci zvolte, pokud budete vyžadovat připojení k prostředkům v privátní síti své společnosti. Pokud například aplikace potřebuje přistupovat k některé z následujících položek:
  
  * souborové servery v síti intranet
  * Quicken
  * databáze za bránou firewall
    
    Tento typ kolekce je obecně vzato užitečnější pro velké společnosti s množstvím prostředků ve svých privátních sítích, které nelze přesunout do cloudu.

Různé typy kolekcí nabízejí různé možnosti, včetně sítí, takže je potřeba zamyslet se nad tím, [který typ kolekce](remoteapp-collections.md) je pro vás nejlepší. 

### <a name="updating-your-collection"></a>Aktualizování kolekce
Jedním z hlavní rozdílů mezi hybridními a cloudovými kolekcemi je způsob zpracování aktualizací softwaru. S cloudovou kolekcí, která využívá předinstalovaný image Office 365 ProPlus nebo Office 2013, se nemusíte o žádné aktualizace starat. Služba se sama udržuje a průběžně aktualizuje aplikace i operační systém.

V případě hybridních kolekcí a cloudových kolekcí, které používají vlastní image šablony odpovídáte za údržbu image a aplikací. V případě imagů připojených k doméně řídíte aktualizace pomocí nástrojů, jako jsou služba Windows Update, zásady skupiny nebo System Center.

Po aktualizaci vlastního image šablony nový image odešlete do cloudu Azure a poté aktualizujete kolekci tak, aby nový image používala. (Můžete to provést z Azure RemoteAppu na stránce **Rychlé Zahájení** nebo na řídicím panelu.)

Další informace najdete v článku [Aktualizace kolekce](remoteapp-update.md).

## <a name="supported-remoteapp-clients"></a>Podporované klienty RemoteAppu
Azure RemoteApp je podporován v klientských aplikací RemoteAppu pro Windows a Windows RT a v aplikacích vzdálené plochy Microsoft pro Mac, iOS a Android. Uživatelé mohou pomocí těchto aplikací na svém mobilním nebo výpočetním zařízení přistupovat k novým programům Azure RemoteAppu.

Další informace o klientech najdete v článku [Přístup k aplikacím v Azure RemoteAppu](remoteapp-clients.md).

## <a name="next-steps"></a>Další kroky
Nyní si můžete vše vyzkoušet. Určitě to udělejte! Tyto články vám pomůžou začít s Azure RemoteAppem:

* [Jaký typ kolekce potřebujete pro Azure RemoteApp?](remoteapp-collections.md)
* [Vytvoření image Azure RemoteAppu](remoteapp-imageoptions.md)
* [Vytvoření cloudové kolekce Azure RemoteAppu](remoteapp-create-cloud-deployment.md)
* [Vytvoření hybridní kolekce Azure RemoteAppu](remoteapp-create-hybrid-deployment.md)
* [Jak v Azure RemoteAppu funguje licencování?](remoteapp-licensing.md)
* [Osvědčené postupy používání Azure RemoteAppu](remoteapp-bestpractices.md)
* [Časté otázky k Azure RemoteAppu](remoteapp-faq.md)

### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že kromě hodnocení tohoto článku a přidání komentářů pod článkem také můžete měnit samotný článek? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek článku, klikněte na **Upravit na GitHubu** nebo na **Upravit** a proveďte změny – změny se nám odešlou ke kontrole a po jejich schválení je doplníme do článku.




<!--HONumber=Nov16_HO2-->


