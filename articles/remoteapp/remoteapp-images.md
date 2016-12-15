---
title: "Co jsou image šablon Azure Remote Appu? | Dokumentace Microsoftu"
description: "Informace o imagích šablon, které jsou součástí Azure RemoteAppu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: fcc53e4073a6957fae263cfb5d38023ed9710d21
ms.openlocfilehash: 21ae6458d1d65698c0c8fc7081696cb7406a918c


---
# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Co jsou image šablon Azure Remote Appu?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Vaše předplatné Azure RemoteAppu obsahuje tři image šablon:

* Windows Server 2012
* Microsoft Office 365 ProPlus (požadováno předplatné služeb Office 365)
* Microsoft Office 2013 Professional Plus (pouze zkušební verze)

> [!IMPORTANT]
> Předplatné Azure RemoteAppu vám uděluje přístup k softwaru v imagích, s výjimkou Office 365 ProPlus, který vyžaduje samostatné předplatné, a Office 2013, který nejde použít v produkčním prostředí. To znamená, že můžete sdílet programy nebo aplikace v imagích šablon s uživateli. Pokud vytvoříte kolekci, která využívá image systému Windows Server 2012 R2, můžete publikovat aplikaci System Center Endpoint Protection uživatelům, kteří k ní pak budou mít přístup přes RemoteApp.
> 
> Další informace naleznete v části [Podrobnosti o licencování RemoteAppu](remoteapp-licensing.md). Informace o licencování Officu najde v části [Použití Officu s Azure RemoteAppem](remoteapp-o365.md).
> 
> 

Přečtěte si podrobnosti o tom, co každý image obsahuje.

## <a name="windows-server-2012-r2-the-vanilla-image"></a>Windows Server 2012 R2 (dále jen image vanilla)
Tento image je založený na operačním systému Microsoft Windows Server 2012 R2 Datacenter a má nainstalované následující role a funkce, aby splnil požadavky pro image šablony Azure RemoteApp:

* Rozhraní .NET Framework 4.5, 3.5.1, 3.5
* Desktopové prostředí
* Služba podpory rukopisu
* Media Foundation
* Hostitel relace vzdálené plochy
* Windows PowerShell 4.0
* Integrované skriptovací prostředí (ISE) v prostředí Windows PowerShell
* Podpora subsystémů WoW64

Tento image má také nainstalované následující aplikace:

* Adobe Flash Player
* Microsoft Silverlight
* Microsoft System Center 2012 Endpoint Protection
* Microsoft Windows Media Player

## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (požadován odběr)
Office 365 je nejpožadovanější aplikace, takže jsme vytvořili „vlastní“ image, se kterým můžete pracovat.

Tento image je rozšířením image vanilla a kromě komponent popsaných v imagi Windows Server 2012 R2 má nainstalované následující komponenty aplikace Microsoft Office 365 ProPlus:

* Access
* Excel
* Lync
* OneNote
* OneDrive pro firmy (použití synchronizačního agenta s Azure RemoteAppem se nepodporuje)
* Outlook
* PowerPoint
* Word
* Nástroje kontroly pravopisu systému Microsoft Office

Image také zahrnuje Visio Pro a Project Pro.

A také následující aplikace:

* Nativní klient SQL
* Ovladač ODBC
* Klient pro získávání dat SQL Server
* Klient MasterDataServices
* Microsoft Publisher
* PowerQuery
* PowerMap

Plná funkčnost aplikací Office 365 ProPlus je k dispozici pouze pro uživatele, kteří mají plán Office 365 ProPlus. Další podrobnosti o plánu předplatného aplikace Office 365 najdete v části [Plány služeb Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Máte další otázky? Přečtěte si informace v části [Office 365 + RemoteApp](remoteapp-o365.md). Také si přečtěte nový článek [Použití předplatného služeb Office 365 s Azure RemoteAppem](remoteapp-officesubscription.md).

Upozorňujeme, že k aplikacím Office 365 ProPlus, Visio Pro a Project Pro potřebujete samostatnou licenci (každá má svou vlastní).

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (pouze zkušební verze)
Během bezplatné zkušební doby můžete testovat službu s imagem Office 2013.

Tento image je rozšířením image vanilla a kromě komponent popsaných v imagi Windows Server 2012 R2 má nainstalované následující komponenty aplikace Microsoft Office 2013 Professional Plus:

* Access
* Excel
* Lync
* OneNote
* OneDrive pro firmy (použití synchronizačního agenta s Azure RemoteAppem se nepodporuje)
* Outlook
* PowerPoint
* Project
* Visio
* Word
* Nástroje kontroly pravopisu systému Microsoft Office

> [!IMPORTANT]
> **Právní informace:** Tento image neobsahuje licenci aplikace Microsoft Office a*nelze ho použít pro produkci*. Image aplikace Professional Plus 2013 Office je určen pouze pro zkušební verzi. Pokud chcete používat aplikace Office v Azure RemoteAppu pro produkci, je třeba použít image Office 365 ProPlus. Další informace o licencování Officu najdete v části [Použití  Officu 365 s Azure RemoteAppem](remoteapp-o365.md).
> 
> 




<!--HONumber=Dec16_HO1-->


