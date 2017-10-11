---
title: "Přehled Azure šifrování | Microsoft Docs"
description: "Další informace o různých možností šifrování v Azure"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: 752568747ab96bc0a9c7fc5f24ff28c3bce4e09f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-encryption-overview"></a>Přehled Azure šifrování

Tento článek obsahuje přehled použití šifrování v Microsoft Azure. Pokrývá hlavními oblastmi šifrování, včetně šifrování v klidovém stavu, šifrování v cestě a správu klíčů s Key Vault. Každá část obsahuje odkazy na podrobnější informace.

## <a name="encryption-of-data-at-rest"></a>Šifrování neaktivních uložených dat

Data v klidovém stavu zahrnují informace, které se nachází v trvalé úložiště na fyzickém médiu v libovolném digitální formátu. To zahrnuje soubory na magnetické nebo optické média, Archivovaná data a data zálohování. Microsoft Azure nabízí celou řadu řešení úložiště dat pro různé účely, včetně souborů, disku, objektů blob a úložiště table. Společnost Microsoft poskytuje také šifrování k ochraně [Azure SQL Database](../sql-database/sql-database-technical-overview.md), [CosmosDB](../cosmos-db/introduction.md)a Azure Data Lake.

Šifrování dat v klidovém stavu je dostupná pro služby v rámci Azure softwaru jako služba (SaaS), platformy jako služba (PaaS) a infrastruktury jako služba (IaaS) cloudu modelů. Tento dokument shrnuje a obsahuje materiály, které vám pomohou při použití možnosti šifrování Azure.

Podrobné informace o tom, jak je šifrované data umístěná v Azure, najdete v dokumentu s názvem [šifrování na Rest Azure dat](azure-security-encryption-atrest.md)

## <a name="azure-encryption-models"></a>Modely Azure šifrování

Azure podporuje různé modely šifrování, včetně šifrování na straně serveru pomocí službu spravovat klíče, pomocí spravovaného zákazníkem klíčů v Azure Key Vault nebo pomocí klíče spravovaného zákazníkem na hardwaru řízenou zákazníka. Šifrování na straně klienta můžete ke správě a ukládání klíčů místně nebo v jiném zabezpečené umístění.

### <a name="client-side-encryption"></a>Šifrování na straně klienta

Šifrování na straně klienta se provádí mimo Azure. Šifrování na straně klienta zahrnuje:

- Data šifrovat aplikace, která běží v datového centra zákazníka nebo aplikace služby
- Data, která už je šifrovaný při obdržení Azure.

Poskytovatel cloudové služby pomocí šifrování na straně klienta nemá přístup k šifrovacím klíčům a nemůže data dešifrovat. Je-li zachovat úplnou kontrolu nad klíče.

### <a name="server-side-encryption"></a>Šifrování na straně serveru

Tři modely šifrování na straně serveru nabízejí různé správy klíčů charakteristiky, které může být zvolena podle požadavků.

- **Službu spravovat klíče** poskytovat kombinaci řízení a usnadnění práce s nízkou režii

- **Spravované zákazníkem klíče** udělení ovládat klíčů, včetně možnosti, aby vaše vlastní klíče (BYOK) nebo vytvořit nové.

- **Službu spravovat klíče v ccustomer controlledhardware** umožňuje spravovat klíče ve vaší vlastní úložiště, které je mimo kontrolu společnosti Microsoft. Tomu se říká hostitele si vlastní klíč (HYOK). Však konfigurace je složitá a nejvíce Azure services nepodporují tento model.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Virtuální počítače Windows a Linux se dají chránit pomocí [Azure Disk Encryption](azure-security-disk-encryption.md), které používá [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) technologie a Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) k ochraně disky operačního systému a datové disky s šifrování celého svazku.

Šifrování klíče a tajné klíče jsou chráněné v vaše [Azure Key Vault](../key-vault/key-vault-whatis.md) předplatné. Můžete zálohovat a obnovit šifrované virtuálních počítačů, které jsou zašifrované s konfigurací KEK pomocí služby zálohování Azure.

### <a name="azure-storage-service-encryption"></a>Azure šifrování služby úložiště

Ve scénářích serverové a klientské se můžou šifrovat data umístěná v úložišti Azure (objektů Blob a souboru).

[Azure šifrování služby úložiště](../storage/storage-service-encryption.md) (SSE) automaticky data můžete šifrovat před uloženo a automaticky je dešifruje při načtení, což uživatele zcela transparentní procesu. Šifrování služby úložiště používá 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejsilnějších bloku šifer k dispozici a zpracovává šifrování, dešifrování a správu klíčů transparentním způsobem.

### <a name="client-side-encryption-of-azure-blobs"></a>Šifrování na straně klienta objektů BLOB Azure

Šifrování na straně klienta objektů BLOB Azure lze provést různými způsoby.

Klientská knihovna pro úložiště Azure pro balíček NuGet pro rozhraní .NET můžete použít k šifrování dat v rámci vaší klientské aplikace před nahráním do úložiště Azure.

Další informace o a stáhnout Klientská knihovna pro úložiště Azure pro balíček NuGet pro rozhraní .NET, naleznete v dokumentu s názvem [Windows Azure Storage 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage)

Při použití šifrování na straně klienta s Azure Key Vault, vaše data se šifrují pomocí jednorázové symetrického obsahu šifrovací klíč (CEK) generovaný klientem úložiště Azure SDK. CEK se šifrují pomocí klíč šifrovacích klíčů (KEK), které mohou být buď symetrický klíč nebo pár asymetrických klíčů. Můžete ho spravovat místně nebo uložit do Azure Key Vault. Šifrovaná data se pak nahrán do služby Azure Storage.

Další informace o šifrování na straně klienta s Azure Key Vault a začít pracovat s postupy, najdete v dokumentu s názvem [kurz: šifrování a dešifrování objektů BLOB v úložišti Microsoft Azure pomocí Azure Key Vault](../storage/storage-encrypt-decrypt-blobs-key-vault.md)

Nakonec můžete taky Klientská knihovna pro úložiště Azure pro jazyk Java k provedení šifrování na straně klienta před nahráním dat do úložiště Azure a k dešifrování dat při stahování do klienta. Tato knihovna také podporuje integraci s [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) pro správu klíčů účtu úložiště.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Šifrování dat v klidovém stavu s databází Azure SQL

[Azure SQL Database](../sql-database/sql-database-technical-overview.md) je služba pro obecné účely relační databáze v Microsoft Azure, který podporuje struktury například relačních dat, formát JSON, prostorových a XML. Azure SQL podporuje šifrování na straně serveru pomocí funkce transparentní šifrování šifrování dat (TDE) i šifrování na straně klienta pomocí funkce Always Encrypted.

#### <a name="transparent-data-encryption"></a>Transparentní šifrování dat

[Šifrování TDE transparentní šifrování dat](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) se používá k šifrování [systému SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL Database](../sql-database/sql-database-technical-overview.md), a [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) datových souborů v reálném čase, pomocí databáze šifrovací klíč (DEK), která je uložena v databázi spouštěcí záznam pro dostupnosti během obnovení.

Šifrování TDE chrání data a soubory protokolu, pomocí algoritmy šifrování AES a 3DES. Šifrování souboru databáze se provádí na úrovni stránky; stránky v databázi chráněnou zašifrována předtím, než se zapisují na disk a jsou dešifrovat, pokud jste načtení do paměti. Ve výchozím nastavení na nově vytvořené databáze Azure SQL je teď povolené šifrování TDE.

#### <a name="always-encrypted"></a>Funkce Always Encrypted

[Vždy šifrována](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) funkce v Azure SQL umožňuje šifrování dat v rámci klientské aplikace před uložením do databáze SQL Azure a umožňuje vám povolit delegování správy místní databáze třetím stranám a udržovat oddělení od uživatelů, kteří vlastní a mohou zobrazovat data a ty, kdo k její správě ale by neměl mít přístup k němu.

#### <a name="cellcolumn-level-encryption"></a>Šifrování na úrovni buněk nebo sloupce

Databáze SQL Azure umožňuje použít symetrického šifrování pro sloupce dat pomocí jazyka Transact-SQL. To se označuje jako [buňky šifrování na úrovni nebo šifrování na úrovni sloupce](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) (Vymazat), protože slouží k šifrování i konkrétní buněk dat nebo konkrétní sloupce s různými šifrovacími klíči. To vám dává podrobnější možnosti šifrování než šifrování TDE, který šifruje data stránky.

VY obsahuje integrované funkce, které můžete použít k šifrování dat pomocí buď symetrický, nebo asymetrické klíče pomocí veřejného klíče certifikátu, nebo přístupové heslo pomocí 3DES.

### <a name="cosmos-db-database-encryption"></a>Šifrování databáze cosmos DB

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) je globálně distribuované a více modelech databáze společnosti Microsoft. Data uloženy v databázi Cosmos ve stálé úložiště (SSD disky) se šifrují ve výchozím nastavení; neexistují žádná opatření, můžete zapnout nebo vypnout. Šifrování v klidovém stavu je implementována pomocí několika technologiemi zabezpečení, včetně systémů zabezpečeného úložiště klíčů, šifrované sítí a kryptografické rozhraní API. Šifrovací klíče se spravují přes Microsoft a otáčejí na interní pokyny společnosti Microsoft.

### <a name="at-rest-encryption-in-azure-data-lake"></a>Šifrování na rest v Azure Data Lake

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) je úložiště celopodnikové každý typ dat shromažďovaných na jednom místě před žádné formální definice požadavků nebo schéma. Azure Data Lake Store podporuje "na standardně" transparentní šifrování dat v klidovém stavu, která se nastavuje během vytváření účtu. Ve výchozím nastavení Data Lake Store spravuje klíče za vás, ale máte možnost spravovat sami.

Tři typy klíčů se používají v šifrování a dešifrování dat: hlavní šifrovací klíč (MEK), datový šifrovací klíč (DEK) a bloku šifrovací klíč (BEK). MEK se používá k šifrování klíče DEK, který je uložený na trvalé média, a BEK je odvozená od klíč DEK a datového bloku. Pokud spravujete vlastní klíče, můžete MEK otočit.

## <a name="encryption-of-data-in-transit"></a>Šifrování dat při přenosu

Azure nabízí mnoho mechanismy pro zachování dat privátní při jejich přesunu z jednoho umístění do druhého.

### <a name="tlsssl-encryption-in-azure"></a>Protokol TLS/SSL šifrování v Azure

Společnost Microsoft používá [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) protokol (TLS) k ochraně dat cestách mezi cloudové služby a zákazníků. Datových centrech společnosti Microsoft vyjednávání TLS připojení s klientské systémy, které se připojují ke službám Azure. Protokol TLS poskytuje silné ověřování, ochrana soukromí zpráv a integritu (povolení detekce zpráva manipulaci, zachycení a padělání), vzájemná funkční spolupráce, flexibilita algoritmu, snadné nasazení a používání.

[Ideální Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) je chrání připojení mezi klientské systémy zákazníků a cloudové služby společnosti Microsoft podle jedinečné klíče. Připojení pomocí také délek klíčů pro šifrování RSA podle 2 048 bitů. Tato kombinace znesnadňuje někomu k zachycení a přístup k datům, která je na cestě.

### <a name="azure-storage-transactions"></a>Azure transakcí úložiště

Pokud při práci s Azure Storage prostřednictvím portálu Azure, všechny transakce proběhnout přes protokol HTTPS. Také můžete rozhraní API REST úložiště přes protokol HTTPS pro interakci s Azure Storage. Při volání rozhraní REST API pro přístup k objektům v účtech úložiště, povolením bezpečnému přenosu požadované pro účet úložiště, můžete vynutit používání protokolu HTTPS.

Sdílené přístupové podpisy ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), který slouží k delegovat přístup k objektům Azure Storage, zahrnují možnost určit, že při použití sdílené přístupové podpisy lze použít pouze protokol HTTPS. Tím se zajistí, že kdokoliv odesílání se propojení s tokeny SAS používá protokol správné.

[Protokol SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption) používá pro přístup k sdílené složky Azure podporuje šifrování a jeho k dispozici v systému Windows Server 2012 R2, Windows 8, Windows 8.1 a Windows 10, povolení přístupu mezi oblastmi a dokonce přejít na ploše.

Šifrování na straně klienta šifruje data před odesláním do služby Azure Storage, takže je šifrovaný při přenosu v síti.

### <a name="smb-encryption-over-azure-virtual-networks"></a>Šifrování paketů SMB přes virtuální sítě Azure 

[Protokol SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) ve virtuálních počítačích Azure se systémem Windows Server 2012 a vyšší vám dává možnost tak, aby data přenosy zabezpečení tím, že šifrování dat při přenosu přes virtuální sítě Azure k ochraně proti manipulaci a útokům odposlechu. Správci mohou povolit šifrování protokolu SMB pro celý server nebo jenom určité sdílené složky.

Ve výchozím nastavení Jakmile se pro sdílenou složku nebo server, zapnuté šifrování protokolu SMB pouze SMB 3 mohou klienti pro přístup ke složkám šifrované.

## <a name="in-transit-encryption-in-azure-virtual-machines"></a>Šifrování během přenosu ve virtuálních počítačích Azure

Data při přenosu do, z a mezi virtuálními počítači Azure s Windows je zašifrovaná různými způsoby v závislosti na povaze připojení.

### <a name="rdp-sessions"></a>Relace RDP

Můžete se připojit a přihlaste se k virtuálnímu počítači Azure pomocí [Remote Desktop Protocol](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) (RDP) z klientského počítače Windows nebo z Mac s klientem RDP nainstalována. Data při přenosu přes síť v relace RDP může být chráněna TLS.

Můžete taky vzdálené plochy pro připojení k virtuální počítač s Linuxem v Azure.

### <a name="secure-access-to-linux-vms-with-ssh"></a>Zabezpečený přístup k virtuální počítače s Linuxem pomocí protokolu SSH

Můžete použít [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) pro připojení k virtuální počítače Linux spuštěné v Azure pro vzdálenou správu. SSH je protokol šifrované připojení, která umožňuje zabezpečený přihlášení přes nezabezpečený připojení. Je výchozím protokolem připojení pro virtuální počítače Linux hostované v Azure. Pomocí klíče SSH pro ověřování eliminují nutnost použití hesel k přihlášení. Protokol SSH používá pár veřejného a privátního klíče (asymetrické šifrování) pro ověřování.

## <a name="azure-vpn-encryption"></a>Šifrování Azure VPN

Může se připojit k Azure přes virtuální privátní sítě, která vytvoří zabezpečené tunelové propojení k ochraně soukromí dat, které se odesílají přes síť.

### <a name="azure-vpn-gateway"></a>Azure VPN Gateway

[Služba Azure VPN gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) lze použít k odesílání šifrované přenosy mezi virtuální sítí a vaše místní umístění prostřednictvím veřejného připojení nebo odesílat provoz mezi virtuálními sítěmi.

Site-to-site VPN používá [IPsec](https://en.wikipedia.org/wiki/IPsec) pro šifrování přenosu. Azure VPN Gateway pomocí sady výchozí návrhů. Můžete nakonfigurovat Azure VPN Gateway používat vlastní zásady protokolu IPsec/IKE službou konkrétní kryptografické algoritmy a klíče síly, nikoli nastaví Azure výchozí zásady.

### <a name="point-to-site-vpn"></a>Point-to-site VPN

Point-to-Site VPN povolí jednotlivých klientských počítačů přístup k virtuální síti Azure. [Secure Socket Tunneling Protocol](https://technet.microsoft.com/library/2007.06.cableguy.aspx) (SSTP) se používá k vytvoření tunelu VPN a mohou procházet brány firewall (tunelové propojení se zobrazí jako připojení HTTPS). Vlastní interní kořenové certifikační Autority infrastruktury veřejných KLÍČŮ můžete použít pro připojení point-to-site.

Můžete nakonfigurovat připojení point-to-site VPN do virtuální sítě pomocí portálu Azure pomocí ověření certifikátu nebo prostředí PowerShell.

Další informace o připojení point-to-site VPN k virtuálním sítím Azure najdete v tématu: [konfigurace připojení typu Point-to-Site k virtuální síti pomocí ověřování certifikace: portál Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) a

[Konfigurace připojení typu Point-to-Site k virtuální síti ověřování pomocí certifikátů: prostředí PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpn"></a>Site-to-site VPN 

Připojení brány VPN typu Site-to-Site slouží k připojení místní sítě k virtuální síti Azure přes tunel VPN IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje místní zařízení VPN, které má přiřazenou veřejnou IP adresu.

Můžete nakonfigurovat připojení site-to-site VPN do virtuální sítě pomocí portálu Azure, PowerShell nebo rozhraní příkazového řádku Azure (CLI).

Přečtěte si tyto další informace:

[Na portálu Azure vytvořit připojení Site-to-Site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Umožňuje vytvořit připojení Site-to-Site](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí rozhraní příkazového řádku](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-azure-data-lake"></a>Šifrování během přenosu v Azure Data Lake

Přenášená data se ve službě Data Lake Store také vždy šifrují. Kromě šifrování dat před uložením na trvalé médium se také vždy šifrují přenášená data pomocí protokolu HTTPS. HTTPS je jediný protokol, který se podporuje v rozhraních REST služby Data Lake Store.

Další informace o šifrování dat během přenosu v Azure Data Lake najdete v dokumentu s názvem [šifrování dat v Azure Data Lake Store.](../data-lake-store/data-lake-store-encryption.md)

## <a name="key-management-with-azure-key-vault"></a>Správa klíčů s Azure Key Vault

Bez správné ochrana a Správa klíčů je vykreslen šifrování nepoužitelné. Azure Key Vault je doporučené řešení společnosti Microsoft pro správu a řízení přístupu k šifrovací klíče používané cloudové služby. Oprávnění pro přístup k klíčů lze přiřadit ke službám nebo pro uživatele přes účty Azure Active Directory.

Azure Key Vault zbavuje kterou přináší nutnost organizací potřeba konfigurovat, opravy a udržovat modulů hardwarového zabezpečení (HSM) a software pro správu klíčů. S Azure Key Vault Microsoft nikdy uvidí vaše klíče a aplikace nemají přímý přístup k nim; můžete zachovat kontrolu. Můžete také importovat nebo generovat klíče v modulech Hsm.

## <a name="next-steps"></a>Další kroky

- [Přehled zabezpečení Azure](security-get-started-overview.md)
- [Přehled zabezpečení sítě Azure](security-network-overview.md)
- [Přehled zabezpečení databáze Azure](azure-database-security-overview.md)
- [Přehled zabezpečení virtuální počítače Azure](security-virtual-machines-overview.md)
- [Šifrování dat v klidovém stavu](azure-security-encryption-atrest.md)
- [Osvědčené postupy šifrování a zabezpečení dat](azure-security-data-encryption-best-practices.md)
