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
ms.date: 01/06/2016
ms.author: cephalin;dariac
ms.openlocfilehash: fcd079306a8968505349bb3f4a805f203a5c9999
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Nasazení aplikace do Azure App Service pomocí FTP/S

Tento článek ukazuje, jak použít protokol FTP nebo FTPS k nasazení vaší webové aplikace, back-end mobilní aplikace nebo aplikaci API [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Koncový bod FTP/S pro aplikace je již aktivní. Žádná konfigurace je nutná pro povolení nasazení FTP/S.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Krok 1: Nastavení přihlašovacích údajů nasazení

Pro přístup k serveru FTP pro vaši aplikaci, musíte nejdřív přihlašovací údaje nasazení. 

Chcete-li nastavit nebo obnovit přihlašovací údaje nasazení, přečtěte si téma [přihlašovací údaje nasazení Azure App Service](app-service-deployment-credentials.md). Tento kurz představuje pomocí pověření uživatele.

## <a name="step-2-get-ftp-connection-information"></a>Krok 2: Získání informace o připojení FTP

1. V [portál Azure](https://portal.azure.com), otevřete v této aplikaci [prostředků stránky](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Vyberte **přehled** v levé nabídce, poznamenejte si hodnoty pro **uživatel FTP/nasazení**, **název hostitele FTP**, a **FTPS název hostitele**. 

    ![Informace o připojení FTP](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Zajistit správné kontextu pro FTP server **uživatel FTP/nasazení** hodnoty zobrazené na portálu Azure zahrnuje název aplikace.
    > Stejné informace můžete najít, když vyberete **vlastnosti** v levé nabídce. 
    >
    > Navíc se nikdy zobrazí nasazení heslo. Pokud zapomenete heslo nasazení, přejděte zpátky k [krok 1](#step1) a resetování hesla nasazení.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Krok 3: Nasazení souborů do Azure

1. Z vašeho klienta FTP (například [Visual Studio](https://www.visualstudio.com/vs/community/) nebo [FileZilla](https://filezilla-project.org/download.php?type=client)), použijte informace o připojení, které jste shromáždili pro připojení k vaší aplikace.
3. Kopírování souborů a jejich odpovídajících adresářovou strukturu pro [ **/web/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo **/lokality/wwwroot/App_Data/úlohy/** adresář pro webové úlohy).
4. Přejděte na adresu URL vaší aplikace k ověření, že aplikace běží správně. 

> [!NOTE] 
> Na rozdíl od [nasazení na základě Git](app-service-deploy-local-git.md), FTP nasazení nepodporuje automatizaci následující nasazení: 
>
> - závislost obnoví (například NuGet, NPM, PIP a autora automatizaci)
> - kompilace rozhraní .NET binárních souborů
> - generování souboru Web.config (tady je [Node.js příklad](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Generovat tyto potřebné soubory ručně na místním počítači a pak je nasadit společně s vaší aplikace.
>
>

## <a name="next-steps"></a>Další postup

Pro pokročilejší scénáře nasazení, zkuste [nasazení do Azure s Gitem](app-service-deploy-local-git.md). Na základě Git nasazení do Azure umožňuje verzí, obnovení balíčků, MSBuild a další.

## <a name="more-resources"></a>Další materiály

* [Přihlašovací údaje pro nasazení služby Azure App Service](app-service-deploy-ftp.md)
