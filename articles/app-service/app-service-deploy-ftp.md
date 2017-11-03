---
title: "Nasazení aplikace do Azure App Service pomocí FTP/S | Microsoft Docs"
description: "Informace o nasazení aplikace do služby Azure App Service pomocí FTP a FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: e3ac2f2156719ad975049b0c2b4cbca81d88e779
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Nasazení aplikace do Azure App Service pomocí FTP/S

Tento článek ukazuje, jak použít protokol FTP nebo FTPS k nasazení vaší webové aplikace, back-end mobilní aplikace nebo aplikaci API [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Koncový bod FTP/S pro aplikace je již aktivní. Žádná konfigurace je nutná pro povolení nasazení FTP/S.

> [!IMPORTANT]
> Budeme průběžně trvá kroky pro zlepšení zabezpečení platforma Microsoft Azure. V rámci této snahy probíhající upgrade webové aplikace je plánovaná pro oblasti Německo centrální a Německo – severovýchod. Během této bude webové aplikace zakazuje použití protokolu ve formátu prostého textu FTP pro nasazení. Naše doporučení na naše zákazníky je přepnout do FTPS pro nasazení. Jsme Nečekejte přerušení k službě během tohoto upgradu, která je plánovaná pro 9/5. Vážíme si můžete podporovat v této úsilí.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Krok 1: Nastavení přihlašovacích údajů nasazení

Pro přístup k serveru FTP pro vaši aplikaci, musíte nejdřív přihlašovací údaje nasazení. 

Chcete-li nastavit nebo obnovit přihlašovací údaje nasazení, přečtěte si téma [přihlašovací údaje nasazení Azure App Service](app-service-deployment-credentials.md). Tento kurz představuje pomocí pověření uživatele.

## <a name="step-2-get-ftp-connection-information"></a>Krok 2: Získání informace o připojení FTP

1. V [portál Azure](https://portal.azure.com), otevřete v této aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Vyberte **přehled** v levé nabídce, poznamenejte si hodnoty pro **uživatel FTP/nasazení**, **název hostitele FTP**, a **FTPS název hostitele**. 

    ![Informace o připojení FTP](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > **Uživatel FTP/nasazení** uživatele hodnotě zobrazené pomocí portálu Azure, včetně názvu aplikace s cílem poskytnout správný kontext pro FTP server.
    > Stejné informace můžete najít, když vyberete **vlastnosti** v levé nabídce. 
    >
    > Navíc se nikdy zobrazí nasazení heslo. Pokud zapomenete heslo nasazení, přejděte zpátky k [krok 1](#step1) a resetování hesla nasazení.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Krok 3: Nasazení souborů do Azure

1. Z vašeho klienta FTP ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client)atd), použijte informace o připojení, které jste shromáždili pro připojení k vaší aplikace.
3. Kopírování souborů a jejich odpovídajících adresářovou strukturu pro [ **/web/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo **/lokality/wwwroot/App_Data/úlohy/** adresář pro webové úlohy).
4. Přejděte na adresu URL vaší aplikace k ověření, že aplikace běží správně. 

> [!NOTE] 
> Na rozdíl od [nasazení na základě Git](app-service-deploy-local-git.md), FTP nasazení nepodporuje automatizaci následující nasazení: 
>
> - obnovení závislosti (například NuGet, NPM, PIP a autora automatizaci)
> - kompilace rozhraní .NET binárních souborů
> - generování souboru Web.config (tady je [Node.js příklad](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Musíte obnovit, sestavení a generovat tyto potřebné soubory ručně na místním počítači a nasadit je spolu s vaší aplikace.
>
>

## <a name="next-steps"></a>Další kroky

Pro pokročilejší scénáře nasazení, zkuste [nasazení do Azure s Gitem](app-service-deploy-local-git.md). Na základě Git nasazení do Azure umožňuje verzí, obnovení balíčků, MSBuild a další.

## <a name="more-resources"></a>Další materiály

* [Přihlašovací údaje pro nasazení služby Azure App Service](app-service-deploy-ftp.md)
