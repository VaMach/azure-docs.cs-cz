---
title: "Příprava na publikování nebo nasadit aplikaci Azure ze sady Visual Studio | Microsoft Docs"
description: "Další postupy pro nastavení cloudu a úložiště účet služby a konfiguraci aplikace Azure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: cc4fb87e559f554634ae062a59bee31f0831da64
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Příprava na publikování nebo nasadit aplikaci Azure ze sady Visual Studio
## <a name="overview"></a>Přehled
Před publikováním projekt cloudové služby, je potřeba nastavit následující služby:

* A **Cloudová služba** ke spuštění vaší rolí v prostředí Azure
* A **účet úložiště** , který poskytuje přístup ke službám Blob, fronty a tabulky.

Pomocí následujících postupů nastavit tyto služby a konfiguraci vaší aplikace

## <a name="create-a-cloud-service"></a>Vytvoření cloudové služby
Chcete-li publikovat Cloudová služba Azure, musí nejdřív vytvořit cloudové služby, který se spouští vaše role v prostředí Azure. Můžete vytvořit cloudové služby v rámci [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), jak je popsáno v části **k vytvoření cloudové služby pomocí portálu Azure classic**dál v tomto tématu. Cloudové služby v sadě Visual Studio můžete také vytvořit pomocí Průvodce přidáním publikování.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Vytvoření cloudové služby, pomocí sady Visual Studio
1. Otevřete místní nabídky pro Azure projekt a zvolte **publikovat**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)
2. Pokud nejste přihlášeni, přihlaste se pomocí uživatelského jména a hesla pro účet Microsoft nebo účtu organizace, který je spojen s předplatným Azure.
3. Vyberte **Další** tlačítko pro přechod **nastavení** stránky.

    ![Běžné nastavení Průvodce publikování](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)
4. V **cloudové služby** vyberte **vytvořit nový**. **Vytvoření služby Azure** otevře se dialogové okno.
5. Zadejte název cloudové služby. Název je součástí adresy URL služby a proto musí být globálně jedinečný. Název není malá a velká písmena.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>K vytvoření cloudové služby pomocí portálu Azure classic
1. Přihlaste se k [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103) na webu společnosti Microsoft.
2. (volitelné) Chcete-li zobrazit seznam cloudových služeb, které jste už vytvořili, zvolte odkaz cloudové služby na levé straně stránky.
3. Vyberte  **+**  ikonu v levém dolním rohu a potom vyberte **Cloudová služba** v zobrazené nabídce. Jiné obrazovky s dvě možnosti, **rychle vytvořit** a **vytvořit vlastní**, se zobrazí. Pokud se rozhodnete **rychle vytvořit**, můžete vytvořit cloudovou službu právě zadáním jeho adresa URL a oblasti, kde se bude fyzicky hostován. Pokud se rozhodnete **vytvořit vlastní**, můžete publikovat Cloudová služba okamžitě zadáním balíček (soubor .cspkg), souboru (.csdef) a certifikát. Vytvořit vlastní není povinné, pokud chcete publikovat pomocí cloudové služby **publikovat** v projektu Azure. **Publikovat** příkaz je k dispozici v místní nabídce pro projektu Azure.
4. Zvolte **rychle vytvořit** později publikovat cloudové služby pomocí sady Visual Studio.
5. Zadejte název pro cloudové služby. Úplnou adresu URL se zobrazí vedle názvu.
6. V seznamu vyberte oblast, kde se nachází většina uživatelů.
7. V dolní části okna, vyberte **vytvoření cloudové služby** odkaz.

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
Účet úložiště poskytuje přístup ke službám Blob, fronty a tabulky. Účet úložiště můžete vytvořit pomocí sady Visual Studio nebo [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Chcete-li vytvořit účet úložiště pomocí sady Visual Studio
1. V **Průzkumníku řešení**, otevřete místní nabídku pro **úložiště** uzel a potom zvolte **vytvořit účet úložiště**.

    ![Vytvořit nový účet úložiště Azure](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)
2. Vyberte nebo zadejte následující informace pro nový účet úložiště **vytvořit účet úložiště** dialogové okno.

   * Předplatné Azure, ke které chcete přidat účet úložiště.
   * Název, který chcete použít pro nový účet úložiště.
   * Oblast nebo skupinu vztahů (například západní USA nebo jihovýchodní Asie).
   * Typ replikace, kterou chcete použít pro účet úložiště, jako je například geograficky redundantní.
3. Až budete hotoví, zvolte **vytvořit**. Nový účet úložiště zobrazí v **úložiště** seznamu v **Průzkumníka serveru**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Chcete-li vytvořit účet úložiště pomocí portálu Azure classic
1. Přihlaste se k [portál Azure classic](http://go.microsoft.com/fwlink/?LinkId=253103) na webu společnosti Microsoft.
2. (Volitelné) Chcete-li zobrazit účty úložiště, zvolte **úložiště** odkaz v panelu na levé straně stránky.
3. V levém horním rohu stránky, vyberte  **+**  ikonu.
4. V nabídce, která se zobrazí, zvolte **úložiště**a potom zvolte **rychle vytvořit**.
5. Zadejte název, který bude mít za následek jedinečnou adresou url účtu úložiště.
6. Zadejte název cloudové služby. Úplnou adresu URL se zobrazí vedle názvu.
7. V seznamu oblastí vyberte v oblasti, kde se nachází většina uživatelů.
8. Zadejte, zda chcete aktivovat geografickou replikaci. Pokud povolíte geografická replikace, data se uloží do více fyzických umístění, abyste snížili riziko ztráty. Díky této funkci úložiště dražší, ale může snížit náklady na povolením geografického umístění, při vytváření účtu úložiště místo přidávání funkci později. Další informace najdete v tématu [geografická replikace](http://go.microsoft.com/fwlink/?LinkId=253108).
9. V dolní části okna, vyberte **vytvořit účet úložiště** odkaz.

Po vytvoření účtu úložiště, zobrazí se adresy URL, které můžete použít pro přístup k prostředkům v každé služby Azure storage a primární a sekundární přístupové klíče pro váš účet. Tyto klíče se používají k ověření požadavků na služby úložiště přístup.

> [!NOTE]
> Sekundární přístupový klíč poskytuje stejný přístup k účtu úložiště jako primární přístupový klíč a je generována jako záložní ohrožené primární přístupový klíč. Kromě toho se doporučuje znovu vygenerovat klíče pro přístup k v pravidelných intervalech. Můžete upravit nastavení připojovacího řetězce používat sekundární klíč obnovit primární klíč, pak můžete upravit ji používat se znova vygeneroval primární klíč obnovit sekundární klíč.
>
>

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Konfigurace aplikace pro službu pro zadaný účet úložiště
Je nutné nakonfigurovat všechny role, který přistupuje k služeb pro úložiště na použití služby Azure storage, které jste vytvořili. K tomuto účelu můžete použít více konfigurace služby pro svůj projekt Azure. Ve výchozím nastavení dva jsou vytvořené v projektu Azure. Pomocí několika konfigurace služby, můžete použít stejný připojovací řetězec v kódu, ale mít jinou hodnotu pro připojovací řetězec v každé konfiguraci služby. Například můžete použít jednu konfiguraci služby pro spouštění a ladění aplikace místně pomocí emulátoru úložiště Azure a konfiguraci jinou službu, kterou chcete publikovat svoji aplikaci do Azure. Další informace o konfiguracích služby najdete v tématu [konfigurace si Azure pomocí více služby konfigurace projektu](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Konfigurace aplikace k používání služby, které poskytuje účtu úložiště
1. V sadě Visual Studio otevřete řešení Azure. V Průzkumníku řešení otevřete místní nabídku pro každou roli v projektu Azure, který přistupuje k služby storage a zvolte **vlastnosti**. Na stránce s názvem role se zobrazí v editoru Visual Studio. Na stránce se zobrazí pole pro **konfigurace** kartě.
2. Na stránkách vlastností pro roli, zvolte **nastavení**.
3. V **konfigurace služby** seznamu, vyberte název konfigurace služby, který chcete upravit. Pokud chcete změnit všechny konfigurace služby pro tuto roli, můžete vybrat **všechny konfigurace**.  Další informace o aktualizaci konfigurace služby, najdete v části **spravovat připojovací řetězce pro účty úložiště** v tématu [konfiguraci rolí pro cloudové služby Azure pomocí sady Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).
4. Chcete-li změnit jakékoli nastavení připojovacího řetězce, vyberte **...** tlačítko vedle nastavení. **Vytvoření připojovacího řetězce úložiště** zobrazí se dialogové okno.
5. V části **připojit pomocí**, vyberte **vaše předplatné** možnost.
6. V **předplatné** vyberte své předplatné. Pokud seznam předplatných neobsahuje ten, který chcete, vyberte **stáhnout nastavení publikování** odkaz.
7. V **název účtu** seznamu, vyberte název účtu úložiště. Přihlašovací údaje účtu úložiště Azure nástroje automaticky získá pomocí souboru .publishsettings. Chcete-li ručně zadat přihlašovací údaje účtu úložiště, zvolte **ručně zadali přihlašovací údaje** možnost a potom pokračujte v tomto postupu. Název účtu úložiště a primární klíč z můžete získat [portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=213885). Pokud nechcete zadat účtu úložiště ručně, vyberte nastavení **OK** tlačítko dialogové okno zavřete.
8. Vyberte **zadejte účet úložiště** odkaz přihlašovací údaje.
9. V **název účtu** pole, zadejte název svého účtu úložiště.

   > [!NOTE]
   > Přihlaste se [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885)a potom zvolte **úložiště** tlačítko. Portál zobrazuje seznam účtů úložiště. Pokud si zvolíte účet, otevře se stránka pro ni. Název účtu úložiště můžete zkopírovat z této stránky. Pokud používáte předchozí verzi portálu classic, název účtu úložiště se zobrazí v **účty úložiště** zobrazení. Zkopírujte tento název, zvýrazněte ho v **vlastnosti** okno tohoto zobrazení a potom vyberte Ctrl-C klíče. Pokud chcete vložit název do sady Visual Studio, vyberte **název účtu** textové pole a potom vyberte klávesy Ctrl + V.
   >
   >
10. V **klíč účtu** pole, zadejte primární klíč, nebo zkopírujte a vložte ho [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
     Zkopírujte tento klíč:

    1. V dolní části stránky pro účet odpovídající úložiště, vyberte **Správa klíčů** tlačítko.
    2. Na **spravovat přístup klíče** vyberte text primární přístupový klíč a potom vyberte klávesy Ctrl + C.
    3. Do nástroje Azure, vložte klíč do **klíč účtu** pole.
    4. Musíte vybrat jednu z následujících možností služby budou pro přístup k účtu úložiště:

       * **Prostřednictvím protokolu HTTP**. Toto je možnost Standardní. Například, `http://<account name>.blob.core.windows.net`.
       * **Použití protokolu HTTPS** pro zabezpečené připojení. Například, `https://<accountname>.blob.core.windows.net`.
       * **Zadejte vlastní koncové body** pro všechny tři služby. Potom můžete zadat těchto koncových bodů do pole pro konkrétní službu.

         > [!NOTE]
         > Pokud vytvoříte vlastní koncové body, můžete vytvořit složitější připojovací řetězec. Pokud použijete tento formát řetězce, můžete zadat koncové body služby úložiště, které obsahují název vlastní domény, který je zaregistrovaný pro váš účet úložiště se službou objektů Blob. Také můžete udělit přístup jen k prostředkům blob v jediný kontejner prostřednictvím sdílený přístupový podpis. Další informace o tom, jak vytvořit vlastní koncové body najdete v tématu [nakonfigurování připojovacích řetězců úložiště Azure](storage/common/storage-configure-connection-string.md).
         >
         >
11. Pokud chcete uložit tyto změny řetězec připojení, vyberte **OK** tlačítko a potom zvolte **Uložit** tlačítka na panelu nástrojů. Po uložení těchto změn, můžete získat hodnotu tento připojovací řetězec v kódu pomocí [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Když publikujete aplikaci do Azure, zvolte konfigurace služby, který obsahuje účet úložiště Azure pro připojovací řetězec. Po publikování aplikace, ověřte, že aplikace funguje podle očekávání proti služby Azure storage

## <a name="next-steps"></a>Další kroky
Další informace o publikování aplikace do Azure ze sady Visual Studio, najdete v části [publikování cloudové služby pomocí nástroje Azure](vs-azure-tools-publishing-a-cloud-service.md).
