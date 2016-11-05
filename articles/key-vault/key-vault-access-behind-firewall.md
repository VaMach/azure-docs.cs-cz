---
title: Přístup ke službě Key Vault za bránou firewall | Microsoft Docs
description: Zjistěte, jak přistupovat ke službě Key Vault z aplikace za bránou firewall
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager

ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/13/2016
ms.author: ambapat

---
# Přístup ke službě Key Vault za bránou firewall
### Otázka: Má klientská aplikace trezoru klíčů musí být za bránou firewall. Jaké porty, hostitele nebo IP adresy bych měl otevřít, jestliže chci umožnit přístup k trezoru klíčů?
Chcete-li přistupovat k trezoru klíčů, musí být vaše klientská aplikace trezoru klíčů schopná přistupovat k více koncovým bodům kvůli různým funkcím.

* Ověření (prostřednictvím Azure Active Directory)
* Správa služby Key Vault (která zahrnuje operace vytvoření, čtení, aktualizace a odstranění a také nastavení zásad přístupu) prostřednictvím Azure Resource Manageru
* Přistupování k objektům (klíče a tajné klíče) uloženým v samotném trezoru klíčů a jejich správa probíhá přes koncový bod konkrétního trezoru klíčů (např. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

V závislosti na vaší konfiguraci a prostředí existuje několik variant.   

## Porty
Veškerý provoz do trezoru klíčů pro všechny tři funkce (ověření, správa a plochý přístup k datům) prochází přes protokol HTTPS na portu 443. Nicméně u seznamu CRL může občas docházet k provozu přes protokol HTTP (na portu 80). Klienti podporující protokol OCSP by se na seznam CRL dostat neměli, občas se ale mohou dostat na [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## Authentication
Klientská aplikace služby Key Vault bude k ověření potřebovat přístup ke koncovým bodům Azure Active Directory. Použitý koncový bod závisí na konfiguraci tenanta AAD, typu objektu zabezpečení (uživatel nebo instanční objekt) a na typu účtu, např. účet Microsoft nebo ID organizace.  

| Typ objektu zabezpečení | Koncový bod:port |
| --- | --- |
| Uživatel používající účet Microsoft<br> (např. uzivatel@hotmail.com) |**Globální:**<br> login.microsoftonline.com:443<br><br> **Azure Čína:**<br> login.chinacloudapi.cn:443<br><br>**Azure USA – vláda:**<br> login-us.microsoftonline.com:443<br><br>**Azure Německo:**<br> login.microsoftonline.de:443<br><br> a <br>login.live.com:443 |
| Uživatel nebo instanční objekt používající ID organizace s AAD (např. uzivatel@contoso.com) |**Globální:**<br> login.microsoftonline.com:443<br><br> **Azure Čína:**<br> login.chinacloudapi.cn:443<br><br>**Azure USA – vláda:**<br> login-us.microsoftonline.com:443<br><br>**Azure Německo:**<br> login.microsoftonline.de:443 |
| Uživatel nebo instanční objekt používající ID organizace a AD FS nebo jiný federovaný koncový bod (např. uzivatel@contoso.com) |Všechny výše uvedené koncové body pro ID organizace a AD FS nebo jiné federované koncové body |

Existují i další možné komplexní scénáře. Další informace najdete v tématech [Tok ověřování Azure Active Directory](/documentation/articles/active-directory-authentication-scenarios/), [Integrace aplikací s Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) a [Ověřovací protokoly Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx).  

## Správa služby Key Vault
Pro správu služby Key Vault (operace CRUD a nastavení zásad přístupu) je nutné, aby klientská aplikace trezoru klíčů měla přístup ke koncovému bodu Azure Resource Manageru.  

| Typ operace | Koncový bod:port |
| --- | --- |
| Ploché operace řízení služby Key Vault<br> prostřednictvím Azure Resource Manageru |**Globální:**<br> management.azure.com:443<br><br> **Azure Čína:**<br> management.chinacloudapi.cn:443<br><br> **Azure USA – vláda:**<br> management.usgovcloudapi.net:443<br><br> **Azure Německo:**<br> management.microsoftazure.de:443 |
| Azure Active Directory Graph API |**Globální:**<br> graph.windows.net:443<br><br> **Azure Čína:**<br> graph.chinacloudapi.cn:443<br><br> **Azure USA – vláda:**<br> graph.windows.net:443<br><br> **Azure Německo:**<br> graph.cloudapi.de:443 |

## Operace služby Key Vault
Pro všechny operace správy objektů trezoru klíčů (klíče a tajné klíče) a kryptografické operace je nutné, aby klient trezoru klíčů měl přístup ke koncovému bodu trezoru klíčů. V závislosti na umístění služby Key Vault se bude lišit přípona DNS koncového bodu. Koncový bod služby Key Vault je ve formátu <název_trezoru>.<přípona_dns_konkrétní_oblasti>, jak je popsáno v tabulce níže.  

| Typ operace | Koncový bod:port |
| --- | --- |
| Operace služby Key Vault, jako například kryptografické operace nad klíči, vytvoření, čtení, aktualizace nebo odstranění klíčů a tajných klíčů, nastavení nebo získání značek a dalších atributů objektů (klíče a tajné klíče) trezoru klíčů |**Globální:**<br> &lt;název_trezoru&gt;.vault.azure.net:443<br><br> **Azure Čína:**<br> &lt;název_trezoru&gt;.vault.azure.cn:443<br><br> **Azure USA – vláda:**<br> &lt;název_trezoru&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Německo:**<br> &lt;název_trezoru&gt;.vault.microsoftazure.de:443 |

## Rozsahy IP adres
Služba Key Vault střídavě využívá další prostředky Azure, například infrastrukturu modelu PaaS, proto není možné poskytnout konkrétní rozsah IP adres, které budu koncové body služby Key Vault mít v daném okamžiku. Nicméně pokud vaše brána firewall podporuje pouze rozsahy IP adres, další informace najdete v dokumentu [Rozsahy IP adres datového centra Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).   Pro ověření a získání identity (Azure Active Directory) se vaše aplikace musí být schopná připojit ke koncovým bodům popsaným v tématu [Adresy pro ověření a získání identity](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## Další kroky
* Máte-li dotazy ke službě Key Vault, navštivte [fóra Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

<!--HONumber=Sep16_HO3-->


