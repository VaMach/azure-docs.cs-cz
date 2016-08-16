<properties
    pageTitle="Přehled hybridních připojení | Microsoft Azure"
    description="Získejte informace o hybridních připojeních, zabezpečení, portech TCP a podporovaných konfiguracích. MABS, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/10/2016"
    ms.author="mandia"/>


# Přehled hybridních připojení
Článek obsahuje úvod do hybridních připojení a uvádí podporované konfigurace a požadované porty TCP.


## Co je hybridní připojení

Hybridní připojení patří mezi funkce služby Azure BizTalk Services. Hybridní připojení poskytují snadný a pohodlný způsob připojení funkce Web Apps služby Azure App Service (dříve Websites) a funkce Mobile Apps služby Azure App Service (dříve Mobile Services) k místním prostředkům za vaší bránou firewall.

![Hybridní připojení][HCImage]

Některé z výhod hybridních připojení:

- Funkce Web Apps a Mobile Apps můžou získat zabezpečený přístup k místním datům a službám.
- Víc funkcí Web Apps nebo Mobile Apps může získávat přístup k místnímu prostředku pomocí sdíleného hybridního připojení.
- K přístupu k vaší síti je potřeba minimální počet portů TCP.
- Aplikace, které používají hybridní připojení, získávají přístup jenom ke konkrétnímu místnímu prostředku, který je publikovaný prostřednictvím daného hybridního připojení.
- Dá se připojit ke kterémukoli místnímu prostředku, který používá statický TCP port, jako je třeba SQL Server, MySQL, webová rozhraní API HTTP a většina vlastních webových služeb.

    > [AZURE.NOTE] V současné době není dostupná podpora služeb založených na protokolu TCP, které používají dynamické porty (jako je třeba pasivní režim FTP nebo rozšířený pasivní režim). Kromě toho není dostupná podpora protokolu LDAP. LDAP sice používá statický port TCP, ale může používat i protokol UDP. Z toho důvodu se nepodporuje.

- Dají se použít u všech rozhraní podporovaných funkcí Web Apps (.NET, PHP, Java, Python, Node.js) a Mobile Apps (Node.js, .NET).
- Funkce Web Apps a Mobile Apps můžou získávat přístup k místním prostředkům úplně stejně, jako kdyby se daná webová nebo mobilní aplikace nacházela ve vaší místní síti. Stejný připojovací řetězec, jaký používáte v místním prostředí, tak můžete používat i v Azure.


Hybridní připojení také poskytují podnikovým správcům kontrolu a přehled o firemních prostředcích, ke kterým přistupují hybridní aplikace, včetně těchto možností:

- Pomocí nastavení zásad skupiny můžou správci povolit hybridní připojení v síti a také určit prostředky, ke kterým můžou získat přístup hybridní aplikace.
- Protokoly událostí a auditu v podnikové síti poskytují přehled o jednotlivých prostředcích, ke kterým se získává přístup pomocí hybridních připojení.


## Ukázkové scénáře

Hybridní připojení podporují následující kombinace rozhraní a aplikací:

- Přístup k systému SQL Server přes rozhraní .NET
- Přístup ke službám HTTP/HTTPS s webovým klientem přes rozhraní .NET
- Přístup PHP k systému SQL Server, MySQL
- Přístup Java k systému SQL Server, MySQL a Oracle
- Přístup Java ke službám HTTP/HTTPS

Pokud používáte hybridní připojení pro přístup k místnímu SQL Serveru, mějte na paměti tyto informace:

- Pojmenované instance SQL Express musí být nakonfigurované na použití statických portů. Ve výchozím nastavení používají pojmenované instance SQL Express dynamické porty.
- Výchozí instance SQL Express používají statický port, ale musí být povolený protokol TCP. Ve výchozím nastavení není protokol TCP povolený.
- Při použití clusteringu nebo skupin dostupnosti se v současné době nepodporuje režim `MultiSubnetFailover=true`.
- V současné době se nepodporuje nastavení `ApplicationIntent=ReadOnly`.
- Může být potřeba nastavit ověřování SQL, které poskytuje metodu ověřování mezi koncovými účastníky podporovanou aplikací Azure a místním serverem SQL.


## Zabezpečení a porty

Hybridní připojení používají k zabezpečení připojení z aplikací Azure a Místního správce hybridního připojení k funkci Hybridní připojení autorizaci pomocí sdíleného přístupového podpisu (SAS). Pro aplikaci a Místního správce hybridního připojení se vytvoří oddělené klíče připojení. Tyto klíče připojení se dají nezávisle převracet a odvolávat.

Hybridní připojení poskytují hladkou a zabezpečenou distribuci klíčů do aplikací a Místního správce hybridního připojení.

Další informace najdete v článku [Vytváření a správa hybridních připojení](integration-hybrid-connection-create-manage.md).

*Autorizace aplikací probíhá odděleně od hybridního připojení*. Dá se použít libovolná vhodná autorizační metoda. Metoda autorizace závisí na metodách autorizace mezi koncovými účastníky podporovaných v cloudu Azure a místních komponentách. Vaše aplikace Azure může třeba získávat přístup k místnímu SQL Serveru. V tomto scénáři může být podporovaná metoda autorizace SQL mezi koncovými účastníky.

#### Porty TCP
Hybridní připojení vyžadují jenom odchozí připojení TCP nebo HTTP z vaší privátní sítě. Nemusíte otevírat žádné porty brány firewall ani měnit konfiguraci hraniční sítě, abyste povolili příchozí připojení do sítě.

Hybridní připojení používají tyto porty TCP:

Port | Proč to potřebujete
--- | ---
9350–9354 | Tyto porty se používají k přenosům dat. Správce služby Service Bus Relay testuje port 9350, aby zjistil, jestli je dostupné připojení TCP. Pokud ano, správce předpokládá, že je dostupný i port 9352. Přenos dat prochází přes port 9352. <br/><br/>Povolte odchozí připojení k těmto portům.
5671 | Pokud se k přenosům dat používá port 9352, port 5671 slouží jako řídicí kanál. <br/><br/>Povolte odchozí připojení k tomuto portu.
80, 443 | Tyto porty se používají při některých žádostech o data zasílaných do Azure. Kromě toho platí, že pokud porty 9352 a 5671 nejsou použitelné, *potom* se pro přenos dat a jako řídicí kanál použijí záložní porty 80 a 443.<br/><br/>Povolte odchozí připojení k těmto portům. <br/><br/>**Poznámka:** Tyto porty nedoporučujeme používat jako záložní porty pro jiné porty TCP. Jako protokol pro datové kanály se místo nativního protokolu TCP používá HTTP/WebSocket. To může způsobit snížení výkonu.



## Další kroky

[Vytváření a správa hybridních připojení](integration-hybrid-connection-create-manage.md)<br/>
[Připojení webu Azure k místnímu prostředku](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Připojení k místnímu SQL Serveru z webové aplikace Azure](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>
[Azure Mobile Services a hybridní připojení](../mobile-services/mobile-services-dotnet-backend-hybrid-connections-get-started.md)


## Viz také

[Rozhraní API REST pro správu služby BizTalk Services v Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md)<br/>
[Vytvoření služby BizTalk pomocí webu Azure Portal](biztalk-provision-services.md)<br/>
[BizTalk Services: Karty Řídicí panel, Sledování a Škálování](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png



<!--HONumber=Jun16_HO2-->


