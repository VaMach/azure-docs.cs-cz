<properties
    pageTitle="Webové role a role pracovních procesů Pythonu při použití nástrojů Python Tools 2.2 for Visual Studio | Microsoft Azure"
    description="Přehled vytváření cloudových služeb Azure včetně webových rolí a rolí pracovního procesu pomocí nástrojů Python Tools pro Visual Studio"
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/30/2015"
    ms.author="adegeo"/>




# Webové role a role pracovních procesů Pythonu při použití nástrojů Python Tools 2.2 for Visual Studio

Tento článek obsahuje přehled používání webových rolí a rolí pracovních procesů pomocí nástrojů [Python Tools for Visual Studio][].

## Požadavky

 - Visual Studio 2013 nebo 2015
 - [Python Tools 2.2 for Visual Studio][] (PTVS)
 - [Azure SDK Tools for VS 2013][] nebo [Azure SDK Tools for VS 2015][]
 - [Python 2.7 (32bitová verze)][] nebo [Python 3.4 (32bitová verze)][]

[AZURE.INCLUDE [create-account-and-websites-note](../includes/create-account-and-websites-note.md)]

## Co jsou webové role a role pracovních procesů Pythonu?

Azure nabízí tři výpočetní modely pro provozování aplikací: [funkce Web Apps ve službě Azure App Service][prováděcí model – weby], [Azure Virtual Machines][prováděcí model – vms] a [Azure Cloud Services][prováděcí model – cloudové služby]. Všechny tři modely podporují Python. Služby Cloud Services, které obsahují webové role a role pracovních procesů, zajišťují *PaaS (Platform as a Service)*. Webová role v rámci cloudové služby poskytuje vyhrazený webový server Internetové informační služby (IIS) pro hostování front-endových webových aplikací, zatímco role pracovních procesů dokážou spouštět asynchronní, dlouhotrvající nebo trvalé úlohy, které jsou nezávislé na vstupu nebo interakci uživatelů.

Další informace najdete v článku [Co je cloudová služba?].

> [AZURE.NOTE] *Chcete si vytvořit jednoduchý web?*
Pokud váš scénář zahrnuje jen jednoduchý front-endový web, zvažte použití odlehčené funkce Web Apps v rámci Azure App Service. V případě potřeby budete moct snadno upgradovat na Cloud Service, až se váš web rozroste a vaše požadavky se změní. Ve <a href="/develop/python/">středisku pro vývojáře programující v Pythonu</a> najdete články věnované vývoji funkce Web Apps v rámci Azure App Service.
<br />


## Vytvoření projektu

V sadě Visual Studio můžete v dialogovém okně **Nový projekt** v části **Python** vybrat **Cloudová služba Azure**.

![Dialogové okno Nový projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

V průvodci cloudovou službou Azure můžete vytvořit novou webovou roli a roli pracovního procesu.

![Dialog Cloudová služba Azure](./media/cloud-services-python-ptvs/new-service-wizard.png)

Součástí šablony role pracovního procesu je často používaný kód k připojení k účtu služeb Azure Storage nebo Azure Service Bus.

![Řešení cloudových služeb](./media/cloud-services-python-ptvs/worker.png)

Do existující cloudové služby můžete kdykoli přidat webovou roli nebo roli pracovního procesu.  Můžete je buď přidat do existujících projektů ve vašem řešení, nebo můžete vytvořit nové.

![Příkaz pro přidání role](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Cloudové služby můžou obsahovat role implementované v různých jazycích.  Můžete mít například webovou roli Pythonu implementovanou pomocí rolí pracovního procesu Django, Python nebo C#.  Mezi svými rolemi můžete snadno komunikovat pomocí front služeb Storage nebo Service Bus.

## Spuštění v místním prostředí

Pokud si projekt cloudové služby nastavíte jako projekt po spuštění a stisknete F5, spustí se cloudová služba v emulátoru místního prostředí Azure.

Nástroje PTVS sice podporují spouštění v emulátoru, nebude ale fungovat ladění (například zarážky).

Když budete chtít webové role a role pracovních procesů ladit, můžete jako projekt po spuštění nastavit projekt role a ladit místo toho ten.  Můžete také nastavit více projektů po spuštění.  Klikněte pravým tlačítkem myši na řešení a pak vyberte **Nastavit projekty po spuštění**.

![Vlastnosti projektu po spuštění pro řešení](./media/cloud-services-python-ptvs/startup.png)

## Publikování aplikací do Azure

Když budete chtít aplikaci publikovat, klikněte pravým tlačítkem na projekt cloudové služby v řešení a pak vyberte **Publikovat**.

![Přihlášení pro publikování v Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

Na stránce nastavení vyberte cloudovou službu, do které chcete aplikaci publikovat.

![Nastavení publikování v Microsoft Azure](./media/cloud-services-python-ptvs/publish-settings.png)

Pokud ještě nemáte dostupnou žádnou cloudovou službu, můžete si vytvořit novou.

![Dialog Vytvořit cloudovou službu](./media/cloud-services-python-ptvs/publish-create-cloud-service.png)

Je také užitečné povolit připojení k počítačům pomocí Vzdálené plochy pro chyby ladění.

![Dialogové okno Konfigurace vzdálené plochy](./media/cloud-services-python-ptvs/publish-remote-desktop-configuration.png)

Po dokončení nastavení konfigurace klikněte na **Publikovat**.

V okně výstupu uvidíte průběh a pak se zobrazí okno Protokoly aktivit Microsoft Azure.

![Okno Protokoly aktivit Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

Pár minut bude probíhat nasazování a pak už vám na Azure začne běžet webová role a role pracovního procesu.

## Další kroky

Další podrobné informace o práci s webovými rolemi a rolemi pracovních procesů v nástrojích Python Tools for Visual Studio najdete v dokumentaci k těmto nástrojům:

- [Projekty cloudových služeb][]

Další podrobnosti o používání služeb Azure z vaší webové role a role pracovního procesu, například pomocí služeb Azure Storage nebo Azure Service Bus, najdete v následujících článcích.

- [Služba objektů blob][]
- [Služba Table][]
- [Služba front][]
- [Fronty služby Service Bus][]
- [Témata služby Service Bus][]


<!--Link references-->

[Co je cloudová služba?]: ./cloud-services/cloud-services-choose-me.md
[prováděcí model – weby]: ./app-service-web/app-service-web-overview.md
[prováděcí model – vms]: ./virtual-machines/virtual-machines-windows-about.md
[prováděcí model – cloudové služby]: ./cloud-services/cloud-services-choose-me.md
[Středisko pro vývojáře programující v Pythonu]: /develop/python/

[Služba objektů blob]: ./storage/storage-python-how-to-use-blob-storage.md
[Služba front]: ./storage/storage-python-how-to-use-queue-storage.md
[Služba Table]: ./storage/storage-python-how-to-use-table-storage.md
[Fronty služby Service Bus]: ./service-bus/service-bus-python-how-to-use-queues.md
[Témata služby Service Bus]: ./service-bus/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Dokumentace k nástrojům Python Tools for Visual Studio]: http://aka.ms/ptvsdocs
[Projekty cloudových služeb]: http://go.microsoft.com/fwlink/?LinkId=624028
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517191



<!--HONumber=Jun16_HO2-->


