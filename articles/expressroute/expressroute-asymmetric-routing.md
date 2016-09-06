<properties
   pageTitle="Asymetrické směrování | Microsoft Azure"
   description="Tento článek vás provede problémy, se kterými se může setkat zákazník s asymetrickým směrováním v síti, pokud má víc propojení k cíli."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="osamazia"/>

# Asymetrické směrování s několika síťovými cestami

Tento článek vysvětluje, jak může dopředný a zpětný provoz využívat různé trasy, pokud je mezi zdrojem a cílem několik dostupných cest.

Pro lepší pochopení asymetrického směrování se nejdřív musíte seznámit se dvěma základními koncepty. Prvním je dopad existence několika síťových cest. Druhým je chování zařízení, která zachovávají stav, jako jsou brány firewall. Pro tato zařízení se používá označení stavová zařízení. Kombinace těchto dvou faktorů vytváří situace, kdy stavové zařízení zahodí přenos, protože zahájení přenosu nešlo přes ně.

## Několik síťových cest

Pokud má podniková síť jenom jedno propojení k internetu prostřednictvím poskytovatele internetových služeb, veškerý provoz směrem do a z internetu prochází stejnou cestu. Často si ale společnosti kupují víc okruhů, které tvoří redundantní cesty a zlepšují dostupnost sítě. V takových případech je možné, že provoz směřující ze sítě do internetu využívá jedno propojení, zatímco zpětný provoz prochází jiným propojením. Tento jev se běžně označuje jako asymetrické směrování, kdy reverzní provoz používá jinou cestu než původní tok.

![Směrování3](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Ačkoli předchozí popis se týká internetu, platí taky pro jiné kombinace více cest. Jako příklad slouží přístup na stejné místo přes internet i soukromou cestu, přes několik soukromých cest atd. 

Každý směrovač na cestě ze zdrojového do cílového umístění vypočítává nejlepší cestu k dosažení cíle na základě svého výpočtu nejlepší cesty. Stanovení nejlepší možné cesty je založené na dvou hlavních faktorech.

1.  Směrování mezi externími sítěmi je založené na směrovacím protokolu označovaném BGP (Border Gateway Protocol). Protokol BGP přijímá inzerování z okolí, provádí s nimi řadu kroků, které mají určit nejlepší cestu do cílového umístění, a umisťuje je do své směrovací tabulky.
2.  Délka masky podsítě přidružené trase. Pokud je přijato víc inzerování pro stejnou adresu, ale s různými maskami podsítě, preferuje se inzerování s delší maskou podsítě, protože se považuje za konkrétnější trasu.

## Stavová zařízení

Směrovače si pro účely směrování čtou hlavičku protokolu IP paketu. Jsou ale zařízení, která se dívají i hlouběji do paketu. Obvykle se tato zařízení dívají na hlavičky vrstvy 4 (TCP/UDP) nebo dokonce vrstvy 7 (aplikační vrstva). Tato zařízení jsou buď zařízení zabezpečení, nebo zařízení optimalizace šířky pásma. Brána firewall je obvyklým příkladem stavového zařízení. Brána firewall povoluje nebo zakazuje průchod paketu svým rozhraním na základě různých polí, jako je protokol, port TCP/UDP nebo hlavička adresy URL. Tato kontrola paketu klade značné nároky na zatížení zařízení. Aby se výkon zlepšil, brána firewall kontroluje první paket toku. Pokud je paket povolen, uchová si informace o toku ve své stavové tabulce. Všechny následné pakety týkající se tohoto toku jsou na základě počátečního rozhodnutí povoleny. Takže když na bránu firewall dorazí paket, který je součástí existujícího toku, a brána firewall o něm nemá žádné předchozí stavové informace, brána firewall takový paket zahodí.

## Asymetrické směrování s ExpressRoute

Když se připojíte k Microsoftu prostřednictvím ExpressRoute, dojde ve vaší síti k následujícím změnám.

1.  Máte k Microsoftu víc propojení. Jedno propojení je vaše stávající připojení k internetu a druhé je prostřednictvím ExpressRoute. Nějaký provoz do Microsoftu může jít prostřednictvím internetu, ale vracet se prostřednictvím ExpressRoute, nebo naopak.
2.  Prostřednictvím ExpressRoute dostáváte mnohem konkrétnější IP adresy. Takže provoz z vaší sítě do Microsoftu pro služby nabízené prostřednictvím ExpressRoute vždycky preferuje ExpressRoute. 

Abyste lépe pochopili vliv uvedených faktů, projdeme si několik scénářů. Předpokládejme, že máte jenom jeden okruh k internetu a že využíváte všechny služby Microsoftu prostřednictvím internetu. Provoz z vaší sítě do Microsoftu a zpátky prochází stejným internetovým propojením a přes bránu firewall. Brána firewall zaznamená tok, jakmile uvidí první paket, a návratové pakety jsou povolené, protože tok existuje ve stavové tabulce.

![Směrování1](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)


Nyní zapnete ExpressRoute a začnete využívat služby nabízené Microsoftem prostřednictvím ExpressRoute. Všechny ostatní služby z Microsoftu se využívají přes internet. Na hranici připojení k ExpressRoute nasadíte samostatnou bránu firewall. Microsoft bude do vaší sítě přes ExpressRoute inzerovat pro určité služby konkrétnější předpony. Vaše infrastruktury směrování zvolí jako upřednostňovanou cestu pro tyto předpony ExpressRoute. Pokud neinzerujete vaše veřejné IP adresy Microsoftu přes ExpressRoute, Microsoft bude komunikovat s vašimi veřejnými IP adresami přes internet. Takže provoz z vaší sítě do Microsoftu bude používat ExpressRoute, ale zpětný provoz z Microsoftu použije internet. Když brána firewall na hranici uvidí paket odezvy pro tok, který nenajde ve stavové tabulce, návratový přenos zahodí. 

Pokud zvolíte použití stejného fondu NAT pro ExpressRoute a pro internet, bude docházet k obdobným problémům i s klienty na privátních IP adresách ve vaší síti. Žádosti o služby, jako je například Windows Update, půjdou přes internet, protože IP adresy pro tyto služby se neinzerují prostřednictvím ExpressRoute. Návratový přenos se ale bude vracet přes ExpressRoute. Pokud Microsoft obdrží IP adresu se stejnou maskou podsítě z internetu i ExpressRoute, upřednostňuje ExpressRoute před internetem. Pokud brána firewall nebo jiné stavové zařízení na hranici vaší sítě které je připojené k ExpressRoute, nemá žádné předchozí informace o toku, zahodí pakety, které do tohoto toku patří. 

## Řešení asymetrického směrování

Existují dva hlavní způsoby řešení problému asymetrického směrování. Jeden je prostřednictvím směrování a druhý prostřednictvím překladu adres na základě zdroje (SNAT). 

1. Směrování 

    - Měli byste zajistit, aby vaše veřejné IP adresy byly inzerované na příslušná připojení WAN. Například když chcete použít internet pro provoz ověřování a ExpressRoute pro provoz vašeho e-mailu. Potom ale nesmíte inzerovat vaše veřejné IP adresy služby AD FS přes ExpressRoute. Obdobně místní server AD FS nesmí být vystaven IP adresám přijímaným přes ExpressRoute. Trasy přijímané přes ExpressRoute jsou konkrétnější, takže se ExpressRoute stane upřednostňovanou cestou pro provoz ověřování do Microsoftu, což způsobuje asymetrické směrování.

    - Pokud chcete použít ExpressRoute pro ověřování, musíte se ujistit, že inzerujete veřejné IP adresy služby AD FS prostřednictvím ExpressRoute bez překladu adres. Tímto způsobem přenos pocházející z Microsoftu do místního serveru AD FS jde přes ExpressRoute, zatímco návratový přenos od zákazníka do Microsoftu bude používat ExpressRoute, protože se upřednostňuje před internetem. 

2. Překlad adres na základě zdroje

    Jiný způsob řešení problémů asymetrického směrování je prostřednictvím překladu adres na základě zdroje (SNAT). Pokud jste například neinzerovali veřejnou IP adresu místního serveru SMTP přes ExpressRoute s úmyslem použít pro tuto komunikaci internet. Požadavek pocházející z Microsoftu na váš místní server SMTP prochází internetem. Provedete u příchozího požadavku překlad adresy zdroje na vnitřní IP adresu. Zpětná přenos ze serveru SMTP půjde na hraniční bránu firewall (která se používá pro překlad adres), a ne na ExpressRoute. Tímto způsobem se návratový přenos vrátí zpátky pomocí internetu. 


![Směrování2](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## Detekce asymetrického směrování

Nejlepším způsobem, jak zajistit, že provoz prochází očekávanou cestou, je traceroute. Pokud budete chtít, aby provoz z vašeho místního serveru SMTP do Microsoftu procházel internetem, proveďte příkaz traceroute ze serveru SMTP na Office 365. Výsledek ověří, že provoz skutečně opouští vaši síť směrem k internetu, a ne směrem k ExpressRoute. 





<!--HONumber=ago16_HO5-->


