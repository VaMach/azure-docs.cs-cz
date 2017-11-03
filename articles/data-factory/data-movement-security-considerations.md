---
title: "Aspekty zabezpečení v Azure Data Factory | Microsoft Docs"
description: "Popisuje základní zabezpečení infrastrukturu, která pomocí služby pro přesun dat v Azure Data Factory zabezpečit data."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2017
ms.author: abnarain
ms.openlocfilehash: 9caea4191a2ca99e6e98cc8ce7ca9ca0c7b8dc87
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - důležité informace o zabezpečení pro přesun dat
Tento článek popisuje základní zabezpečení infrastrukturu, která pomocí služby pro přesun dat v Azure Data Factory zabezpečit data. Správa prostředků Azure Data Factory jsou postaveny na infrastrukturu zabezpečení Azure a použít všechny možné bezpečnostní opatření, které nabízí Azure.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aspekty zabezpečení přesunu dat pro datovou továrnu verze 1](v1/data-factory-data-movement-security-considerations.md).

V řešení Data Factory vytváříte jeden nebo více datových [kanálů](concepts-pipelines-activities.md). Kanál je logické seskupení aktivit, které dohromady provádějí určitou úlohu. Tyto kanály nacházet v oblasti, kde byl vytvořen služby data factory. 

I když objekt pro vytváření dat je k dispozici v pouze **východní USA** a **východní USA 2** oblast (preview verze 2), služba pro přesun dat je k dispozici [globálně v několika oblastech](concepts-integration-runtime.md#azure-ir). Pokud ještě není nasazený služba pro přesun dat do této oblasti, služba Data Factory zajistí data nesmí zůstat zeměpisná oblast nebo oblasti Pokud dáte pokyn explicitně službu na používání alternativního oblast. 

Azure Data Factory, samotné neukládá všechna data s výjimkou pověření propojené služby pro cloudové úložiště dat, které jsou zašifrované pomocí certifikátů. Umožňuje vytvářet pracovní postupy řízené daty k orchestraci přesouvání dat mezi [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) a zpracování dat pomocí [výpočetních služeb](compute-linked-services.md) v jiných oblastech nebo v místním prostředí. Umožňuje také můžete monitorovat a spravovat pracovní postupy pomocí sady SDK a monitorování Azure.

Přesun dat pomocí Azure Data Factory byl **certifikované** pro:
-   [HIPAA NEBO HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA) 
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018)
-   [CSA HVĚZDIČKOU](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)

Pokud vás zajímá Azure dodržování předpisů a jak Azure zabezpečuje svou vlastní infrastrukturu, navštivte [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

V tomto článku jsme zkontrolujte důležité informace o zabezpečení v následujících scénářích přesun dvě dat: 

- **Scénář cloudu**– v tomto scénáři jsou veřejně přístupná prostřednictvím Internetu zdrojový i cílový. Mezi ně patří spravované cloudové úložiště služby Azure Storage, Azure SQL Data Warehouse, databáze SQL Azure, Azure Data Lake Store, Amazon S3, Amazon Redshift, služby SaaS, jako je například služby Salesforce a webové protokoly například protokoly FTP a OData. Úplný seznam podporovaných zdrojů dat můžete najít [zde](copy-activity-overview.md#supported-data-stores-and-formats).
- **Scénář hybridního**– v tomto scénáři vaše zdrojové nebo cílové je za bránou firewall nebo uvnitř podnikové sítě na místní nebo data úložiště je v privátní síti / virtuální sítě (nejčastěji zdroje) a není veřejně přístupná. Databázové servery hostované ve virtuálních počítačích také spadají pod tento scénář.

## <a name="cloud-scenarios"></a>Scénáře cloudu
###<a name="securing-data-store-credentials"></a>Zabezpečení údaje k úložišti dat.
- Uložení zašifrované přihlašovací údaje v Azure Data Factory spravované úložiště.

   Azure Data Factory chrání přihlašovací údaje k úložišti dat pomocí **šifrování** je pomocí **certifikáty, které spravuje Microsoft**. Tyto certifikáty otáčejí každých **dva roky** (která zahrnuje obnovení certifikátu a migrace přihlašovacích údajů). Tyto zašifrované přihlašovací údaje jsou bezpečně uložené v **Azure Storage spravuje služby Azure Data Factory pro**. Další informace o zabezpečení Azure Storage, najdete v části [Přehled zabezpečení služby Azure Storage](../security/security-storage-overview.md).
- Uložení přihlašovacích údajů v Azure Key Vault 

   Teď můžete zvolit uložení přihlašovacích údajů úložiště dat v [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), nechte Azure Data Factory k načtení během provádění aktivity. Další informace najdete v tématu [přihlašovací údaje úložiště v Azure Key Vault](store-credentials-in-key-vault.md).

   > [!NOTE]
   > V současné době pouze [Dynamics konektor](connector-dynamics-crm-office-365.md) tuto funkci podporuje. 

### <a name="data-encryption-in-transit"></a>Šifrování dat při přenosu
Pokud cloudové úložiště dat podporuje protokol HTTPS nebo TLS, všech dat přenesených mezi služby pro přesun dat v datové továrně a cloudové úložiště dat jsou prostřednictvím zabezpečeného kanálu HTTPS nebo TLS.

> [!NOTE]
> Všechna připojení k **Azure SQL Database** a **Azure SQL Data Warehouse** vždy vyžadovat šifrování (SSL/TLS) se při přenosu dat, do a z databáze. Při vytváření kanálu pomocí JSON, přidejte **šifrování** vlastnost a nastavte ji na **true** v **připojovací řetězec**. Pro **Azure Storage**, můžete použít **HTTPS** v připojovacím řetězci.

### <a name="data-encryption-at-rest"></a>Šifrování v klidovém stavu
Některá data ukládá podpora šifrování dat v klidovém stavu. Doporučujeme, abyste povolili mechanismus šifrování dat pro tyto datové úložiště. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparentní dat šifrování (TDE) v Azure SQL Data Warehouse pomáhá s provedením v reálném čase šifrování a dešifrování dat v klidovém stavu ochrany proti riziko, že škodlivé aktivity. Toto chování je pro klienta transparentní. Další informace najdete v tématu [zabezpečení databáze v SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL Database podporuje také transparentní šifrování dat (šifrování TDE), která pomáhá s ochrany proti riziko, že škodlivé aktivity provedením v reálném čase šifrování a dešifrování dat bez nutnosti změny aplikace. Toto chování je pro klienta transparentní. Další informace najdete v tématu [transparentní šifrování dat s Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store taky zajišťuje šifrování dat uložených v účtu. Když je povolené, úložiště Data Lake store automaticky šifruje data před uložením a dešifruje před načtení, což transparentní klientovi přístupu k datům. Další informace najdete v tématu [zabezpečení v Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Storage a Azure Table Storage
Úložiště Azure Blob Storage a Azure Table podporuje šifrování služby úložiště (SSE), který automaticky šifruje vaše data před uložením do úložiště a dešifruje před načtení. Další informace najdete v tématu [šifrování služby úložiště Azure pro Data v klidovém stavu](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 podporuje klient i server šifrování dat v klidovém stavu. Další informace najdete v tématu [ochranu šifrování dat pomocí](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Objekt pro vytváření dat v současné době nepodporuje Amazon S3 uvnitř virtuální privátní cloud (VPC).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift podporuje šifrování clusteru pro neaktivní uložená data. Další informace najdete v tématu [šifrování databáze Redshift Amazon](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Objekt pro vytváření dat v současné době nepodporuje Amazon Redshift uvnitř VPC. 

#### <a name="salesforce"></a>Salesforce
Salesforce podporuje šifrování štítu platformy, která umožňuje šifrování všech souborů, přílohy, vlastní pole. Další informace najdete v tématu [Princip toku webového serveru pro ověřování OAuth](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-self-hosted-integration-runtime"></a>Hybridní scénáře (pomocí integrace s vlastním hostováním runtime)
Hybridní scénáře vyžadují integraci s vlastním hostováním runtime nainstalují v místní síti nebo uvnitř virtuální sítě (Azure) nebo virtuální privátní cloud (Amazon). Modul runtime vlastním hostováním integrace musí být mít přístup k místní datové úložiště. Další informace o vlastním hostováním integrace modulu runtime najdete v tématu [hostovanou na vlastním integrace runtime](create-self-hosted-integration-runtime.md). 

![Integrace s vlastním hostováním runtime kanály](media/data-movement-security-considerations/data-management-gateway-channels.png)

**Příkaz kanál** umožňuje komunikaci mezi služby pro přesun dat v datové továrně a integrace s vlastním hostováním runtime. Komunikace obsahuje informace související s aktivity. Datový kanál se používá pro přenos dat mezi místním úložištím dat a úložiště dat v cloudu.    

### <a name="on-premises-data-store-credentials"></a>Místní přihlašovací údaje k úložišti dat
Přihlašovací údaje pro vaše místní úložiště dat jsou vždy šifrována a uložené. Ho může být buď uložených místně na počítači runtime vlastním hostováním integrace nebo v Azure Data Factory spravované úložiště (stejně jako cloudové úložiště přihlašovacích údajů). 

1. Můžete se rozhodnout **ukládat přihlašovací údaje místně**. Pokud chcete k šifrování a uložení přihlašovacích údajů místně na vlastním hostováním integrace modulu runtime, postupujte podle kroků v [šifrování přihlašovacích údajů na vlastním hostováním integrace runtime](encrypt-credentials-self-hosted-integration-runtime.md). Všechny konektory podporu této možnosti. Modul runtime vlastním hostováním integrace používá Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) se šifrovat důvěrné osobní údaje nebo informace na přihlašovací údaje. 

   Použití **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** rutiny se šifrování přihlašovacích údajů propojené služby nebo šifrování citlivých podrobnosti v propojené službě. Pak můžete použít JSON vrátil (s **EncryptedCredential** element v **connectionString**) k vytvoření propojené služby podle **Set-AzureRmDataFactoryV2LinkedSevrice**rutiny.  

2. Pokud nepoužijete **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** rutiny jako popsané v předchozím kroku a místo toho použijte přímo **Set-AzureRmDataFactoryV2LinkedSevrice** rutiny s připojení řetězce / credentials vložené v kódu JSON, pak bude propojená služba **šifrované a uložené v Azure Data Factory spravované úložiště**. Důvěrné informace stále šifrován pomocí certifikátu a tyto certifikáty jsou spravovány společností Microsoft.



#### <a name="ports-used-during-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Porty používané při dešifrování propojená služba na vlastním hostováním integrace modulu runtime
Ve výchozím nastavení, prostředí PowerShell používá port **8050** na počítači s vlastním hostováním integrace runtime pro zabezpečenou komunikaci. V případě potřeby můžete změnit tento port.  

![Port HTTPS pro brány](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Šifrování během přenosu
Všechny přenosy dat jsou prostřednictvím zabezpečeného kanálu **HTTPS** a **TLS přes protokol TCP** aby se zabránilo útokům man-in-the-middle při komunikaci se službami Azure.

Můžete také použít [IPSec pro síť VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) nebo [Express Route](../expressroute/expressroute-introduction.md) na další Zabezpečte komunikační kanál mezi místní sítí a Azure.

Virtuální síť se znázornění logické sítě v cloudu. Můžete připojit k místní síti do virtuální sítě Azure (VNet) pomocí nastavení sítě VPN IPSec (site-to-site) nebo Express Route (soukromý partnerský vztah)     

Následující tabulka shrnuje sítě a doporučených konfigurací runtime vlastním hostováním integrace založený na různé kombinace zdrojové a cílové umístění pro přesun dat hybridní.

| Zdroj      | Cíl                              | Konfigurace sítě                    | Nastavení integrace modulu runtime                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Lokálně | Virtuální počítače a cloudové služby nasazené ve virtuálních sítích | IPSec pro síť VPN (point-to-site nebo site-to-site) | Modul runtime vlastním hostováním integrace lze nainstalovat místně nebo v Azure virtuální počítač (VM) ve virtuální síti |
| Lokálně | Virtuální počítače a cloudové služby nasazené ve virtuálních sítích | ExpressRoute (soukromý partnerský vztah)           | Modul runtime vlastním hostováním integrace může být nainstalované místně nebo na virtuální počítač Azure ve virtuální síti |
| Lokálně | Služeb založených na Azure, které mají veřejný koncový bod | ExpressRoute (veřejný partnerský vztah)            | Modul runtime vlastním hostováním integrace musí být nainstalované na místě |

Následující obrázky znázorňují použití vlastním hostováním integrace modulu runtime pro přesun dat mezi místní databázi a službami Azure pomocí expressroute a VPN IPSec (s virtuální sítě):

**Express Route:**

![Použití Express Route s bránou](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec pro síť VPN:**

![IPSec pro síť VPN s bránou](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-self-hosted-integration-runtime"></a>Konfigurace bran firewall a vytvoření seznamu povolených IP adres (vlastním hostováním integrace runtime)

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Požadavky na brány firewall pro ve místní nebo soukromé síti  
V podniku **podniková brána firewall** běží na směrovači centrální organizace. A, **brány Windows firewall** spouští se jako démon na místním počítači, ve kterém je nainstalován modul runtime vlastním hostováním integrace. 

Následující tabulka obsahuje **odchozí port** a požadavky domény **podniková brána firewall**.

| Názvy domén                  | Odchozí porty | Popis                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443, 80        | Požadované pro připojení k služby pro přesun dat v datové továrně modulem runtime integrace s vlastním hostováním |
| `*.core.windows.net`          | 443            | Používá modul runtime vlastním hostováním integrace pro připojení k účtu úložiště Azure při použití [připravený kopie](copy-activity-performance.md#staged-copy) funkce. |
| `*.frontend.clouddatahub.net` | 443            | Požadované modulem runtime vlastním hostováním integrace pro připojení ke službě Azure Data Factory. |
| `*.database.windows.net`      | 1433           | (Volitelné) je nutné zadat cíl je Azure SQL Database / Azure SQL datového skladu. Použijte funkci dvoufázové instalace kopírování zkopírovat data do Azure SQL Database nebo Azure SQL Data Warehouse bez otevření portu 1433. |
| `*.azuredatalakestore.net`    | 443            | (Volitelné) je nutné zadat cíl je Azure Data Lake store |

> [!NOTE] 
> Možná budete muset spravovat porty / vytvoření seznamu povolených domén v podnikové brány firewall na úrovni podle potřeby podle příslušné datové zdroje. Tato tabulka pouze používá databázi SQL Azure, Azure SQL Data Warehouse, Azure Data Lake Store jako příklady.   

Následující tabulka obsahuje **příchozí port** požadavky **brány Windows firewall**.

| Příchozí porty | Popis                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | Požadované rutiny prostředí PowerShell šifrování, jak je popsáno v [šifrování přihlašovacích údajů na vlastním hostováním integrace runtime](encrypt-credentials-self-hosted-integration-runtime.md)/ přihlašovacích údajů správce aplikací a bezpečně nastavte přihlašovací údaje pro místní úložiště dat na vlastním hostováním integrace modulu runtime. |

![Požadavky na porty brány](media\data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurationswhitelisting-in-data-store"></a>Konfigurace protokolu IP a povolených v úložišti dat.
Některé datové úložiště v cloudu i vyžadovat povolených IP adresu počítače k nim přistupovat. Ujistěte se, že IP adresa počítače runtime vlastním hostováním integrace je seznam povolených adres a správně nakonfigurována v bráně firewall.

Následující cloudové úložiště dat vyžaduje povolených IP adresu počítače runtime vlastním hostováním integrace. Některé z těchto dat úložiště, ve výchozím nastavení, ale nemusí vyžadovat vytvoření seznamu povolených IP adresy. 

- [Azure SQL Database](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Otázka:** vlastním hostováním integrace runtime v lze sdílet různé datové továrny?
**Odpověď:** tato funkce ještě nepodporujeme. Aktivně pracujeme na něm.

**Otázka:** jaké jsou požadavky na portu pro integraci s vlastním hostováním runtime pro práci?
**Odpověď:** vlastním hostováním integrace runtime umožňuje založené na protokolu HTTP připojení k Internetu otevřete. **Odchozí porty 443 a 80** musí být otevřen pro integraci s vlastním hostováním runtime pro toto připojení. Otevřete **příchozí Port 8050** pouze na úrovni počítače (ne na úrovni podniková brána firewall) pro aplikaci správce přihlašovacích údajů. Pokud se databáze SQL Azure nebo Azure SQL Data Warehouse používá jako zdroj / cíl a pak muset otevřít **1433** také portu. Další informace najdete v tématu [konfigurace a vytvoření seznamu povolených IP adresy brány Firewall](#firewall-configurations-and-whitelisting-ip-address-of gateway) části. 


## <a name="next-steps"></a>Další kroky
Informace o výkonu aktivitě kopírování najdete v tématu [zkopírujte aktivity výkonu a vyladění průvodce](copy-activity-performance.md).

 
