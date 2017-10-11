---
title: "Povolení vzdálené plochy v cloudové službě Azure | Microsoft Docs"
description: "Postup konfigurace aplikace služby azure cloud umožňující připojení ke vzdálené ploše"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 413e72e9a39fcde84f56bfc61a6bc72dbadf1c97
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Portál Azure Classic](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

Můžete povolit připojení ke vzdálené ploše v příslušné roli během vývoje zahrnutím moduly vzdálené plochy v definice služby nebo můžete povolit vzdálené plochy prostřednictvím vzdálené plochy rozšíření. Použití rozšíření vzdálené plochy, protože vzdálená plocha můžete povolit i poté, co je aplikace nasazena, aniž by museli znovu nasaďte aplikaci je žádoucí.

## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Konfigurace vzdálené plochy z portálu Azure classic
Portál Azure classic používá vzdálené plochy rozšíření přístup, takže Vzdálená plocha můžete povolit i poté, co je aplikace nasazená. **Konfigurace** stránka pro cloudové služby umožňuje povolit vzdálené plochy, změňte místní účet správce používá k připojení k virtuálním počítačům, certifikát používané v ověřování a nastavit datum vypršení platnosti.

1. Klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **konfigurace**.
2. Klikněte **vzdáleného** tlačítko dole.

    ![Cloudové služby vzdálené](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > Všechny instance role se restartuje, při prvním povolení služby Vzdálená plocha a klikněte na tlačítko OK (zaškrtnutí). Abyste zabránili restartování, musí být nainstalovaný certifikát použitý k šifrování hesla v roli. Abyste zabránili restartování, [nahrát certifikát pro cloudové služby](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) a pak se vraťte do tohoto dialogového okna.

3. V **role**, vyberte roli, kterou chcete aktualizovat nebo vyberte **všechny** u všech rolí.
4. Proveďte některou z následujících změn:

   * Chcete-li povolit vzdálená plocha, vyberte **povolení vzdálené plochy** zaškrtávací políčko. Zakázání vzdálené plochy, zrušte zaškrtnutí políčka.
   * Vytvořte účet, který chcete použít v připojení ke vzdálené ploše na instancí role.
   * Aktualizace hesla pro existující účet.
   * Vyberte odeslaný certifikát bude používat pro ověření (nahrát certifikát pomocí **nahrát** na **certifikáty** stránky) nebo vytvořit nový certifikát.
   * Změňte na datum vypršení platnosti konfigurace vzdálené plochy.

5. Po dokončení aktualizace vaše konfigurace, klikněte na tlačítko **OK** (zaškrtnutí).

## <a name="remote-into-role-instances"></a>Vzdálené do instance rolí
Jakmile povolíte vzdálené plochy na role můžete vzdálené do instance role pomocí různých nástrojů.

Chcete-li se připojit k instanci role z klasického portálu Azure:

1. Klikněte na tlačítko **instance** otevřete **instance** stránky.
2. Vyberte instanci role, která má nakonfigurované připojení ke vzdálené ploše.
3. Klikněte na tlačítko **Connect**a postupujte podle pokynů na ploše otevřete.
4. Klikněte na tlačítko **otevřete** a potom **Connect** spustit připojení ke vzdálené ploše.

### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Použijte sadu Visual Studio na vzdálený do role instance
V sadě Visual Studio Průzkumníka serveru:

1. Rozbalte **Azure** > **cloudové služby** > **[název cloudové služby]** uzlu.
2. Rozbalte **pracovní** nebo **produkční**.
3. Rozbalte jednotlivé role.
4. Pravým tlačítkem na jednu z instancí role, klikněte na tlačítko **připojit pomocí vzdálené plochy...** a pak zadejte uživatelské jméno a heslo.

![Vzdálená plocha Průzkumníka serveru](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-to-get-the-rdp-file"></a>Chcete-li získat soubor RDP pomocí prostředí PowerShell
Můžete použít [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) rutiny se načíst soubor RDP. Pak můžete soubor RDP s připojení ke vzdálené ploše pro přístup ke cloudové službě.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Prostřednictvím kódu programu stáhněte soubor RDP prostřednictvím REST API pro správu služby
Můžete použít [stáhnout soubor RDP](https://msdn.microsoft.com/library/jj157183.aspx) operace REST stáhnout soubor RDP.

## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Konfigurace vzdálené plochy v souboru definice služby
Tato metoda umožňuje povolení vzdálené plochy pro aplikací během vývoje. Tento přístup vyžaduje šifrované hesla ukládat v konfiguraci služby souboru a všechny aktualizace konfigurace vzdálené plochy by vyžadovaly opětovné nasazení aplikace. Pokud chcete, aby se zabránilo tyto downsides, měli byste použít vzdálené plochy rozšíření na základě popsané výše.  

Můžete použít Visual Studio [povolit připojení ke vzdálené ploše](../vs-azure-tools-remote-desktop-roles.md) přístup souboru definice služby.  
Níže uvedené kroky popisují změny potřebné pro soubory modelu služby k povolení služby Vzdálená plocha. Visual Studio se tyto změny automaticky provede při publikování.

### <a name="set-up-the-connection-in-the-service-model"></a>Nastavení připojení v modelu služby
Použití **importy** elementu, který chcete importovat **RemoteAccess** modulu a **RemoteForwarder** modulu [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) souboru.

Soubor definice služby by mělo být podobné v následujícím příkladu se `<Imports>` elementu přidány.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
[Souboru ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) soubor by měl být podobně jako v následujícím příkladu, Upozorňujeme `<ConfigurationSettings>` a `<Certificates>` elementy. Zadaný certifikát musí být [nahrán do cloudové služby](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Další zdroje
[Postup konfigurace cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)
