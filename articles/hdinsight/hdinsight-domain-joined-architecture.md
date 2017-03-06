---
title: "Architektura služby Azure HDInsight připojené k doméně | Dokumentace Microsoftu"
description: "Naučte se plánovat službu HDInsight připojenou k doméně."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: 4ba44128ec19d3937643ac934ca3e787cb9819a3
ms.openlocfilehash: 690ba97d2b0634548a0ec424d441ad34d70667b9
ms.lasthandoff: 02/27/2017


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Plánování clusterů Azure Hadoop připojených k doméně ve službě HDInsight

Tradiční Hadoop je jednouživatelský cluster. Je vhodný pro většinu společností, které mají menší aplikační týmy sestavující úlohy velkých objemů dat. Jak Hadoop získává na oblíbenosti, mnoho podniků přechází k modelu, kdy clustery spravují IT týmy a sdílí je několik aplikačních týmů. Proto jsou funkce zahrnující víceuživatelské clustery mezi nejžádanějšími funkcemi ve službě Azure HDInsight.

Místo vytváření vlastního víceuživatelského ověřování a autorizace služba HDInsight spoléhá na nejoblíbenějšího zprostředkovatele identity – Azure Active Directory (Azure AD). Efektivní funkci zabezpečení v Azure AD lze použít ke správě víceuživatelské autorizace v HDInsight. Díky integraci služby HDInsight s Azure AD můžete komunikovat s clustery pomocí vašich přihlašovacích údajů služby Azure AD. HDInsight mapuje uživatele služby Azure AD na místního uživatele Hadoopu, takže všechny služby spuštěné v HDInsight (Ambari, server Hive, Ranger, server Spark Thrift a další) pro ověřeného uživatele bezproblémově fungují.

## <a name="integrate-hdinsight-with-azure-ad"></a>Integrace služby HDInsight s Azure AD

Díky integraci služby HDInsight s Azure AD jsou uzly clusteru HDInsight připojené k doméně služby Azure AD. HDInsight vytváří pro služby Hadoop spuštěné v clusteru instanční objekty a umisťuje je v rámci zadané organizační jednotky (OU) ve službě Azure AD. HDInsight také vytváří reverzní mapování DNS v doméně služby Azure AD pro IP adresy uzlů, které jsou připojené k doméně.

Tohoto uspořádání můžete docílit pomocí několika architektur. Můžete si vybrat z následujících architektur.

**Služba HDInsight integrovaná s Azure AD běžící na Azure IAAS**

Toto je nejjednodušší architektura pro integraci služby HDInsight s Azure AD. Řadič domény služby Azure AD je spuštěný na jednom nebo několika virtuálních počítačích (VM) v Azure. Tyto virtuální počítače se obvykle nacházejí v rámci virtuální sítě. Vytvoříte další virtuální síť pro cluster HDInsight. Aby měla služba HDInsight službu Azure AD na dohled, budete muset pro tyto virtuální sítě vytvořit [partnerský vztah dvou virtuálních sítí](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md).

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> V této architektuře nemůžete použít Azure Data Lake Store s clusterem HDInsight.


Požadavky pro Azure AD:

* Musí být vytvořená [organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které umístíte virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* Musí být nastavené [protokoly LDAP](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (Lightweight Directory Access Protocol) pro komunikaci s Azure AD. Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory

**Služba HDInsight integrovaná s výhradně cloudovou službou Azure AD**

Pro výhradně cloudovou službu Azure AD nakonfigurujte řadič domény tak, aby HDInsight bylo možné integrovat s Azure AD. Toho můžete dosáhnout použitím služby [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Azure AD DS vytvoří počítače řadiče domény v cloudu a poskytne vám jejich IP adresy. Kvůli vyšší dostupnosti vytvoří dva řadiče domény.

Azure AD DS v současné době existuje pouze v klasických virtuálních sítích. Je dostupná jenom pomocí portálu Azure Classic. Virtuální síť HDInsight existuje na webu Azure Portal, takže bude potřeba vytvořit partnerský vztah dvou virtuálních sítí s klasickou virtuální sítí.

> [!NOTE]
> Partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Azure Resource Manageru vyžaduje, aby obě virtuální sítě byly ve stejné oblasti a obě spadaly pod stejné předplatné Azure.

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Požadavky pro Azure AD:

* Musí být vytvořená [organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které umístíte virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* Při konfiguraci služby Azure AD DS musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Musí se synchronizovat [hodnoty hash hesel](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) ze služby Azure AD do Azure AD DS.
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Azure AD

**Služba HDInsight integrovaná s místní službou Active Directory přes síť VPN**

Tato architektura se podobá službě HDInsight integrované s Azure AD běžící na Azure IAAS. Jediným rozdílem je, že služba Azure AD je místní a dohlednost služby Azure AD pro HDInsight se provádí prostřednictvím [připojení sítě VPN z Azure do místní sítě](../expressroute/expressroute-introduction.md).

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_3.png)

> [!NOTE]
> V této architektuře nemůžete použít Azure Data Lake Store s clusterem HDInsight.

Požadavky pro Azure AD:

* Musí být vytvořená [organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které umístíte virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* Musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) pro komunikaci s Azure AD. Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Azure AD

**Služba HDInsight integrovaná s místní službou Active Directory synchronizovanou s Azure AD**

Tato architektura se podobá službě HDInsight integrované s výhradně cloudovou službou Azure AD. Jediným rozdílem je, že se místní služba Active Directory synchronizuje s Azure AD. Nakonfigurujte řadič domény v cloudu tak, aby HDInsight bylo možné integrovat s Azure AD. Toho můžete dosáhnout použitím služby [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Azure AD DS vytvoří počítače řadiče domény v cloudu a poskytne vám jejich IP adresy. Kvůli vyšší dostupnosti vytvoří dva řadiče domény.

Azure AD DS v současné době existuje pouze v klasických virtuálních sítích. Je dostupná jenom pomocí portálu Azure Classic. Virtuální síť HDInsight existuje na webu Azure Portal, takže bude potřeba vytvořit partnerský vztah dvou virtuálních sítí s klasickou virtuální sítí.

> [!NOTE]
> Partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Azure Resource Manageru vyžaduje, aby obě virtuální sítě byly ve stejné oblasti a obě spadaly pod stejné předplatné Azure.

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Požadavky pro Azure AD:

* Musí být vytvořená [organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které umístíte virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* Při konfiguraci služby Azure AD DS musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Musí se synchronizovat [hodnoty hash hesel](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) ze služby Azure AD do Azure AD DS.
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory

**Služba HDInsight integrovaná se službou Azure AD jinou než výchozí (doporučeno pouze pro vývoj a testování)**

Tato architektura se podobá službě HDInsight integrované s výhradně cloudovou službou Azure AD. Pro většinu společností je přístup správce ke službě Azure AD omezený jenom na určité jednotlivce. Proto pokud chcete udělat testování konceptu nebo vyzkoušet vytvoření clusteru připojeného k doméně, může být výhodné vytvořit instanci služby Azure AD v rámci předplatného, místo abyste čekali, až správce nakonfiguruje požadavky ve službě Azure AD. Vzhledem k tomu, že je se jedná o instanci služby Azure AD, kterou jste vytvořili vy, máte úplná oprávnění k této službě Azure AD pro konfiguraci služby Azure AD DS.

Azure AD DS vytvoří počítače řadiče domény v cloudu a poskytne vám jejich IP adresy. Kvůli vyšší dostupnosti vytvoří dva řadiče domény.

Azure AD DS existuje pouze v klasických virtuálních sítích, proto ke konfiguraci služby Azure AD DS potřebujete přístup k portálu Azure Classic a musíte vytvořit klasickou virtuální síť. Virtuální síť HDInsight existuje na webu Azure Portal, takže bude potřeba vytvořit partnerský vztah dvou virtuálních sítí s klasickou virtuální sítí.

> [!NOTE]
> Partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Azure Resource Manageru vyžaduje, aby obě virtuální sítě byly ve stejné oblasti a obě spadaly pod stejné předplatné Azure.

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Požadavky pro Azure AD:

* Musí být vytvořená [organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které umístíte virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* Při konfiguraci služby Azure AD DS musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Ke konfiguraci protokolu LDAPS můžete vytvořit [certifikát podepsaný svým držitelem](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Abyste ale mohli použít certifikát podepsaný svým držitelem, musíte požádat o výjimku na adrese <a href="mailto:hdipreview@microsoft.com">hdipreview@microsoft.com</a>.
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Musí se synchronizovat [hodnoty hash hesel](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) ze služby Azure AD do Azure AD DS.
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Azure Active Directory

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).
* Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md).
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Pokud chcete spouštět dotazy Hivu pomocí SSH na clusterech HDInsight připojených k doméně, přečtěte si téma [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

