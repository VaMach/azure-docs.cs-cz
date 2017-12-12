---
title: "Generování sestav Azure Active Directory audit ukázky rozhraní API | Microsoft Docs"
description: "Jak začít pracovat s Azure Active Directory Reporting API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: de8b8ec3-49b3-4aa8-93fb-e38f52c99743
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/31/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: b9e0fb21986b82f19d90f999f5d905fbf95d2cc9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-reporting-audit-api-samples"></a>Azure Active Directory, vytváření sestav ukázky auditu rozhraní API
Toto téma je součástí kolekce témat o službě Azure Active Directory, vytváření sestav rozhraní API.  
Generování sestav služby Azure AD poskytuje rozhraní API, která umožňuje přístup k datům auditování pomocí kódu nebo související nástroje.
Obor tohoto tématu je poskytnout ukázkový kód pro **audit rozhraní API**.

Přejděte na téma:

* [Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace
* [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro generování sestav.

Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Požadavky
Před použitím ukázky v tomto tématu, které potřebujete k dokončení [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md).  

## <a name="known-issue"></a>Známý problém
Pokud váš klient je v oblasti, Evropa, ověřování aplikace nebude fungovat. Použijte ověřování uživatele pro přístup k rozhraní API auditu jako alternativní řešení, dokud jsme problém opravte. 

## <a name="powershell-script"></a>Skript PowerShellu
    # This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

    # Constants
    $ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
    $ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
    $loginURL       = "https://login.microsoftonline.com"     # AAD Instance; for example https://login.microsoftonline.com
    $tenantdomain   = "your-tenant-domain.onmicrosoft.com"    # AAD Tenant; for example, contoso.onmicrosoft.com
    $resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
    $7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
    Write-Output "Searching for events starting $7daysago"

    # Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    # Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
    if ($oauth.access_token -ne $null) {   
        $i=0
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
        $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&$filter=activityDate gt ' + $7daysago

        # loop through each query page (1 through n)
        Do{
            # display each event on the console window
            Write-Output "Fetching data using Uri: $url"
            $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
            foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
                Write-Output ($event | ConvertTo-Json)
            }

            # save the query page to an output file
            Write-Output "Save the output to a file audit$i.json"
            $myReport.Content | Out-File -FilePath audit$i.json -Force
            $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
            $i = $i+1
        } while($url -ne $null)
    } else {
        Write-Host "ERROR: No Access Token"
        }

    Write-Host "Press any key to continue ..."
    $x = $host.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown")


### <a name="executing-the-powershell-script"></a>Provádění skriptu prostředí PowerShell
Se vrátí po ukončení úprav skript, spouštět a ověřte, že očekávaná data z auditu protokoluje sestavy.

Skript vrátí výstupní ze sestavy auditu ve formátu JSON. Vytvoří také `audit.json` soubor s stejný výstup. Můžete experimentovat změnou skript, který chcete vrátit data z jiných sestavy a komentář výstupní formáty, které nepotřebujete.

## <a name="bash-script"></a>Bash skriptu
    #!/bin/bash

    # Author: Ken Hoff (kenhoff@microsoft.com)
    # Date: 2015.08.20
    # NOTE: This script requires jq (https://stedolan.github.io/jq/)

    CLIENT_ID="your-application-client-id-here"         # Should be a ~35 character string insert your info here
    CLIENT_SECRET="your-application-client-secret-here" # Should be a ~44 character string insert your info here
    LOGIN_URL="https://login.microsoftonline.com"
    TENANT_DOMAIN="your-directory-name-here.onmicrosoft.com"    # For example, contoso.onmicrosoft.com

    TOKEN_INFO=$(curl -s --data-urlencode "grant_type=client_credentials" --data-urlencode "client_id=$CLIENT_ID" --data-urlencode "client_secret=$CLIENT_SECRET" "$LOGIN_URL/$TENANT_DOMAIN/oauth2/token?api-version=1.0")

    TOKEN_TYPE=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.token_type')
    ACCESS_TOKEN=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.access_token')

    # get yesterday's date

    YESTERDAY=$(date --date='1 day ago' +'%Y-%m-%d')

    URL="https://graph.windows.net/$TENANT_DOMAIN/activities/audit?api-version=beta&$filter=activityDate%20gt%20$YESTERDAY"


    REPORT=$(curl -s --header "Authorization: $TOKEN_TYPE $ACCESS_TOKEN" $URL)

    echo $REPORT | ./jq-win64.exe -r '.value' | ./jq-win64.exe -r ".[]"

## <a name="python-script"></a>Skript v jazyce Python
    # Author: Michael McLaughlin (michmcla@microsoft.com)
    # Date: January 20, 2016
    # This requires the Python Requests module: http://docs.python-requests.org

    import requests
    import datetime
    import sys

    client_id = 'your-application-client-id-here'
    client_secret = 'your-application-client-secret-here'
    login_url = 'https://login.microsoftonline.com/'
    tenant_domain = 'your-directory-name-here.onmicrosoft.com'

    # Get an OAuth access token
    bodyvals = {'client_id': client_id,
                'client_secret': client_secret,
                'grant_type': 'client_credentials'}

    request_url = login_url + tenant_domain + '/oauth2/token?api-version=1.0'
    token_response = requests.post(request_url, data=bodyvals)

    access_token = token_response.json().get('access_token')
    token_type = token_response.json().get('token_type')

    if access_token is None or token_type is None:
        print "ERROR: Couldn't get access token"
        sys.exit(1)

    # Use the access token to make the API request
    yesterday = datetime.date.strftime(datetime.date.today() - datetime.timedelta(days=1), '%Y-%m-%d')

    header_params = {'Authorization': token_type + ' ' + access_token}
    request_string = 'https://graph.windows.net/' + tenant_domain + '/activities/audit?api-version=beta&$filter=activityDate%20gt%20' + yesterday   
    response = requests.get(request_string, headers = header_params)

    if response.status_code is 200:
        print response.content
    else:
        print 'ERROR: API request failed'





## <a name="next-steps"></a>Další kroky
* Chcete přizpůsobit ukázky v tomto tématu? Podívejte se [auditování Azure Active Directory referenční dokumentace rozhraní API](active-directory-reporting-api-audit-reference.md). 
* Pokud chcete zobrazit úplný přehled pomocí Azure Active Directory, vytváření sestav rozhraní API, najdete v části [Začínáme s Azure Active Directory, vytváření sestav rozhraní API](active-directory-reporting-api-getting-started.md).
* Pokud chcete získat další informace o vytváření sestav Azure Active Directory, přečtěte si téma [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).  

