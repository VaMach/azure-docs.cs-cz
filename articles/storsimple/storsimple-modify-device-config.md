---
title: "Změna konfigurace zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak používat službu StorSimple Manager změna konfigurace zařízení StorSimple, která již byla nasazena."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: bb679264-8d46-429c-9ef7-630dc3b4a423
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: v-sharos
ms.openlocfilehash: 5bfbeb038dbedae2bf77016abbc19458c3dc22c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>Změňte konfiguraci zařízení StorSimple pomocí služby StorSimple Manager
## <a name="overview"></a>Přehled
Portál Azure classic **konfigurace** stránka obsahuje všechny parametry zařízení, které můžete změnit konfiguraci na zařízení StorSimple, který je spravovaný nástrojem služby StorSimple Manager. Tento kurz popisuje, jak můžete použít **konfigurace** můžete provádět následující úlohy úrovni zařízení:

* Upravit nastavení zařízení 
* Upravit nastavení času 
* Změna nastavení DNS 
* Upravit síťová rozhraní
* Swap – nebo změna přiřazení IP adresy

## <a name="modify-device-settings"></a>Upravit nastavení zařízení
Nastavení zařízení zahrnují popisný název zařízení a popis zařízení.

> [!NOTE] 
> Nelze upravit název zařízení na portálu Azure classic. Přejmenování zařízení není podporována.

Zařízení StorSimple, který je připojený ke službě StorSimple Manager je přiřazen výchozí název. Výchozí název odráží obvykle sériové číslo zařízení. Výchozí název zařízení, která je 15 znaků, jako je například 8600-SHX0991003G44HT, například označuje následující:

* **8600** – označuje model zařízení.
* **TVX** – označuje místo výroby.
* **0991003** -označuje konkrétní produkt.
* **G44HT**-posledních 5 číslic se zvýší, chcete-li vytvořit jedinečný sériová čísla. Toto nemusí být po sobě jdoucích sadu.

Můžete zadat popis zařízení. Popis zařízení obvykle pomáhá identifikovat vlastníka a fyzické umístění zařízení. Pole popisu musí obsahovat méně než 256 znaků.

## <a name="modify-time-settings"></a>Upravit nastavení času
Zařízení musí synchronizovat čas pro ověřování u vašeho poskytovatele služeb úložiště v cloudu. Vyberte časové pásmo z rozevíracího seznamu a zadejte až dva servery protokolu NTP (Network Time). Primární server NTP je vyžadován a je zadána při použití prostředí Windows PowerShell pro StorSimple pro konfiguraci zařízení. Můžete zadat výchozí nastavení systému Windows Server **time.windows.com** jako NTP server. Konfigurace primárního serveru NTP prostřednictvím portálu Azure classic můžete zobrazit, ale musíte použít rozhraní Windows PowerShell ho změnit.

Sekundární konfigurace serveru NTP je volitelné. Klasický portál můžete použít ke konfiguraci sekundární server NTP. 

Při konfiguraci serveru NTP, ujistěte se, že vaše síť umožňuje přenos dat NTP z vašeho datového centra na Internet. Při zadávání veřejného serveru NTP, musí se ujistěte, že vaše síťové brány firewall a dalších zařízení zabezpečení jsou nakonfigurované na povolit protokol NTP provoz do a z vnější síti. Pokud přenos dat NTP obousměrného není povolena, je nutné použít interní server NTP (řadič domény systému Windows poskytuje funkce). Pokud vaše zařízení nelze synchronizovat čas, nemusí být schopen komunikovat s poskytovatelem úložiště cloudu.

Chcete-li zobrazit seznam veřejné servery NTP, přejděte na [webové servery NTP](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Co se stane, když zařízení je nasazena v jiném časovém pásmu?
Pokud zařízení je nasazena v jiném časovém pásmu, změní se na časové pásmo zařízení. Vzhledem k tomu, že všechny zásady zálohování použít časové pásmo zařízení, budou automaticky upravit zásady zálohování v souladu s novým časovým pásmem. Není vyžadován žádný zásah uživatele.

## <a name="modify-dns-settings"></a>Změna nastavení DNS
DNS server se používá, když se zařízení pokusí o komunikaci se svým poskytovatelem služeb úložiště v cloudu. Pro vysokou dostupnost musíte nakonfigurovat primární a sekundární servery DNS při nasazení počáteční zařízení. Chcete-li překonfigurovat primární server DNS, budete muset použít rozhraní Windows PowerShell na zařízení StorSimple.

Pokud chcete upravit sekundární server DNS, můžete portál Azure classic.

## <a name="modify-network-interfaces"></a>Upravit síťová rozhraní
Vaše zařízení má šest zařízení síťových rozhraní, čtyři z nich jsou 1gbe a dvě z nich jsou 10 GbE. Tato rozhraní jsou označeny jako DATA 0 – DATA 5. DATA 0, 1 dat, DATA 4 a 5 dat jsou 1gbe, zatímco 10 GbE síťová rozhraní DATA 2 a DATA 3.

Konfigurace **nastavení síťového rozhraní** pro všechny rozhraní, který se má použít. Abyste zajistili vysokou dostupnost, doporučujeme mít aspoň dvě rozhraní iSCSI a dvě rozhraní povolenou podporu cloudu na zařízení. Jsme doporučujeme, ale nevyžadují zakázat nepoužívané rozhraní.

Když konfigurujete žádné síťových rozhraní, musíte nakonfigurovat virtuální IP (VIP).

Rozhraní DATA 0 má povolenou podporu cloudu ve výchozím nastavení. Při konfiguraci DATA 0, jsou také potřeba nakonfigurovat dva pevné IP adresy, jednu pro každý kontroler. Tyto pevné IP adresy lze použít pro přístup k řadiče zařízení přímo a jsou užitečné, když instalujete aktualizace na zařízení nebo při přístupu k řadičům pro účely odstraňování potíží.

V zařízení StorSimple 8000 řady Update 1, metriky směrování dat 0 nastavena na nejnižší; Proto pokud vaše zařízení používá StorSimple 8000 řady Update 1, všechny přenosy dat cloudové budou směrovány přes DATA 0. Poznamenejte si tohoto Pokud máte více než jedno povolenou podporu cloudu síťové rozhraní na zařízení StorSimple.

> [!NOTE]
> Pevné IP adresy pro řadič slouží k doručování aktualizací do zařízení. Pevné IP adresy proto musí být směrovatelné a schopné připojení k internetu.
> 
> 

Pro každé síťové rozhraní zobrazí se následující parametry:

* **Rychlost** – není uživatelsky konfigurovatelného parametr. DATA 0, 1 dat, DATA 4 a 5 dat jsou vždy 1 GbE, zatímco 10 GbE rozhraní DATA 2 a DATA 3.
  
  > [!NOTE]
  > Rychlost a duplexní režim jsou vždy automaticky vyjednal. Rámce typu Jumbo nejsou podporovány.
  > 
  > 
* **Stav rozhraní** – rozhraní můžete povolit nebo zakázat. Pokud je povoleno, zařízení se pokusí použít rozhraní. Doporučujeme, aby byl povolen pouze rozhraní, které jsou připojené k síti a použít. Zakažte všechny rozhraní, které nepoužíváte.
* **Typ rozhraní** – tento parametr umožňuje izolovat přenosy iSCSI od provozu cloudu úložiště. Tento parametr může být jedna z následujících akcí:
  
  * **Cloud povolené** – Pokud je povoleno, zařízení bude používat toto rozhraní ke komunikaci s cloudem.
  * **iSCSI povoleno** – Pokud je povoleno, zařízení bude používat toto rozhraní ke komunikaci s hostitelem iSCSI.
    
    Doporučujeme, abyste izolovat přenosy iSCSI od provozu cloudu úložiště. Všimněte si, že pokud je váš hostitel ve stejné podsíti jako zařízení, není potřeba přiřadit bránu; ale pokud váš hostitel je v jiné podsíti, než zařízení, musíte přiřadit bránu.
* **IP adresa** – to může být IPv4 nebo IPv6 nebo obojí. Rodiny adres IPv4 a IPv6 jsou podporovány pro síťová rozhraní zařízení. Při použití protokolu IPv4, zadejte IP adresu 32-bit (*xxx.xxx.xxx.xxx*) v notaci tečkou decimal. Pokud používáte IPv6, jednoduše zadejte předponu 4 číslice a adresu 128-bit budou automaticky generovány pro vaše zařízení síťové rozhraní na základě této předpony.
* **Podsíť** – to odkazuje na masku podsítě a je nakonfigurován pomocí rozhraní Windows PowerShell.
* **Brána** – to je výchozí brána, který se použije tímto rozhraním při jeho pokusu o komunikaci s uzly, které nejsou v rámci stejné adresní prostor IP adres (podsítě). Výchozí brány musí být stejné adresního prostoru (podsítě) jako rozhraní IP adresu, určeného masku podsítě.
* **Pevná IP adresa** – toto pole je dostupné pouze tehdy, když nakonfigurujete DATA 0 rozhraní. Pro operace, například aktualizace nebo řešení potíží s zařízení musíte připojit přímo k řadiči zařízení. Pevnou IP adresu můžete použít pro přístup k aktivní a pasivní řadiče na vašem zařízení.

Prostřednictvím portálu Azure classic můžete znovu nakonfigurovat řadič 0 a řadič 1.

> [!NOTE]
> * Chcete-li zajistit správnou funkci, ověřte rychlost rozhraní a duplexní režim na přepínači, která je každé rozhraní zařízení připojena k. Přepínač rozhraní by měl buď vyjednat s nebo se dá nakonfigurovat pro adaptéry Gigabit Ethernet (1000 Mb/s) a být plně duplexní. Rozhraní pracující na nižší rychlost nebo v poloduplexní způsobí problémy s výkonem.
> * Chcete-li minimalizovat narušení a výpadky, doporučujeme, abyste povolili portfast na všechny porty přepínače, které síťové rozhraní iSCSI vašeho zařízení se připojují k. Tím bude zajištěno, že v případě selhání můžete rychle vytvořit připojení k síti.
> 
> 

## <a name="swap-or-reassign-ips"></a>Swap – nebo změna přiřazení IP adresy
V současné době Pokud žádné síťové rozhraní na řadiči je přiřazenou VIP, která je používána (ve stejném zařízení nebo jiné zařízení v síti), pak řadičem bude převzetí služeb při selhání. Proto je nutné správné postup použít v případě, že jsou vzájemná záměna virtuální IP adresy pro síťové rozhraní zařízení, protože se vytvoří duplicitní IP situaci.

Proveďte následující kroky pro odkládacího souboru nebo změnu přiřazení virtuální IP adresy pro některý z rozhraní sítě:

#### <a name="to-reassign-ips"></a>Přiřazení IP adresy
1. Zrušte IP adresu pro obě rozhraní.
2. Po IP adresy jsou vymazány, přiřaďte nové IP adresy do příslušných rozhraní.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [konfigurace funkce MPIO pro zařízení StorSimple](storsimple-configure-mpio-windows-server.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

