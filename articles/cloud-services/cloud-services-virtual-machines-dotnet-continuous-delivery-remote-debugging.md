---
title: "Povolení vzdáleného ladění s nastavené průběžné doručování | Microsoft Docs"
description: "Postup povolení vzdáleného ladění při použití nastavené průběžné doručování k nasazení do Azure"
services: cloud-services
documentationcenter: .net
author: mikejo
manager: ghogen
editor: 
ms.assetid: 7d423639-3b2f-4ca5-ac5a-9ac19a217c29
ms.service: cloud-services
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/18/2016
ms.author: mikejo
ms.openlocfilehash: 1a30b42e6e84edf9a7cef861aaf6a60e87c473d0
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Aktivace vzdáleného ladění při použití průběžného odesílání k publikování v Azure
Můžete povolit vzdálené ladění v Azure, pro cloudové služby nebo virtuálního počítače, při použití [nastavené průběžné doručování](cloud-services-dotnet-continuous-delivery.md) publikovat do Azure pomocí následujících kroků.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Povolení vzdáleného ladění pro cloudové služby
1. V agentovi sestavení nastavení počáteční prostředí pro Azure jak je uvedeno v [sestavení příkazového řádku pro Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Protože je vyžadován pro balíček vzdáleného ladění runtime (msvsmon.exe), nainstalujte **nástrojů pro vzdálenou pro Visual Studio**.

   * [Nástroje pro vzdálenou pro Visual Studio 2017](https://go.microsoft.com/fwlink/?LinkId=746570)
   * [Nástroje pro vzdálenou pro Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=615470)
   * [Nástroje pro vzdálenou pro Visual Studio 2013 Update 5](https://www.microsoft.com/download/details.aspx?id=48156)
    
   Jako alternativu můžete zkopírovat binární soubory pro vzdálené ladění ze systému, který má nainstalovanou sadu Visual Studio.

3. Vytvoření certifikátu, jak je uvedeno v [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md). Udržujte .pfx a kryptografický otisk certifikátu protokolu RDP a nahrání certifikátu ke cloudové službě cíl.
4. Pomocí následujících možností v příkazovém řádku MSBuild sestavení a balíček s vzdáleného ladění povoleno. (Nahraďte skutečné cesty k souborům systému a projektů pro položky v závorkách úhel.)
   
   ```cmd
   msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"
   ```
   
   `VSX64RemoteDebuggerPath`je cesta ke složce obsahující msvsmon.exe v nástrojích pro vzdálenou pro sadu Visual Studio.
   `RemoteDebuggerConnectorVersion`je verze sady SDK Azure v rámci cloudové služby. Je také by měl odpovídat verzi nainstalované s Visual Studio.

5. Publikujte ke cloudové službě cíl s použitím souboru balíčku a .cscfg vygenerované v předchozím kroku.
6. Importujte certifikátu (soubor .pfx) do počítače, který má Visual Studio s Azure SDK pro .NET nainstalované. Zajistěte, aby k importu do `CurrentUser\My` úložiště certifikátů, jinak se připojuje k v ladicím programu sady Visual Studio se nezdaří.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Povolení vzdáleného ladění pro virtuální počítače
1. Vytvořte virtuální počítač Azure. V tématu [vytvoření virtuálního počítače se systémem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [vytvářet a spravovat virtuální počítače Azure v sadě Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
2. Na portálu Azure] (http://go.microsoft.com/fwlink/p/?LinkID=269851), přejděte na virtuální počítač k virtuálnímu počítači **kryptografický OTISK certifikátu protokolu RDP**. Tato hodnota se používá pro `ServerThumbprint` hodnota v konfiguraci rozšíření.
3. Vytvoření certifikátu klienta, jak je uvedeno v [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md) (Uchovávejte .pfx a kryptografický otisk certifikátu protokolu RDP).
4. Nainstalovat Azure Powershell (verze 0.7.4 nebo novější) jak je uvedeno v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
5. Spusťte následující skript, který chcete povolit rozšíření RemoteDebug. Nahraďte cesty a osobní data vlastní, například název odběru, název služby a kryptografický otisk.
   
   > [!NOTE]
   > Tento skript je nakonfigurován pro Visual Studio 2015. Pokud používáte Visual Studio 2013 nebo Visual Studio 2017, upravte `$referenceName` a `$extensionName` přiřazení níže, aby `RemoteDebugVS2013` nebo `RemoteDebugVS2017`.

    ```powershell   
    Add-AzureAccount

    Select-AzureSubscription "My Microsoft Subscription"

    $vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"

    $endpoints = @(
                    ,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
                    ,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
                )

    foreach($endpoint in $endpoints)
    {
        Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
    }

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
    $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
    $extensionName = "RemoteDebugVS2015"
    $version = "1.*"
    $publicConfiguration = "<PublicConfig><Connector.Enabled>true</Connector.Enabled><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension -ReferenceName $referenceName -Publisher $publisher -ExtensionName $extensionName -Version $version -PublicConfiguration $publicConfiguration

    foreach($extension in $vm.VM.ResourceExtensionReferences)
    {
        if(($extension.ReferenceName -eq $referenceName) `
        -and ($extension.Publisher -eq $publisher) `
        -and ($extension.Name -eq $extensionName) `
        -and ($extension.Version -eq $version))
        {
            $extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
            break
        }
    }

    $vm | Update-AzureVM
    ```

6. Importujte certifikátu (.pfx) do počítače, který má Visual Studio s Azure SDK pro .NET nainstalované.

