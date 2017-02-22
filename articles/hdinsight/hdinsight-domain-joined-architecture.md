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
ms.sourcegitcommit: b5cd321f3cd4c415f5e97ca550b7ab1679324921
ms.openlocfilehash: 94ef8147f1f73b293e818fc508096789373df7fc


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Plánování clusterů Azure Hadoop připojených k doméně ve službě HDInsight

Tradiční Hadoop je jednouživatelský cluster. Vyhovuje většině společností, které mají menší aplikační týmy sestavující úlohy velkých objemů dat. Jak Hadoop získává na oblíbenosti, mnoho podniků přechází k modelu, kdy clustery spravují IT týmy a sdílí je několik aplikačních týmů. Proto jsou víceuživatelské clustery jednou z funkcí, které jsou ve službě HDInsight nejžádanější.

Místo vytváření vlastního víceuživatelského ověřování a autorizace HDInsight spoléhá na nejoblíbenějšího zprostředkovatele identity – Active Directory (AD). Efektivní funkci skupin zabezpečení v Active Directory lze použít ke správě víceuživatelské autorizace v HDInsight. Díky integraci služby HDInsight s Active Directory můžou uživatelé služby Active Directory komunikovat s clustery pomocí svých přihlašovacích údajů služby Active Directory. HDInsight mapuje uživatele služby Active Directory na místního uživatele Hadoopu, takže všechny služby spuštěné v HDInsight (Ambari, server Hive, Ranger, server Spark Thrift a další) pro ověřeného uživatele bezproblémově fungují.

## <a name="integrate-hdinsight-with-active-directory"></a>Integrace služby HDInsight s Active Directory

Díky integraci služby HDInsight s Active Directory jsou uzly clusteru HDInsight připojené k doméně služby Active Directory. HDInsight vytváří pro služby Hadoop spuštěné v clusteru instanční objekty a umisťuje je v rámci zadané organizační jednotky (OU) ve službě Active Directory. HDInsight také vytváří reverzní mapování DNS v doméně služby Active Directory pro IP adresy uzlů, které jsou připojené k doméně.

Abyste dosáhli tohoto uspořádání, existuje několik architektur, podle kterých můžete postupovat. Musíte se rozhodnout, které architektura vám nejlépe vyhovuje.

**1. Služba HDInsight integrovaná s AD běžící na Azure IAAS**

Toto je nejjednodušší architektura pro integraci služby HDInsight s Active Directory. Řadič domény služby Active Directory je spuštěný na jednom nebo několika virtuálních počítačích (VM) v Azure. Obvykle se tyto virtuální počítače nacházejí v rámci virtuální sítě. Vytvoříte další virtuální síť pro cluster HDInsight. Aby měla služba HDInsight službu Active Directory na dohled, budete muset pro tyto virtuální sítě vytvořit [partnerský vztah dvou virtuálních sítí](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md).

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> V této architektuře nemůžete použít Azure Data Lake Store s clusterem HDInsight.
 

Předpoklady pro službu Active Directory:

* Musí být vytvořená [Organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které chcete umístit virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* Musí být nastaven protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) pro komunikaci se službou Active Directory. Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Je potřeba účet služby nebo účet uživatele, který se používá k vytvoření clusteru HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory

**2. Služba HDInsight integrovaná s výhradně cloudovou službou Azure AD**

Pro výhradně cloudovou službu Azure Active Directory (Azure AD) budete muset nakonfigurovat řadič domény tak, aby HDInsight bylo možné integrovat s Azure Active Directory. Toho můžete dosáhnout použitím služby [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Azure AD DS vytvoří počítače řadiče domény v cloudu a poskytne vám jejich IP adresy. Kvůli vyšší dostupnosti vytvoří dva řadiče domény.

V současné době Azure AD DS existuje pouze v klasických virtuálních sítích. Je dostupná jenom pomocí portálu Azure Classic. Virtuální síť HDInsight existuje na webu Azure Portal, takže bude potřeba vytvořit partnerský vztah dvou virtuálních sítí s klasickou virtuální sítí.

> [!NOTE]
> Partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Azure Resource Manageru vyžaduje, aby obě virtuální sítě byly ve stejné oblasti a obě spadaly pod stejné předplatné Azure.

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Předpoklady pro službu Active Directory:

* Musí být vytvořená [Organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které chcete umístit virtuální počítače clusteru HDInsight a instanční objekty používané clusterem. 
* Při konfiguraci služby AD DS musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDI (například 10.2.0.0/24 na předchozím obrázku). 
* Musí se synchronizovat [hodnoty hash hesel](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) ze služby Azure AD do AD DS.
* Je potřeba účet služby nebo účet uživatele, který se používá k vytvoření clusteru HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory

**3. Služba HDInsight integrovaná s místní službou AD přes síť VPN**

Tato architektura se podobá architektuře č. 1. Jediným rozdílem je, že služba Active Directory je místní a dohlednost služby Active Directory pro HDInsight se provádí prostřednictvím [připojení sítě VPN z Azure do místní sítě](../expressroute/expressroute-introduction.md).

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_3.png)

> [!NOTE]
> V této architektuře nemůžete použít Azure Data Lake Store s clusterem HDInsight.

Předpoklady pro službu Active Directory:

* Musí být vytvořená [Organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které chcete umístit virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* Musí být nastaven protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) pro komunikaci se službou Active Directory. Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDI (například 10.2.0.0/24 na předchozím obrázku).
* Je potřeba účet služby nebo účet uživatele, který se používá k vytvoření clusteru HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory

**4. Služba HDInsight integrovaná s místní službou AD synchronizovanou s Azure AD**

Tato architektura se podobá architektuře č. 2. Jediným rozdílem je, že je místní služba Active Directory se synchronizuje s Azure Active Directory. Budete muset nakonfigurovat řadič domény v cloudu tak, aby HDInsight bylo možné integrovat s Azure Active Directory. Toho můžete dosáhnout použitím služby [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (AD DS). AD DS vytvoří počítače řadiče domény v cloudu a poskytne vám jejich IP adresy. Kvůli vyšší dostupnosti vytvoří dva řadiče domény.

V současné době Azure AD DS existuje pouze v klasických virtuálních sítích. Je dostupná jenom pomocí portálu Azure Classic. Virtuální síť HDInsight existuje na webu Azure Portal, takže bude potřeba vytvořit partnerský vztah dvou virtuálních sítí s klasickou virtuální sítí.

> [!NOTE]
> Partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Azure Resource Manageru vyžaduje, aby obě virtuální sítě byly ve stejné oblasti a obě spadaly pod stejné předplatné Azure.

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Předpoklady pro službu Active Directory:

* Musí být vytvořená [Organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které chcete umístit virtuální počítače clusteru HDInsight a instanční objekty používané clusterem. 
* Při konfiguraci služby AD DS musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDI (například 10.2.0.0/24 na předchozím obrázku). 
* Musí se synchronizovat [hodnoty hash hesel](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) ze služby Azure AD do AD DS.
* Je potřeba účet služby nebo účet uživatele, který se používá k vytvoření clusteru HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory

**5. Služba HDInsight integrovaná se službou Azure AD jinou než výchozí (doporučeno pouze pro vývoj a testování)**

Tato architektura se podobá architektuře č. 2. Pro většinu společností je přístup správce ke službě Active Directory omezený jenom na určité jednotlivce. Proto pokud chcete udělat testování konceptu nebo jenom vyzkoušet vytvoření clusteru připojeného k doméně, místo abyste čekali, až správce nakonfiguruje požadavky na službu Active Directory, může být výhodné jenom vytvořit novou službu Azure Active Directory v rámci předplatného. Vzhledem k tomu, že je se jedná o službu Azure AD, kterou jste vytvořili vy, máte úplná oprávnění k této službě Azure AD pro konfiguraci AD DS.

AD DS vytvoří počítače řadiče domény v cloudu a poskytne vám jejich IP adresy. Kvůli vyšší dostupnosti vytvoří dva řadiče domény.

Služba AD DS dnes existuje jenom v klasických virtuálních sítích, proto potřebujete přístup k portálu Classic a musíte vytvořit klasickou virtuální síť pro konfiguraci AD DS. Virtuální síť HDInsight existuje na webu Azure Portal, takže bude potřeba vytvořit partnerský vztah dvou virtuálních sítí s klasickou virtuální sítí.

> [!NOTE]
> Partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Azure Resource Manageru vyžaduje, aby obě virtuální sítě byly ve stejné oblasti a obě spadaly pod stejné předplatné Azure.

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Předpoklady pro službu Active Directory:

* Musí být vytvořená [Organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které chcete umístit virtuální počítače clusteru HDInsight a instanční objekty používané clusterem. 
* Při konfiguraci služby AD DS musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Ke konfiguraci protokolu LDAPS můžete vytvořit [certifikát podepsaný svým držitelem](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Abyste ale mohli použít certifikát podepsaný svým držitelem, musíte požádat o výjimku na adrese <a href="mailto:hdipreview@microsoft.com">hdipreview@microsoft.com</a>.
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDI (například 10.2.0.0/24 na předchozím obrázku). 
* Musí se synchronizovat [hodnoty hash hesel](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) ze služby Azure AD do AD DS.
* Je potřeba účet služby nebo účet uživatele, který se používá k vytvoření clusteru HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).
* Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md).
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Pokud chcete spouštět dotazy Hivu pomocí SSH na clusterech HDInsight připojených k doméně, přečtěte si téma [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md).




<!--HONumber=Feb17_HO1-->


