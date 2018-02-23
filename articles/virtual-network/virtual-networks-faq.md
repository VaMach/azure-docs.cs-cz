---
title: "Virtuální síť Azure – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi na nejčastější dotazy týkající se virtuálních sítí Microsoft Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/18/2017
ms.author: jdial
ms.openlocfilehash: 2042bc44df7d3d61bf52d28a910dae1b125b9fdb
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Virtuální síť Azure nejčastější dotazy (FAQ)

## <a name="virtual-network-basics"></a>Základy virtuální sítě

### <a name="what-is-an-azure-virtual-network-vnet"></a>Co je virtuální síť Azure (VNet)?
Virtuální síť Azure (VNet) je reprezentace vlastní sítě v cloudu. Je to logická izolace cloudu Azure vyhrazeného pro vaše předplatné. Můžete zřídit pomocí virtuální sítě a spravovat virtuální privátní sítě (VPN) v Azure a, volitelně propojení virtuálních sítí jiných virtuálních sítí v Azure nebo s místní infrastrukturu IT na vytvořit hybridní nebo mezi různými místy řešení. Každý virtuální sítě, které vytvoříte má svou vlastní blok CIDR a lze propojit s jinými virtuálními sítěmi a místními sítěmi, dokud se nepřekrývají bloků CIDR. Máte také kontrolu nad nastavení serveru DNS pro virtuální sítě a segmentaci sítě vnet do podsítě.

Použijte virtuální sítě na:

* Vytvořte vyhrazené privátní cloudové virtuální síť někdy nevyžadují konfigurace mezi různými místy pro vaše řešení. Když vytvoříte virtuální síť, služeb a virtuálních počítačů v rámci virtuální sítě můžete přímo a bezpečně vzájemně komunikovat v cloudu. To zajišťuje provoz bezpečně v rámci virtuální sítě, ale stále umožňuje konfiguraci koncového bodu připojení pro virtuální počítače a služby, které vyžadují internetovou komunikaci v rámci vašeho řešení.
* Bezpečně prodloužit datové centrum s virtuální sítě, můžete vytvořit tradiční site-to-site (S2S) VPN bezpečně škálování kapacitu vašeho datového centra. S2S VPN pomocí protokolu IPSEC pro zajištění zabezpečeného spojení mezi vaší podnikové brány sítě VPN a Azure.
* Povolit hybridní cloudové scénáře virtuální sítě poskytují flexibilitu na podporu řadu hybridní cloudové scénáře. Cloudové aplikace libovolného typu v místním systému, jako je například sálové počítače a systémů Unix můžete bezpečně připojit.

### <a name="how-do-i-know-if-i-need-a-vnet"></a>Jak poznám, kdy je třeba virtuální sítě?
[Přehled virtuálních sítí](virtual-networks-overview.md) článek obsahuje rozhodovací tabulky, které vám pomohou rozhodnout, nejlepší možnost návrh sítě pro vás.

### <a name="how-do-i-get-started"></a>Jak mám začít?
Přejděte [dokumentaci virtuální sítě](https://docs.microsoft.com/azure/virtual-network/) začít pracovat. Tato část poskytuje přehled a nasazení informace pro všechny funkce sítě VNet.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Můžete použít virtuální sítě bez připojení mezi různými místy?
Ano. Můžete vytvořit virtuální síť bez použití hybridní připojení. To je zvlášť užitečné, pokud chcete spustit v Azure řadiče domény Microsoft Windows Server Active Directory a farmy služby SharePoint.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Můžete provést optimalizace sítě WAN mezi virtuálními sítěmi nebo virtuální síť a Moje místní datové centrum?

Ano. Můžete nasadit [WAN optimalizace sítě virtuálního zařízení](https://azure.microsoft.com/marketplace/?term=wan+optimization) od více dodavatelů přes Azure Marketplace.

## <a name="configuration"></a>Konfigurace

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Jaké nástroje se používá k vytvoření virtuální sítě?
K vytvoření nebo konfiguraci virtuální sítě můžete použít následující nástroje:

* Portál Azure (pro classic a Resource Manager sítě Vnet).
* Soubor konfigurace sítě (netcfg - pro virtuální sítě classic pouze). Najdete v článku [konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md) článku.
* Prostředí PowerShell (pro classic a Resource Manager sítě Vnet).
* Rozhraní příkazového řádku Azure (pro classic a Resource Manager sítě Vnet).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Jaké rozsahy adres můžete použít v mé virtuální sítě?
Všechny rozsah IP adres definované v [RFC 1918](http://tools.ietf.org/html/rfc1918). Například 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Může mít veřejné IP adresy v mé virtuální sítě?
Ano. Další informace o rozsahů veřejných IP adres najdete v tématu [adresní prostor veřejných IP adres ve virtuální síti](virtual-networks-public-ip-within-vnet.md) článku. Veřejné IP adresy nebudou přímo přístupné z Internetu.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>Existuje limit pro počet podsítí ve virtuální síti?
Ano. Pro čtení [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku. Adresní prostory podsítě se nesmí překrývat jednu na druhou.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Existují nějaká omezení na pomocí IP adresy v rámci těchto podsítí?
Ano. Azure si vyhrazuje některé IP adresy v rámci každé podsíti. První a poslední IP adresy podsítí jsou vyhrazené pro udržování souladu s protokoly a další 3 adresy se používají pro služby Azure.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Jak malý a jak velká může být virtuální sítě a podsítě?
Nejmenší podsítě, které podporujeme je/29 a na největší podsíť/8 (pomocí definice podsítě CIDR).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Mohu převést Moje sítě VLAN do Azure pomocí virtuální sítě?
Ne. Virtuální sítě jsou překryvy vrstvy 3. Azure nepodporuje žádné sémantiku vrstvy 2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Můžete zadat vlastní zásady směrování na můj virtuální sítě a podsítě?
Ano. Můžete vytvořit uživateli definované směrování (UDR). Další informace o UDR najdete na adrese [trasy definované uživatelem a předávání IP](virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Podporují virtuální sítě vícesměrového vysílání nebo všesměrového vysílání?
Ne. Nepodporujeme vícesměrového vysílání nebo všesměrového vysílání.

### <a name="what-protocols-can-i-use-within-vnets"></a>Jaké protokoly můžete použít v rámci virtuální sítě?
Můžete použít protokoly TCP, UDP a ICMP TCP/IP v rámci virtuálních sítí. Jednosměrového vysílání je podporována v rámci virtuálních sítí, s výjimkou dynamické hostitele konfigurace Protocol (DHCP) pomocí jednosměrového vysílání (zdrojový port UDP/68 / cílový port UDP/67). V rámci virtuální sítě jsou blokovány vícesměrového vysílání, všesměrové vysílání, IP-in-IP zapouzdřené pakety a pakety zapouzdření GRE (Generic Routing). 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Můžete příkaz ping Moje výchozí směrovačů v rámci virtuální sítě?
Ne.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Můžete použít tracert diagnostikovat připojení?
Ne.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Můžete přidat podsítě, po vytvoření virtuální sítě?
Ano. Podsítě můžete přidat k virtuálním sítím kdykoli, dokud adresu podsíť není součástí jinou podsítí ve virtuální síti.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Můžete upravit velikost mou podsíť, po jeho vytvoření?
Ano. Můžete přidat, odebrat, zvětšení nebo zmenšení podsíť, pokud nejsou žádné virtuální počítače nebo službám nasazeným v něm.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Můžete změnit po vytvoření je podsítě?
Ano. Můžete přidat, odebrat a upravit bloků CIDR používaných ve virtuální síti.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Možné pokud používám Moje služby ve virtuální síti připojit k Internetu?
Ano. Všechny služby nasazuje v rámci virtuální sítě můžete připojit k Internetu. Každé cloudové služby nasazené v Azure má veřejně adresovatelné VIP přiřazen. Je nutné zadat vstupních koncových bodů pro rolí PaaS a koncové body pro virtuální počítače, aby tyto služby tak, aby přijímal připojení z Internetu.

### <a name="do-vnets-support-ipv6"></a>Podporují virtuální sítě IPv6?
Ne. V tuto chvíli nelze použít protokol IPv6 s virtuální sítě.

### <a name="can-a-vnet-span-regions"></a>Virtuální síť, která může mít rozsah oblastí?
Ne. Virtuální síť, která je omezená na jedné oblasti.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Můžete připojit virtuální síť k jiné virtuální síti v Azure?
Ano. Jedna virtuální síť můžete připojit k jiné virtuální sítě pomocí:
- Služby Azure VPN Gateway. Pro čtení [konfigurace připojení typu VNet-to-VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) článku. 
- Virtuální síť partnerský vztah. Pro čtení [VNet partnerského vztahu přehled](virtual-network-peering-overview.md) článku.

## <a name="name-resolution-dns"></a>Překlad adres (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Jaké jsou možnosti DNS pro virtuální sítě?
Pomocí tabulky rozhodnutí na [překlad názvů pro virtuální počítače a instance rolí](virtual-networks-name-resolution-for-vms-and-role-instances.md) stránky vás provedou všechny DNS možnosti k dispozici.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Můžete určit servery DNS pro virtuální síť?
Ano. V nastavení virtuální sítě můžete zadat IP adresy serverů DNS. To se použijí jako servery DNS výchozí pro všechny virtuální počítače ve virtuální síti.

### <a name="how-many-dns-servers-can-i-specify"></a>Počet serverů DNS, které můžete zadat
Odkaz [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Můžete upravit Moje servery DNS, po vytvoření sítě?
Ano. Seznam serverů DNS pro vaši virtuální síť můžete změnit kdykoli. Pokud změníte váš seznam serverů DNS, musíte se k restartování všech virtuálních počítačích ve vaší virtuální síti v pořadí pro ně načíst na nový server DNS.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Co je Azure DNS a pracuje s virtuálními sítěmi?
Azure DNS je služba DNS víceklientské nabízených společností Microsoft. Azure zaregistruje všechny virtuální počítače a instance role cloudové služby v rámci této služby. Tato služba poskytuje překlad název hostitele pro virtuální počítače a instance rolí, které jsou obsaženy v rámci stejné cloudové služby a plně kvalifikovaný název domény pro virtuální počítače a instance rolí ve stejné virtuální síti. Pro čtení [překlad názvů pro virtuální počítače a instance rolí](virtual-networks-name-resolution-for-vms-and-role-instances.md) článku Další informace o službě DNS.

> [!NOTE]
> Existuje omezení během této doby první 100 cloudové služby ve virtuální síti mezi klienta překladu IP adres pomocí DNS poskytnutých platformou Azure. Pokud používáte serveru DNS, toto omezení neplatí.
> 
> 

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Můžete přepsat nastavení DNS na jednotlivé virtuální počítače / service základ?
Ano. Servery DNS můžete nastavit na základě jednotlivých cloudů služby přepsat výchozí nastavení sítě. Doporučujeme však používat co nejvíc celé sítě DNS.

### <a name="can-i-bring-my-own-dns-suffix"></a>Můžete zahrnout vlastní příponu DNS?
Ne. Nelze zadat vlastní přípony DNS pro vaše virtuální sítě.

## <a name="connecting-virtual-machines"></a>Propojování virtuálních počítačů

### <a name="can-i-deploy-vms-to-a-vnet"></a>Můžete nasadit virtuální počítače k virtuální síti?
Ano. Všech síťových rozhraní (NIC) připojené k virtuálnímu počítači nasazené prostřednictvím modelu nasazení Resource Manager musí být připojen k virtuální síti. Virtuální počítače nasazené prostřednictvím modelu nasazení classic můžete volitelně připojen k virtuální síti.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>Jaké jsou různé typy IP adresy, kterou lze přiřadit virtuální počítače?
* **Privátní:** přiřazené pro každou síťovou kartu v rámci jednotlivých virtuálních počítačů. Pomocí buď metodu statickou nebo dynamickou je přiřazena adresa. Privátní IP adresy přiřazené z rozsahu, který jste zadali v nastavení podsítě pro vaši virtuální síť. Prostředky nasazené prostřednictvím modelu nasazení classic jsou přiřazeny soukromé IP adresy, i když nejsou připojeni k virtuální síti. Privátní IP adresy dynamické metoda má přiřazeny zůstane přiřazená k prostředku, až do odstranění prostředku (virtuální počítače nebo Cloudová služba nasazovací sloty). Privátní IP adresy přiřazeny dynamické metoda může změnit při restartování virtuálního počítače poté, co bylo v zastaveném stavu (deallocated). Privátní IP adresy se statickou metodou přiřazeny zůstane přiřazená k prostředku, až do odstranění prostředku. Pokud je potřeba zajistit, že privátní IP adresu pro prostředek nikdy změní až do odstranění prostředku, přiřadíte privátní IP adresy s statickou metodu.
* **Veřejná:** volitelně přiřazené k síťové adaptéry připojené k virtuální počítače nasazené prostřednictvím modelu nasazení Azure Resource Manager. Pomocí metody statické nebo dynamické přidělování lze přiřadit adresu. Všechny virtuální počítače a cloudové služby instance rolí, které jsou nasazené prostřednictvím modelu nasazení classic existovat v rámci cloudové služby, který je přiřazen *dynamické*, veřejné virtuální adresy IP (VIP). Veřejné *statické* IP adresu, názvem [vyhrazená IP adresa](virtual-networks-reserved-public-ip.md), volitelně může být přiřazen jako virtuální IP adresu. Veřejné IP adresy můžete přiřadit pro jednotlivé virtuální počítače nebo cloudové služby instance rolí, které jsou nasazené prostřednictvím modelu nasazení classic. Tyto adresy se nazývají [Instance úrovně veřejnou IP adresu (splnění](virtual-networks-instance-level-public-ip.md) adresy a lze dynamicky přiřadit.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Můžete vyhradit privátní IP adresy pro virtuální počítač, který vytvořím později?
Ne. Nelze rezervovat privátní IP adresy. Pokud je k dispozici privátní IP adresy bude jí přiřazeno k instanci virtuálního počítače nebo role Server DHCP. Tento virtuální počítač může nebo nemusí být ten, který chcete přiřadit k privátní IP adresu. Privátní IP adresa již vytvořené virtuální počítač můžete však změnit na libovolné dostupné privátní IP adresy.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Proveďte privátní měnit IP adresy pro virtuální počítače ve virtuální síti?
To záleží na okolnostech. Dynamické privátní IP adresy zůstat v případě virtuálních počítačů do jeho zastaveném (nepřiřazeném) nebo odstranit. Z virtuálního počítače nejsou vydané statickou privátní IP adresy, dokud nebude odstraněn.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>Může I ručně přiřadit IP adresy síťové adaptéry v rámci operačního systému virtuálního počítače?
Ano, ale není to doporučováno. Ruční změna IP adresy pro síťový adaptér v rámci operačního systému Virtuálního počítače může potenciálně vést k ke ztrátě připojení k virtuálnímu počítači, pokud změnit IP adresu přiřazené k síťovému adaptéru ve virtuálním počítači Azure.

### <a name="what-happens-to-my-ip-addresses-if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system"></a>Co se stane Moje IP adresy, je-li zabránit slot nasazení cloudové služby nebo vypnutí virtuálního počítače z operačního systému?
Nic. Adresy IP (veřejné VIP, veřejné a privátní) zůstanou zařazené do slotu nasazení cloudové služby nebo virtuálního počítače. Dynamické adresy vydávají pouze pokud je virtuální počítač je zastavena (deallocated) nebo odstraněn, nebo slotu nasazení cloudové služby se odstraní. Kliknutím **Zastavit** tlačítko pro virtuální počítač v rámci portálu Azure nastaví její stav na zastaveném (nepřiřazeném). Virtuální počítač v takovém případě dojde ke ztrátě jeho IP adresy.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Lze přesunout virtuální počítače z jedné podsítě do jiné podsítě ve virtuální síti bez opětovného nasazení?
Ano. Můžete najít další informace v [postup přesunutí virtuálních počítačů nebo role instance do jiné podsítě](virtual-networks-move-vm-role-to-subnet.md) článku.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Můžete nakonfigurovat statickou adresu MAC pro virtuální počítač?
Ne. Adresa MAC se nedá nakonfigurovat staticky.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Adresa MAC zůstane stejný pro virtuální počítač po jeho vytvoření?
Ano, adresa MAC zůstává stejná pro virtuální počítač nasadit prostřednictvím modelu nasazení classic i Resource Manager, dokud nebude odstraněn. Adresa MAC dříve, byla vydána, pokud virtuální počítač zastavila (deallocated), ale nyní je adresa MAC zachovány i v případě, že virtuální počítač je ve stavu deallocated.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Možné připojit k Internetu z virtuálního počítače ve virtuální síti?
Ano. Všechny virtuální počítače a cloudové služby instance rolí, které jsou nasazené v rámci virtuální sítě můžete připojit k Internetu.

## <a name="azure-services-that-connect-to-vnets"></a>Služby Azure, které se připojují k virtuálním sítím

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Můžete použít Azure App Service Web Apps s virtuální sítě?
Ano. Můžete nasadit webové aplikace uvnitř virtuální sítě pomocí App Service Environment (App Service Environment). Všechny webové aplikace můžete bezpečně připojit a přístup k prostředkům ve vaší virtuální síti Azure, pokud máte nakonfigurované pro virtuální síť připojení point-to-site. Další informace najdete v následujících článcích:

* [Vytvoření webové aplikace ve službě App Service Environment](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Integrace aplikace pomocí virtuální sítě Azure](../app-service/web-sites-integrate-with-vnet.md)
* [Integrace virtuální sítě a hybridních připojení pomocí webové aplikace](../app-service/web-sites-integrate-with-vnet.md#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Můžete nasadit cloudové služby s webových a pracovních rolí (PaaS) ve virtuální síti?
Ano. (Volitelně) můžete nasadit instance role cloudové služby v rámci virtuálních sítí. Uděláte to tak, zadejte název virtuální sítě a mapování role a podsítě v části Konfigurace sítě konfigurace služby. Není nutné aktualizovat všechny vaše binárních souborů.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-to-a-vnet"></a>Můžete připojit virtuálnímu počítači škálování nastavit (VMSS) k virtuální síti?
Ano. VMSS musí připojit k virtuální síti.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Můžete přesunout Moje služby do aplikace a z virtuální sítě?
Ne. Nelze přesunout služby do aplikace a z virtuální sítě. Je nutné odstranit a znovu nasadit službu, aby ho přesunout do jiné virtuální síti.

## <a name="security"></a>Zabezpečení

### <a name="what-is-the-security-model-for-vnets"></a>Co je model zabezpečení pro virtuální sítě?
Virtuální sítě jsou naprosto izolované od sebe navzájem a další služby hostované v infrastruktuře Azure. Virtuální síť je hranice vztahu důvěryhodnosti.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>Můžete omezit příchozí nebo odchozí přenosy na prostředky pro připojené virtuální sítě?
Ano. Můžete použít [skupin zabezpečení sítě](virtual-networks-nsg.md) pro jednotlivé podsítě v rámci virtuální sítě, síťové adaptéry připojené k virtuální síti, nebo obojí.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Můžete implementovat brány firewall mezi prostředky připojené virtuální sítě?
Ano. Můžete nasadit [virtuální zařízení brány firewall sítě](https://azure.microsoft.com/en-us/marketplace/?term=firewall) od více dodavatelů přes Azure Marketplace.

### <a name="is-there-information-available-about-securing-vnets"></a>Je k dispozici informace o zabezpečení virtuální sítě k dispozici?
Ano. Najdete v článku [Přehled zabezpečení sítě Azure](../security/security-network-overview.md) článku.

## <a name="apis-schemas-and-tools"></a>Rozhraní API, schémat a nástroje

### <a name="can-i-manage-vnets-from-code"></a>Můžete spravovat virtuální sítě z kódu?
Ano. Můžete použít rozhraní REST API pro virtuální sítě v [Azure Resource Manager](https://msdn.microsoft.com/library/mt163658.aspx) a [classic (správou služeb)](http://go.microsoft.com/fwlink/?LinkId=296833)) modely nasazení.

### <a name="is-there-tooling-support-for-vnets"></a>Je k dispozici podpora nástrojů pro virtuální sítě?
Ano. Další informace o používání:
- Portál Azure k nasazení virtuálních sítí pomocí [Azure Resource Manager](virtual-networks-create-vnet-arm-pportal.md) a [classic](virtual-networks-create-vnet-classic-pportal.md) modely nasazení.
- Prostředí PowerShell pro správu virtuálních sítí nasazené prostřednictvím [Resource Manager](/powershell/resourcemanager/azurerm.network/v3.1.0/azurerm.network.md) a [classic](/powershell/module/azure/?view=azuresmps-3.7.0) modely nasazení.
- [Rozhraní příkazového řádku Azure (CLI)](../virtual-machines/azure-cli-arm-commands.md#azure-network-commands-to-manage-network-resources) ke správě virtuálních sítí nasazené prostřednictvím obou modelů nasazení.  
