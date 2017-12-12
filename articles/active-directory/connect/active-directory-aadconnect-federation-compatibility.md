---
title: Seznam kompatibilit pro federaci Azure AD
description: "Tato stránka obsahuje poskytovatelů identit třetích stran, které lze použít k implementaci jednotného přihlašování."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: billmath
ms.openlocfilehash: 86c247b6a9d197e89040109ac6b8686e3310dbf5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-federation-compatibility-list"></a>Seznam kompatibilit pro federaci Azure AD
Azure Active Directory poskytuje jednotné přihlašování a rozšířené zabezpečení přístupu aplikace pro Office 365 a dalším službám Microsoft Online pro implementace jenom pro cloud a hybridní bez nutnosti jakéhokoli řešení od jiných výrobců. Office 365, jako je většina Online službách společnosti Microsoft, je integrovaná do Azure Active Directory directory services, ověřování a autorizace. Jednotné přihlašování k tisícům aplikací SaaS také poskytuje Azure Active Directory a místní webové aplikace. Najdete v galerii aplikací Azure Active Directory pro podporovaných aplikací SaaS.

Pro organizace, které investovaly do jiných společností než Microsoft federačních řešení Toto téma obsahuje pokyny pro konfiguraci jednotné přihlašování pro své uživatele systému Windows Server Active Directory se služeb Microsoft Online services pomocí poskytovatelů identit třetích stran ze "Azure Active Directory federation kompatibility seznamu". 

![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Skupina počítačů Oxford](http://oxfordcomputergroup.com/), třetích stran, jménem společnosti Microsoft, testovat tyto jeden přihlašování pomocí poskytovatelů identit třetích stran oproti sadě běžné případy použití služby Azure Active Directory.

Informace o tom, jak můžete získat tady zprostředkovatele identity jiných výrobců, obraťte se na skupinu počítačů Oxford v [ idp@oxfordcomputergroup.com ](mailto:idp@oxfordcomputergroup.com).

> [!IMPORTANT]
> Skupina počítačů Oxford otestovat pouze funkci federační z těchto scénářů jednom přihlášení. Skupina počítačů Oxford nebyla provedena žádné testování synchronizace, dvoufaktorové ověřování, atd. součástí těchto scénářů jednom přihlášení.
> 
> Použití přihlášení k UPN alternativní ID není otestována také tohoto programu.
> 
> 

* [Azure Active Directory](#azure-active-directory)
* [AuthAnvil jednotného přihlašování 4.5](#authanvil-single-sign-on-45)
* [BIG-IP s verze správce zásad přístupu BIG-IP 11.3 x – 11, 6 x](#big-ip-with-access-policy-manager-big-ip-ver-113x--116x) 
* [BitGlass](#bitglass)
* [Zabezpečené cloudové certifikační Autority](#ca-secure-cloud) 
* [Certifikační Autority SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
* [Centrify](#centrify) 
* [V7.1 Dell jeden správce přístup cloudových identit](#dell-one-identity-cloud-access-manager-v71) 
* [DigitalPersona složené ověřování](#digitalpersona-composite-authentication)
* [Verze 5.x správu ForgeRock Identity platformy přístup](#forgerock-identity-platform-access-management-v5x)
* [Federované Identity Manager 6.2.2 IBM Tivoli](#ibm-tivoli-federated-identity-manager-622) 
* [IceWall Federation verze 3.0](#icewall-federation-version-30) 
* [Memority](#memority)
* [Správce přístupu k NetIQ 4.x](#netiq-access-manager-4x) 
* [Okta](#okta) 
* [OneLogin](#onelogin) 
* [Optimální IDM Identity virtuální Server Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
* [PingFederate 6.11, 7.2, 8.x](#pingfederate-611-72-8x)
* [RadiantOne CFS 3.0](#radiantone-cfs-30) 
* [Sailpoint IdentityNow](#sailpoint-identitynow)
* [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
* [Podepsat & přejděte 5.3](#signgo-53) 
* [Technologie SoftBank Online služby brány](#softbank)
* [Pracovní prostor VMware jeden](#vmware-workspace-one)



> [!IMPORTANT]
> Vzhledem k tomu, že jsou tyto produkty třetích stran, společnost Microsoft neposkytuje podporu pro nasazení, konfiguraci, Poradce při potížích, osvědčené postupy, atd. problémy a otázky týkající se těchto poskytovatelů identit. Pro podporu a dotazy týkající se těchto poskytovatelů identit obraťte se na podporovaných jiných výrobců přímo.
> 
> Tyto poskytovatelů identit třetích stran byly testovaný na interoperabilitu s cloudovým službám Microsoftu pomocí protokolu WS-Federation a pouze protokoly WS-Trust. Testování nezahrnuli pomocí protokolu SAML.
> 


## <a name="azure-active-directory"></a>Azure Active Directory

Toto je matici podpory scénář pro toto prostředí přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |
| Moderní aplikace používá ADAL, jako je například Office 2016 |Podporuje se |Žádný |

Další informace o používání služby Azure Active Directory se službou AD FS najdete v části [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

Další informace o používání služby Azure Active Directory se synchronizací hesla najdete v části [Azure AD Connect](active-directory-aadconnect.md).

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil jednotného přihlašování 4.5

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace najdete v tématu [AuthAnvil jednotné přihlašování.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-).


## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP s verze správce zásad přístupu BIG-IP 11.3 x – 11, 6 x

Toto je matici podpory scénář pro toto prostředí jeden přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Nepodporuje se |Nepodporuje se |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o modulu snap-in Správce zásad přístupu BIG-IP najdete v tématu [správce zásad přístupu BIG-IP.](https://f5.com/products/modules/access-policy-manager) 

Správce zásad přístupu BIG-IP pokyny o tom, jak nakonfigurovat tuto službu tokenů zabezpečení zajistit jeden přihlašování pro vaše uživatele služby Active Directory, stáhnete pdf [BIG-IP](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf).

## <a name="bitglass"></a>BitGlass

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o BitGlass najdete v části [BitGlass](http://www.bitglass.com).

## <a name="ca-secure-cloud"></a>Zabezpečené cloudové certifikační Autority

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o cloudu zabezpečení certifikační Autority najdete v tématu [cloudu zabezpečení certifikační Autority](http://www.ca.com/us/products/security-as-a-service.aspx).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>Certifikační Autority SiteMinder 12.52 SP1 kumulativní verze 4

Toto je matici podpory scénář pro toto prostředí jeden přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o SiteMinder certifikační Autority najdete v tématu [certifikační Autority SiteMinder Federation](http://www.ca.com/us/products/ca-single-sign-on.html). 

## <a name="centrify"></a>Centrify

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Řízení přístupu klienta není podporována. |

Další informace o Centrify najdete v tématu [Centrify](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp).

## <a name="dell-one-identity-cloud-access-manager-v71"></a>V7.1 Dell jeden správce přístup cloudových identit

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o Dell jeden Cloud přístup správce identit najdete v tématu [správce přístupu k cloudu jeden Identity Dell](http://software.dell.com/products/cloud-access-manager).

 Pokyny ke konfiguraci této služby tokenů zabezpečení pro Office 365 uživatelům poskytnout jeden přihlašování najdete v tématu [konfigurace uživatelé služeb Office 365](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365). 

## <a name="digitalpersona-composite-authentication"></a>DigitalPersona složené ověřování  

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows není podporována.|
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows není podporována.|
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace najdete v části [DigitalPersona složené ověřování](http://www.crossmatch.com/uploadedFiles/Support/Reference_Material/DigitalPersona-Office-365-Deployment-Guide.pdf).

## <a name="forgerock-identity-platform-access-management-v5x"></a>Verze 5.x správu ForgeRock Identity platformy přístup

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný|
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný|
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace najdete v části [ForgeRock identitu platformy přístup správu V5.x](https://backstage.forgerock.com/knowledge/kb/article/a98278517).

## <a name="ibm-tivoli-federated-identity-manager-622"></a>Federované Identity Manager 6.2.2 IBM Tivoli

Toto je matici podpory scénář pro toto prostředí jeden přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o IBM Tivoli federované Identity Manager najdete v tématu [IBM správce přístupu k zabezpečení pro Microsoft Applications](http://www-01.ibm.com/support/docview.wss?uid=swg24029517).

## <a name="icewall-federation-version-30"></a>IceWall Federation verze 3.0

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o IceWall Federation najdete v tématu [IceWall Federation verze 3.0](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) a [IceWall Federation s Office 365](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html).

## <a name="memority"></a>Memority

Toto je matici podpory scénář pro toto prostředí přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o používání Memority najdete v části [Memority](http://www.memority.com).


## <a name="netiq-access-manager-4x"></a>Správce přístupu k NetIQ 4.x

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný|
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný|
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace najdete v tématu [správce přístupu k NetIQ](https://www.netiq.com/documentation/access-manager-43/admin/data/b65ogn0.html#b12iqp0m).

## <a name="okta"></a>Okta

Toto je matici podpory scénář pro toto prostředí jeden přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows vyžaduje instalaci dalších webový server a Okta aplikace. |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o Okta najdete v tématu [Okta](https://www.okta.com/).

## <a name="onelogin"></a>OneLogin

Toto je matici podpory scénář pro toto prostředí jeden přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o OneLogin najdete v tématu [OneLogin](https://www.onelogin.com/).

## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimální IDM Identity virtuální Server Federation Services

Následuje tento scénář podporu této jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |

Pro další informace o přístupu klienta zásady najdete v tématu [omezení přístupu k Office 365 služeb na základě umístění klienta](https://technet.microsoft.com/library/hh526961.aspx).





## <a name="pingfederate-611-72-8x"></a>PingFederate 6.11, 7.2, 8.x

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

PingFederate pokynů ke konfiguraci této služby tokenů zabezpečení služby Active Directory uživatelům poskytnout jeden přihlašování najdete v jednom z následujících akcí: 

- [PingFederate 6.11](http://go.microsoft.com/fwlink/?LinkID=266321)
- [PingFederate 7.2](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)
- [PingFederate 8.x](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0

Toto je matici podpory scénář pro toto prostředí jeden přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o RadiantOne CFS najdete v tématu [RadiantOne CFS](http://www.radiantlogic.com/products/radiantone-cfs/).

## <a name="sailpoint-identitynow"></a>Sailpoint IdentityNow

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace najdete v tématu [Sailpoint IdentityNow](https://www.sailpoint.com/idaas-identity-as-a-service-identitynow/).

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0

Toto je matici podpory scénář pro toto prostředí jeden přihlašování: 

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Žádný |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o SecureAuth najdete v tématu [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).














## <a name="signgo-53"></a>Podepsat & přejděte 5.3

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Podporované kontrakty protokolu Kerberos |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Žádný |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Přihlašovací & přejděte 5.3 podporuje ověřování protokolem Kerberos prostřednictvím konfigurace kontraktu protokolu Kerberos.  Pomoc s touto konfigurací, obraťte se na Ilex nebo zobrazit v Průvodci instalací [přihlašovací & Přejít](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)

## <a name="softbank-technology-online-service-gate"></a>Technologie SoftBank Online služby brány

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace o SoftBank technologie Online služby brány najdete v části [Softbank](https://www.softbanktech.jp/service/list/osg-pro-ent/)

## <a name="vmware-workspace-one"></a>Pracovní prostor VMware jeden

Toto je matici podpory scénář pro toto prostředí jeden přihlašování:

| Klient | Podpora | Výjimky |
| --- | --- | --- |
| Webových klientů, jako například webový přístup k systému Exchange a SharePoint Online |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Aplikace Rich client například Lync, předplatného systému Office, CRM |Podporuje se |Integrované ověřování systému Windows není podporována. |
| Bohaté e-mailových klientů, jako je Outlook a ActiveSync |Podporuje se |Žádný |

Další informace najdete v tématu [jeden prostoru VMware](http://www.vmware.com/pdf/vidm-office365-saml.pdf)

