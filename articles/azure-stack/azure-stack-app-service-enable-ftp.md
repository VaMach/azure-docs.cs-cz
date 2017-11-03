---
title: "Povolit FTP ve službě App Service v Azure zásobníku | Microsoft Docs"
description: "Kroky k dokončení povolení FTP ve službě App Service v Azure zásobníku"
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/6/2017
ms.author: erikje
ms.openlocfilehash: 9cadc57831ac7f7e5d32b10a4a87dab3fac02958
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="enable-ftp-in-app-service-on-azure-stack"></a>Povolení FTP ve službě App Service v Azure Stacku

Pokud chcete povolit publikování FTP tak, aby klientům můžete nahrát svoje soubory aplikace a obsah, úspěšně jste nasadili v zásobníku Azure App Service, budete některé další kroky, které je potřeba dokončit.  V budoucích verzích se automatizované tyto kroky.

> [!NOTE]
> Tyto kroky jsou u služby nebo konfigurace aplikační služby pro poskytovatele prostředků Azure zásobníku Enterprise Administrators.

## <a name="enable-ftp"></a>Povolit FTP

1.  Přihlaste se k portálu Azure zásobníku jako správce služeb.
2.  Přejděte do **síťových rozhraní** a vyberte **FTP-NIC** pod **skupiny prostředků** - **místní služby App Service**. ![Síťová rozhraní Azure zásobníku][1]
3.  Poznámka: **veřejnou IP adresu** z **FTP-NIC**. 
![Podrobnosti o rozhraní síti Azure zásobníku][2]
4.  Dále přejděte do **virtuální počítače** a vyberte **FTP0-VM**. ![Virtuální počítače Azure zásobníku][3]
5.  Otevřete relaci vzdálené plochy na virtuální počítač pomocí **Connect** tlačítko a přihlaste se k relaci pomocí přihlašovacích údajů správce můžete nastavit během nasazení služby App Service.  
![Podrobnosti virtuálního počítače Azure zásobníku][4]
6.  Otevřete **Správce Internetové informační služby (IIS)** na virtuálním počítači FTP (FTP0 virtuálních počítačů).
7.  V části **lokality** vyberte **hostování serveru FTP**.
8.  Otevřete **podporu brány FTP Firewall**. ![Správce služby IIS na FTP0 virtuálních počítačů služby App Service][5]
9.  Zadejte veřejnou IP adresu serveru FTP seskupování a klikněte na tlačítko **použít** ![podpora brány Firewall FTP Správce služby IIS][6]

## <a name="validate-the-enabling-of-ftp"></a>Ověření povolení FTP

1.  Přihlaste se k portálu Azure zásobníku jako správce služeb nebo jako klient.
2.  Přejděte do **App Services** a vyberte Web, mobilní nebo jste vytvořili aplikaci API. ![App Services][7]
3.  V poznámce podrobnosti o aplikaci **název hostitele FTP** a **uživatelské jméno FTP/nasazení**. ![Podrobnosti o aplikaci služby App Service][8]
> [!NOTE]
> Pokud nevidíte položky pod **uživatelské jméno FTP/nasazení**, je nutné nastavit na nasazení pověření první pomocí **přihlašovací údaje nasazení** okno.

4.  Otevřete Průzkumníka Windows, zadejte název hostitele FTP do souboru adresního například ftp://ftp.appservice.azurestack.local
5.  Po zobrazení výzvy zadejte **přihlašovací údaje nasazení** jste si poznamenali v kroku 3, pokud byla povolená funkce uvidíte adresářů aplikace služby app obsahu. ![FTP souboru se seznamem][9]
<!--Image references-->
[1]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interfaces.png
[2]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interface-details.png
[3]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines.png
[4]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines-FTP0-VM.png
[5]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager.png
[6]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager-FTP-Firewall-Support.png
[7]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-services.png
[8]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-service-app-detail.png
[9]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-ftp-file-listing.png
