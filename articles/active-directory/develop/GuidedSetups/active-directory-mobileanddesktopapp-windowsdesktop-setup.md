---
title: "Azure AD v2 Windows Desktop získávání spuštěno – nastavení | Microsoft Docs"
description: "Jak aplikace Windows Desktop .NET (XAML) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4065727aef04d7969d438c6ef79127bb44568be1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="set-up-your-project"></a>Nastavení projektu

Tato část obsahuje podrobné pokyny pro vytvoření nového projektu do ukazují, jak integrovat aplikace Windows Desktop .NET (XAML) s *přihlášení se společností Microsoft* , se můžete dotazovat webové rozhraní API, která vyžaduje token.

Aplikace vytvořené v této příručce zpřístupní tlačítko graf a zobrazit výsledky na obrazovce a odhlášení tlačítko.

> Místo toho stáhněte projekt Visual Studio Tato ukázka dávají přednost? [Stažení projektu](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) a pokračujte [krok konfigurace](#create-an-application-express) před provedením konfigurace ukázka kódu.


### <a name="create-your-application"></a>Vytvoření aplikace
1. V sadě Visual Studio:`File` > `New` > `Project`<br/>
2. V části *šablony*, vyberte možnost`Visual C#`
3. Vyberte `WPF App` (nebo *aplikaci WPF* v závislosti na verzi sady Visual Studio)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Do projektu přidejte knihovny ověřování společnosti Microsoft (MSAL)
1. V sadě Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Zkopírujte a vložte následující v okně konzoly Správce balíčků:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> Výše uvedené balíček nainstaluje Microsoft ověřování knihovny (MSAL). MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci toskens uživatel používá pro přístup k rozhraní API, které jsou chráněné službou Azure Active Directory v2.

## <a name="add-the-code-to-initialize-msal"></a>Přidat kód pro inicializaci MSAL
Tento krok vám pomůže vytvořit třídu pro zpracování interakci s MSAL knihovnu, jako je například zpracování tokenů.

1. Otevřete `App.xaml.cs` souboru a odkaz na MSAL knihovny přidejte do třídy:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Aktualizace třídy aplikace takto:
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is the clientId of your app registration. 
    //You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a>Vytvoření uživatelského rozhraní aplikace
Následující části ukazuje, jak aplikaci můžete dotazovat chráněných back-end serveru, jako je Microsoft Graph. Soubor MainWindow.xaml by měl automaticky vytvoří jako součást vaše šablona projektu. Tento soubor otevřít tento soubor a pak postupujte podle pokynů níže:

Nahraďte aplikace `<Grid>` být následující:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
