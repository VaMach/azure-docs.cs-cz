---
title: Co jsou Mobile Apps
description: "Zjistěte, jaké výhody App Service přináší pro vaše firemní mobilní aplikace."
services: app-service\mobile
documentationcenter: 
author: adrianhall
manager: yochayk
editor: 
ms.assetid: 4e96cb9d-a632-4cf6-8219-0810d8ade3f9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 43ad434b21fd358b8d72702445126dea5ce179cb


---
# <a name="getting-started"> </a>Co jsou Mobile Apps?
Azure App Service nabízí plně spravovanou [platformu jako službu](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) pro profesionální vývojáře. Přináší bohatou sadu možností pro webové, mobilní a integrační scénáře. Služba *Mobile Apps* v *Azure App Service* nabízí vysoce škálovatelnou a globálně dostupnou platformu pro vývoj mobilních aplikací určenou pro vývojáře a integrátory systémů ve velkých firmách. Přináší bohatou sadu funkcí pro vývojáře pro mobilní zařízení.

![Mobile Apps](./media/app-service-mobile-value-prop/overview.png)

## <a name="why-mobile-apps"></a>Proč Mobile Apps?
Služba *Mobile Apps* v *Azure App Service* nabízí vysoce škálovatelnou a globálně dostupnou platformu pro vývoj mobilních aplikací určenou pro vývojáře a integrátory systémů ve velkých firmách. Přináší bohatou sadu funkcí pro vývojáře pro mobilní zařízení. S Mobile Apps můžete:

* **Sestavovat nativní a multiplatformní aplikace** – ať již vytváříte nativní aplikace pro iOS, Android a Windows, nebo multiplatformní aplikace pomocí Xamarinu či Cordovy (Phonegap), můžete využívat výhod App Service a nativních sad SDK.
* **Nastavit připojení k firemním systémům** – s Mobile Apps můžete v řádu minut přidat firemní přihlašování a připojovat se k lokálním nebo cloudovým firemním prostředkům.
* **Vytvářet aplikace připravené pro offline režim se synchronizací dat** – produktivitu mobilních pracovníků zajistíte sestavením aplikací, které pracují offline a využívají Mobile Apps k synchronizaci dat na pozadí, když se obnoví připojení se zdroji firemních dat nebo s rozhraními API SaaS.
* **Odesílat nabízená oznámení milionům příjemců v řádu sekund** – zákazníky oslovíte pomocí rychlých nabízených oznámení, která můžete odesílat na jakékoli zařízení, personalizovat podle jejich potřeb a zasílat v nejvhodnější dobu.

## <a name="mobile-app-features"></a>Funkce mobilních aplikací
Pro vývoj mobilních řešení využívajících cloud jsou důležité následující funkce:

* **Ověřování a autorizace** – vyberte si ze stále se rozšiřujícího seznamu zprostředkovatelů identity, mezi které patří Azure Active Directory pro podnikové ověřování i služby sociálních sítí, jako je Facebook, Google, Twitter a účet Microsoft.  Služba Azure Mobile Apps poskytuje službu OAuth 2.0 pro každého zprostředkovatele.  Můžete také integrovat sadu SDK pro daného zprostředkovatele, aby se zajistila specifická funkcionalita.
  
  Projděte si další informace o našich [funkcích ověřování].
* **Přístup k datům** – Služba Azure Mobile Apps poskytuje zdroj dat OData verze&3;, který skvěle funguje s mobilními zařízeními a je propojen se službou SQL Azure nebo místním systémem SQL Server.  Tato služba může být založena na Entity Framework, což vám umožní snadnou integraci s jinými zprostředkovateli dat NoSQL i SQL, včetně [Azure Table Storage], MongoDB, [DocumentDB] a rozhraní API pro SaaS, jako je Office 365 a Salesforce.com.
* **Synchronizace offline dat** – naše klientské sady SDK usnadňují vytváření robustních a rychle reagujících mobilních aplikací, které budou pracovat s offline datovou sadou. Tu pak lze automaticky synchronizovat s back-endovými daty, a to i s podporou řešení konfliktů.
  
  Projděte si další informace o našich [datových funkcích].
* **Nabízená oznámení** – naše klientské sady SDK se hladce integrují s registračními funkcemi Azure Notification Hubs, což vám umožní současně zasílat nabízená oznámení milionům uživatelů.
  
  Projděte si další informace o našich [funkcích pro nabízená oznámení].
* **Klientské sady SDK** – poskytujeme kompletní paletu klientských sad SDK, které zahrnují nativní vývoj ([iOS], [Android] a [Windows]), multiplatformní vývoj ([Xamarin pro iOS a Android], [Xamarin Forms]) a vývoj hybridních aplikací ([Apache Cordova]).  Všechny klientské sady SDK jsou k dispozici s licencí MIT a jako open-source.

## <a name="azure-app-service-features"></a>Funkce Azure App Service
U systémů pro mobilní zařízení v provozním prostředí jsou obecně užitečné následující funkce:

* **automatické škálování** – App Service umožňuje rychlé vertikální nebo horizontální navýšení kapacity, aby bylo možné zvládnout jakoukoli zátěž vyplývající z příchozích zákaznických požadavků. Počet a velikost virtuálních počítačů můžete vybrat ručně nebo je možné u back-endu mobilní aplikace nastavit automatické škálování podle plánu nebo momentální zátěže.
  
  Projděte si další informace o [automatické škálování].
* **přípravná prostředí** – App Service umožňuje spuštění několika verzí webu, takže můžete provádět testování A/B, testovat v provozním prostředí v rámci většího plánu DevOps, jakož i lokálně a fázovaně připravovat nový back-end.
  
  Projděte si další informace o [přípravná prostředí].
* **Průběžné nasazování** – App Service umožňuje integraci s běžnými systémy SCM, což vám umožní automaticky nasadit novou verzi back-endu formou větvení tohoto systému.
  
  Projděte si další informace o [možnostech nasazení].
* **Virtuální sítě** – App Service umožňuje připojení k místním prostředkům přes virtuální síť, ExpressRoute nebo hybridní připojení.
  
  Projděte si další informace pro [hybridní připojení], [virtuální sítě] a [ExpressRoute].
* **Izolovaná/vyhrazená prostředí** – App Service je možné spustit v plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service ve velkém měřítku.  Je to ideální pro aplikační procesy vyžadující velmi velký rozsah, izolaci nebo zabezpečený přístup.
  
  Projděte si další informace o [Prostředí App Service].

## <a name="getting-started"></a>Začínáme
Pokud chcete začít s Mobile Apps, postupujte podle kurzu [Začínáme].  Ten přináší základní informace o vytvoření mobilního back-endu a klienta podle vašeho výběru a následné integraci ověřování, offline synchronizace a nabízených oznámení.  Kurz [Začínáme] si můžete projít několikrát – pro každou klientskou aplikaci.

Pokud chcete další informace o Azure Mobile Apps, nahlédněte prosím do naší [mapa kurzů].
Další informace o platformě Azure App Service najdete v článku [Azure App Service].

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/mobile/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

<!-- URLs. -->
[Migrate your Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Začínáme]: app-service-mobile-ios-get-started.md
[Azure Table Storage]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[funkcích ověřování]: ./app-service-mobile-auth.md
[datových funkcích]: ./app-service-mobile-offline-data-sync.md
[funkcích pro nabízená oznámení]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin pro iOS a Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[automatické škálování]: ../app-service-web/web-sites-scale.md
[přípravná prostředí]: ../app-service-web/web-sites-staged-publishing.md
[možnostech nasazení]: ../app-service-web/web-sites-deploy.md
[hybridní připojení]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[virtuální sítě]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[Prostředí App Service]: ../app-service-web/app-service-app-service-environment-intro.md
[mapa kurzů]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/



<!--HONumber=Jan17_HO3-->


