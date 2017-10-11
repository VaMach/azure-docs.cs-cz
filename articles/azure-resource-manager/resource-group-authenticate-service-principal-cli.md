---
title: "Vytvoření identity pro aplikaci Azure pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Popisuje, jak používat rozhraní příkazového řádku Azure k vytvoření aplikace Azure Active Directory a objektu zabezpečení a jí udělit přístup k prostředkům prostřednictvím řízení přístupu na základě rolí. Ukazuje, jak k ověření aplikace pomocí hesla nebo certifikátu."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 3c5826d58887ff1af4df8e66999d9c1a1643bcc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Vytvoření instančního objektu pro přístup k prostředkům pomocí Azure CLI

Pokud máte aplikace nebo skriptu, která potřebuje přístup k prostředkům, můžete nastavit identity aplikace a ověřit aplikaci s svoje vlastní přihlašovací údaje. Tato identita se označuje jako hlavní název služby. Tento přístup umožňuje:

* Přiřadíte oprávnění k identitě aplikace, která se liší od vlastní oprávnění. Tato oprávnění jsou obvykle omezené na přesně co aplikaci je třeba provést.
* Při provádění bezobslužného skriptu, použijte certifikát pro ověřování.

V tomto článku se dozvíte, jak používat [Azure CLI 1.0](../cli-install-nodejs.md) nastavit aplikaci pro spuštění pod svou vlastní pověření a identity. Nainstalujte nejnovější verzi [Azure CLI 1.0](../cli-install-nodejs.md) prostředí odpovídá v příkladech v tomto článku.

## <a name="required-permissions"></a>Požadovaná oprávnění
K dokončení tohoto tématu, musíte mít dostatečná oprávnění v Azure Active Directory a vašeho předplatného Azure. Konkrétně musí být schopná vytvořit aplikaci ve službě Azure Active Directory a přiřazení objektu služby roli. 

Nejjednodušším způsobem, jak zkontrolovat, jestli má váš účet dostatečná oprávnění, je použít k tomu portál. Informace najdete v článku [Kontrola požadovaných oprávnění na portálu](resource-group-create-service-principal-portal.md#required-permissions).

Nyní, přejít na část pro buď [heslo](#create-service-principal-with-password) nebo [certifikát](#create-service-principal-with-certificate) ověřování.

## <a name="create-service-principal-with-password"></a>Vytvoření instančního objektu s heslem
V této části můžete provést kroky k vytvoření aplikace AD s heslem a přiřadit role Čtenář instanční objekt.

1. Přihlaste se ke svému účtu.
   
   ```azurecli
   azure login
   ```
2. K vytvoření identity aplikace, zadejte název aplikace a hesla, jak je znázorněno v následujícím příkazu:
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   Vrátí se nový instanční objekt. Id objektu je potřeba při udělování oprávnění. Identifikátor guid uvedený s textem hlavní názvy služby je potřeba při přihlášení. Tento identifikátor guid je stejnou hodnotu jako id aplikace. V ukázkových aplikací, tato hodnota se označuje jako `Client ID`. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. Udělte oprávnění objektu služby pro vaše předplatné. V tomto příkladu přidáte objektu služby roli čtečka, která uděluje oprávnění ke čtení všechny prostředky v rámci předplatného. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md). Pro parametr objectid zadejte Id objektu, který jste použili při vytvoření aplikace. Před spuštěním tohoto příkazu, musíte povolit nějakou dobu nového objektu služby, aby se rozšířily změny v rámci Azure Active Directory. Když ručně spustíte tyto příkazy, obvykle dostatek času uplynul mezi úkoly. Ve skriptu, měli byste přidat krok do režimu spánku mezi příkazy (například `sleep 15`). Pokud se zobrazí chyba oznamující, že objekt zabezpečení neexistuje v adresáři, spusťte příkaz znovu.
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
A to je vše! Aplikaci AD a instanční objekt se nastavují. V další části se dozvíte, jak k přihlášení pomocí přihlašovacích údajů prostřednictvím rozhraní příkazového řádku Azure. Pokud chcete použít přihlašovací údaje v kódu aplikace, není potřeba před pokračováním v tomto tématu. Můžete přejít na [ukázkové aplikace](#sample-applications) příklady přihlášení pomocí id aplikace a heslo. 

### <a name="provide-credentials-through-azure-cli"></a>Zadejte přihlašovací údaje prostřednictvím rozhraní příkazového řádku Azure
Nyní budete muset přihlásit jako aplikace k provádění operací.

1. Vždy, když se přihlásíte jako hlavní název služby, je třeba zadat id klienta adresáře pro vaši aplikaci AD. Klient je instance služby Azure Active Directory. Pro načtení id klienta pro vaše předplatné aktuálně ověřený, použijte:
   
   ```azurecli
   azure account show
   ```
   
   Která vrací:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     Pokud potřebujete získat id klienta jiné předplatné, použijte následující příkaz:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Pokud budete potřebovat načíst id klienta, který chcete použít pro přihlášení, použijte:
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     Hodnota, která má použít pro přihlášení je identifikátor guid uvedený v hlavní názvy služby.
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. Přihlaste se jako objekt služby.
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    Zobrazí se výzva k zadání hesla. Zadejte heslo, které jste zadali při vytváření aplikace AD.
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

Nyní jste přihlášeni jako objekt služby pro objekt služby, kterou jste vytvořili.

Alternativně můžete vyvolat operace REST z příkazového řádku k přihlášení. Z odpovědi ověřování může získat přístupový token pro použití s dalšími operacemi. Příklad získání tokenu přístupu vyvoláním operace REST, naleznete v části [generování tokenu přístupu](resource-manager-rest-api.md#generating-an-access-token).

## <a name="create-service-principal-with-certificate"></a>Vytvoření instančního objektu s certifikátem
V této části proveďte postup:

* Vytvořit certifikát podepsaný svým držitelem
* Vytvořte aplikaci AD se certifikát a instanční objekt
* přiřadit role Čtenář instančního objektu

K provedení těchto kroků, musíte mít [OpenSSL](http://www.openssl.org/) nainstalována.

1. Vytvořte certifikát podepsaný svým držitelem.
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. V předchozím kroku vytvořili dva soubory - privkey.pem a cert.pem. Veřejné a soukromé klíče zkombinovat do jednoho souboru.

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. Otevřete **examplecert.pem** souborů a vyhledejte dlouhé posloupnosti znaků mezi **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---**. Zkopírujte data certifikátu. Při vytváření služby hlavní předáte tato data jako parametr.

4. Přihlaste se ke svému účtu.

   ```azurecli
   azure login
   ```
5. Pokud chcete vytvořit objekt služby, zadejte název aplikace a data certifikátu, jak je znázorněno v následujícím příkazu:
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   Vrátí se nový instanční objekt. Id objektu je potřeba při udělování oprávnění. Identifikátor guid uvedený s textem hlavní názvy služby je potřeba při přihlášení. Tento identifikátor guid je stejnou hodnotu jako id aplikace. V ukázkových aplikací tato hodnota se označuje jako ID klienta. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. Udělte oprávnění objektu služby pro vaše předplatné. V tomto příkladu přidáte objektu služby roli čtečka, která uděluje oprávnění ke čtení všechny prostředky v rámci předplatného. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md). Pro parametr objectid zadejte Id objektu, který jste použili při vytvoření aplikace. Před spuštěním tohoto příkazu, musíte povolit nějakou dobu nového objektu služby, aby se rozšířily změny v rámci Azure Active Directory. Když ručně spustíte tyto příkazy, obvykle dostatek času uplynul mezi úkoly. Ve skriptu, měli byste přidat krok do režimu spánku mezi příkazy (například `sleep 15`). Pokud se zobrazí chyba oznamující, že objekt zabezpečení neexistuje v adresáři, spusťte příkaz znovu.
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a>Zadejte certifikát pomocí automatizované skriptu rozhraní příkazového řádku Azure
Nyní budete muset přihlásit jako aplikace k provádění operací.

1. Vždy, když se přihlásíte jako hlavní název služby, je třeba zadat id klienta adresáře pro vaši aplikaci AD. Klient je instance služby Azure Active Directory. Pro načtení id klienta pro vaše předplatné aktuálně ověřený, použijte:
   
   ```azurecli
   azure account show
   ```
   
   Která vrací:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   Pokud potřebujete získat id klienta jiné předplatné, použijte následující příkaz:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Pokud chcete načíst kryptografický otisk certifikátu a odeberte nepotřebné znaky, použijte:
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   Která vrací podobně jako hodnota kryptografického otisku:
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. Pokud budete potřebovat načíst id klienta, který chcete použít pro přihlášení, použijte:
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   Hodnota, která má použít pro přihlášení je identifikátor guid uvedený v hlavní názvy služby.
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. Přihlaste se jako objekt služby.
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

Nyní jste přihlášeni jako hlavní služby pro aplikaci Azure Active Directory, kterou jste vytvořili.

## <a name="change-credentials"></a>Změnit pověření

Chcete-li změnit přihlašovací údaje pro aplikaci AD, buď kvůli ohrožení zabezpečení nebo vypršení platnosti pověření použít `azure ad app set`.

Chcete-li změnit heslo, použijte:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

Chcete-li změnit hodnotu certifikát, použijte:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a>Ladění

Při vytváření objektu služby se můžete setkat s následujícími chybami:

* **"Authentication_Unauthorized"** nebo **"žádné předplatné nalezena v kontextu."** -Se zobrazí tato chyba, pokud váš účet nemá [požadovaná oprávnění](#required-permissions) v Azure Active Directory pro registraci aplikace. Obvykle se zobrazí tato chyba při jenom správci ve vašem Azure Active Directory můžete zaregistrovat aplikace a váš účet není správce. Požádejte správce buď můžete přiřadit role správce nebo umožňuje uživatelům registrovat aplikace.

* Váš účet **"nemá oprávnění k provedení akce 'Microsoft.Authorization/roleAssignments/write' u rozsahu: /subscriptions/ {guid}'."**  -Zobrazí tato chyba, pokud se váš účet nemá dostatečná oprávnění k přiřazení role identity. Požádejte správce předplatného můžete přidat do role správce přístupu uživatelů.

## <a name="sample-applications"></a>Ukázkové aplikace
Informace o protokolování jako aplikace prostřednictvím různých platforem najdete v tématu:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Další kroky
* Podrobné pokyny k integraci aplikace do Azure pro správu prostředků najdete v tématu [Příručka pro vývojáře k autorizaci s rozhraním API pro Azure Resource Manager](resource-manager-api-authentication.md).
* Chcete-li získat další informace o používání certifikátů a rozhraní příkazového řádku Azure, najdete v části [ověřování prostřednictvím certifikátu s objekty služby Azure z příkazového řádku Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
* Seznam dostupných akcí, které může být povolen nebo odepřen uživatelům najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).
