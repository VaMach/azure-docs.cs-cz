---
title: "Zabezpečení Hadoop – clustery HDInsight připojené k doméně – Azure | Dokumentace Microsoftu"
description: "Zjistěte..."
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
ms.date: 10/31/2016
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: cd57f3a43142b3af3546eafd9749123fadd333c2
ms.openlocfilehash: 27cb0cda5d836e042e9eca3c053577db0bd8c148


---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters-preview"></a>Úvod do zabezpečení Hadoop s clustery HDInsight připojenými k doméně (Preview)

Služba Azure HDInsight až doteď podporovala jako místního správce pouze jednoho uživatele. To dobře fungovalo pro menší aplikace, týmy nebo oddělení. S tím, jak se úlohy založené na systému Hadoop stávaly oblíbenějšími v podnikovém sektoru, zvyšovala se i potřeba schopností na podnikové úrovni, jako například ověřování ve službě Active Directory, podpora více uživatelů a řízení přístupu na základě rolí. Pomocí clusterů HDInsight připojených k doméně můžete vytvořit cluster HDInsight připojený k doméně služby Active Directory a nakonfigurovat seznam zaměstnanců podniku, kteří se pro přihlášení ke clusteru mohou ověřovat prostřednictvím služby Azure Active Directory. Nikdo jiný mimo podnik se ke clusteru HDInsight nemůže přihlásit ani připojit. Podnikový správce může za účelem zabezpečení Hivu nakonfigurovat řízení přístupu založené na rolích pomocí [Apache Ranger](http://hortonworks.com/apache/ranger/) a tím omezit přístup k datům na nutné minimum. Kromě toho může správce také provádět audit přistupování k datům zaměstnanci a jakýchkoli změn provedených v zásadách řízení přístupu. Tím dosáhne vysokého stupně dohledu nad firemními prostředky.

> [!NOTE]
> Nové funkce popsané v této verzi Preview jsou dostupné jenom na clusterech HDInsight založených na Linuxu pro úlohy Hivu. Další úlohy, jako například HBase, Spark, Storm a Kafka, budou povoleny v budoucích verzích.
>
>

## <a name="benefits"></a>Výhody
Zabezpečení podniku se skládá ze čtyř hlavních pilířů – Zabezpečení perimetru, Ověřování, Autorizace a Šifrování.

![Pilíře výhod clusterů HDInsight připojených k doméně](./media/hdinsight-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Zabezpečení perimetru
Služba HDInsight zajišťuje zabezpečení perimetru pomocí virtuálních sítí a služby Gateway. Dnes může podnikový správce vytvořit cluster HDInsight uvnitř virtuální sítě a pomocí skupin zabezpečení sítě (příchozí nebo odchozí pravidla brány firewall) omezit přístup k této virtuální síti. Pouze IP adresy definované v příchozích pravidlech brány firewall budou moci komunikovat s clusterem HDInsight a tak je zajištěno zabezpečení perimetru. Další vrstvu zabezpečení perimetru zajišťuje služba Gateway. Gateway je služba, která slouží jako první linie obrany pro veškeré příchozí požadavky do clusteru HDInsight. Tato služba požadavek přijme, ověří jej a teprve pak mu umožní průchod do dalších uzlů v clusteru. Tím zajišťuje zabezpečení ostatních jmenných a datových uzlů v clusteru.

### <a name="authentication"></a>Authentication
V této verzi Public Preview může podnikový správce zřídit cluster HDInsight připojený k doméně ve [virtuální síti](https://azure.microsoft.com/services/virtual-network/). Uzly clusteru HDInsight budou připojeny k doméně spravované podnikem. Toho můžete dosáhnout s použitím služby [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Všechny uzly v clusteru jsou připojeny k doméně spravované podnikem. S tímto nastavením se mohou zaměstnanci podniku přihlašovat ke clusteru pomocí svých přihlašovacích údajů domény. Přihlašovací údaje domény mohou použít také k ověřování v dalších schválených koncových bodech, jako jsou Hue, Ambari Views, rozhraní ODBC, JDBC, PowerShell a rozhraní REST API, a pracovat tak s clusterem. Správce má úplnou kontrolu nad omezením počtu uživatelů pracujících s clusterem přes tyto koncové body.

### <a name="authorization"></a>Autorizace
Osvědčeným postupem, který dodržuje většina podniků, je, že ne každý zaměstnanec má přístup ke všem podnikovým prostředkům. Stejně tak v této verzi může správce pro prostředky clusteru definovat zásady řízení přístupu na základě rolí. Správce může například nakonfigurovat [Apache Ranger](http://hortonworks.com/apache/ranger/), aby nastavil zásady řízení přístupu pro Hive. Tato funkce zajišťuje, že uživatelů budou mít přístup pouze k nezbytnému množství dat, která potřebují k úspěšné práci. Přístup ke clusteru přes SSH je také omezen pouze pro správce.

### <a name="auditing"></a>Auditování
Kromě ochrany prostředků clusteru HDInsight před neautorizovanými uživateli a zabezpečení dat je nezbytné provádění auditu veškerých přístupů k prostředkům a datům clusteru za účelem sledování neautorizovaného nebo nechtěného přístupu k prostředkům. V této verzi Preview může správce zobrazit a vytvářet sestavy o všech přístupech k prostředkům a datům clusteru HDInsight. Správce může také zobrazit a vytvářet sestavy o všech změnách v zásadách řízení přístupu provedených v koncových bodech podporovaných v Apache Ranger. Cluster HDInsight připojený k doméně používá k prohledávání protokolů auditu známé uživatelské rozhraní Apache Ranger. Na back-endu používá Ranger k ukládání a prohledávání protokolů [Apache Solr](http://hortonworks.com/apache/solr/).

### <a name="encryption"></a>Šifrování
Ochrana dat je důležitá pro splnění požadavků na zabezpečení organizace a dodržování předpisů. Kromě omezení přístupu k datům neautorizovanými uživateli by data měla být zabezpečena také šifrováním. Obě úložiště dat pro clustery HDInsight (Azure Storage Blob a úložiště Azure Data Lake) podporují transparentní [šifrování neaktivních uložených dat](../storage/storage-service-encryption.md) na straně serveru. Zabezpečené clustery HDInsight budou bez problému fungovat s touto schopností šifrování neaktivních uložených dat na straně serveru.

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).
* Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md).
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Pokud chcete spouštět dotazy Hivu pomocí SSH na clusterech HDInsight připojených k doméně, přečtěte si téma [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).



<!--HONumber=Feb17_HO3-->


