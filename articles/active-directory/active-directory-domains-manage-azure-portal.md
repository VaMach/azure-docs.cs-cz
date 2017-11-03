---
title: "Správa vlastních názvů domén v Azure Active Directory | Microsoft Docs"
description: "Koncepty správy a postupy pro správu název domény v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5063cd0a-dba2-4ba9-aa65-b8117490d73a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.openlocfilehash: 1e58af1f8d26b03c07b27d69f13868bccaaa33aa
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Správa vlastních názvů domén v Azure Active Directory
Název domény, je důležitou součástí identifikátoru pro mnoho prostředků adresáře: je součástí uživatelského jména nebo e-mailové adresy pro uživatele, část adresy pro skupinu a můžou být součástí identifikátor ID URI aplikace pro aplikaci. Prostředek v Azure Active Directory (Azure AD) může zahrnovat název domény, který už je ověřený jako vlastníkem adresáře, která obsahuje daný prostředek. Globální správce můžete provádět úlohy správy domény ve službě Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Nastavte název primární domény pro váš adresář Azure AD
Při vytváření adresáře, název domény, jako je například 'contoso.onmicrosoft.com', je také název primární doménu. Primární doména je výchozí název domény pro nového uživatele při vytváření nového uživatele. Nastavení primární doménu zjednodušuje proces pro správce k vytváření nových uživatelů na portálu. Chcete-li změnit název primární domény:

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **Azure Active Directory**.
   
   ![Správa uživatelů otevírání](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Vyberte **názvy domén**.
4. Vyberte název domény, který chcete nastavit jako primární doménu.
5. Vyberte **nastavit jako primární** příkaz. Potvrďte volbu po zobrazení výzvy.
   
   ![Zkontrolujte název domény, primární](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Můžete změnit název primární domény pro váš adresář na všechny ověřené vlastní doménu, která není federovaný. Změna primární domény pro váš adresář nedojde ke změně uživatelská jména pro všechny stávající uživatele.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Přidat vlastní názvy domén do služby Azure AD
> Můžete přidat na maximálně 900 názvů spravované domény. Pokud chcete konfigurovat všechny domén pro federaci s místní služby Active Directory, můžete přidat k maximální 450 názvů domén v každý adresář. Další informace najdete v tématu [federovaný a názvy spravované domény](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain-concepts#federated-and-managed-domain-names).

## <a name="add-subdomains-of-a-custom-domain"></a>Přidat subdomény vlastní domény
Pokud chcete přidat název domény třetí úrovně například 'europe.contoso.com' do vašeho adresáře, měli byste nejprve přidat a ověřit domény druhé úrovně, například contoso.com. Subdoméně bude automaticky ověřit pomocí služby Azure AD. Pokud chcete zjistit, zda byla ověřena subdomény, kterou jste právě přidali, aktualizujte stránku v prohlížeči, který obsahuje seznam domén.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Co dělat, když změníte registrátora DNS pro vlastní název domény
Pokud změníte registrátora DNS pro vlastní název domény, můžete nadále používat vlastní název domény se službou Azure AD samotné bez přerušení a bez další konfigurace úlohy. Pokud používáte vlastní název domény pro Office 365, Intune nebo jiné služby, které jsou závislé na vlastních názvů domén ve službě Azure AD, naleznete v dokumentaci pro tyto služby.

## <a name="delete-a-custom-domain-name"></a>Odstranit vlastní název domény
Vlastní název domény můžete odstranit ze služby Azure AD, pokud vaše organizace už používá název domény, nebo pokud budete muset použít název domény s jinou Azure AD.

Chcete-li odstranit vlastní název domény, musíte nejdřív zkontrolovat, že žádné prostředky ve vašem adresáři spoléhají na název domény. Název domény nelze odstranit z adresáře, pokud:

* Každý uživatel má uživatelské jméno, e-mailovou adresu nebo adresu proxy serveru, který zahrnuje název domény.
* Všechny skupiny má e-mailovou adresu nebo adresu proxy serveru, který zahrnuje název domény.
* Všechny aplikace ve službě Azure AD má aplikaci identifikátor ID URI, který zahrnuje název domény.

Musíte změnit nebo odstranit takových prostředků v adresáři služby Azure AD, chcete-li odstranit vlastní název domény.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Použijte PowerShell nebo rozhraní Graph API, které slouží ke správě názvů domén
Většinu úloh správy pro názvy domén v Azure Active Directory je možné dokončit také pomocí Microsoft PowerShell nebo programově pomocí rozhraní Azure AD Graph API.

* [Použití Powershellu ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Pomocí rozhraní Graph API ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Další kroky
* [Přidat vlastní názvy domén](add-custom-domain.md)

