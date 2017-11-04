---
title: "Architektura Azure HDInsight připojený k doméně | Microsoft Docs"
description: "Naučte se plánovat službu HDInsight připojenou k doméně."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/21/2017
ms.author: saurinsh
ms.openlocfilehash: 9deb5e4dbd925c4fdc523d8d21afbcfbf85947b8
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Plánování clusterů Azure Hadoop připojených k doméně ve službě HDInsight

Tradiční Hadoop je jednouživatelský cluster. Je vhodný pro většinu společností, které mají menší aplikační týmy sestavující úlohy velkých objemů dat. Jak Hadoop získává na oblíbenosti, mnoho podniků přechází k modelu, kdy clustery spravují IT týmy a sdílí je několik aplikačních týmů. Proto jsou funkce zahrnující víceuživatelské clustery mezi nejžádanějšími funkcemi ve službě Azure HDInsight.

Místo vytváření vlastní víceuživatelská ověřování a autorizaci, HDInsight využívá nejoblíbenější zprostředkovatele identity – Active Directory (AD). Funkci efektivní zabezpečení ve službě Active Directory lze použít ke správě více uživatelů autorizace v HDInsight. Integrací HDInsight s AD může komunikovat s clustery pomocí svých přihlašovacích údajů AD. HDInsight mapuje Active Directory na místní uživatel Hadoop, takže všechny služby spuštěné v HDInsight (Ambari, Hive serveru škálu, Spark thrift serveru a dalších) pracovní bezproblémově pro ověřené uživatele.

## <a name="integrate-hdinsight-with-active-directory"></a>Integrace služby HDInsight s Active Directory

Pokud integrujete HDInsight se službou Active Directory, jsou uzly clusteru HDInsight domény připojený k doméně AD. HDInsight vytvoří objekty služby pro služby Hadoop běžící v clusteru a umístí se do zadané organizační jednotce (OU) v doméně. HDInsight vytvoří také zpětné mapování DNS v doméně pro IP adresy uzly, které jsou připojené k doméně.

Existují dvě možnosti nasazení pro službu Active Directory:
* **[Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md):** tato služba poskytuje spravované doméně služby Active Directory, která je plně kompatibilní s Windows Server Active Directory. Microsoft má na starosti správu, opravy a monitorování doméně AD. Cluster můžete nasadit bez starostí o správu řadičů domény. Uživatelé, skupiny a hesla se synchronizují ze služby Azure Active Directory, uživatelům se přihlásit ke clusteru pomocí své podnikové přihlašovací údaje.

* **Doménu služby Active Directory pro Windows Server na virtuálních počítačích Azure IaaS:** tuto možnost, můžete nasadit a spravovat vlastní domény Windows Server Active Directory na virtuálních počítačích Azure IaaS. 

Tohoto uspořádání můžete docílit pomocí několika architektur. Můžete si vybrat z následujících architektur.


### <a name="hdinsight-integrated-with-an-azure-ad-domain-services-managed-ad-domain"></a>HDInsight integrované s Azure AD Domain Services spravované domény služby AD
Můžete nasadit [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS) spravované domény. Azure AD DS poskytuje spravované doméně AD v Azure, což je spravovat, aktualizovat a monitorovat společností Microsoft. Vytvoří dva řadiče domény pro zajištění vysoké dostupnosti a zahrnuje služby DNS. Potom může HDInsight cluster integrovat této spravované domény. Tato možnost nasazení není potřeba starat o správu, opravy, aktualizace a monitorování řadičů domény.

![Topologie clusteru HDInsight připojeného k doméně](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Předpoklady pro integraci s Azure AD Domain Services:

* [Zřídit Azure AD Domain Services spravované domény](../../active-directory-domain-services/active-directory-ds-getting-started.md).
* Vytvoření [organizační jednotky](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve kterém umístit cluster HDInsight virtuální počítače a objekty služby používaný v clusteru.
* Instalační program [LDAPS](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md), při konfiguraci služby Azure AD DS. Certifikát použitý k nastavení LDAPS musí být vydaný z veřejné certifikační autority (není certifikát podepsaný svým držitelem).
* Vytvořte zpětné zóny DNS na spravované doméně pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Konfigurace [synchronizace hodnot hash hesel, které jsou požadované pro ověřování NTLM a Kerberos](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) z Azure AD k spravované doméně služby Azure AD DS.
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Azure AD


### <a name="hdinsight-integrated-with-windows-server-ad-running-on-azure-iaas"></a>Integrované HDInsight se systémem Windows Server AD systémem Azure IaaS

Můžete nasadit roli systému Windows Server Active Directory Domain Services na jednu (nebo víc) virtuálních počítačů (VM) v Azure a mají být řadiče domény povýšit. Tyto virtuální počítače řadiče domény se dá nasadit pomocí modelu nasazení resource manager do stejné virtuální síti jako HDInsight cluster. Pokud řadiče domény jsou nasazeny do jinou virtuální síť, budete muset peer tyto virtuální sítě pomocí [VNet-to-VNet peering](../../virtual-network/virtual-network-create-peering.md). 

[Další informace - nasazení systému Windows Server Active Directory na virtuálních počítačích Azure](../../active-directory/virtual-networks-windows-server-active-directory-virtual-machines.md)

![Topologie clusteru HDInsight připojeného k doméně](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> V této architektuře nemůžete použít Azure Data Lake Store s clusterem HDInsight.


Předpoklady pro integraci s Windows Server Active Directory na virtuálních počítačích Azure:

* Musí být vytvořená [organizační jednotka](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které umístíte virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* [Protokoly přístup Lightweight Directory](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) musí být nastavení pro komunikaci se službou AD. Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory


## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](apache-domain-joined-configure.md).
* Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](apache-domain-joined-manage.md).
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](apache-domain-joined-run-hive.md).
* Ke spouštění dotazů Hive pomocí protokolu SSH na doméně clusterů HDInsight, najdete v části [použití SSH s HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
