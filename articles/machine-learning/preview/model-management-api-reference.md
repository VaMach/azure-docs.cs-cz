---
title: "Vytvoření bitové kopie Docker Model správy v nástroji Azure Machine Learning | Microsoft Docs"
description: "Tento článek popisuje kroky pro vytvoření image Docker pro webovou službu."
services: machine-learning
author: chhavib
ms.author: chhavib
manager: neerajkh
editor: jasonwhowell
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 134971e4a663baefa4e1051f087038d3debcb969
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>Azure Machine Learning Model správy účtu API – referenční informace

Informace o nastavení prostředí pro nasazení najdete v tématu [nastavení modelu správce účtu](deployment-setup-configuration.md).

Azure Machine Learning Model správy účtu API implementuje následující operace:

- Registrace modelu
- Vytvoření manifestu
- Vytvoření bitové kopie docker
- Vytvoření webové služby

Tuto bitovou kopii můžete použít k vytvoření webové služby místně nebo na vzdálený cluster Azure Container Service nebo jiné podporované Docker prostředí podle svého výběru.

## <a name="prerequisites"></a>Požadavky
Zajistěte, aby jste prošli kroků instalace v [instalovat a vytvořte rychlý Start](quickstart-installation.md) dokumentu.

Toto jsou požadovány, než budete pokračovat:
1. Model správy zřizování účtu
2. Vytvoření prostředí pro nasazení a správu modelů
3. Model Machine Learning

### <a name="azure-ad-token"></a>Azure AD token
Pokud používáte Azure CLI, přihlaste se pomocí `az login`. Rozhraní příkazového řádku používá váš token zabezpečení Azure Active Directory (Azure AD) ze souboru .azure. Pokud chcete použít rozhraní API, máte následující možnosti.

##### <a name="acquire-the-azure-ad-token-manually"></a>Získání tokenu Azure AD ručně
Můžete použít `az login` a získat nejnovější token ze souboru .azure na domovský adresář.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>Získání tokenu Azure AD prostřednictvím kódu programu
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Po vytvoření objektu služby, uložte si výstup. Teď, můžete použít k získání tokenu z Azure AD:

```cs
 private static async Task<string> AcquireTokenAsync(string clientId, string password, string authority, string resource)
{
        var creds = new ClientCredential(clientId, password);
        var context = new AuthenticationContext(authority);
        var token = await context.AcquireTokenAsync(resource, creds).ConfigureAwait(false);
        return token.AccessToken;
}
```
Token je uvést v hlavičce autorizace pro volání rozhraní API.


## <a name="register-a-model"></a>Zaregistrovat modelu

Krok registrace modelu zaregistruje váš model Machine Learning s Azure Model správy účtu, který jste vytvořili. Tato registrace umožňuje sledování modelů a jejich verze, které jsou přiřazeny v době registrace. Uživatel poskytuje název modelu. Následující registraci modelů pod stejným názvem generuje novou verzí a ID.

### <a name="request"></a>Žádost

| Metoda | Identifikátor URI požadavku |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / modelů 
 |
### <a name="description"></a>Popis
Zaregistruje modelu.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| model | Text | Datové části, která se používá k registraci modelu. | Ano | [Model](#model) |


### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | OK. Model registrace byla úspěšná. | [Model](#model) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Dotaz na seznam modelů v účtu
### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / modelů 
 |
### <a name="description"></a>Popis
Dotazuje seznam modelů v účtu. Můžete filtrovat seznam výsledků podle značky a názvu. Pokud není předán žádný filtr dotaz uvádí všechny modely v účtu. Vráceném seznamu jsou čísla stránek vložena a počet položek v každé stránce je volitelný parametr.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| jméno | query | Název objektu. | Ne | řetězec |
| Značka | query | Značka modelu. | Ne | řetězec |
| Počet | query | Počet položek načíst na stránce. | Ne | řetězec |
| $skipToken | query | Token pro pokračování pro načtení další stránky. | Ne | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [PaginatedModelList](#paginatedmodellist) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Získat podrobné informace o modelu
### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /models/ {id}  
 |

### <a name="description"></a>Popis
Získá model podle ID.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID objektu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [Model](#model) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Zaregistrovat manifestu registrované modelu a všechny závislosti

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / manifesty | 

### <a name="description"></a>Popis
Zaregistruje manifestu se registrované modelu a všechny jeho závislé součásti.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| manifestRequest | Text | Datová část, která slouží k registraci manifestu. | Ano | [Manifest](#manifest) |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Manifestu registrace byla úspěšná. | [Manifest](#manifest) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>Dotaz na seznam manifesty na účtu

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / manifesty | 

### <a name="description"></a>Popis
Dotazuje seznam manifesty v účtu. Můžete filtrovat seznam výsledků podle modelu ID a název manifestu. Pokud není předán žádný filtr dotaz uvádí všechny manifesty v účtu. Vráceném seznamu jsou čísla stránek vložena a počet položek v každé stránce je volitelný parametr.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| %{ModelID/ | query | ID modelu. | Ne | řetězec |
| ManifestName | query | Název souboru manifestu. | Ne | řetězec |
| Počet | query | Počet položek načíst na stránce. | Ne | řetězec |
| $skipToken | query | Token pro pokračování pro načtení další stránky. | Ne | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [PaginatedManifestList](#paginatedmanifestlist) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Získat podrobnosti o manifestu

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /manifests/ {id} | 

### <a name="description"></a>Popis
Získá manifest podle ID.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID objektu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [Manifest](#manifest) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Vytvoření image

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / bitové kopie | 

### <a name="description"></a>Popis
Vytvoří bitovou kopii jako obrázek na Docker v registru kontejner Azure.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| imageRequest | Text | Datová část, která se používá k vytvoření image. | Ano | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Odezvy
| Kód | Popis | Záhlaví | Schéma |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Adresa URL umístění asynchronní operaci. Volání GET zobrazí stav úlohy vytváření bitové kopie. | Operace umístění |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>Dotaz na seznam obrázků v účtu

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / bitové kopie | 

### <a name="description"></a>Popis
Dotazy v seznamu bitových kopií v účtu. Můžete filtrovat seznam výsledků manifestu ID a název. Pokud není předán žádný filtr dotaz uvádí všechny bitové kopie v účtu. Vráceném seznamu jsou čísla stránek vložena a počet položek v každé stránce je volitelný parametr.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| ID manifestu | query | ID manifestu. | Ne | řetězec |
| ManifestName | query | Název souboru manifestu. | Ne | řetězec |
| Počet | query | Počet položek načíst na stránce. | Ne | řetězec |
| $skipToken | query | Token pro pokračování pro načtení další stránky. | Ne | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [PaginatedImageList](#paginatedimagelist) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Načíst podrobnosti image.

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /images/ {id} | 

### <a name="description"></a>Popis
Získá bitovou kopii podle ID.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID obrázku. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [Bitové kopie](#image) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Vytvoření služby

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / služby | 

### <a name="description"></a>Popis
Vytvoří službu z bitové kopie.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| elementu serviceRequest | Text | Datová část, která se používá k vytvoření služby. | Ano | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Odezvy
| Kód | Popis | Záhlaví | Schéma |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Adresa URL umístění asynchronní operaci. Volání GET zobrazí stav úlohy vytváření služby. | Operace umístění |
| 409 | Služba se zadaným názvem již existuje. |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Dotaz na seznam služeb v účtu

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / služby | 

### <a name="description"></a>Popis
Dotazuje seznam služeb v účtu. Můžete filtrovat seznam výsledků podle názvu nebo ID modelu, manifestu název nebo ID, ID bitové kopie, název služby nebo ID Machine Learning výpočetních prostředků. Pokud není předán žádný filtr dotaz uvádí všechny služby v účtu. Vráceném seznamu jsou čísla stránek vložena a počet položek v každé stránce je volitelný parametr.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| ServiceName | query | Název služby. | Ne | řetězec |
| %{ModelID/ | query | Název modelu. | Ne | řetězec |
| %{ModelName/ | query | ID modelu. | Ne | řetězec |
| ID manifestu | query | ID manifestu. | Ne | řetězec |
| ManifestName | query | Název souboru manifestu. | Ne | řetězec |
| ID obrázku | query | ID obrázku. | Ne | řetězec |
| computeResourceId | query | ID počítače Learning výpočetních prostředků. | Ne | řetězec |
| Počet | query | Počet položek načíst na stránce. | Ne | řetězec |
| $skipToken | query | Token pro pokračování pro načtení další stránky. | Ne | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [PaginatedServiceList](#paginatedservicelist) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Získat podrobnosti služby

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Popis
Získá službu podle ID.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID objektu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [ServiceResponse](#serviceresponse) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Aktualizace služby

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| PUT |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Popis
Aktualizuje existující službu.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID objektu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| serviceUpdateRequest | Text | Datová část, která se používá k aktualizaci existující službu. | Ano |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Odezvy
| Kód | Popis | Záhlaví | Schéma |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Adresa URL umístění asynchronní operaci. Volání GET zobrazí stav úlohy aktualizace služby. | Operace umístění |
| 404 | Služba se zadaným ID neexistuje. |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Odstranění služby

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| ODSTRANIT |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Popis
Odstraní služby.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID objektu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch |  |
| 204 | Služba se zadaným ID neexistuje. |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Získat klíče služby

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / klíče | 

### <a name="description"></a>Popis
Získá klíče služby.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID služby. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [AuthKeys](#authkeys)
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Obnovit klíče služby

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / klíče | 

### <a name="description"></a>Popis
Regeneruje klíče služby a vrátí tyto adresy.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID služby. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| regenerateKeyRequest | Text | Datová část, která se používá k aktualizaci existující službu. | Ano | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [AuthKeys](#authkeys)
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Dotaz na seznam nasazení v účtu

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / nasazení | 

### <a name="description"></a>Popis
Dotazuje seznamu nasazení v účtu. Můžete filtrovat seznam výsledků podle ID služby, které se vrátí jenom nasazení, které jsou vytvořené pro konkrétní službu. Pokud není předán žádný filtr dotaz uvádí všechna nasazení v účtu.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |
| serviceId | query | ID služby. | Ne | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [DeploymentList](#deploymentlist) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Získat podrobnosti o nasazení

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /deployments/ {id} | 

### <a name="description"></a>Popis
Získá nasazení podle ID.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID nasazení. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [Nasazení](#deployment) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>Získat podrobnosti o operaci

### <a name="request"></a>Žádost
| Metoda | Identifikátor URI požadavku |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /operations/ {id} | 

### <a name="description"></a>Popis
Získá stav asynchronní operace podle ID operace.

### <a name="parameters"></a>Parametry
| Název | Umístěný v | Popis | Požaduje se | Schéma
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | Cesta | ID předplatného Azure. | Ano | řetězec |
| Název skupiny prostředků | Cesta | Název skupiny prostředků, ve kterém je umístěn účet modelu správy. | Ano | řetězec |
| název účtu | Cesta | Název účtu pro správu modelu. | Ano | řetězec |
| id | Cesta | ID operace. | Ano | řetězec |
| verze rozhraní API. | query | Verze zprostředkovatele prostředků Microsoft.Machine.Learning rozhraní API používat. | Ano | řetězec |
| Autorizace | záhlaví | Autorizační token. Měla by vypadat jako "Nosiče XXXXXX." | Ano | řetězec |

### <a name="responses"></a>Odezvy
| Kód | Popis | Schéma |
|--------------------|--------------------|--------------------|
| 200 | Úspěch | [OperationStatus](#asyncoperationstatus) |
| výchozí | Odpovědi na chybu, která popisuje, proč operace se nezdařila. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Definice

<a name="asset"></a>
### <a name="asset"></a>Prostředek
Objekt asset, který bude potřeba při vytváření Docker obrázku.


|Název|Popis|Schéma|
|---|---|---|
|**ID**  <br>*volitelné*|ID prostředku.|řetězec|
|**mimeType**  <br>*volitelné*|Typ MIME modelu obsahu. Další informace o typu MIME najdete v tématu [seznam typů médií IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|řetězec|
|**Rozbalte**  <br>*volitelné*|Určuje, které je třeba rozbalit obsah během vytváření bitové kopie Docker.|Logická hodnota|
|**Adresa URL**  <br>*volitelné*|Adresa URL umístění Asset.|řetězec|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
Stav asynchronní operace.

*Typ*: výčtu (NotStarted, spuštění, zrušeno, bylo úspěšné, neúspěšné)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
Stav operace.


|Název|Popis|Schéma|
|---|---|---|
|**createdTime**  <br>*volitelné*  <br>*jen pro čtení*|Asynchronní operace vytvoření čas (UTC).|řetězec (datum a čas)|
|**čas ukončení**  <br>*volitelné*  <br>*jen pro čtení*|Asynchronní operace koncový čas (UTC).|řetězec (datum a čas)|
|**Chyba**  <br>*volitelné*||[ErrorResponse](#errorresponse)|
|**ID**  <br>*volitelné*|ID asynchronní operace.|řetězec|
|**Typ operace**  <br>*volitelné*|Typ asynchronní operace.|výčet (bitové kopie, služba)|
|**resourceLocation**  <br>*volitelné*|Prostředek vytvořit nebo aktualizovat asynchronní operace.|řetězec|
|**Stav**  <br>*volitelné*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
Ověřovací klíče pro službu.


|Název|Popis|Schéma|
|---|---|---|
|**primaryKey**  <br>*volitelné*|Primární klíč.|řetězec|
|**sekundární klíč**  <br>*volitelné*|Sekundární klíč.|řetězec|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Nastavení autoscaler.


|Název|Popis|Schéma|
|---|---|---|
|**autoscaleEnabled**  <br>*volitelné*|Povolit nebo zakázat autoscaler.|Logická hodnota|
|**maxReplicas**  <br>*volitelné*|Maximální počet replik pod až škálování.  <br>**Minimální hodnota**:`1`|celé číslo|
|**minReplicas**  <br>*volitelné*|Minimální počet replik pod škálování dolů na.  <br>**Minimální hodnota**:`0`|celé číslo|
|**refreshPeriodInSeconds**  <br>*volitelné*|Aktualizace času automatického škálování aktivační událost.  <br>**Minimální hodnota**:`1`|celé číslo|
|**targetUtilization**  <br>*volitelné*|Procento využití, která aktivuje automatické škálování.  <br>**Minimální hodnota**:`0`  <br>**Maximální hodnota**:`100`|celé číslo|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
Machine Learning výpočetní prostředky.


|Název|Popis|Schéma|
|---|---|---|
|**ID**  <br>*volitelné*|ID prostředku.|řetězec|
|**Typ**  <br>*volitelné*|Typ prostředku.|výčet (Cluster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Konfigurace tak, aby vyhradil prostředky pro kontejner v clusteru.


|Název|Popis|Schéma|
|---|---|---|
|**využití procesoru**  <br>*volitelné*|Určuje rezervace procesoru. Formát pro Kubernetes: najdete v části [význam procesoru](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|řetězec|
|**paměť**  <br>*volitelné*|Určuje paměť rezervace. Formát pro Kubernetes: najdete v části [význam paměti](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|řetězec|


<a name="deployment"></a>
### <a name="deployment"></a>Nasazení
Instance nasazení služby Azure Machine Learning.


|Název|Popis|Schéma|
|---|---|---|
|**createdAt**  <br>*volitelné*  <br>*jen pro čtení*|Čas vytvoření nasazení (UTC).|řetězec (datum a čas)|
|**expiredAt**  <br>*volitelné*  <br>*jen pro čtení*|Platnost nasazení čas (UTC).|řetězec (datum a čas)|
|**ID**  <br>*volitelné*|ID nasazení.|řetězec|
|**ID obrázku**  <br>*volitelné*|ID obrázku přidružený toto nasazení.|řetězec|
|**serviceName**  <br>*volitelné*|Název služby.|řetězec|
|**Stav**  <br>*volitelné*|Aktuální stav nasazení.|řetězec|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Pole objektů nasazení.

*Typ*: <[nasazení](#deployment)> pole


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Model správy podrobnosti o chybě služby.


|Název|Popis|Schéma|
|---|---|---|
|**kód**  <br>*požadované*|Kód chyby.|řetězec|
|**zpráva**  <br>*požadované*|Chybová zpráva.|řetězec|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Objekt modelu správy služby chyby.


|Název|Popis|Schéma|
|---|---|---|
|**kód**  <br>*požadované*|Kód chyby.|řetězec|
|**Podrobnosti**  <br>*volitelné*|Pole objektů podrobnosti o chybě.|<[ErrorDetail](#errordetail)> pole|
|**zpráva**  <br>*požadované*|Chybová zpráva.|řetězec|
|**statusCode**  <br>*volitelné*|Stavový kód HTTP.|celé číslo|


<a name="image"></a>
### <a name="image"></a>Image
Obrázek Azure Machine Learning.


|Název|Popis|Schéma|
|---|---|---|
|**computeResourceId**  <br>*volitelné*|ID prostředí vytvořené v Machine Learning výpočetní prostředek.|řetězec|
|**createdTime**  <br>*volitelné*|Čas vytvoření bitové kopie (UTC).|řetězec (datum a čas)|
|**creationState**  <br>*volitelné*||[AsyncOperationState](#asyncoperationstate)|
|**Popis**  <br>*volitelné*|Text popisu Image.|řetězec|
|**Chyba**  <br>*volitelné*||[ErrorResponse](#errorresponse)|
|**ID**  <br>*volitelné*|ID obrázku.|řetězec|
|**imageBuildLogUri**  <br>*volitelné*|Identifikátor URI nahrané protokoly ze sestavení bitové kopie.|řetězec|
|**Vlastnost imageLocation**  <br>*volitelné*|Azure řetězec umístění kontejneru registru pro vytvoření bitové kopie.|řetězec|
|**imageType**  <br>*volitelné*||[ImageType](#imagetype)|
|**manifest**  <br>*volitelné*||[Manifest](#manifest)|
|**Jméno**  <br>*volitelné*|Název bitové kopie.|řetězec|
|**verze**  <br>*volitelné*|Verze bitové kopie nastavit službou Management modelu.|celé číslo|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Požadavek na vytvoření image Azure Machine Learning.


|Název|Popis|Schéma|
|---|---|---|
|**computeResourceId**  <br>*požadované*|ID prostředí vytvořené v Machine Learning výpočetní prostředek.|řetězec|
|**Popis**  <br>*volitelné*|Text popisu Image.|řetězec|
|**imageType**  <br>*požadované*||[ImageType](#imagetype)|
|**ID manifestu**  <br>*požadované*|ID manifestu, ze kterého se vytvoří bitovou kopii.|řetězec|
|**Jméno**  <br>*požadované*|Název bitové kopie.|řetězec|


<a name="imagetype"></a>
### <a name="imagetype"></a>Typ obrázku
Určuje typ obrázku.

*Typ*: výčtu (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifest
Azure Machine Learning manifest.


|Název|Popis|Schéma|
|---|---|---|
|**prostředky**  <br>*požadované*|Seznam prostředků.|<[Asset](#asset)> pole|
|**createdTime**  <br>*volitelné*  <br>*jen pro čtení*|Vytvoření manifestu čas (UTC).|řetězec (datum a čas)|
|**Popis**  <br>*volitelné*|Manifest text popisu.|řetězec|
|**driverProgram**  <br>*požadované*|Ovladač program manifestu.|řetězec|
|**ID**  <br>*volitelné*|ID manifestu.|řetězec|
|**modelIds**  <br>*volitelné*|Seznam ID modelu registrované modelů. Žádost se nezdaří, pokud nejsou registrované žádné zahrnuté modelů.|<string>pole|
|**modelType**  <br>*volitelné*|Určuje, že modely jsou již registrovaný ve službě správy modelu.|výčet (registrovaná)|
|**Jméno**  <br>*požadované*|Název souboru manifestu.|řetězec|
|**targetRuntime**  <br>*požadované*||[TargetRuntime](#targetruntime)|
|**verze**  <br>*volitelné*  <br>*jen pro čtení*|Verze manifestu přiřazené službou modelu správy.|celé číslo|
|**webserviceType**  <br>*volitelné*|Určuje požadovaný typ webové služby, která bude vytvořena z manifestu.|výčet (v reálném čase)|


<a name="model"></a>
### <a name="model"></a>Model
Instance model Azure Machine Learning.


|Název|Popis|Schéma|
|---|---|---|
|**createdAt**  <br>*volitelné*  <br>*jen pro čtení*|Model vytváření čas (UTC).|řetězec (datum a čas)|
|**Popis**  <br>*volitelné*|Textový popis modelu.|řetězec|
|**ID**  <br>*volitelné*  <br>*jen pro čtení*|ID modelu.|řetězec|
|**mimeType**  <br>*požadované*|Typ MIME obsah modelu. Další informace o typu MIME najdete v tématu [seznam typů médií IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|řetězec|
|**Jméno**  <br>*požadované*|Název modelu.|řetězec|
|**značek**  <br>*volitelné*|Seznam značek modelu.|<string>pole|
|**Rozbalte**  <br>*volitelné*|Určuje, jestli je potřeba rozbalte modelu během vytváření bitové kopie Docker.|Logická hodnota|
|**Adresa URL**  <br>*požadované*|Adresa URL modelu. Obvykle jsme uvést adresu URL sdílený přístupový podpis sem.|řetězec|
|**verze**  <br>*volitelné*  <br>*jen pro čtení*|Verze modelu přiřazené službou modelu správy.|celé číslo|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Informace o modelu dat kolekce.


|Název|Popis|Schéma|
|---|---|---|
|**eventHubEnabled**  <br>*volitelné*|Povolte Centru událostí pro službu.|Logická hodnota|
|**storageEnabled**  <br>*volitelné*|Povolte úložiště pro službu.|Logická hodnota|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Seznam stránkované bitové kopie.


|Název|Popis|Schéma|
|---|---|---|
|**odkaz nextLink**  <br>*volitelné*|Pokračování odkaz na další stránku výsledků v seznamu (absolutní identifikátor URI).|řetězec|
|**Hodnota**  <br>*volitelné*|Pole objektů modelu.|<[Obrázek](#image)> pole|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Seznam stránkované manifesty.


|Název|Popis|Schéma|
|---|---|---|
|**odkaz nextLink**  <br>*volitelné*|Pokračování odkaz na další stránku výsledků v seznamu (absolutní identifikátor URI).|řetězec|
|**Hodnota**  <br>*volitelné*|Pole objektů manifestu.|<[Manifest](#manifest)> pole|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Stránkované seznam modelů.


|Název|Popis|Schéma|
|---|---|---|
|**odkaz nextLink**  <br>*volitelné*|Pokračování odkaz na další stránku výsledků v seznamu (absolutní identifikátor URI).|řetězec|
|**Hodnota**  <br>*volitelné*|Pole objektů modelu.|<[Model](#model)> pole|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Stránkované seznam služeb.


|Název|Popis|Schéma|
|---|---|---|
|**odkaz nextLink**  <br>*volitelné*|Pokračování odkaz na další stránku výsledků v seznamu (absolutní identifikátor URI).|řetězec|
|**Hodnota**  <br>*volitelné*|Pole objektů služby.|<[ServiceResponse](#serviceresponse)> pole|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Požadavek na vytvoření služby.


|Název|Popis|Schéma|
|---|---|---|
|**appInsightsEnabled**  <br>*volitelné*|Povolte application insights pro služby.|Logická hodnota|
|**autoScaler**  <br>*volitelné*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*požadované*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*volitelné*||[ContainerResourceReservation](#containerresourcereservation)|
|**Element dataCollection**  <br>*volitelné*||[ModelDataCollection](#modeldatacollection)|
|**ID obrázku**  <br>*požadované*|Obrázek se vytvořit službu.|řetězec|
|**maxConcurrentRequestsPerContainer**  <br>*volitelné*|Maximální počet souběžných požadavků.  <br>**Minimální hodnota**:`1`|celé číslo|
|**Jméno**  <br>*požadované*|Název služby.|řetězec|
|**numReplicas**  <br>*volitelné*|Počet replik pod systémem kdykoli. Nelze zadat, zda je povoleno Autoscaler.  <br>**Minimální hodnota**:`0`|celé číslo|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Požadavek na znovu vygenerovat klíč pro službu.


|Název|Popis|Schéma|
|---|---|---|
|**Typ_klíče.**  <br>*volitelné*|Určuje, který klíč znovu vygenerovat.|výčet (primární, sekundární)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Podrobný stav služby.


|Název|Popis|Schéma|
|---|---|---|
|**createdAt**  <br>*volitelné*|Vytvoření služby čas (UTC).|řetězec (datum a čas)|
|**ID**  <br>*volitelné*|ID služby.|řetězec|
|**bitové kopie**  <br>*volitelné*||[Bitové kopie](#image)|
|**manifest**  <br>*volitelné*||[Manifest](#manifest)|
|**modely**  <br>*volitelné*|Seznam modelů.|<[Model](#model)> pole|
|**Jméno**  <br>*volitelné*|Název služby.|řetězec|
|**scoringUri**  <br>*volitelné*|Identifikátor URI pro vyhodnocování službu.|řetězec|
|**Stav**  <br>*volitelné*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*volitelné*|Poslední aktualizace čas (UTC).|řetězec (datum a čas)|
|**appInsightsEnabled**  <br>*volitelné*|Povolte application insights pro služby.|Logická hodnota|
|**autoScaler**  <br>*volitelné*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*požadované*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*volitelné*||[ContainerResourceReservation](#containerresourcereservation)|
|**Element dataCollection**  <br>*volitelné*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*volitelné*|Maximální počet souběžných požadavků.  <br>**Minimální hodnota**:`1`|celé číslo|
|**numReplicas**  <br>*volitelné*|Počet replik pod systémem kdykoli. Nelze zadat, zda je povoleno Autoscaler.  <br>**Minimální hodnota**:`0`|celé číslo|
|**Chyba**  <br>*volitelné*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>serviceUpdateRequest
Žádost o aktualizaci služby.


|Název|Popis|Schéma|
|---|---|---|
|**appInsightsEnabled**  <br>*volitelné*|Povolte application insights pro služby.|Logická hodnota|
|**autoScaler**  <br>*volitelné*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*volitelné*||[ContainerResourceReservation](#containerresourcereservation)|
|**Element dataCollection**  <br>*volitelné*||[ModelDataCollection](#modeldatacollection)|
|**ID obrázku**  <br>*volitelné*|Obrázek se vytvořit službu.|řetězec|
|**maxConcurrentRequestsPerContainer**  <br>*volitelné*|Maximální počet souběžných požadavků.  <br>**Minimální hodnota**:`1`|celé číslo|
|**numReplicas**  <br>*volitelné*|Počet replik pod systémem kdykoli. Nelze zadat, zda je povoleno Autoscaler.  <br>**Minimální hodnota**:`0`|celé číslo|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Typ cílový modul runtime.


|Název|Popis|Schéma|
|---|---|---|
|**Vlastnosti**  <br>*požadované*||< řetězec, řetězec > mapy|
|**runtimeType**  <br>*požadované*|Určuje modulu runtime.|výčet (SparkPython, Python)|

