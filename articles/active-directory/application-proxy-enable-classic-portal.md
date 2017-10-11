---
title: "Povolení Proxy aplikace Azure AD na portálu classic | Microsoft Docs"
description: "Zapněte proxy aplikace v portálu Azure Classic a nainstalujte konektory pro reverzní proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: ea97fdc8d146ed524a932018b572ceda0982738b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-application-proxy-in-the-classic-portal-and-download-connectors"></a>Povolení Proxy aplikace v portálu classic a stáhnout konektory
Tento článek vás provede postupem, který umožňuje povolit proxy aplikace u cloudového adresáře služby Microsoft Azure AD.

Pokud netušíte, čím vám může proxy aplikace prospět, přečtěte si článek o tom, [jak poskytnout zabezpečený vzdálený přístup k místním aplikacím](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Požadavky na proxy aplikace
Předtím, než budete moct povolit a používat služby proxy aplikace, musíte mít:

* [Základní nebo prémiové předplatné služby Microsoft Azure AD](active-directory-editions.md) a adresář služby Azure AD, u kterého jste globální správce.
* Serveru se systémem Windows Server 2012 R2 nebo 2016, na který nainstalujete konektor Proxy aplikace. Server odesílá požadavky službám proxy aplikací v cloudu pomocí protokolu HTTPS a musí mít připojení protokolu HTTP nebo HTTPS k aplikacím, které publikujete.
  * Pokud chcete v publikovaných aplikacích používat jednotné přihlašování, měl by být tento počítač připojený ke stejné doméně služby AD jako aplikace, které publikujete. Informace najdete v tématu [jednotné přihlašování pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
* Pokud vaše organizace používá proxy servery pro připojení k Internetu, přečtěte si [pracují se stávající místní proxy servery](application-proxy-working-with-proxy-servers.md) podrobnosti o tom, jak je nakonfigurovat.

## <a name="open-your-ports"></a>Otevřít vaše porty

Při přípravě svého prostředí pro Azure AD Application Proxy, musíte nejprve povolit komunikaci datových center Azure. Pokud stojí v cestě brána firewall, zajistěte, aby byla otevřená a konektor mohl proxy aplikace posílat požadavky prostřednictvím protokolu HTTPS (TCP).

1. Otevřít následující porty pro **odchozí** provozu:

   | Číslo portu | Jak se používají |
   | --- | --- |
   | 80 | Stahování odvolání certifikátu seznamy (CRL) při ověřování certifikátu SSL |
   | 443 | Všechny odchozí komunikace se službou Proxy aplikace |

   Pokud brána firewall vynucuje přenos v závislosti na zdroji uživatelů, otevřete tyto porty pro přenos ze služeb systému Windows, které běží jako síťové služby.

   > [!IMPORTANT]
   > Změna portu požadavky pro konektor verze 1.5.132.0 a novějších. Pokud máte starší verzi konektoru, musíte také povolit následující porty: 5671, 8080, 9090, 9091, 9350, 9352 a 10100 – 10120.
   >
   >Informace o aktualizaci vašeho konektory na nejnovější verzi najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md#automatic-updates).

2. Pokud brána firewall nebo proxy server umožňuje vytvoření seznamu povolených DNS, můžete seznam povolených adres připojení k msappproxy.net a servicebus.windows.net. Pokud ne, musíte povolit přístup k [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653), které jsou aktualizovány každý týden.

3. Použití [Azure AD Application proxy serveru konektoru porty nástroj pro testování](https://aadap-portcheck.connectorporttest.msappproxy.net/) k ověření, že vaše konektoru může kontaktovat službu Proxy aplikace. Minimálně Ujistěte se, že oblasti střed USA a oblasti nejblíže k vám mají všechny zelené značky zaškrtnutí. Kromě toho další zelené značky zaškrtnutí znamená větší odolnost proti chybám.

## <a name="enable-application-proxy-in-azure-ad"></a>Povolení Proxy aplikace ve službě Azure AD
1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com/).
2. Přejděte do služby Active Directory a vyberte adresář, u kterého chcete povolit proxy aplikace.

    ![Active Directory – ikona](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Na stránce adresáře vyberte **Konfigurovat** a přejděte dolů k **Proxy aplikace**.
4. Přepněte možnost **Povolit pro tento adresář služby proxy aplikace** na **Povoleno**.

    ![Povolení proxy aplikace](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Vyberte **Stáhnout**. **Azure AD stažení konektoru Proxy aplikace** otevře. Přečtěte si a přijměte licenční podmínky, klikněte na **Stáhnout** a uložte soubor Instalační služby Windows (.exe) konektoru.

## <a name="install-and-register-the-connector"></a>Instalace a registrace konektoru
1. Spusťte aplikaci **AADApplicationProxyConnectorInstaller.exe** na serveru, který jste připravili podle uvedených požadavků.
2. Nainstalujte podle pokynů instalačního průvodce.
3. Během instalace zobrazí se výzva k registraci konektoru k Proxy aplikace klienta služby Azure AD.

   * Zadejte vaše přihlašovací údaje globálního správce pro službu Azure AD. Klient globálního správce se může lišit od vašich přihlašovacích údajů ke službě Microsoft Azure.
   * Ujistěte se, že správce, který zaregistruje konektor, je ve stejném adresáři, ve kterým jste povolili službu proxy aplikací. Pokud je třeba doména tenanta contoso.com, správce by měl být admin@contoso.com nebo jakýkoli jiný alias v této doméně.
   * Pokud **konfigurace rozšířeného zabezpečení aplikace Internet Explorer** je nastaven na **na** na serveru, může být blokovány na obrazovce pro registraci. Chcete-li povolit přístup, postupujte podle pokynů v chybové zprávě. Ujistěte se, že je rozšířené zabezpečení aplikace Internet Explorer vypnuto.
   * Pokud registrace konektoru selže, podívejte se do článku [Poradce při potížích s proxy aplikace](active-directory-application-proxy-troubleshoot.md).  
4. Po dokončení instalace se k serveru přidají dvě nové služby:

   * **Microsoft AAD Application Proxy Connector** umožňuje propojení

     * **Microsoft AAD Application Proxy Connector Updater** je služba pro automatickou aktualizaci. Pravidelně kontroluje nové verze konektoru a aktualizuje konektoru podle potřeby.

     ![Služby konektoru proxy aplikace – snímek obrazovky](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. V okně instalace klikněte na **Dokončit**.

Informace o konektorech najdete v [popisu konektorů proxy aplikace služby Azure AD](application-proxy-understand-connectors.md).

V zájmu vyšší dostupnosti byste měli nasadit aspoň dva konektory. Chcete-li nasadit více konektorů, opakujte kroky 2 a 3. Každý konektor musí být zaregistrovaný samostatně.

Pokud chcete konektor odinstalovat, odinstalujte službu Connector i službu Updater. Restartujte počítač, aby se službě úplně odebrala.

## <a name="next-steps"></a>Další kroky
Nyní jste připraveni k [publikování aplikací pomocí proxy aplikace](active-directory-application-proxy-publish.md).

Pokud máte aplikace, které jsou v různých sítích nebo na různých místech, můžete pomocí skupin konektorů uspořádat různé konektory do logických jednotek. Další informace získáte v článku [Práce s konektory proxy aplikací](active-directory-application-proxy-connectors.md).
