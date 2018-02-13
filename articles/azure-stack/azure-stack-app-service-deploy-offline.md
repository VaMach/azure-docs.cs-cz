---
title: "Nasazení služby App Service v režimu offline: zásobník Azure | Microsoft Docs"
description: "Podrobné pokyny k nasazení služby App Service v odpojeném prostředí Azure zásobníku zabezpečeným službou AD FS."
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: anwestg
ms.openlocfilehash: 2e527620825a3b419c0191244ba0baff4b74f0fa
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>Přidání poskytovatele prostředků služby App Service pro odpojené prostředí Azure zásobníku zabezpečeným službou AD FS
*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Podle pokynů v tomto článku, můžete nainstalovat [zprostředkovatele prostředků služby App Service](azure-stack-app-service-overview.md) do prostředí Azure zásobníku, které je:
- není připojen k Internetu
- zabezpečeny již ve službě Active Directory Federation Services (AD FS).

Pro přidání poskytovatele prostředků služby App Service pro offline nasazení zásobník Azure, musíte provést tyto úlohy nejvyšší úrovně:

1. Dokončení [požadovaných krocích](azure-stack-app-service-before-you-get-started.md) (jako nákup certifikátů, která může trvat několik dní přijímat).
2. [Stažení a extrakci instalační a pomocné soubory](azure-stack-app-service-before-you-get-started.md) pro počítač připojený k Internetu.
3. Vytvoření balíčku offline instalace.
4. Spusťte soubor appservice.exe Instalační služby.

## <a name="create-an-offline-installation-package"></a>Vytvoření offline instalačního balíčku

Abyste mohli nasadit služby App Service v odpojeném prostředí, musíte nejdřív vytvořit offline instalačního balíčku na počítači, který je připojený k Internetu.

1. Spusťte instalační program AppService.exe na počítači, který je připojený k Internetu.

2. Klikněte na tlačítko **Upřesnit** > **vytvořit offline instalační balíček**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy-offline/image01.png)   

3. Instalační program služby App Service vytvoří offline instalačního balíčku a zobrazuje cestu k němu. Můžete kliknout na **otevřít složku** otevření složky v Průzkumníku vaší souborů.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy-offline/image02.png)   

4. Zkopírujte instalační program (AppService.exe) a offline instalačního balíčku do vaší zásobník Azure hostitelský počítač.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>Dokončení offline instalace služby App Service v Azure zásobníku

1. Na hostitelském počítači odpojené zásobník Azure spusťte jako azurestack\clouadmin appservice.exe.

2. Klikněte na tlačítko **Upřesnit** > **dokončení offline instalace**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy-offline/image03.png)   

3. Přejděte do umístění, do režimu offline instalačního balíčku, který jste dříve vytvořili a pak klikněte na **Další**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy-offline/image04.png)   

4. Zkontrolujte a přijměte licenční podmínky pro Software společnosti Microsoft a pak klikněte na tlačítko **Další**.

5. Zkontrolujte a přijměte podmínky licenční třetích stran a pak klikněte na **Další**.

6. Ujistěte se, zda je správný informace pro konfiguraci cloudu App Service. Pokud jste použili výchozí nastavení během nasazování Azure zásobníku Development Kit, můžete přijmout výchozí hodnoty. Ale pokud jste si přizpůsobili možností při nasazení zásobník Azure, je nutné upravit hodnoty v tomto okně tak, aby odrážela který. Například pokud používáte mycloud.com příponu domény, musíte změnit váš koncový bod na management.mycloud.com. Jakmile potvrdíte vaše informace, klikněte na tlačítko **Další**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image02.png)

7. Na další stránce:
    1. Klikněte na tlačítko **připojit** vedle položky **předplatných Azure zásobníku** pole.
        - Pokud používáte Azure Active Directory (Azure AD), zadejte účet správce Azure AD a heslo, které jste zadali při nasazení Azure zásobníku. Klikněte na tlačítko **přihlášení**.
        - Pokud používáte služby Active Directory Federation Services (AD FS), zadejte účet správce. Například, cloudadmin@azurestack.local. Zadejte své heslo a klikněte na tlačítko **přihlásit**.
    2. V **předplatných Azure zásobníku** pole, vyberte své předplatné.
    3. V **umístění zásobník Azure** , vyberte umístění, které odpovídá oblast, že nasazujete. Vyberte například **místní** Pokud vaše nasazení do Azure zásobníku Development Kit.
    4. Zadejte **název skupiny prostředků** pro vaše nasazení služby App Service. Ve výchozím nastavení je nastavena na **místní služby App Service**.
    5. Zadejte **název účtu úložiště** má služby App Service k vytvoření jako součást instalace. Ve výchozím nastavení je nastavena na **appsvclocalstor**.
    6. Klikněte na **Další**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image03.png)

8. Zadejte informace pro sdílené složky a potom klikněte na **Další**. Adresu sdílené složky musí použít plně kvalifikovaný název domény souborového serveru, například \\\appservicefileserver.local.cloudapp.azurestack.external\websites nebo IP adresa, například \\\10.0.0.1\websites.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image04.png)

9. Na další stránce:
    1. V **ID aplikace Identity** zadejte identifikátor GUID pro aplikaci, kterou používáte pro identitu.
    2. V **soubor certifikátu Identity aplikace** pole, zadejte (nebo vyhledejte) umístění souboru certifikátu.
    3. V **heslo certifikátu Identity aplikace** zadejte heslo pro certifikát. Toto heslo je ten, který jste si poznamenali při skript jste použili k vytvoření certifikátů.
    4. V **soubor kořenového certifikátu Azure Resource Manager** pole, zadejte (nebo vyhledejte) umístění souboru certifikátu.
    5. Klikněte na **Další**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image05.png)

10. Pro každou z jsou tři políčka soubor certifikátu, klikněte na tlačítko **Procházet** a přejděte k souboru příslušný certifikát a zadejte heslo. Tyto certifikáty jsou ty, které jste vytvořili v [vytvořit požadované certifikáty krok](azure-stack-app-service-deploy.md). Klikněte na tlačítko **Další** po zadání všech informací.

    | Box | Příklad názvu souboru certifikátu |
    | --- | --- |
    | **Soubor certifikátu protokolu SSL výchozí služby App Service** | \_.appservice.local.AzureStack.external.pfx |
    | **Soubor certifikátu SSL služby App Service API** | api.appservice.local.AzureStack.external.pfx |
    | **Soubor certifikátu SSL služby vydavatele aplikace** | ftp.appservice.local.AzureStack.external.pfx |

    Pokud jste použili příponu jinou doménu, když jste vytvořili certifikáty, nepoužívejte názvy souborů certifikátů *místní. AzureStack.external*. Místo toho používejte vaše informace vlastní doménu.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image06.png)    

11. Zadejte podrobnosti serveru SQL Server instance serveru použitý pro hostování databází zprostředkovatele prostředků služby App Service a pak klikněte na tlačítko **Další**. Instalační program ověří vlastnosti připojení SQL.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image07.png)    

12. Zkontrolujte možnosti role instance a SKU. Výchozí hodnoty se naplní minimální počet instancí a minimální SKU pro každou roli v ASDK nasazení. Souhrn požadavků virtuální procesory a paměť je určena k plánování nasazení. Po provedení výběru klikněte na tlačítko **Další**.

     > [!NOTE]
     > Pro nasazení v produkčním prostředí, následující pokyny v [kapacitní plánování rolí serveru služby Azure App Service v Azure zásobníku](azure-stack-app-service-capacity-planning.md).
     >
     >

    | Role | Minimální instancí | Minimální SKU | Poznámky |
    | --- | --- | --- | --- |
    | Kontroler | 1 | Standard_A1 - (1 virtuální procesor, 1792 MB) | Spravuje a udržuje stav cloudové služby App Service. |
    | Správa | 1 | Standard_A2 - (2 Vcpu, 3584 MB) | Spravuje koncových bodů aplikace služby Azure Resource Manageru a rozhraní API, portálu rozšíření (správce, klienta funkce portálu.) a službu data. Pro podporu převzetí služeb při selhání, vyšší doporučenou instancí 2. |
    | Vydavatel | 1 | Standard_A1 - (1 virtuální procesor, 1792 MB) | Publikuje obsahu prostřednictvím FTP a webové nasazení. |
    | FrontEnd | 1 | Standard_A1 - (1 virtuální procesor, 1792 MB) | Směruje požadavky na aplikace služby App Service. |
    | Sdílených pracovních | 1 | Standard_A1 - (1 virtuální procesor, 1792 MB) | Hostitele web nebo aplikace API a aplikace Azure Functions. Můžete přidat víc instancí. Jako operátor můžete definovat vaši nabídku a vyberte vrstvy jakékoli SKU. Vrstvy musí mít minimálně jeden virtuální procesor. |

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image08.png)    

    > [!NOTE]
    > **Jádro systému Windows Server 2016 není image podporované platformy pro použití se službou Azure App Service v Azure zásobníku**.

13. V **vyberte Image platformy** vyberte bitovou kopii vašeho nasazení systému Windows Server 2016 virtuálního počítače od těch, které jsou dostupné na poskytovateli výpočetních prostředků pro cloudové služby App Service. Klikněte na **Další**.

14. Na další stránce:
     1. Zadejte uživatelské jméno správce virtuálního počítače Role pracovního procesu a heslo.
     2. Zadejte další role virtuálního počítače správce uživatelské jméno a heslo.
     3. Klikněte na **Další**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image09.png)    

15. Na stránce Shrnutí:
    1. Zkontrolujte provedený výběr. Chcete-li změnit, použijte **předchozí** tlačítka pro návštěvu předchozí stránky.
    2. Pokud konfigurace jsou správné, zaškrtněte políčko.
    3. Chcete-li spustit nasazení, klikněte na tlačítko **Další**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image10.png)    

16. Na další stránce:
    1. Sledovat průběh instalace. Služby App Service v Azure zásobníku trvá asi 60 minut pro nasazení založené na výchozí nastavení.
    2. Po instalační program úspěšně dokončí, klikněte na tlačítko **ukončení**.

    ![Instalační program aplikace služby](media/azure-stack-app-service-deploy/image11.png)    


## <a name="validate-the-app-service-on-azure-stack-installation"></a>Ověření služby App Service na instalaci Azure zásobníku

1. Na portálu správy Azure zásobníku přejít na **správy – Služba App Service**.

2. V přehledu v části stav, zkontrolujte, který **stav** ukazuje **všech rolí jsou připravené**.

    ![Správa služby App Service](media/azure-stack-app-service-deploy/image12.png)    


## <a name="test-drive-app-service-on-azure-stack"></a>Vyzkoušejte službu App Service v Azure zásobníku

Po nasazení a registrace zprostředkovatele prostředků služby App Service, otestujte a ujistit se, zda mohou uživatelé nasadit web a aplikace API.

> [!NOTE]
> Budete muset vytvořit nabídku, který má obor názvů Microsoft.Web v rámci plánu. Pak musíte mít předplatné klienta, která si předplatí v rámci této nabídky. Další informace najdete v tématu [vytvořit nabídku](azure-stack-create-offer.md) a [vytvořit plán](azure-stack-create-plan.md).
>
Můžete *musí* mít předplatné klienta k vytvoření aplikace, které používají služby App Service v Azure zásobníku. Jedinou možností, které správce služby můžete dokončit v rámci portálu správce souvisejí se správou zprostředkovatele prostředků služby App Service. Tyto možnosti zahrnují přidání kapacitu, konfigurace nasazení zdroje a přidání pracovní úrovně a SKU.
>
Od verze třetí technical preview k vytvoření webové rozhraní API a Azure funkce aplikace, musíte použít portál pro klienty a mít předplatné klienta.

1. Na portálu Azure zásobníku klienta, klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.

2. Na **webové aplikace** okno, zadejte název v **webové aplikace** pole.

3. V části **skupiny prostředků**, klikněte na tlačítko **nový**. Zadejte název do **skupiny prostředků** pole.

4. Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**.

5. Na **plán služby App Service** okno, zadejte název v **plán služby App Service** pole.

6. Klikněte na tlačítko **cenová úroveň** > **volné sdílené** nebo **sdílené sdílené** > **vyberte**  >   **OK** > **vytvořit**.

7. V pod minuty, dlaždice pro nové webové aplikace se zobrazí na řídicím panelu. Klikněte na dlaždici.

8. Na **webové aplikace** okně klikněte na tlačítko **Procházet** zobrazit výchozí web pro tuto aplikaci.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Nasazení na web WordPress, DNN nebo Django (volitelné)

1. Na portálu Azure zásobníku klienta, klikněte na tlačítko  **+** , přejděte do Azure Marketplace, nasazení webu Django a čekání na úspěšné dokončení. Webová platforma Django používá databázi na základě systému souborů. Nevyžaduje žádné další prostředků poskytovatelé, například SQL nebo MySQL.

2. Pokud jste nasadili také MySQL poskytovatele prostředků, můžete nasadit web WordPress v Marketplace. Když se zobrazí výzva k databázi parametry, zadejte uživatelské jméno jako  *User1@Server1* , uživatelské jméno a název serveru podle svého výběru.

3. Pokud jste nasadili také poskytovatele prostředků systému SQL Server, můžete nasadit na web DNN z Marketplace. Když se zobrazí výzva k parametry databáze, zvolte databázi v počítači se systémem SQL Server, který je připojený ke zprostředkovateli prostředků.

## <a name="next-steps"></a>Další postup

Můžete také zkusit dalších [platforma jako služba (PaaS) služby](azure-stack-tools-paas-services.md).

- [Poskytovatel prostředků SQL serveru](azure-stack-sql-resource-provider-deploy.md)
- [Poskytovatel prostředků MySQL](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
