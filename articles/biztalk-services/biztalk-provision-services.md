---
title: "Vytvoření služby Azure BizTalk Services na webu Azure Portal | Dokumentace Microsoftu"
description: "Naučte se na webu Azure Portal zřizovat nebo vytvářet službu Azure BizTalk Services; MABS, WABS."
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 71f9dd111ebdbe885f33d162b2ea320dfaa167bb
ms.openlocfilehash: a2e289de2719be64a9468cb9b24ff4101b2e93db


---
# <a name="create-biztalk-services-using-the-azure-portal"></a>Vytvoření služby BizTalk Services pomocí webu Azure Portal
Vytvořte službu BizTalk Services na webu Azure Portal.

> [!TIP]
> Abyste se mohli přihlásit na webu Azure Portal, potřebujete účet Azure a předplatné Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podívejte se na stránku [bezplatné zkušební verze Azure](http://go.microsoft.com/fwlink/p/?LinkID=239738).
> 
> 

## <a name="create-a-biztalk-service"></a>Vytvoření služby BizTalk
V závislosti na edici, kterou si zvolíte, nemusí být dostupná všechna nastavení služby BizTalk.

1. Přihlaste se na web [Azure Portal ](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Ve spodním navigačním podokně vyberte **NOVÁ**:  
   ![Výběr tlačítka Nová][NEWButton]
3. Vyberte možnosti **APP SERVICES** > **BIZTALK SERVICE** (Služba BizTalk) > **CUSTOM CREATE** (Vytvořit vlastní):  
   ![Výběr služby BizTalk a možnosti Custom Create (Vytvořit vlastní)][NewBizTalkService]
4. Zadejte nastavení služby BizTalk:
   
    <table border="1">
    <tr>
    <td><strong>BizTalk service name (Název služby BizTalk)</strong></td>
    <td>Můžete zadat libovolný název, ale buďte konkrétní. Možné příklady:<br/><br/>
    <em>moje_firma</em>.biztalk.windows.net<br/>
    <em>moje_firma_moje_aplikace</em>.biztalk.windows.net<br/>
    <em>moje_aplikace</em>.biztalk.windows.net<br/><br/>K zadanému názvu se automaticky připojí „.biztalk.windows.net“. Tím se vytvoří adresa URL, která se bude používat k přístupu ke službě BizTalk, třeba <strong>https://<em>moje_aplikace</em>.biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edice</strong></td>
    <td>Pokud jste ve fázi testování/vývoje, vyberte možnost <strong>Developer</strong>. Pokud jste v produkční fázi, pomocí článku <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">Služba BizTalk Services: Tabulka edic</a> určete, jestli pro vaši obchodní situaci nejvhodnější edice <strong>Premium</strong>, <strong>Standard</strong>, nebo <strong>Basic</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Oblast</strong></td>
    <td>Vyberte zeměpisnou oblast pro hostování vaší služby BizTalk.</td>
    </tr>
    <tr>
    <td><strong>Domain URL (Adresa URL domény)</strong></td>
    <td><strong>Volitelné</strong>. Výchozí adresa URL domény je <em>název_vaší_služby_BizTalk</em>.biztalk.windows.net. Můžete ale zadat i vlastní doménu. Pokud máte třeba doménu <em>contoso</em>, můžete zadat: <br/><br/>
    <em>moje_firma</em>.contoso.com<br/>
    <em>moje_firma_moje_aplikace</em>.contoso.com<br/>
    <em>moje_aplikace</em>.contoso.com<br/>
    <em>název_vaší_služby_BizTalk</em>.contoso.com<br/>
    </td>
    </tr>
    </table>
   Vyberte šipku DALŠÍ.
5. Zadejte nastavení úložiště a databáze:
   
    <table border="1">
    <tr>
    <td><strong>Monitoring/Archiving storage account (Účet úložiště pro monitorování/archivaci)</strong></td>
    <td>Vyberte stávající účet úložiště nebo vytvořte nový. <br/><br/>Pokud vytvoříte nový účet úložiště, zadejte položku <strong>Název účtu úložiště</strong>.</td>
    </tr>
    <tr>
    <td><strong>Tracking database (Databáze sledování)</strong></td>
    <td>Pokud použijete existující službu Azure SQL Database, nesmí ji používat žádná jiná služba BizTalk. Potřebujete přihlašovací jméno a heslo zadané při vytváření serveru služby Azure SQL Database.<br/><br/><strong>TIP</strong> Databázi sledování a účet úložiště pro monitorování/archivaci vytvořte ve stejné oblasti jako službu BizTalk.</td>
    </tr>
    </table>
   Vyberte šipku DALŠÍ.
6. Zadejte nastavení databáze:
   
    <table border="1">
    <tr>
    <td><strong>Název</strong></td>
    <td>Tato možnost je dostupná, jenom pokud jste na předchozí obrazovce vybrali možnost <strong>Create a new SQL Database instance</strong> (Vytvořit novou instanci služby SQL Database).
    <br/><br/>
    Zadejte název služby SQL Database, kterou má vaše služba BizTalk používat.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td>Tato možnost je dostupná, jenom pokud jste na předchozí obrazovce vybrali možnost <strong>Create a new SQL Database instance</strong> (Vytvořit novou instanci služby SQL Database).
    <br/><br/>
    Vyberte existující server služby SQL Database nebo vytvořte nový.</td>
    </tr>
    <tr>
    <td><strong>Server login name (Jméno pro přihlášení na server)</strong></td>
    <td>Zadejte přihlašovací jméno uživatele.</td>
    </tr>
    <tr>
    <td><strong>Server login password (Heslo pro přihlášení na server)</strong></td>
    <td>Zadejte heslo pro přihlášení.</td>
    </tr>
    <tr>
    <td><strong>Oblast</strong></td>
    <td>Tato možnost je dostupná, jenom pokud je vybraná možnost <strong>Create a new SQL Database instance</strong> (Vytvořit novou instanci služby SQL Database). Vyberte zeměpisnou oblast, ve které se má hostovat služba SQL Database.</td>
    </tr>
    </table>

Průvodce dokončete výběrem symbolu zaškrtnutí. Zobrazí se ikona průběhu:  
![Po dokončení se zobrazí ikona průběhu][ProgressComplete]

Po dokončení průvodce je služba Azure BizTalk vytvořená a připravená na vaše aplikace. Výchozí nastavení je dostatečné. Pokud chcete výchozí nastavení změnit, vyberte v levém navigačním podokně **BIZTALK SERVICES** a potom vyberte svoji službu BizTalk. Na [kartách Dashboard (Řídicí panel), Monitor (Sledování) a Scale (Škálování)](biztalk-dashboard-monitor-scale-tabs.md) v horní části se zobrazí další nastavení.

V závislosti na stavu služby BizTalk se může stát, že se některé operace nebudou dát provést. Seznam těchto operací najdete v článku [BizTalk Services: Tabulka stavů](biztalk-service-state-chart.md).

## <a name="postprovisioning-steps"></a>Kroky pro zřízení
* [Instalace certifikátu na místním počítači](#InstallCert)
* [Přidání certifikátu pro produkční prostředí](#AddCert)
* [Získání oboru názvů řízení přístupu](#ACS)

#### <a name="a-nameinstallcertainstall-the-certificate-on-a-local-computer"></a><a name="InstallCert"></a>Instalace certifikátu na místním počítači
V rámci zřizování služby BizTalk se vytvoří certifikát podepsaný svým držitelem a přidruží se k vašemu předplatnému služby BizTalk. Tento certifikát si musíte stáhnout a nainstalovat ho na počítačích, ze kterých buď nasazujete aplikace služby BizTalk, nebo odesíláte zprávy do koncového bodu služby BizTalk.

1. Přihlaste se na web [Azure Portal ](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně vyberte **BIZTALK SERVICES** a potom vyberte své předplatné služby BizTalk.
3. Vyberte kartu **Dashboard** (Řídicí panel).
4. Vyberte **Download SSL Certificate** (Stáhnout certifikát SSL):  
   ![Úprava certifikátu SSL][QuickGlance]
5. Dvakrát klikněte na certifikát a pomocí průvodce ho nainstalujte. Ujistěte se, že certifikát instalujete do úložiště **Trusted Root Certificate Authorities** (Důvěryhodné kořenové certifikační autority).

#### <a name="a-nameaddcertaadd-a-productionready-certificate"></a><a name="AddCert"></a>Přidání certifikátu pro produkční prostředí
Certifikát podepsaný svým držitelem, který se automaticky vytvoří při vytváření služby BizTalk Services, je určený jenom k použití ve vývojovém prostředí. V produkčních scénářích ho nahraďte certifikátem pro produkční prostředí.

1. Na kartě **Dashboard** (Řídicí panel) vyberte **Update SSL Certificate** (Aktualizovat certifikát SSL).
2. Přejděte na svůj soukromý certifikát SSL (*název_certifikátu*.pfx), který obsahuje název vaší služby BizTalk, zadejte heslo a potom klikněte na symbol zaškrtnutí.

#### <a name="a-nameacsaget-the-access-control-namespace"></a><a name="ACS"></a>Získání oboru názvů řízení přístupu
1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně vyberte **BIZTALK SERVICES** a potom vyberte svoji službu BizTalk.
3. Na hlavním panelu vyberte **Informace o připojení**:  
   ![Výběr možnosti Informace o připojení][ACSConnectInfo]
4. Zkopírujte hodnoty řízení přístupu.

Tento obor názvů řízení přístupu zadáváte při nasazení projektu služby BizTalk v sadě Visual Studio. Obor názvů řízení přístupu se pro vaši službu BizTalk vytvoří automaticky.

Hodnoty řízení přístupu se dají použít s libovolnou aplikací. Při vytváření služby Azure BizTalk Services tento obor názvů řízení přístupu řídí ověřování ve vašem nasazení služby BizTalk. Pokud chcete změnit předplatné nebo spravovat obor názvů, v levém navigačním podokně vyberte **ACTIVE DIRECTORY** a potom zvolte svůj obor názvů. Možnosti jsou uvedené na hlavním panelu.

Kliknutím na **Manage** (Správa) otevřete portál pro správu řízení přístupu. Na portálu pro správu řízení přístupu používá služba BizTalk **identity služby**:  
![Identita služby ACS na portálu pro správu řízení přístupu][ACSServiceIdentities]

Identita služby Access Control Service je sada přihlašovacích údajů, která umožňuje aplikacím nebo klientům přímé ověření u řízení přístupu a získání tokenu.

> [!IMPORTANT]
> Služba BizTalk používá výchozí identitu služby **Vlastník** a hodnotu **Heslo**. Pokud místo hodnoty Heslo použijete hodnotu Symetrický klíč, může dojít k následující chybě.<br/><br/>*Zadané přihlašovací údaje neumožňují připojení k účtu služby pro správu řízení přístupu.*
> 
> 

V článku [Správa oboru názvů služby ACS](https://msdn.microsoft.com/library/azure/hh674478.aspx) najdete některé pokyny a doporučení.

## <a name="requirements-explained"></a>Vysvětlení požadavků
Tyto požadavky se netýkají edice Free.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Co potřebujete</strong></td>
        <td><strong>Proč to potřebujete</strong></td>
</tr>
<tr>
<td>Předplatné Azure</td>
<td>Předplatné určuje, kdo se může přihlásit na web Azure Portal. Držitel účtu vytvoří předplatné na stránce <a HREF="https://account.windowsazure.com/Subscriptions">předplatných Azure</a>.
<br/><br/>
Účet Azure může obsahovat víc předplatných a může ho spravovat každý, kdo k tomu má oprávnění. Třeba váš držitel účtu Azure vytvoří předplatné s názvem <em>Predplatne_služby_BizTalk</em> a dá k tomuto předplatnému přístup správcům služby BizTalk z vaší společnosti (třeba ContosoBTSAdmins@live.com)). V tomto scénáři budou mít správci služby BizTalk po přihlášení na web Azure Portal úplná práva správce ke všem hostovaným službám v daném předplatném, včetně služby Azure BizTalk Services. Správci služby BizTalk nejsou držiteli účtu Azure, takže nemají přístup k informacím o fakturaci.
<br/><br/>Další informace najdete v článku 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Správa předplatných a účtů úložiště na webu Azure Portal</a>.
</td>
</tr>
<tr>
<td>Azure SQL Database</td>
<td>Slouží k ukládání tabulek, zobrazení a uložených procedur používaných službou BizTalk, včetně dat sledování.
<br/><br/>
Když vytváříte službu BizTalk, můžete použít existující server SQL Azure nebo službu Azure SQL Database nebo můžete automaticky vytvořit nový server nebo databázi.
<br/><br/>
Škálování služby SQL Database se konfiguruje automaticky. Pro službu BizTalk většinou stačí výchozí škálování. Změna škálování má vliv na ceny. Další informace najdete v tématu <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">o účtech a cenách služby Azure SQL Database</a>
.<br/><br/>
<strong>Poznámky</strong>
<br/>
<ul>
<li> Při vytváření nového serveru SQL Azure a služby SQL Database automaticky dojde k povolení služeb Azure. Služba BizTalk vyžaduje povolení služeb Azure.</li>
<li>Pokud vytvoříte novou službu Azure SQL Database na existujícím serveru SQL Azure, pravidla brány firewall serveru se nezmění. V důsledku toho je možné, že jiné služby Azure nebudou mít přístup k databázím serveru.</li>
</ul>
</td>
</tr>
<tr>
<td>Obor názvů řízení přístupu</td>
<td>Ověřuje se u služby Azure BizTalk Services. Tento obor názvů řízení přístupu zadáváte při nasazení projektu služby BizTalk v sadě Visual Studio. Při vytváření služby BizTalk se obor názvů řízení přístupu vytvoří automaticky.</td>
</tr>

<tr>
<td>Účet služby Azure Storage</td>
<td>Poskytuje přístup k tabulkám, objektům blob a frontám, které vaše služba BizTalk používá k ukládání těchto informací:

<ul>
<li>Soubory protokolu, které monitorují službu BizTalk. Výstup monitorování se také zobrazuje na kartě **Sledování** na webu Azure Portal.</li>
<li>Při vytváření smlouvy X12 nebo AS2 mezi partnery můžete povolit funkci archivace, která umožňuje ukládat vlastnosti správ. Tato data se ukládají na účet služby Storage.</li>
</ul>
<br/>
Při vytváření služby BizTalk můžete použít stávající účet služby Storage nebo automaticky vytvořit nový.
<br/><br/>
Pro službu BizTalk stačí výchozí nastavení služby Storage.
<br/><br/>
Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč. Tyto klíče řídí přístup k vašemu účtu služby Storage. Služba BizTalk automaticky používá primární klíč.
<br/><br/>
Další informace najdete v článku o <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">službě Storage</a>.
</td>
</tr>

<tr>
<td>Privátní certifikát SSL</td>
<td>
Při vytváření služby Azure BizTalk se vytvoří také adresa URL HTTPS, která obsahuje název vaší služby BizTalk. Tato adresa URL je automaticky nakonfigurovaná tak, aby používala certifikát podepsaný svým držitelem určený jenom pro vývojové prostředí. V produkčním prostředí budete potřebovat privátní certifikát SSL.
<br/><br/>
<strong>Důležité informace o certifikátu SSL</strong>

<ul>
<li>Datum vypršení platnosti certifikátu musí být za dobu kratší než 5 let.</li>
<li>Všechny privátní certifikáty vyžadují heslo. Heslo si zapamatujte a jako osvědčený postup také doporučujeme ho sdělit vašim správcům.</li>
<li>Certifikáty podepsané svým držitelem se používají v testovacím/vývojovém prostředí. Pokud používáte certifikáty podepsané svým držitelem, importujte certifikát do úložiště osobních certifikátů a do úložiště důvěryhodných kořenových certifikačních autorit.</li>
</ul>
<br/>Při odesílání požadavku na certifikát pro produkční prostředí vaší certifikační autoritě uveďte následující vlastnosti certifikátu:
<br/>

<ul>
<li><strong>Použití rozšířeného klíče</strong>: Služba Azure BizTalk Services vyžaduje minimálně ověření serveru.</li>
<li><strong>Běžný název</strong>: Zadejte plně kvalifikovaný název domény (FQDN) adresy URL služby Azure BizTalk. Projděte si část <a HREF="#BizTalk">Vytvoření služby BizTalk</a> v tomto článku.</li>
</ul>
<br/>
Po vytvoření služby BizTalk můžete přidat nový nebo jiný certifikát.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hybridní připojení
Při vytváření služby BizTalk Azure je dostupná karta **Hybridní připojení**:

![Karta Hybridní připojení][HybridConnectionTab]

Hybridní připojení slouží k připojení webu Azure nebo mobilní služby Azure k jakémukoli místnímu prostředku, který používá statický port TCP, jako je třeba SQL Server, MySQL, webové rozhraní API HTTP, Mobile Services a většina vlastních webových služeb.  Hybridní připojení se liší od služby BizTalk Adapter Service. Služba BizTalk Adapter Service se používá k připojení služby Azure BizTalk Services k místnímu obchodnímu systému (LOB).

 Další informace, třeba o vytváření a správě hybridních připojení, najdete v článku [Hybridní připojení](integration-hybrid-connection-overview.md).

## <a name="next-steps"></a>Další kroky
Služba BizTalk je teď vytvořená a vy se můžete seznámit s různými kartami: [BizTalk Services: Karty Řídicí panel, Sledování a Škálování](biztalk-dashboard-monitor-scale-tabs.md). Služba BizTalk je připravená pro vaše aplikace. Pokud chcete začít vytvářet aplikace, přejděte na článek [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Viz také
* [BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: Tabulka stavů](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Zálohování a obnovení](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Omezování](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Název a klíč vystavitele](biztalk-issuer-name-issuer-key.md)<br/>
* [Jak začít používat sadu SDK Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Hybridní připojení](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png



<!--HONumber=Nov16_HO2-->


