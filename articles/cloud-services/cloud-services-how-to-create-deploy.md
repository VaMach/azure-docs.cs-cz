---
title: "Postup vytvoření a nasazení cloudové služby | Microsoft Docs"
description: "Zjistěte, jak vytvořit a nasadit v Azure pomocí metody rychlého vytvoření cloudové služby."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 2a2172a78bfd3ac923edbc9de366b035629dd27b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Postup vytvoření a nasazení cloudové služby
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-create-deploy.md)
> 
> 

Portál Azure classic nabízí dva způsoby, kterými můžete vytvořit a nasadit cloudové služby: **rychle vytvořit** a **vytvořit vlastní**.

Toto téma vysvětluje, jak lze pomocí metody rychlého vytvoření vytvořit novou cloudovou službu a pak použít **nahrát** nahrání a nasazení balíčku cloudové služby v Azure. Pokud použijete tuto metodu, umožňuje portálu Azure classic k dispozici užitečné odkazy pro dokončení všechny požadavky rovnou. Pokud jste připravení nasadit cloudové služby, když ho vytvoříte, můžete provést jak na současně pomocí **vytvořit vlastní**.

> [!NOTE]
> Pokud hodláte publikovat cloudové služby z Visual Studio Team Services (služby VSTS), použijte **rychle vytvořit**a poté nastavit publikování služby VSTS z **rychlý Start** nebo na řídicím panelu.
> 
> 

## <a name="concepts"></a>Koncepty
Tři součásti jsou nutné k nasazení aplikace jako cloudové služby v Azure:

* **Definice služby**  
  Soubor definice služby (.csdef) cloudu definuje model služeb, včetně počet rolí.
* **Konfigurace služby**  
  Cloudové služby konfiguračního souboru (.cscfg) poskytuje nastavení konfigurace pro cloudové služby a jednotlivé role, včetně počtu instancí role.
* **Balíček služby**  
  Balíček služby (.cspkg) obsahuje kódu aplikace a konfigurace a soubor definice služby.

Další informace o těchto a postup vytvoření balíčku [zde](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Příprava aplikace
Před nasazením cloudové služby, musíte vytvořit balíček cloudové služby (.cspkg) z kódu aplikace a cloudové služby konfiguračního souboru (.cscfg). Azure SDK poskytuje nástroje pro přípravu tyto soubory požadované k nasazení. Můžete nainstalovat sadu SDK z [Azure stáhne](https://azure.microsoft.com/downloads/) stránky v jazyce, ve kterém chcete vyvíjet kódu aplikace.

Funkce služby tři cloudu vyžadovat zvláštní konfigurace před exportem balíčku služby:

* Pokud chcete nasadit cloudovou službu, která používá Secure Sockets Layer (SSL) pro šifrování dat [konfiguraci aplikace](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) pro protokol SSL.
* Pokud chcete nakonfigurovat připojení ke vzdálené ploše do instance rolí, [konfiguraci rolí](cloud-services-role-enable-remote-desktop.md) pro vzdálenou plochu.
* Pokud chcete nakonfigurovat podrobné monitorování pro cloudové služby, povolte pro Cloudová služba Azure Diagnostics. *Minimální monitorování* (výchozí možnost monitorování úroveň) použije čítače výkonu shromážděné z hostitele operační systémy pro instance rolí (virtuální počítače). "Podrobné monitorování * shromažďuje další metriky na základě dat o výkonu v rámci instance rolí povolit blíže analyzovat problémy, ke kterým došlo během zpracování aplikace. Postup povolení Azure Diagnostics naleznete v tématu [povolení diagnostiky v Azure](cloud-services-dotnet-diagnostics.md).

Postup vytvoření cloudové služby se nasazení webové role nebo rolí pracovního procesu, je nutné [vytvořit balíček služby](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Než začnete
* Pokud jste nenainstalovali sadu Azure SDK, klikněte na tlačítko **instalovat sadu SDK Azure** otevřete [Azure stáhne stránky](https://azure.microsoft.com/downloads/)a pak stažení sady SDK pro jazyk, ve kterém chcete vyvíjet kódu. (Budete mít příležitost udělat to později.)
* Pokud všechny instance role vyžadovat certifikát, vytvořte certifikáty. Cloudové služby vyžadují soubor .pfx s privátním klíčem. Můžete [nahrát certifikáty do Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) jak vytvořit a nasadit cloudové služby.
* Pokud plánujete nasadit cloudovou službu do skupiny vztahů, vytvořte skupinu vztahů. Skupiny vztahů můžete použít k nasazení cloudové služby a jinými službami Azure do stejného umístění, v oblasti. Můžete vytvořit skupinu vztahů v **sítě** oblasti Azure classic na portálu **skupiny vztahů** stránky.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Postupy: vytvoření cloudové služby pomocí metody rychlého vytvoření
1. V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **nový**>**výpočetní**>**Cloudová služba**>**rychle vytvořit**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. V **URL**, zadejte název subdomény pro použití v veřejnou adresu URL pro přístup k vaší cloudové služby v rámci nasazení v produkčním prostředí. Formát adresy URL pro nasazení v produkčním prostředí je: http://*myURL*. cloudapp.net.
3. V **oblast nebo skupinu vztahů**, vyberte zeměpisnou oblast nebo skupinu vztahů nasadit cloudové službě. Pokud chcete nasadit cloudové služby na stejné umístění jako jinými službami Azure v rámci oblasti, vyberte skupinu vztahů.
4. Klikněte na **Vytvořit cloudovou službu**.
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    V oblasti zpráv v dolní části okna můžete sledovat stav procesu.
   
    **Cloudové služby** oblasti otevře s novou cloudovou službu, zobrazí. Když se stav změní na vytvořen, vytvoření cloudové služby byla úspěšně dokončena.
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Postupy: nahrát na server certifikát pro cloudové služby
1. V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **certifikáty**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Klikněte na možnost **nahrání certifikátu** nebo **nahrát**.
3. V **soubor**, použijte **Procházet** výběr certifikátu (soubor .pfx).
4. V **heslo**, zadejte privátní klíč pro certifikát.
5. Klikněte na tlačítko **OK** (zaškrtnutí).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    Můžete sledovat průběh nahrávání v oblasti zpráv vidíte níže. Po dokončení nahrávání se certifikát přidat do tabulky. V oblasti zpráv klikněte na tlačítko OK zprávu zavřete.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Postupy: nasazení cloudové služby
1. V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **řídicí panel**.
2. Klikněte na možnost **nahrát nový produkční nasazení** nebo **nahrát**.
3. V **označení nasazení**, zadejte název nové nasazení – například MyCloudServicev4.
4. V **balíček**, použijte **Procházet** vyberte souboru balíku služeb (.cspkg) používat.
5. V **konfigurace**, použijte **Procházet** a vyberte soubor konfigurace služby (.cscfg) používat.
6. Pokud cloudové služby bude obsahovat všechny role s jedinou instancí, vyberte **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** zaškrtávacího políčka umožníte nasazení, aby bylo možné pokračovat.
   
    Azure můžete pouze zaručit 99,95 % přístup ke cloudové službě během údržby a Služba aktualizací Pokud má aspoň dvě instance každé role. V případě potřeby můžete přidat další role instance na **škálování** stránky po nasazení cloudové služby. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).
7. Klikněte na tlačítko **OK** (zaškrtnutí) zahájíte nasazení cloudové služby.
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    Můžete sledovat stav nasazení v oblasti zpráv. Kliknutím na tlačítko OK skrýt zprávy.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Zkontrolujte vaše nasazení bylo úspěšně dokončeno
1. Klikněte na tlačítko **řídicí panel**.
   
    Stav by měl zobrazit, zda je služba **systémem**.
2. V části **rychlého přehledu**, klikněte na adresu URL webu ve webovém prohlížeči otevřít cloudové služby.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* [Správa služby cloud](cloud-services-how-to-manage.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).

