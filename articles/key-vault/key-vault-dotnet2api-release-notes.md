---
title: "Klíč trezoru .NET 2.x API poznámky k verzi | Microsoft Docs"
description: "Vývojáři rozhraní .NET bude používat toto rozhraní API do kódu pro Azure Key Vault"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
editor: bruceper
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bruceper
ms.openlocfilehash: 5b03f5092ee4236ca3e7b12db37dc47bd6d3a309
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Key Vault rozhraní .NET 2.0 – poznámky k verzi a Průvodci migrací
Jsou Tyhle poznámky a pokyny pro vývojáře, kteří pracují s Azure Key Vault .NET nebo knihovna jazyka C#. Při přechodu z verze 1.0 na verzi 2.0, byly provedeny počet aktualizací se bude vyžadovat migrace práci ve vašem kódu v pořadí, abyste mohli využívat funkční zdokonalení a funkci doplňky, jako **Key Vault certifikáty** podporovat.

## <a name="key-vault-certificates"></a>Certifikáty Key Vault

Podpora certifikáty Key Vault poskytuje správu vašeho x509 certifikáty a následující chování:  

* Umožňuje vlastníkovi certifikátů k vytvoření certifikátu provede procesem vytvoření trezoru klíč nebo prostřednictvím importu stávajícího certifikátu. To zahrnuje i podepsaného svým držitelem a vygeneruje certifikáty certifikační autority.
* Umožňuje vlastníka certifikátu Key Vault pro implementaci zabezpečeného úložiště a správu X509 certifikáty bez interakce s materiál privátního klíče.  
* Umožňuje vlastníkovi certifikátů k vytvoření zásady, která přesměruje Key Vault pro správu životního cyklu certifikátu.  
* Umožňuje vlastníkům certifikátu zadejte kontaktní údaje pro oznámení o události životního cyklu vypršení platnosti a obnovení certifikátu.  
* Podporuje automatické obnovení se vybrané vystavitelů - Key Vault partnera X509 certifikátu zprostředkovatelé / certifikační autority.
  
  * Všimněte si --spolupráci zprostředkovatelé/autority také mohou, ale nebude podporovat funkci automatického obnovení.

## <a name="net-support"></a>Podpora v rozhraní .NET

* **Rozhraní .NET 4.0** nepodporuje 2.0 verzi knihovny Azure Key Vault .NET
* **Rozhraní .NET framework 4.5.2** podporuje verze 2.0 knihovny Azure Key Vault .NET
* **.NET standardní 1.4** podporuje verze 2.0 knihovny Azure Key Vault .NET

## <a name="namespaces"></a>Názvové prostory

* Obor názvů pro **modely** změněn z **Microsoft.Azure.KeyVault** k **Microsoft.Azure.KeyVault.Models**.
* **Microsoft.Azure.KeyVault.Internal** obor názvů se ukončí.
* Obor názvů závislosti sady Azure SDK došlo ke změně z **Hyak.Common** a **Hyak.Common.Internals** k **Microsoft.Rest** a **Microsoft.Rest.Serialization**

## <a name="type-changes"></a>Typ změny

* *Tajný klíč* změnit tak, aby *SecretBundle*
* *Slovník* změnit tak, aby *IDictionary*
* *Seznam<T>, string []* změnit tak, aby *rozhraní IList.<T>*
* *NextList* změnit tak, aby *NextPageLink*

## <a name="return-types"></a>Návratové typy

* **KeyList** a **SecretList** vrátí *IPage<T>*  místo *ListKeysResponseMessage*
* Generovaný objekt **BackupKeyAsync** vrátí *BackupKeyResult* obsahující *hodnotu* (blob zálohování). Předtím, než byl uzavřen metodu a vrátí pouze hodnotu.

## <a name="exceptions"></a>Výjimky

* *KeyVaultClientException* se změní na *KeyVaultErrorException*
* Chyba služby se změnilo z *výjimka. Chyba* k *výjimka. Body.Error.Message*.
* Další informace o odeberou v chybové zprávě **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktory

* Místo přijetí *HttpClient* jako argument konstruktoru, konstruktor přijímá pouze *HttpClientHandler* nebo *[DelegatingHandler]*.

## <a name="downloaded-packages"></a>Stažených balíčků

Když klient je zpracování závislost na Key Vault následující byly staženy

### <a name="previous-package-list"></a>Předchozí seznam balíčků

* verze balíčku id="Hyak.Common" = "1.0.2" targetFramework = "net45"
* verze balíčku id="Microsoft.Azure.Common" = "verze 2.0.4" targetFramework = "net45"
* verze balíčku id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"
* verze balíčku id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"
* verze balíčku id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"
* verze balíčku id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"
* verze balíčku id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"
* verze balíčku id="Microsoft.Net.Http" = "2.2.22" targetFramework = "net45"

### <a name="current-package-list"></a>Aktuální seznam balíčků

* verze balíčku id="Microsoft.Azure.KeyVault" = "2.0.0-preview" targetFramework = "net45"
* verze balíčku id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"
* verze balíčku id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"

## <a name="class-changes"></a>Změny – třída

* **UnixEpoch** odebrala – třída
* **Base64UrlConverter** třída je přejmenován na **Base64UrlJsonConverter**

## <a name="other-changes"></a>Další změny

* Na tuto verzi rozhraní API byla přidána podpora pro konfiguraci zásady opakování operace KV na přechodných chyb.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Pro operace, které vrátil *trezoru*, návratový typ byl třídu, která obsahovala vlastnosti trezoru. Návratový typ je nyní *trezoru*.
* *PermissionsToKeys* a *PermissionsToSecrets* jsou nyní *Permissions.Keys* a *Permissions.Secrets*
* Některé změny návratové typy platí pro ovládací prvek roviny také.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Balíček je porušený než **Microsoft.Azure.KeyVault.Extensions** a **Microsoft.Azure.KeyVault.Cryptography** pro operace šifrování.

