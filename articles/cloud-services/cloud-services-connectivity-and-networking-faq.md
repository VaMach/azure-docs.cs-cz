---
title: "Problémy s připojením a sítě pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy o připojení a sítě pro Microsoft Azure Cloud Services."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: genli
ms.openlocfilehash: 7b435b6904b05228a63e3ed3a9fed78747b843c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problémy s připojením a sítě pro Azure Cloud Services: Časté otázky (FAQ)

Tento článek obsahuje nejčastější dotazy týkající se problémů s připojením a sítě pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Můžete také obrátit [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I nelze rezervovat IP adresy v cloudové službě více virtuálních IP adres
Zkontrolujte, jestli je zapnutá instanci virtuálního počítače, který se pokoušíte rezervovat IP adresu pro. Poté se ujistěte, že používáte vyhrazené IP adresy pro pracovní a provozní nasazení. **Nechcete** změnit nastavení při nasazení je upgradu.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Jak mohu vzdálené plochy Pokud skupinu NSG?
Přidat pravidla k této skupině, která povolí komunikaci na portech **3389** a **20000**.  Vzdálená plocha používá port **3389**.  Instance cloudové služby jsou Vyrovnávané, takže nemůže přímo řídit kterou instanci pro připojení k.  *RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a umožňují klientu odesílat soubor cookie s RDP a zadejte jednotlivé instance pro připojení k.  *RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000** otevřít, což může být zablokován, pokud máte skupinu NSG.

## <a name="can-i-ping-a-cloud-service"></a>Můžete příkaz ping Cloudová služba?

Ne, nikoli pomocí normální "ping" / protokolu ICMP. Protokol ICMP, nemá oprávnění prostřednictvím nástroje pro vyrovnávání zatížení Azure.

K testování připojení, doporučujeme, abyste provedli port ping. Zatímco Ping.exe používá ICMP, jiné nástroje, jako je například Pspingu, Nmap a telnet umožňují test připojení na určitém portu TCP.

Další informace najdete v tématu [používat příkazy ping portu místo ICMP k testování připojení virtuálního počítače Azure](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-to-the-cloud-service"></a>Jak zabráním přijetí tisíc přístupů z neznámého IP adres, které indikují nějaká napadením se zlými úmysly do cloudové služby?
Azure implementuje zabezpečení s více vrstvami sítě k ochraně jeho služby platformy proti útokům (DDoS) distribuované denial of service. V systému Azure DDoS obrany je součástí Azure nepřetržité monitorování procesu, který je neustále zlepšit prostřednictvím průnikům testování. Tento systém obrany DDoS je určena pro odolat nejen útokům zvenku, ale taky od ostatních klientů Azure. Další podrobnosti naleznete v [zabezpečení sítě Microsoft Azure](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Můžete také vytvořit úlohu spuštění pro selektivní blokování některé konkrétní IP adresy. Další informace najdete v tématu [blokovat konkrétní IP adresu](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Při pokusu protokolu RDP pro instance Moje cloudové služby se zobrazí zpráva, "vypršela platnost uživatelského účtu."
Při vyřazení datum vypršení platnosti, který je nakonfigurovaný v nastavení protokolu RDP, může se zobrazit chybová zpráva "Tento uživatelský účet vypršela". Datum vypršení platnosti z portálu můžete změnit pomocí následujících kroků:
1. Přihlaste se ke konzole pro správu Azure (https://manage.windowsazure.com), přejděte do cloudové služby a vyberte **konfigurace** kartě.
2. Vyberte **vzdáleného**.
3. Změňte na datum "Vyprší dne" a pak konfiguraci uložte.

Nyní nyní byste měli mít pro připojení RDP k vašemu počítači.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Proč není nástroj pro vyrovnávání zatížení vyrovnávání přenosů stejně?
Informace o tom, jak interní funguje nástroj pro vyrovnávání zatížení najdete v tématu [nový režim distribuce Vyrovnávání zatížení Azure](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

Jako distribuční algoritmus používá je 5-n-tice (zdrojové IP adresy, zdrojového portu, cílové adresy IP, cílový port, protokol typu) hash k mapování provoz na dostupné servery. Poskytuje věrnosti pouze v rámci relace přenosu. Pakety ve stejné relaci TCP nebo UDP se přesměruje na stejnou instanci datacenter IP (DIP) za vyrovnáváním zatížení koncového bodu. Když klient zavře a znovu otevře připojení nebo spustí novou relaci ze stejné zdrojové IP adresy, zdrojového portu změny a způsobí, že provoz přejít k jinému koncovému bodu vyhrazené IP adresy.

## <a name="how-can-i-redirect-the-incoming-traffic-to-my-default-url-of-cloud-service-to-a-custom-url"></a>Jak můžete přesměrovat příchozí přenosy na můj výchozí adresa URL cloudové služby na vlastní adresu URL? 

Adresu URL přepisování modulu IIS může přesměrovat provoz přicházející do výchozí adresa URL pro cloudové služby (například \*. cloudapp.net) pro některé vlastní název nebo adresa URL služby DNS. Vzhledem k tomu, že modul přepisování adres URL je ve výchozím nastavení zapnuta webové role a její pravidla jsou nakonfigurovány v souboru web.config aplikace, vždycky je k dispozici pro virtuální počítač bez ohledu na restartování nebo reimages. Další informace naleznete v tématu:

- [Modul pro přepisování vytváření pravidel přepisování pro adresu URL](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Postup odebrání výchozí odkaz](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-the-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Jak můžete I bloku nebo zakázat příchozí přenosy na adresu URL výchozí Moje cloudové služby? 

Příchozí provoz může zabránit výchozí adresa URL nebo název cloudové služby (například \*. cloudapp.net) nastavením hlavičku hostitele pro vlastní název DNS (například www. MyCloudService.com) v části Konfigurace vazeb webu v cloudu soubor definice služby (*.csdef), které je uvedené níže: 
 

    <?xml version="1.0" encoding="utf-8"?> 
    <ServiceDefinition name="AzureCloudServicesDemo" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6"> 
      <WebRole name="MyWebRole" vmsize="Small"> 
        <Sites> 
          <Site name="Web"> 
            <Bindings> 
              <Binding name="Endpoint1" endpointName="Endpoint1" hostHeader="www.MyCloudService.com" /> 
            </Bindings> 
          </Site> 
        </Sites> 
        <Endpoints> 
          <InputEndpoint name="Endpoint1" protocol="http" port="80" /> 
        </Endpoints> 
        <ConfigurationSettings> 
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" /> 
        </ConfigurationSettings> 
      </WebRole> 
    </ServiceDefinition> 
 
Jako tuto vazbu hlavičky hostitele se vynucuje prostřednictvím csdef souborů, služba by pouze být přístupné přes vlastní název 'www.MyCloudService.com', zatímco všechny příchozí požadavky na "*. cloudapp.net' domény by vždy selže. Blokování výchozí adresa URL nebo název služby, který uvedená, pokud se ale použít vlastní test paměti SLB nebo interní nástroj v rámci služby, mohou ovlivňovat testování chování. 

## <a name="how-to-make-sure-the-public-facing-ip-address-of-a-cloud-service-aka-vip-never-changes-so-that-it-could-be-customarily-whitelisted-by-few-specific-clients"></a>Postup, ujistěte se, že přístupných veřejnou IP adresu cloudové služby (neboli, VIP) nikdy změní tak, že může být infrastruktura seznam povolených adres několik konkrétní klienty?

Pro vytvoření seznamu povolených IP adresu z vašich cloudových služeb doporučujeme, abyste měli vyhrazená IP adresa přidružená jinak, které virtuální IP adresy poskytované platformou Azure bude zrušeno ze svého předplatného, pokud odstraníte nasazení. Upozorňujeme, že pro úspěšné operace prohození virtuální IP adresy potřebovali byste jednotlivé vyhrazené IP adresy pro produkční a pracovní sloty, bez kterých prohození se nezdaří. Postupujte podle těchto článcích rezervovat IP adresu a přidružte ji k vašim Cloudovým službám:  
 
- [Rezervovat IP adresu existující cloudové služby](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Pomocí konfigurační soubor služby přidružit vyhrazenou IP adresu do cloudové služby](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file) 

Tak dlouho, dokud máte více než jedna instance pro role, přidružení RIP s Cloudovou službou by nemělo způsobit výpadek. Alternativně můžete vytvořit bílou rozsah IP adres vašeho datového centra Azure. Můžete najít všechny rozsahy Azure IP [zde](https://www.microsoft.com/en-us/download/details.aspx?id=41653). 

Tento soubor obsahuje rozsahy IP adres (včetně rozsahů pro Compute, SQL a službu Storage), které se používají v datacentrech Microsoft Azure. Každý týden se zveřejňuje aktualizovaný soubor odrážející aktuálně nasazené rozsahy a všechny nadcházející změny rozsahů IP adres. Rozsahy, které jsou v souboru uvedeny nově, se v datacentrech nebudou používat ještě minimálně týden. Každý týden si stáhněte nový soubor XML a proveďte na svém webu potřebné změny pro zajištění správné identifikace služeb spuštěných v Azure. Uživatelé ExpressRoute si můžou všimnout, že se tento soubor používá k aktualizaci inzerování prostoru Azure pomocí protokolu BGP, která probíhá první týden každého měsíce. 

## <a name="how-can-i-use-azure-resource-manager-vnets-with-cloud-services"></a>Jak můžete použít virtuální sítě Azure Resource Manager s cloudovými službami? 

Cloudové služby nelze umístit do virtuální sítě Azure Resource Manager, ale mohou být připojeny virtuálním sítím Azure Resource Manager a virtuální sítě Classic prostřednictvím partnerského vztahu. Další informace najdete v tématu [partnerský vztah virtuální sítě](../virtual-network/virtual-network-peering-overview.md).