---
title: "Ovládací prvky stránky Azure API Management | Microsoft Docs"
description: "Další informace o ovládací prvky stránky, která je k dispozici pro použití v šablonách portálu vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 03e0ac8d-64ff-4e9a-b029-d7be14fb31e3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 6aa7a25a9addceee78abe027fb3a19351940464e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-api-management-page-controls"></a>Ovládací prvky stránky Azure API Management
Azure API Management poskytuje následující ovládací prvky pro použití v vývojář portálu šablony.  
  
 Použití ovládacího prvku, umístěte ho na požadované místo v šabloně portálu pro vývojáře. Některé ovládací prvky, jako například [akcí aplikace](#app-actions) řízení, musí mít parametry, jak je znázorněno v následujícím příkladu.  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 Hodnoty parametrů jsou předána jako součást datový model pro šablonu. Ve většině případů můžete jednoduše vložit zadaný příklad pro každý ovládací prvek pro mohl správně fungovat. Další informace o hodnoty parametrů uvidíte oddíl modelu dat ke každé šabloně, ve kterém může použít ovládacího prvku.  
  
 Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
## <a name="developer-portal-template-page-controls"></a>Ovládací prvky stránky šablony portálu vývojáře  
  
-   [Akce aplikace](#app-actions)  
  
-   [Basic přihlášení](#basic-signin)  
  
-   [ovládací prvek stránkování](#paging-control)  
  
-   [Zprostředkovatelé](#providers)  
  
-   [ovládací prvek vyhledávání](#search-control)  
  
-   [registrace](#sign-up)  
  
-   [přihlášení k odběru tlačítko](#subscribe-button)  
  
-   [zrušení předplatného](#subscription-cancel)  
  
##  <a name="app-actions"></a>Akce aplikace  
 `app-actions` Řízení poskytuje uživatelské rozhraní pro interakci s aplikacemi na stránce profilu uživatele na portál pro vývojáře.  
  
 ![aplikace & č. 45; akce řízení](./media/api-management-page-controls/APIM-app-actions-control.png "APIM akcí aplikace řízení")  
  
### <a name="usage"></a>Využití  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parametry  
  
|Parametr|Popis|  
|---------------|-----------------|  
|appId|Id aplikace.|  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 `app-actions` Řízení je možné použít následující šablony portálu vývojáře.  
  
-   [Aplikace](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a>Basic přihlášení  
 `basic-signin` Řízení poskytuje ovládací prvek pro shromažďování informací v přihlašovací stránku v portálu pro vývojáře přihlášení uživatele.  
  
 ![Basic & č. 45; přihlášení řízení](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic přihlášení řízení")  
  
### <a name="usage"></a>Využití  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 `basic-signin` Řízení je možné použít následující šablony portálu vývojáře.  
  
-   [Přihlásit se](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a>ovládací prvek stránkování  
 `paging-control` Poskytuje funkce stránkování vývojáře portálu stránky zobrazující seznam položek.  
  
 ![stránkování řízení](./media/api-management-page-controls/APIM-paging-control.png "APIM stránkování ovládacího prvku")  
  
### <a name="usage"></a>Využití  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 `paging-control` Řízení je možné použít následující šablony portálu vývojáře.  
  
-   [Rozhraní API seznamu](api-management-api-templates.md#APIList)  
  
-   [Seznam problémů](api-management-issue-templates.md#IssueList)  
  
-   [Seznam produktů](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a>Zprostředkovatelé  
 `providers` Řízení poskytuje ovládací prvek pro výběr zprostředkovatelů ověřování v stránku v portálu pro vývojáře pro přihlášení.  
  
 ![poskytovatelé řízení](./media/api-management-page-controls/APIM-providers-control.png "APIM poskytovatelé řízení")  
  
### <a name="usage"></a>Využití  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 `providers` Řízení je možné použít následující šablony portálu vývojáře.  
  
-   [Přihlásit se](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a>ovládací prvek vyhledávání  
 `search-control` Poskytuje funkce hledání vývojáře portálu stránky zobrazující seznam položek.  
  
 ![hledání řízení](./media/api-management-page-controls/APIM-search-control.png "APIM vyhledávání řízení")  
  
### <a name="usage"></a>Využití  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 `search-control` Řízení je možné použít následující šablony portálu vývojáře.  
  
-   [Rozhraní API seznamu](api-management-api-templates.md#APIList)  
  
-   [Seznam produktů](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a>registrace  
 `sign-up` Řízení poskytuje ovládací prvek pro shromažďování informací o profilu uživatele na přihlašovací stránku v portálu pro vývojáře.  
  
 ![přihlašovací & č. 45; až řízení](./media/api-management-page-controls/APIM-sign-up-control.png "APIM registrace ovládacího prvku")  
  
### <a name="usage"></a>Využití  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 `sign-up` Řízení je možné použít následující šablony portálu vývojáře.  
  
-   [Registrace](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a>přihlášení k odběru tlačítko  
 `subscribe-button` Poskytuje ovládací prvek pro přihlášení k odběru uživatele k produktu.  
  
 ![přihlášení k odběru & č. 45; tlačítko – ovládací prvek](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM přihlášení k odběru tlačítko – ovládací prvek")  
  
### <a name="usage"></a>Využití  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parametry  
 Žádné.  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 `subscribe-button` Řízení je možné použít následující šablony portálu vývojáře.  
  
-   [Produktu](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a>zrušení předplatného  
 `subscription-cancel` Řízení poskytuje ovládací prvek pro zrušení předplatného pro určitý produkt v profilu uživatele na stránce portálu pro vývojáře.  
  
 ![předplatné & č. 45; zrušit řízení](./media/api-management-page-controls/APIM-subscription-cancel-control.png "řízení APIM zrušit předplatné")  
  
### <a name="usage"></a>Využití  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parametry  
  
|Parametr|Popis|  
|---------------|-----------------|  
|subscriptionId|Id předplatného zrušit.|  
|cancelUrl|Adresa URL zrušit předplatné.|  
  
### <a name="developer-portal-templates"></a>Šablony na portálu vývojáře  
 `subscription-cancel` Řízení je možné použít následující šablony portálu vývojáře.  
  
-   [Produktu](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Další kroky
Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).