<properties
   pageTitle="Resetování služby Azure VPN Gateway | Microsoft Azure"
   description="Tento článek vás provede procesem resetování služby Azure VPN Gateway. Tento článek se týká bran VPN vytvořených pomocí modelu nasazení Classic."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="cherylmc"/>

# Resetování služby Azure VPN Gateway pomocí prostředí PowerShell


Tento článek vás provede procesem resetování služby Azure VPN Gateway pomocí rutin prostředí PowerShell. Tyto pokyny platí pro model nasazení Classic.  Zatím nemáme rutinu nebo rozhraní REST API umožňující resetování brány VPN pro virtuální sítě vytvořené pomocí modelu nasazení Resource Manager. Na těch se pracuje. Jestli byla vaše brána vytvořena pomocí modelu nasazení Classic zjistíte zobrazením svojí virtuální sítě na webu Azure Portal. Virtuální sítě vytvořené pomocí modelu nasazení Classic jsou uvedeny v části Virtuální sítě (klasické) na webu Azure Portal.

Resetování služby Azure VPN Gateway je užitečné v případě ztráty propojování více míst na jednom nebo více tunelových propojeních VPN typu Site-to-Site. V této situaci vaše místní zařízení VPN fungují správně, ale nejsou schopná vytvořit tunelová propojení prostřednictvím protokolu IPsec s branami Azure VPN. Když použijete rutinu *Reset-AzureVNetGateway*, brána se restartuje, a poté se na ni znovu použije konfigurace pro více míst. Brána si ponechá stejnou veřejnou IP adresu. To znamená, že nebudete muset aktualizovat konfiguraci směrovače VPN na novou veřejnou IP adresu brány Azure VPN.  


Před restartováním brány ověřte pro každé tunelové propojení VPN typu Site-to-Site (S2S) s protokolem IPsec klíčové body následujícího seznamu. Jakákoli neshoda v těchto bodech způsobí odpojení tunelových propojení VPN typu S2S. Ověřením a opravením konfigurace pro brány místní VPN a brány Azure VPN předejdete zbytečnému resetování a přerušení ostatních fungujících připojení na těchto branách.

Před resetováním brány ověřte následující body.

- Internetové IP adresy (virtuální IP adresy) pro bránu Azure VPN a bránu místní VPN jsou správně nakonfigurovány v zásadách Azure VPN i v zásadách místní VPN.
- Předsdílený klíč musí být stejný v bráně Azure VPN i v bráně místní VPN.
- Pokud použijete určitou konfiguraci protokolu IPsec/IKE, jako například šifrování, algoritmy hash nebo metodu Perfect Forward Secrecy (PFS), ujistěte se, že je stejně nakonfigurovaná brána Azure VPN i brána místní sítě.


## Resetování služby VPN Gateway pomocí prostředí PowerShell

Rutina prostředí PowerShell pro resetování brány Azure VPN je *Reset-AzureVNetGateway*. Každá brána Azure VPN se skládá ze dvou instancí virtuálního počítače spuštěných v konfiguraci aktivní-pohotovostní. Po vydání příkazu se okamžitě resetuje právě aktivní instance brány Azure VPN. Během převzetí služeb při selhání pohotovostní instancí od aktivní instance (která se resetuje) dojde ke krátké prodlevě. Prodleva by neměla být delší než jedna minuta. 

Následující příklad resetuje bránu Azure VPN pro virtuální síť s názvem „ContosoVNet“.
 
        Reset-AzureVNetGateway –VnetName “ContosoVNet” 

        Error          :
        HttpStatusCode : OK
        Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
        Status         : Successful
        RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
        StatusCode     : OK


Pokud po prvním resetování nedojde k obnovení připojení, znovu vydejte stejný příkaz pro resetování druhé instance virtuálního počítače (nové aktivní brány). Pokud se vyžadují dvě restartování za sebou, bude doba, po kterou se obě instance virtuálního počítače (aktivní a pohotovostní) restartují, trochu delší. To způsobí delší prodlevu propojování VPN – 2 až 4 minuty, než se dokončí resetování virtuálních počítačů.

Pokud i po dvou resetováních stále přetrvávají problémy s propojováním více míst, kontaktujte prosím podporu Microsoft Azure otevřením lístku podpory na portálu Azure Classic.

## Další kroky
    
Další informace o této rutině najdete v [Manuálu k prostředí PowerShell](https://msdn.microsoft.com/library/azure/mt270366.aspx).









<!--HONumber=Aug16_HO4-->


