---
title: "Rozhraní REST API Resource Manager | Microsoft Docs"
description: "Přehled příklady ověřování a použití rozhraní REST API Resource Manager"
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 2f7ba23775545637de865f9ef63680ae22c62164
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="resource-manager-rest-apis"></a>Rozhraní REST API pro Správce prostředků
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure Portal](resource-group-portal.md) 
> * [REST API](resource-manager-rest-api.md)
> 
> 

Za každé volání do Azure Resource Manager, za každou nasazené šablony za každý účet úložiště nejsou jeden nebo více volání rozhraní RESTful API Azure Resource Manager. Toto téma je věnována těchto rozhraní API a jak je můžete volat bez použití vůbec žádné sady SDK. Tento přístup je užitečné, pokud chcete plnou kontrolu nad požadavky do Azure, nebo pokud sada SDK pro preferovaný jazyk není k dispozici nebo nepodporuje operace, které potřebujete.

Tento článek neprochází každé rozhraní API, které je vystaven v Azure, ale spíš používá některé operace, jako příklady, jak se připojíte k nim. Jakmile porozumíte základům, můžete si přečíst [referenci rozhraní API REST Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) naleznete podrobné informace o tom, jak používat zbytek rozhraní API.

## <a name="authentication"></a>Authentication
Ověřování pro službu správce prostředků se zpracovává pomocí Azure Active Directory (AD). Pro připojení k jakéhokoli rozhraní API, musíte nejprve ověřit pomocí služby Azure AD pro příjem ověřovací token, který můžete předat každou žádost. Jak jsme jsou popisující přímo čistý volání rozhraní REST API, předpokládáme, že nechcete ověřit tak, že se zobrazí výzva k zadání uživatelského jména a hesla. Také předpokládáme, že nepoužíváte dvou mechanismů Multi-Factor authentication. Proto vytvoříme, co se nazývá aplikaci Azure AD a službu objektu zabezpečení, které se používají k přihlášení. Ale mějte na paměti, který Azure AD podporuje několik postupů ověřování, a všechny z nich může získat token tuto ověřování, které potřebujeme pro následné žádosti rozhraní API.
Postupujte podle [aplikace vytvořit Azure AD a instanční objekt](resource-group-create-service-principal-portal.md) pokyny krok za krokem.

### <a name="generating-an-access-token"></a>Generování Token přístupu
Ověřování služby Azure AD se provádí volání do služby Azure AD, nachází v login.microsoftonline.com. K ověření, musíte mít následující informace:

* ID klienta Azure AD (název se použije k přihlášení, často stejný jako vaší společnosti, ale není nezbytné služby Azure AD)
* ID aplikace (prováděné během kroku vytvoření aplikace Azure AD)
* Heslo (který jste vybrali při vytváření aplikace Azure AD)

V následující požadavek HTTP nezapomeňte nahradit správné hodnoty "ID klienta Azure AD", "ID aplikace" a "Password".

**Obecná žádost HTTP:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... (Pokud je ověřování úspěšné) bude mít za následek odpověď podobná následující odpověď:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Access_token v předchozí odpovědi byla zkrácena zvýšit čitelnost)

**Generování přístupového tokenu pomocí Bash:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generování přístupového tokenu pomocí prostředí PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Odpověď obsahuje přístupový token, informace o tom, jak dlouho je tento token platný a informace o jaké prostředků můžete použít tento token pro.
Přístupový token, který jste obdrželi v předchozích volání protokolu HTTP, musí být předán v pro všechny žádosti do rozhraní API Správce prostředků. Předejte ji jako hodnotu záhlaví s názvem "Autorizace" s hodnotou "Nosiče YOUR_ACCESS_TOKEN". Všimněte si, mezeru mezi "Nosiče" a přístupový token.

Jak vidíte z výše uvedených výsledku HTTP, pro určitou dobu dobu, během které by měly ukládat do mezipaměti a opakovaně používat tento stejný token je platný token. I když je možné k ověřování na základě Azure AD u jednotlivých volání API, by bylo velmi neefektivní.

## <a name="calling-resource-manager-rest-apis"></a>Rozhraní API REST volání Resource Manager
Toto téma pouze pomocí několik rozhraní API vysvětlují základní použití operace REST. Informace o všech operacích, najdete v tématu [rozhraní API REST Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).

### <a name="list-all-subscriptions"></a>Zobrazí seznam všech odběrů
Jedním z nejjednodušší operace, které můžete provést je do seznamu dostupných předplatných, kterým můžete přistupovat. V následující žádosti uvidíte, jak přístupový token je předána jako hlavičku:

(Nahraďte YOUR_ACCESS_TOKEN vaše skutečné přístup k tokenu.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... a v důsledku toho získat seznam předplatná, která tento objekt zabezpečení služby je povolen přístup k

(ID odběru byla zkrácena čitelnější)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Zobrazí seznam všech skupin prostředků v konkrétní předplatné
Všechny prostředky, které jsou k dispozici rozhraní API Resource Manager jsou vnořeny uvnitř skupiny prostředků. Resource Manager můžete dotazovat pro existující skupiny prostředků ve vašem předplatném pomocí následující požadavek HTTP GET. Všimněte si, jak ID předplatného se předávají v jako část adresy URL této doby.

(Nahraďte YOUR_ACCESS_TOKEN a ID_ODBĚRU ID skutečného přístupu token a předplatného)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Odpovědi můžete získat závisí na tom, jestli máte definované žádné skupiny prostředků a pokud ano, kolik.

(ID odběru byla zkrácena čitelnější)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Zatím jsme jste pouze byla dotaz na rozhraní API Resource Manager informace. Je čas vytvoření některé prostředky a Začněme tím nejjednodušší ze všech, skupinu prostředků. Následující požadavek HTTP vytvoří skupinu prostředků v oblast nebo umístění podle vaší volby a přidá značku na ni.

(Nahraďte YOUR_ACCESS_TOKEN, ID_ODBĚRU, RESOURCE_GROUP_NAME se skutečné tokenu přístupu, ID předplatného a název skupiny prostředků, kterou chcete vytvořit)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Pokud bylo úspěšné, můžete získat odpovědi, která je podobná následující odpověď:

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Úspěšně jste vytvořili skupinu prostředků v Azure. Blahopřejeme!

### <a name="deploy-resources-to-a-resource-group-using-a-resource-manager-template"></a>Nasadit prostředky do skupiny prostředků pomocí šablony Resource Manageru
S Resource Managerem, můžete nasadit své prostředky pomocí šablony. Šablona definuje několik prostředků a jejich závislosti. Pro tento oddíl předpokladu, že jste se seznámili s šablony Resource Manageru a jsme právě ukazují, jak můžete využít ke spuštění nasazení volání rozhraní API. Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).

Nasazení šablony není lišit na tom, jak volat jiná rozhraní API. Jeden aspekt důležité je, že nasazení šablony může trvat poměrně dlouhou dobu. Volání rozhraní API právě vrátí, a je to pro vás jako vývojáři dotaz na stav nasazení a zjistěte, pokud se provádí nasazení. Další informace najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).

V tomto příkladu používáme k dispozici na veřejně vystavené šablonu [Githubu](https://github.com/Azure/azure-quickstart-templates). Šablona, kterou používáme nasadí virtuální počítač s Linuxem do oblasti západní USA. I když tento příklad používá šablonu k dispozici v veřejného úložiště jako GitHub, můžete místo toho předat kompletní šablonou jako součást požadavku. Všimněte si, že jsme zadejte hodnoty parametrů v žádosti, které se používají v šabloně nasazené.

(Náhrada ID_ODBĚRU, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD a DNS_NAME_FOR_PUBLIC_IP hodnoty, které jsou vhodné pro vaši žádost o)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Dlouhé JSON odpovědi na tento požadavek se vynechá ke zlepšení čitelnosti této dokumentace. Odpověď obsahuje informace o podle šablony nasazení, které jste vytvořili.

## <a name="next-steps"></a>Další kroky

- Další informace o zpracování asynchronní operace REST najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).
