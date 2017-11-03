---
title: "Přehled služby Vyrovnávání zatížení Azure | Microsoft Docs"
description: "Přehled funkce Vyrovnávání zatížení Azure, architekturu a implementace. Zjistěte, jak funguje nástroj pro vyrovnávání zatížení a využít v cloudu."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 0f313dc0-f007-4cee-b2b9-f542357925a3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: ecf1fc38d2b9fd54fe5b00db616224a0848179fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-load-balancer-overview"></a>Azure Load Balancer – přehled

Azure Load Balancer zajišťuje vysokou dostupnost a výkon sítě pro vaše aplikace. Je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která distribuuje příchozí komunikaci mezi pořádku instancí služby definované v sadě s vyrovnáváním zatížení.

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure nástroj pro vyrovnávání zatížení můžete nakonfigurovat:

* Načtěte vyrovnávání příchozí přenosy z Internetu k virtuálním počítačům. Tato konfigurace se označuje jako [Vyrovnávání zatížení internetového](load-balancer-internet-overview.md).
* Přenosy Vyrovnávání zatížení mezi virtuálními počítači ve virtuální síti, mezi virtuálními počítači v cloudové služby nebo mezi místními počítači a virtuální počítače ve virtuální síti mezi různými místy. Tato konfigurace se označuje jako [Vyrovnávání zatížení pro vnitřní](load-balancer-internal-overview.md).
* Předat dál externích přenosů pro konkrétní virtuální počítač.

Všechny prostředky v cloudu potřebovat veřejnou IP adresu, která být dosažitelný z Internetu. Infrastruktura cloudu v Azure používá směrovat IP adresy pro její prostředky. Azure používá překlad síťových adres (NAT) s veřejné IP adresy pro komunikaci k Internetu.

## <a name="load-balancer-features"></a>Funkce nástroje pro vyrovnávání zatížení

* Na základě hodnoty hash distribuce

    Azure nástroj pro vyrovnávání zatížení používá algoritmus hash na základě distribuce. Ve výchozím nastavení použije hodnotu hash 5 řazené kolekce členů tvořený zdrojové IP adresy, zdrojového portu, cílové adresy IP, cílový port a typ protokolu mapovat provoz dostupné servery. Poskytuje věrnosti pouze *v rámci* relace přenosu. Pakety ve stejné relaci TCP nebo UDP se přesměruje na stejnou instanci za koncový bod Vyrovnávání zatížení sítě. Pokud klient zavře a znovu otevře připojení nebo spustí novou relaci ze stejné zdrojové IP adresy, zdrojového portu se změní. To může způsobit provoz přejít na jiný koncový bod v jiném datovém centru.

    Další podrobnosti najdete v tématu [režim distribuce nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md). Následující obrázek znázorňuje rozdělení na základě hodnoty hash:

    ![Na základě hodnoty hash distribuce](./media/load-balancer-overview/load-balancer-distribution.png)

    Obrázek - Hash na základě distribuce

* Přesměrování portu

    Azure poskytuje nástroj pro vyrovnávání zatížení můžete řídit, je spravovaný přes jak příchozí komunikaci. Tato komunikace zahrnuje provozu iniciované z hostitele Internet, virtuální počítače v jiných cloudových služeb nebo virtuálních sítí. Tento ovládací prvek je reprezentována koncový bod (také nazývané vstupní koncový bod).

    Vstupní koncový bod naslouchá na veřejný port a předává přenos pro interní port. Můžete namapovat stejné porty pro koncový bod interních nebo externích nebo použít jiný port pro ně. Například můžete mít webový server nakonfigurován pro naslouchání na portu 81 při mapování veřejný koncový bod je port 80. Vytvoření veřejný koncový bod se aktivuje vytvoření instance služby Vyrovnávání zatížení.

    Při použití portálu Azure, portálu automaticky vytvoří koncové body k virtuálnímu počítači pro protokol RDP (Remote Desktop) a vzdálenou komunikaci relace prostředí Windows PowerShell. Tyto koncové body můžete použít ke vzdálené správě virtuálního počítače přes Internet.

* Automatické překonfigurování

    Azure nástroj pro vyrovnávání zatížení okamžitě automatických při změně velikosti instancí nahoru nebo dolů. Například tuto změnu konfigurace se stane, když zvýšíte počet instancí pro web/role pracovního procesu v cloudové službě, nebo při přidání dalších virtuálních počítačů do stejné sady vyrovnáváním zatížení.

* Monitorování služeb

    Azure nástroj pro vyrovnávání zatížení můžete testovat stav různých instancí serveru. Když sondu přestane reagovat, nástroje pro vyrovnávání zatížení zastaví odesílání nových připojení k instancím není v pořádku. Nejsou ovlivněná existující připojení.

    Jsou podporovány tři typy sondy:

    + **Test agenta hosta (na platformě jako služby pouze ve virtuálních počítačích):** nástroje pro vyrovnávání zatížení využívá agent hosta ve virtuálním počítači. Agent hosta, který přijímá a odpoví odpověď HTTP 200 OK jenom v případě, že instance je ve stavu připraveno (tj. instance není ve stavu jako o volném čase, recyklace nebo ukončení). Když agent přestane reagovat s 200 OK protokolu HTTP, nástroje pro vyrovnávání zatížení označí instance jako reagovat a zastaví odesílání provozu do této instance. Nástroje pro vyrovnávání zatížení bude nadále příkazem ping otestovat instance. Pokud agenta hosta odpoví HTTP 200, bude nástroj pro vyrovnávání zatížení znovu odeslat provoz tuto instanci. Pokud používáte webovou roli, kód webu obvykle běží v w3wp.exe, který není monitorován pomocí Azure prostředků infrastruktury nebo hostovaného agenta. To znamená, že selhání v w3wp.exe (např. odpovědi HTTP 500), nebudou ohlášena v agentovi hosta a nástroje pro vyrovnávání zatížení nebude vědět o trvat tuto instanci mimo otočení.
    + **Vlastní test paměti HTTP:** tento test přepíše výchozí kontroly (agenta hosta). Můžete ho vytvořit vlastní logiky k určení stavu role instance. Nástroje pro vyrovnávání zatížení bude pravidelně testů váš koncový bod (každých 15 sekund, ve výchozím nastavení). Instance považován za v otočení, pokud ji odpoví TCP ACK nebo HTTP 200 během časového limitu (výchozí hodnoty 31 sekund). To je užitečné pro implementaci vlastní logiky k odebrání instance otočení Vyrovnávání zatížení. Můžete například nakonfigurovat instanci systému na vrátit stav bez 200, pokud je instance vyšší než 90 % využití procesoru. Pro webové role, které používají w3wp.exe, můžete také získat automatické monitorování vašeho webu, protože selhání ve vašem kódu webu vrátit stav bez 200 do sondy.
    + **Vlastní test paměti TCP:** tento test spoléhá na úspěšné vytvoření relace TCP port definované testu.

    Další informace najdete v tématu [LoadBalancerProbe schématu](https://msdn.microsoft.com/library/azure/jj151530.aspx).

* Zdroj NAT

    Všechny odchozí přenosy na Internet, která pochází z vaší služby projde zdroj NAT (SNAT) pomocí stejné adresy VIP jako příchozí přenosy. Překládat pomocí SNAT poskytuje důležité výhody:

    + Umožňuje snadno upgradu a zotavení po havárii služeb, od VIP může dynamicky mapovat k jiné instanci služby.
    + Ho usnadňuje správu seznamu ACL řízení přístupu. Seznamy ACL, vyjádřené jako virtuální IP adresy neměňte jako služby škálování nahoru, dolů nebo získat znovu nasazena.

    Konfigurace služby Vyrovnávání zatížení podporuje úplné kuželový NAT pro UDP. Úplné kuželový NAT je typ zařízení NAT, kde port umožňuje příchozí připojení z jakékoli externího hostitele (v odpovědi na požadavek odchozí).

    Pro každý nový odchozí připojení, které zahájí virtuálního počítače je přidělena odchozí port taky nástroje pro vyrovnávání zatížení. Externího hostitele vidí provoz s virtuální port přidělené virtuální IP adresy IP. Pro scénáře, které vyžadují hodně odchozí připojení, doporučuje se použít [veřejná IP adresa na úrovni instance](../virtual-network/virtual-networks-instance-level-public-ip.md) adresy tak, aby virtuální počítače odchozí vyhrazenou IP adresu pro překládat pomocí SNAT. Tím se snižuje riziko vyčerpání portu.

    Najdete v tématu [odchozí připojení](load-balancer-outbound-connections.md) článku Další podrobnosti k tomuto tématu.

### <a name="support-for-multiple-load-balanced-ip-addresses-for-virtual-machines"></a>Podpora pro více Vyrovnávání zatížení sítě IP adres pro virtuální počítače
Sada virtuálních počítačů můžete přiřadit více než jeden Vyrovnávání zatížení veřejnou IP adresu. Se tato možnost může být hostitelem více webů SSL nebo více naslouchací procesy skupiny dostupnosti AlwaysOn systému SQL Server na stejnou sadu virtuálních počítačů. Další informace najdete v tématu [několika virtuálními IP adresami za cloudové služby](load-balancer-multivip.md).

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="limitations"></a>Omezení

Fondy back-end pro vyrovnávání zatížení může obsahovat všechny SKU virtuálních počítačů s výjimkou základní vrstvě.

## <a name="next-steps"></a>Další kroky

- Další informace o [internetového nástroj pro vyrovnávání zatížení](load-balancer-internet-overview.md)

- Další informace o [přehled nástroje pro vyrovnávání zatížení interní](load-balancer-internal-overview.md)

- Vytvoření [internetového nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-portal.md)

- Přečtěte si o některých dalších klíče [sítě možnosti](../networking/networking-overview.md) Azure

