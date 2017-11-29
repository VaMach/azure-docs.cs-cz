---
title: "Přehled zabezpečení v Data Lake Store | Microsoft Docs"
description: "Pochopit, jak Azure Data Lake Store je bezpečnější úložiště velkých objemů dat"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 52e176711f512e8a3788309a58011c8484821a1e
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="security-in-azure-data-lake-store"></a>Zabezpečení v Azure Data Lake Store
Mnoho podniků jsou využívat výhod analýzy velkých objemů dat podnikových statistik pomáhá jim inteligentní rozhodnutí. Organizace může mít složitý a regulovaná prostředí s roste počet různých uživatelů. Je důležité pro organizace a ujistěte se, bezpečněji, uložení kritickými podnikovými daty s správné úrovně udělení přístupu k jednotlivým uživatelům. Azure Data Lake Store je navržená tak, abyste splňovat tyto požadavky na zabezpečení. V tomto článku se dozvíte o funkcích zabezpečení Data Lake Store, včetně:

* Authentication
* Autorizace
* Izolace sítě
* Ochrana dat
* Auditování

## <a name="authentication-and-identity-management"></a>Ověřování a identita správy
Ověřování je proces, podle kterého je ověřit identitu uživatele, když uživatel pracuje s Data Lake Store nebo s jakoukoli službu, která se připojuje k Data Lake Store. Pro správu identit a ověření Data Lake Store využívá [Azure Active Directory](../active-directory/active-directory-whatis.md), komplexní přístupu a identit a správy cloudové řešení, které zjednodušuje správu uživatelů a skupin.

Každé předplatné Azure může být přidružen k instanci služby Azure Active Directory. Pouze uživatele a identity služby, které jsou definované ve službě Azure Active Directory přístup účtu Data Lake Store pomocí portálu Azure, nástroje příkazového řádku, nebo prostřednictvím klientské aplikace sestavení vaší organizace pomocí Azure Data Lake Sada SDK úložiště. Klíčové výhody použití služby Azure Active Directory jako mechanismus řízení centralizované přístupu jsou:

* Zjednodušená správa životního cyklu identity. Identity uživatele nebo služby (hlavní identity služby) můžete rychle vytvořit a rychle odvolán jednoduše odstranit nebo zakázat účet v adresáři.
* Služba Multi-Factor authentication. [Služba Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication.md) poskytuje další úroveň zabezpečení pro uživatelská přihlášení a transakce.
* Ověřování z libovolného klienta přes standardní otevřete protokol, jako je například účtu OAuth nebo OpenID.
* Federaci se enterprise adresářových služeb a zprostředkovatelů identity cloudu.

## <a name="authorization-and-access-control"></a>Autorizace a řízení přístupu
Po Azure Active Directory ověřuje uživatele tak, že má uživatel přístup Azure Data Lake Store, ovládací prvky autorizace přístupová oprávnění pro Data Lake Store. Data Lake Store odděluje autorizace aktivity souvisejícím s účtem a data související s následujícím způsobem:

* [Řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) (RBAC) poskytovaný platformou Azure pro správu účtu
* POSIX seznamu ACL pro přístup k datům v úložišti

### <a name="rbac-for-account-management"></a>RBAC pro správu účtu
Čtyři základní role jsou definovány pro Data Lake Store ve výchozím nastavení. Role umožňují různé operace v účtu Data Lake Store prostřednictvím portálu Azure, rutiny prostředí PowerShell a rozhraní REST API. Role vlastník a Přispěvatel provádět celou řadu funkcí správy na účet. Můžete přiřadit role Čtenář pro uživatele, kteří pouze interagovat s daty.

![Role RBAC](./media/data-lake-store-security-overview/rbac-roles.png "role RBAC")

Všimněte si, že i když role přiřazené pro správu účtu, některé role ovlivnit přístup k datům. Budete muset použít seznamy řízení přístupu k řízení přístupu k operace, které může uživatel provádět v systému souborů. Následující tabulka obsahuje souhrn práva pro správu a data přístupová práva pro výchozí role.

| Role | Práva pro správu | Data přístupová práva | Vysvětlení |
| --- | --- | --- | --- |
| Žádné přiřazenou roli |Žádný |Řídí seznamu ACL |Uživatel nemůže použít portál Azure nebo rutin prostředí Azure PowerShell procházet Data Lake Store. Uživatel může použít jenom nástroje příkazového řádku. |
| Vlastník |Všechny |Všechny |Role vlastníka je superuživatele. Tato role můžou spravovat všechno a má úplný přístup k datům. |
| Čtenář |jen pro čtení |Řídí seznamu ACL |Role čtenáře můžou zobrazit všechno týkající se správy účtů, například která je přiřazena uživatele které role. Role čtenáře nelze provést žádné změny. |
| Přispěvatel |Všechny kromě přidání a odebrání rolí |Řídí seznamu ACL |Role Přispěvatel můžete spravovat některé aspekty účtu, jako je například nasazení a vytváření a Správa výstrah. Role Přispěvatel nelze přidat nebo odebrat role. |
| Správce přístupu uživatelů |Přidání a odebrání rolí |Řídí seznamu ACL |Role správce přístupu uživatelů můžete spravovat přístup uživatelů k účtům. |

Pokyny najdete v tématu [přiřadit uživatele nebo skupiny zabezpečení účtů Data Lake Store](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Pomocí seznamů řízení přístupu pro operace v systémech souborů.
Data Lake Store je systém souborů hierarchické jako Hadoop Distributed File System (HDFS) a podporuje [seznamy ACL POSIX](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Ovládá pro čtení (r), zápis (w) a spouštět (oprávnění k prostředkům pro roli vlastníka pro vlastníky skupiny a pro ostatní uživatele a skupiny x). V Data Lake Store Public Preview (aktuální verze) jde seznamy řízení přístupu zapnout u kořenové složky, podsložek a jednotlivých souborů. Další informace o fungování seznamů řízení přístupu v souvislosti s Data Lake Storem najdete v tématu [Řízení přístupu v Data Lake Storu](data-lake-store-access-control.md).

Doporučujeme, abyste definovat seznamy ACL pro více uživatelů pomocí [skupiny zabezpečení](../active-directory/active-directory-groups-create-azure-portal.md). Přidat uživatele do skupiny zabezpečení a pak mu přiřaďte seznamy ACL pro soubor nebo složku do této skupiny zabezpečení. To je užitečné, pokud chcete zadat vlastní přístup, protože jste omezeni na přidání maximálně devět položek pro vlastní přístup. Další informace o tom, jak lépe zabezpečit data uložená v Data Lake Store pomocí skupin zabezpečení služby Azure Active Directory najdete v tématu [přiřadit uživatele nebo skupiny zabezpečení jako seznamy řízení přístupu k systému souborů Azure Data Lake Store](data-lake-store-secure-data.md#filepermissions).

![Standardní a vlastní přístup](./media/data-lake-store-security-overview/adl.acl.2.png "standardní a vlastní přístup")

## <a name="network-isolation"></a>Izolace sítě
Použití Data Lake Store můžete lépe řízení přístupu k úložišti dat na úrovni sítě. Můžete určit brány firewall a definovat rozsah IP adres pro klienty důvěryhodné. Díky rozsah IP adres můžete připojit pouze klienti, kteří mají IP adresu v definovaném rozsahu do Data Lake Store.

![Nastavení brány firewall a IP přístup](./media/data-lake-store-security-overview/firewall-ip-access.png "nastavení a IP adresu brány Firewall")

## <a name="data-protection"></a>Ochrana dat
Azure Data Lake Store chrání vaše data v průběhu jejich životního cyklu. Pro data během přenosu Data Lake Store využívá standardní protokol zabezpečení TLS (Transport Layer) k zabezpečení dat přes síť.

![Šifrování v Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "šifrování v Data Lake Store")

Data Lake Store také zajišťuje šifrování dat, která jsou uložená v účtu. Můžete zvolit, aby se vaše data šifrovala, nebo zvolit možnost bez šifrování. Pokud můžete vyjádřit výslovný souhlas pro šifrování, je před ukládání na trvalé média šifrovat data uložená v Data Lake Store. V takovém případě Data Lake Store automaticky šifruje data před uložením a dešifruje data před načtení, takže je pro klienta přístupu k datům zcela transparentní. Neexistuje žádná změna kódu vyžaduje k šifrování a dešifrování dat na straně klienta.

Pro správu klíčů Data Lake Store poskytuje dva režimy pro správu hlavní šifrovacích klíčů (MEKs), které jsou požadovány pro dešifrování žádná data, která je uložená v Data Lake Store. Můžete je nechat buď Data Lake Store můžete spravovat MEKs nebo zachovejte vlastnictví MEKs pomocí účtu Azure Key Vault. Zadejte režim správy klíčů při při vytváření účtu Data Lake Store. Další informace o tom, jak provést konfiguraci související se šifrováním, najdete v tématu [Začínáme s Azure Data Lake Storem pomocí webu Azure Portal](data-lake-store-get-started-portal.md).

## <a name="auditing-and-diagnostic-logs"></a>Protokoly auditování a diagnostiky
Můžete použít protokoly auditování a diagnostiky, v závislosti na tom, jestli hledáte protokoly pro týkajících se správy aktivit nebo aktivit souvisejících s daty.

* Aktivity související s správy pomocí rozhraní API Správce Azure Resource Manager a jsou prezentované na portálu Azure přes protokoly auditu.
* Aktivity související s data pomocí rozhraní REST API WebHDFS a jsou prezentované na portálu Azure prostřednictvím diagnostické protokoly.

### <a name="auditing-logs"></a>Protokoly auditování
Abyste dosáhli souladu s předpisy, organizace může vyžadovat záznamy odpovídající auditu, pokud potřebuje a dostanete se do konkrétní incidenty. Data Lake Store má integrované sledování a auditování a protokoluje všechny aktivity správy účtu.

Účet správy pro záznamy pro audit zobrazit a vybrat sloupce, které chcete protokolovat. Protokoly auditu a také můžete exportovat do služby Azure Storage.

![Protokoly auditu](./media/data-lake-store-security-overview/audit-logs.png "Protokoly auditu")

### <a name="diagnostic-logs"></a>Diagnostické protokoly
Můžete nastavit záznamy auditu přístupu k datům na portálu Azure (v nastavení pro diagnostiku) a vytvoření účtu úložiště objektů Blob v Azure, kde jsou uloženy protokoly.

![Diagnostické protokoly](./media/data-lake-store-security-overview/diagnostic-logs.png "diagnostické protokoly")

Po dokončení konfigurace nastavení diagnostiky, můžete zobrazit protokoly na **diagnostické protokoly** kartě.

Další informace o práci s diagnostické protokoly s Azure Data Lake Store najdete v tématu [přístupu k diagnostickým protokolům pro Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Souhrn
Podnikoví zákazníci potřebují cloudové platformy analýzy dat, která jsou v bezpečí a snadno použitelný. Azure Data Lake Store je určena k usnadnění adresu, kterou tyto požadavky přes správu identit a ověření pomocí integrace služby Azure Active Directory, ověření na základě seznamu ACL, izolace sítě, šifrování dat při transitu i v rest (k dispozici v budoucnosti ) a auditování.

Pokud chcete zobrazit nové funkce v Data Lake Store, pošlete nám svůj názor [Data Lake Store UserVoice fórum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Začínáme s Data Lake Store](data-lake-store-get-started-portal.md)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)

