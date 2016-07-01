<properties
    pageTitle="Licencování Azure RemoteAppu | Microsoft Azure"
    description="Přečtěte si, jak funguje licencování v Azure RemoteAppu."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/18/2016"
    ms.author="elizapo" />


# Jak v Azure RemoteAppu funguje licencování?


Takže jste si nastavili službu Azure RemoteApp, vytvořili šablony a jste nyní připraveni publikovat aplikace pro vaše uživatele. Ale zbývá dořešit ještě jednu věc – licencování. Jak tedy funguje licencování pro RemoteApp a pro aplikace, které sdílíte prostřednictvím RemoteAppu?

RemoteApp nevyžaduje žádné licence Windows ani licence CAL pro Vzdálenou plochu. Licenci pro samotnou službu RemoteApp pokryje vaše předplatné. (Podívejte se na podrobnosti [cenových možností](https://azure.microsoft.com/pricing/details/remoteapp).)

Pokud použijete jeden z imagů, které jsou součástí vašeho předplatného, můžete sdílet jakékoli aplikace nainstalované v tomto imagi bez nutnosti samostatné licence. Pokud například k sestavení své kolekce použijete image šablony Windows Server 2012 R2, můžete s uživateli sdílet aplikaci System Center Endpoint Protection. Jedinou výjimkou tohoto pravidla jsou image Office 365 ProPlus (vyžaduje samostatné předplatné) a Office 2013 (nelze sdílet v produkční kolekci).

Pokud chcete použít image šablony Office 365, která se dodává s RemoteAppem, musíte mít *existující* plán Office 365 ProPlus. Totéž platí pro všechny aplikace Office 365, které publikujete pomocí vlastní šablony. Tyto aplikace musíte aktivovat pomocí svého vlastního předplatného. To platí pro zkušební i placené předplatné. Pokud chcete použít image šablony Office 365 během zkušební doby *a ještě nemáte předplatné*, přejděte na stránku Office 365 a [zaregistrujte se](https://go.microsoft.com/fwlink/p/?LinkID=403802) pro zkušební předplatné. Další informace najdete v článku [Jak spolu fungují RemoteApp a Office?](remoteapp-o365.md)

Pokud během tohoto zkušebního období nechcete získat zkušební předplatné Office 365, použijte image šablony Office 2013 Professional Plus dodávanou s RemoteAppem. Tento image šablony lze používat pouze 30 dnů a nelze jej převést na placenou kolekci.

Pro jiné aplikace musíte zajistit, že máte licenci pro sdílení aplikace.

To dává smysl, ne? Můžete publikovat jakoukoli aplikaci, kterou jste zákonně oprávněni sdílet. A musíte se ujistit, že máte skutečně právo sdílet své programy.

Upozorňujeme, že v cloudové kolekci nemůžete používat licence CAL ani multilicenční smlouvu. Multilicenční smlouvu *můžete* použít k aktivaci aplikací v hybridní kolekci (kromě aplikací Office). Stačí je jen nainstalovat do image šablony z multilicenčního média. Postupujte podle informací od dodavatele aplikace a nainstalujte licence v prostředí Vzdálené plochy.



<!--HONumber=Jun16_HO2-->


