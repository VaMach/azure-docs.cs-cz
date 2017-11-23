---
title: "Jak používat Azure virtuálního počítače spravované služby Identity pro přihlášení a tokenu akvizic"
description: "Podrobné pokyny k používání MSI virtuálních počítačů Azure služby objekt zabezpečení při přihlášení a pro získání tokenu přístupu."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/17/2017
ms.author: bryanla
ms.openlocfilehash: 168b2ab3676d3f3e2830966f850e14adbe579f85
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in-and-token-acquisition"></a>Jak používat Azure virtuálního počítače spravované služby Identity (MSI) pro pořízení přihlášení a tokenu 
[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]Poté, co jste povolili MSI ve virtuálním počítači Azure, můžete použít soubor MSI pro přihlášení a požádat o token přístupu. Tento článek popisuje různé způsoby použití souboru MSI [instanční objekt](develop/active-directory-dev-glossary.md#service-principal-object) pro přihlášení a získat [jen aplikace přístupový token](develop/active-directory-dev-glossary.md#access-token) přistupovat k dalším prostředkům, včetně:

- Silent/bezobslužné přihlášení z prostředí PowerShell nebo rozhraní příkazového řádku Azure
- Získání tokenu pro různé klienty, včetně .NET, prostředí PowerShell, Bash/CURL, REST
- Přihlášení pomocí Azure SDK, včetně .NET, Java, Node.js, Python, Ruby

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Pokud budete chtít použít příklady prostředí PowerShell v tomto článku, je nutné nainstalovat [prostředí Azure PowerShell verze 4.3.1](https://www.powershellgallery.com/packages/AzureRM) nebo vyšší. Pokud máte v plánu používat rozhraní příkazového řádku Azure příklady v tomto článku, máte tři možnosti:
- Použití [prostředí cloudu Azure](../cloud-shell/overview.md) z portálu Azure.
- Používání embedded prostředí cloudu Azure prostřednictvím "Zkuste ho" tlačítko, umístěné v pravém horním rohu bloky kódu rozhraní příkazového řádku Azure.
- [Nainstalujte nejnovější verzi 2.0 rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 nebo novější) Pokud dáváte přednost používání rozhraní příkazového řádku v místní příkazový řádek. 


> [!IMPORTANT]
> - Všechny ukázkový kód nebo skript v tomto článku předpokládá klienta běží na virtuálním počítači povolená MSI. Informace o povolení MSI na virtuálním počítači, v [konfigurace virtuálních počítačů spravovaných služba Identity (MSI) pomocí portálu Azure](msi-qs-configure-portal-windows-vm.md), nebo jeden z typu variant článků (pomocí prostředí PowerShell, rozhraní příkazového řádku, šablonu nebo Azure SDK). 
> - Chcete-li zabránit chybám autorizace (403/AuthorizationFailed) v příkladech kódu nebo skriptu identity Virtuálního počítače musí být poskytnut "Čtečky" přístup v oboru virtuálních počítačů umožňující Azure Resource Manager operace na virtuálním počítači. V tématu [přiřadit identita spravované služby (MSI) přístup k prostředku na portálu Azure](msi-howto-assign-access-portal.md) podrobnosti.
> - Než budete pokračovat na jednu z těchto částí, použijte funkci "Připojit" virtuální počítač na portálu Azure vzdálené připojení k virtuálnímu počítači povolit MSI.

## <a name="how-to-sign-in-from-powershell-or-cli-using-msi"></a>Jak se přihlásit z prostředí PowerShell nebo rozhraní příkazového řádku pomocí Instalační služby MSI

Dříve spouštění skriptu v rámci své vlastní identity určená:
- registrace jako důvěrné nebo webové aplikaci klienta s Azure AD
- přihlášení pomocí pověření ID nebo tajný klíč klienta aplikace.

Pomocí Instalační služby MSI vašeho skriptu klienta už nepotřebuje registraci s Azure AD ani zadejte přihlašovací údaje. V následujících příkladech ukážeme, jak používat MSI Virtuálního počítače instanční objekt pro přihlášení.

### <a name="azure-powershell"></a>Azure PowerShell

Následující skript ukazuje, jak:

- Získejte přístupový token MSI pro virtuální počítač Azure
- použití tokenu přístupu pro přihlášení k Azure AD v části odpovídající MSI instanční objekt
- pomocí objektu služby MSI volat pomocí Azure Resource Manager, chcete-li získat číslo ID objektu služby

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token

# Use the access token to sign in under the MSI service principal
Login-AzureRmAccount -AccessToken $access_token -AccountId “CLIENT”

# The MSI service principal is now signed in for this session.
# Next, a call to Azure Resource Manager is made to get the service principal ID for the VM's MSI. 
$spID = (Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>).identity.principalid
echo "The MSI service principal ID is $spID"
```
   
### <a name="azure-cli"></a>Azure CLI

Následující skript ukazuje, jak:

- Přihlaste se k Azure AD v rámci objektu služby MSI Virtuálního počítače
- pomocí objektu služby MSI volat pomocí Azure Resource Manager, chcete-li získat číslo ID objektu služby

```azurecli-interactive
az login --msi
spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
echo The MSI service principal ID is $spID
```

## <a name="how-to-acquire-an-access-token-from-msi"></a>Získání tokenu přístupu z MSI

Pomocí Instalační služby MSI, vaše aplikace nebo skriptu klienta už nepotřebuje zaregistrovat Azure AD ani ID a tajný klíč k získání tokenu přístupu k dispozici svého klienta. Váš klient jednoduše požádat místní koncový bod MSI pro přístupový token, bez nutnosti poskytnout přihlašovací údaje aplikací. Token k přístupu jen aplikace je vydán pro MSI objektu služby, vhodné pro použití jako token nosiče v [service-to-service volá vyžadují pověření klienta](active-directory-protocols-oauth-service-to-service.md).

V následujících příkladech ukážeme, jak používat MSI Virtuálního počítače pro získání tokenu.

### <a name="net"></a>.NET

> [!IMPORTANT]
> Azure AD Authentication Library (ADAL) není integrovaná s MSI. Pokud chcete získat přístupový token pomocí Instalační služby MSI, vytvořte žádost na místní koncový bod MSI ve virtuálním počítači. Další informace najdete v tématu [MSI nejčastější dotazy a známé problémy](msi-known-issues.md#frequently-asked-questions-faqs).

```csharp
// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

### <a name="azure-powershell-token"></a>Prostředí Azure PowerShell

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"
```

### <a name="bashcurl"></a>Bash/CURL

```bash
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

### <a name="rest"></a>REST

Ukázková žádost:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Element | Popis |
| ------- | ----------- |
| `GET` | Příkaz HTTP, která určuje, že chcete načíst data z koncového bodu. V takovém případě OAuth přístupový token. | 
| `http://localhost:50342/oauth2/token` | MSI koncového bodu, kde 50342 je výchozím portem a je možné konfigurovat. |
| `resource` | Parametr řetězce dotazu, která udává, identifikátor ID URI aplikace cílového prostředku. Zobrazuje se taky v `aud` vydaný token deklarace identity (cílové skupiny). V tomto příkladu jsme žádají o token pro přístup k Azure Resource Manager, který má identifikátor ID URI aplikace z https://management.azure.com/. |
| `Metadata` | Pole hlavičky požadavku HTTP, vyžadují MSI jako zmírnění proti útoku serveru straně požadavek padělání (SSRF). Tato hodnota musí být nastavena na hodnotu true, všechny malými písmeny.

Ukázková odpověď:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Element | Popis |
| ------- | ----------- |
| `access_token` | Požadovaný přístupový token. Při volání rozhraní REST API, je token vložený do `Authorization` pole hlavičky požadavku jako token "nosiče", umožňuje rozhraní API pro ověření volající. | 
| `refresh_token` | Není používán MSI. |
| `expires_in` | Počet sekund, po které přístupový token je stále platný, před vypršením platnosti od času vystavování. Čas vystavení lze nalézt v tokenu `iat` deklarací identity. |
| `expires_on` | Časový interval, když vyprší platnost přístupového tokenu. Datum je reprezentován jako počet sekund od "pod hodnotou 1970-01-01T0:0:0Z UTC" (odpovídá na token `exp` deklarace identity). |
| `not_before` | Časový interval, když se projeví přístupový token a nelze přijmout. Datum je reprezentován jako počet sekund od "pod hodnotou 1970-01-01T0:0:0Z UTC" (odpovídá na token `nbf` deklarace identity). |
| `resource` | Prostředek přístupový token byl požadován pro, který odpovídá `resource` požadavku parametr řetězce dotazu. |
| `token_type` | Typ tokenu, což je přístupový token "Nosiče", což znamená, že prostředek můžete poskytnout přístup k nosiče tohoto tokenu. |

## <a name="how-to-use-msi-with-azure-sdk-libraries"></a>Jak používat MSI s knihovnami Azure SDK

Azure podporuje více platforem programovací prostřednictvím řady [sady Azure SDK](https://azure.microsoft.com/downloads). Několik z nich byly aktualizovány pro podporu přihlašování pomocí souboru MSI a zadejte odpovídající ukázky k předvedení využití. Tento seznam je aktualizovat, protože je přidat další podporu:

| Sada SDK | Ukázka |
| --- | ------ | 
| .NET | [Nasazení šablony ARM z virtuálního počítače Windows pomocí identita spravované služby](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core | [Volání služby Azure z virtuálního počítače s Linuxem pomocí identita spravované služby](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js| [Správa prostředků pomocí identita spravované služby](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python | [K ověření jednoduše z uvnitř virtuálního počítače použít MSI](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby   | [Spravovat prostředky z virtuálního počítače povoleným MSI](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) | 

## <a name="additional-information"></a>Další informace

### <a name="token-expiration"></a>Vypršení platnosti tokenu

Subsystém místní MSI tokeny ukládá do mezipaměti. Proto můžete volat ho tak často, jak se vám líbí a volání na přenosu do služby Azure AD výsledků pouze, pokud:
- dojde k neúspěšnému přístupu do mezipaměti z důvodu žádné token v mezipaměti
- vypršela platnost tokenu

Pokud jste do mezipaměti tokenu ve vašem kódu, byste měli být připravení zpracovávat scénáře, kde prostředek udává, že vypršela platnost tokenu.

### <a name="resource-ids-for-azure-services"></a>ID prostředků pro služby Azure

V tématu [služeb Azure, podpora Azure AD ověření](msi-overview.md#azure-services-that-support-azure-ad-authentication) seznam služeb, které podporují MSI a jejich odpovídající ID prostředku.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="sign-in-or-token-acquisition-fails"></a>Selhání přihlášení nebo s tokenem akvizic

Ověřte, že soubor MSI je povoleno správně. Přejděte zpět na virtuálním počítači Azure v [portál Azure](https://portal.azure.com) a:

- Podívejte se na stránce "Konfigurace" a zkontrolujte MSI, povoleno = "Yes".
- Podívejte se na stránce "Rozšíření" a zkontrolujte příponou MSI úspěšně nasazena.

Pokud je buď nesprávný, musíte znovu nasaďte MSI v prostředku znovu, nebo vyřešit potíže s selhání nasazení.

### <a name="http-request-error-responses"></a>Chybové odpovědi protokolu HTTP žádosti

- *bad_request_102: není zadána hlavička požadovaná metadata*

  Buď `Metadata` pole hlavičky požadavku ze svého požadavku chybí nebo je v nesprávném formátu. Hodnota musí být zadány jako `true`, všechny malými písmeny. V části "Ukázková žádost" v [předcházející části REST](#rest) příklad.

- *neznámé: Nepodařilo se získat token ze služby Active directory. Podrobnosti najdete v tématu protokoly v \<cesta k souboru\>*

  Ověřte, že žádost HTTP GET URI správně naformátován, zejména prostředek, který je zadaný identifikátor URI v řetězci dotazu. Najdete v části "Ukázková žádost" v [předcházející části REST](#rest) příklad, nebo [služeb Azure, podpora Azure AD ověření](msi-overview.md#azure-services-that-support-azure-ad-authentication) seznam služeb a jejich odpovídající ID prostředku.

- *unknown_source: neznámého zdroje \<identifikátor URI\>*

  Ověřte, že žádost HTTP GET URI správně naformátován. `scheme:host/resource-path` Část musí být zadány jako `http://localhost:50342/oauth2/token`. V části "Ukázková žádost" v [předcházející části REST](#rest) příklad.

## <a name="related-content"></a>Související obsah

- Přehled MSI najdete v tématu [identita spravované služby přehled](msi-overview.md).
- Pokud chcete povolit MSI ve virtuálním počítači Azure, najdete v části [konfigurace Azure virtuálního počítače spravované služby Identity (MSI) pomocí prostředí PowerShell](msi-qs-configure-powershell-windows-vm.md).

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.

