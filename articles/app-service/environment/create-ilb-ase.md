---
title: "Vytváření a používání vyrovnávání zatížení interní pomocí služby Azure App Service environment"
description: "Podrobnosti o tom, jak vytvořit a používat Azure App Service environment izolované Internetu"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: cc7bdd7860506c20187dc913b72111824d1737ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Vytvořit a použít vyrovnávání zatížení interní s služby App Service environment #

 Azure App Service Environment je nasazení služby Azure App Service do používanou podsíť virtuální sítě Azure (VNet). Existují dva způsoby, jak nasadit služby App Service environment (App Service Environment): 

- V VIP na externí IP adresu často označuje jako externí App Service Environment.
- S virtuální IP adresu na interní IP adresu často říká App Service Environment ILB protože vnitřní koncový bod je vyrovnávání interní zatížení (ILB). 

Tento článek ukazuje, jak vytvořit App Service Environment ILB. Přehled App Service Environment, najdete v části [Úvod do služby App Service Environment][Intro]. Naučte se vytvářet externí App Service Environment, najdete v tématu [vytvořit externí App Service Environment][MakeExternalASE].

## <a name="overview"></a>Přehled ##

App Service Environment se koncový bod přístupné z Internetu nebo IP adresu můžete nasadit ve vaší virtuální síti. Chcete-li nastavit adresu IP adresu virtuální sítě, musí být nasazený App Service Environment s ILB. Při nasazení vaší App Service Environment s ILB, je nutné zadat:

-   Vlastní domény, který použijete při vytváření aplikace.
-   Certifikát použitý pro protokol HTTPS.
-   Správa služby DNS pro vaši doménu.

Naopak může provádět akce, jako:

-   Hostování aplikací intranetu bezpečně v cloudu, což přistupujete přes síť site-to-site nebo Azure ExpressRoute VPN.
-   Aplikace hostitele, v cloudu, které nejsou uvedené v veřejné servery DNS.
-   Vytvořte samostatný internet back-end aplikace, které aplikace front-endu může bezpečně integrovat.

### <a name="disabled-functionality"></a>Zakázané funkce ###

Existují některé věci, které nejde dělat, když používáte ILB App Service Environment:

-   Použijte SSL založenou na protokolu IP.
-   Přiřaďte IP adresy na konkrétní aplikace.
-   Zakoupení a použití certifikátu s aplikací prostřednictvím portálu Azure. Můžete získat certifikáty přímo od certifikační autority a jejich používání s vaší aplikací. Nelze získat je prostřednictvím portálu Azure.

## <a name="create-an-ilb-ase"></a>Vytvořit App Service Environment ILB ##

Chcete-li vytvořit ILB App Service Environment:

1. Na portálu Azure vyberte **nový** > **Web + mobilní** > **App Service Environment**.

2. Vyberte své předplatné.

3. Vyberte nebo vytvořte skupinu prostředků.

4. Vyberte nebo vytvořte virtuální síť.

5. Pokud vyberete stávající virtuální síť, budete muset vytvořit podsíť pro uložení App Service Environment. Nezapomeňte nastavit velikost podsítě dostatečně velký pro přizpůsobení případnému budoucímu růstu vašeho App Service Environment. Doporučujeme velikost `/25`, která je 128 adresy a dokáže zpracovat App Service Environment maximální velikost. Je minimální velikost, můžete vybrat `/28`. Po infrastruktury potřebuje, tato velikost je možné rozšířit, aby nesmí být delší než 11 instancí.

    * Překročila maximální výchozí počet instancí 100 v plánu služby App Service.

    * Škálování téměř 100, ale s více Rychlé škálování front-endu.

6. Vyberte **virtuální sítě nebo umístění** > **konfigurace virtuální sítě**. Nastavte **VIP typ** k **interní**.

7. Zadejte název domény. Tato doména je používaný pro aplikace vytvořené v této App Service Environment. Platí určitá omezení. Nemůže být:

    * NET   

    * azurewebsites.NET

    * p.azurewebsites.NET

    * &lt;asename&gt;. p.azurewebsites.net

   Použít vlastní název domény pro aplikace a název domény používaný vaší App Service Environment se nesmí překrývat. Pro App Service Environment ILB s názvem domény _contoso.com_, nemůžete použít vlastní názvy domén pro vaše aplikace, jako je:

    * www.contoso.com

    * Abcd.def.contoso.com

    * Abcd.contoso.com

   Pokud znáte názvy vlastních domén pro vaše aplikace, zvolte doménu pro App Service Environment ILB, který nebude došlo ke konfliktu s těmito názvy vlastních domén. V tomto příkladu můžete použít něco jako *contoso-internal.com* pro doménu vaší App Service Environment vzhledem k tomu, který nebude v konfliktu s názvy vlastních domén, které končí *. contoso.com*.

8. Vyberte **OK**a potom vyberte **vytvořit**.

    ![Vytvoření služby ASE][1]

Na **virtuální sítě** okna, je **konfigurace virtuální sítě** možnost. Můžete ho vybrat externí VIP nebo interní VIP. Výchozí hodnota je **externí**. Pokud vyberete **externí**, vaše App Service Environment používá přístupné z Internetu VIP. Pokud vyberete **interní**, vaše App Service Environment je nakonfigurován s ILB na IP adresu ve virtuální síti.

Po výběru **interní**, možnost přidávat další IP adresy k vaší App Service Environment se odebere. Místo toho je třeba zadat doménu App Service Environment. V App Service Environment externí VIP název App Service Environment se používá v doméně pro aplikace vytvořené v této App Service Environment.

Pokud nastavíte **VIP typ** k **interní**, vaše jméno App Service Environment není v doméně používat pro App Service Environment. Zadejte doménu explicitně. Pokud vaše doména je *contoso.corp.net* a vytvoříte aplikaci v tom, že s názvem App Service Environment *timereporting*, adresa URL pro tuto aplikaci je timereporting.contoso.corp.net.


## <a name="create-an-app-in-an-ilb-ase"></a>Vytvoření aplikace v App Service Environment ILB ##

Vytvoření aplikace v App Service Environment ILB stejným způsobem, obvykle vytvoříte aplikaci v App Service Environment.

1. Na portálu Azure vyberte **nový** > **Web + mobilní** > **webové** nebo **Mobile** nebo **rozhraní API Aplikace**.

2. Zadejte název aplikace.

3. Vyberte předplatné.

4. Vyberte nebo vytvořte skupinu prostředků.

5. Vyberte nebo vytvořte plán služby App Service. Pokud chcete vytvořit nový plán aplikační služby, vyberte jako umístění vaší App Service Environment. Vyberte fond pracovních procesů, kam chcete plán služby App Service, který se má vytvořit. Když vytvoříte plán služby App Service, vyberte vaše App Service Environment jako umístění a fondu pracovních procesů. Když zadáte název aplikace, domény v části název vaší aplikace je nahrazena domény pro váš App Service Environment.

6. Vyberte **Vytvořit**. Pokud chcete aplikaci se zobrazují na řídicím panelu, vyberte **připnout na řídicí panel** zaškrtávací políčko.

    ![Vytvoření plánu služby App Service][2]

    V části **název aplikace**, název domény se aktualizuje tak, aby odrážela doménu vaší App Service Environment.

## <a name="post-ilb-ase-creation-validation"></a>Ověření vytvoření POST-ILB App Service Environment ##

App Service Environment ILB je trochu jiná než jiných - ILB App Service Environment. Jak je již uvedeno potřebujete spravovat vlastní DNS. Je nutné také zadat svůj vlastní certifikát pro připojení prostřednictvím protokolu HTTPS.

Po vytvoření vaší App Service Environment, název domény zobrazuje doménu, kterou jste zadali. Nová položka se zobrazí v **nastavení** nabídky s názvem **ILB certifikát**. App Service Environment se vytvoří certifikát, který není zadejte doménu ILB App Service Environment. Pokud používáte App Service Environment pomocí certifikátu, prohlížeč informuje, že je neplatný. Tento certifikát usnadňuje testování HTTPS, ale budete muset nahrát vlastní certifikát, který je vázaný na doménu ILB App Service Environment. Tento krok je nutný bez ohledu na to, jestli je vašeho certifikátu podepsaného svým držitelem nebo získali od certifikační autority.

![Název domény ILB App Service Environment][3]

Vaše ILB App Service Environment musí platný certifikát SSL. Použít interní certifikační úřady, koupit certifikát od externí vystavitele nebo použít certifikát podepsaný svým držitelem. Bez ohledu na zdroji certifikátu protokolu SSL musí být správně nakonfigurované následující atributy certifikátu:

* **Předmět**: Tento atribut musí být nastaven na *.your kořenové domény sem.
* **Alternativní název subjektu**: Tento atribut musí obsahovat i **.your kořenové domény sem* a **.scm.your-kořenové-domény-zde*. Připojení SSL k webu SCM/Kudu spojené s každou aplikaci použít adresu ve tvaru *your-app-name.scm.your-root-domain-here*.

Převést nebo uložení certifikátu SSL jako soubor .pfx. Soubor .pfx, musíte zahrnout všechny zprostředkující a kořenové certifikáty. Zabezpečte ji pomocí hesla.

Pokud chcete vytvořit certifikát podepsaný svým držitelem, můžete tady použít příkazy prostředí PowerShell. Nezapomeňte použít název domény App Service Environment ILB místo *internal.contoso.com*: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Certifikát, který tyto příkazy prostředí PowerShell generovat označen příznakem pomocí prohlížeče, protože certifikát nebyl vytvořen z certifikační autority, který je v prohlížeči řetěz certifikátů. Chcete-li získat certifikát, který váš prohlížeč vztahy důvěryhodnosti, zakoupit od komerční certifikační autority v prohlížeči řetěz certifikátů. 

![Nastavte ILB certifikát][4]

Nahrát vlastní certifikáty a otestovat přístup:

1. Po vytvoření App Service Environment, přejděte na rozhraní App Service Environment. Vyberte **App Service Environment** > **nastavení** > **ILB certifikát**.

2. Pokud chcete nastavit certifikát ILB, vyberte soubor .pfx certifikátu a zadejte heslo. Tento krok trvá určitou dobu ke zpracování. Zobrazí se zpráva, že operace nahrávání probíhá.

3. Získáte adresu ILB pro vaše App Service Environment. Vyberte **App Service Environment** > **vlastnosti** > **virtuální IP adresa**.

4. Vytvoření webové aplikace ve vaší App Service Environment, po vytvoření App Service Environment.

5. Vytvoření virtuálního počítače, pokud nemáte v této virtuální sítě.

    > [!NOTE] 
    > Nemáte pokuste se vytvořit tento virtuální počítač ve stejné podsíti jako App Service Environment, protože se nezdaří nebo způsobit problémy.
    >
    >

6. Nastavení DNS pro doménu služby App Service Environment. Můžete použít zástupný znak s doménou v DNS. Chcete-li provést několik jednoduchých testů, upravte souboru hostitelů na vašem virtuálním počítači nastavit název webové aplikace na virtuální IP adresy IP adresu:

    a. Pokud název domény vaší App Service Environment _. ilbase.com_ a vytvoříte webovou aplikaci s názvem _mytestapp_, je určena v _mytestapp.ilbase.com_. Pak můžete nastavit _mytestapp.ilbase.com_ přeložit na adresu ILB. (V systému Windows, soubor hostitele je v _C:\Windows\System32\drivers\etc\_.)

    b. Testování nasazení publikování na webu nebo přístup ke konzole rozšířené, vytvořte záznam pro _mytestapp.scm.ilbase.com_.

7. Použít prohlížeč na tento virtuální počítač a přejděte k http://mytestapp.ilbase.com. (Nebo přejděte na ať je název vaší webové aplikace s vaší domény.)

8. Použít prohlížeč na tento virtuální počítač a přejděte k https://mytestapp.ilbase.com. Pokud používáte certifikát podepsaný svým držitelem, přijměte nedostatek zabezpečení.

    IP adresu pro vaše ILB je uveden v části **IP adresy**. Tento seznam má také IP adres používaných externí virtuální IP adresy a pro správu příchozí přenosy.

    ![ILB IP adresa][5]

## <a name="web-jobs-functions-and-the-ilb-ase"></a>Webové úlohy, funkce a App Service Environment ILB ##

Funkce a webové úlohy jsou podporovány v App Service Environment ILB, ale pro tento portál pro práci s nimi musí mít přístup k síti na web SCM.  To znamená, že váš prohlížeč musí být buď na hostitele, který je buď v nebo připojení k virtuální síti.  

Pokud používáte Azure Functions v App Service Environment ILB, může získat chybovou zprávu, která říká "Nedokážeme teď načíst vaše funkce. Zkuste to prosím znovu později." K této chybě dojde, protože uživatelské rozhraní funkce využívá webu SCM přes protokol HTTPS a kořenový certifikát není v prohlížeči řetěz certifikátů. Webové úlohy je podobný problém. Tento problém můžete provést jednu z těchto možností:

- Přidání certifikátu do úložiště důvěryhodných certifikátů. To odblokuje okraj a prohlížeče Internet Explorer.
- Použijte Chrome a přejděte na web SCM nejprve, přijměte nedůvěryhodný certifikát a potom přejděte na portál.
- Použijte komerční certifikát, který je v řetězu certifikátů váš prohlížeč.  Toto je nejlepší možnost.  

## <a name="dns-configuration"></a>Konfigurace služby DNS ##

Pokud používáte externí VIP, DNS spravuje Azure. Všechny aplikace vytvořená v vaší App Service Environment se automaticky přidá do Azure DNS, který je veřejném DNS. ILB App Service Environment musí spravovat vlastní DNS. Pro danou doménu, jako _contoso.net_, budete muset vytvořit záznamy DNS A v serveru DNS, které odkazují na vaši adresu ILB pro:

- *. contoso.net
- *. scm.contoso.net

Pokud doménu ILB App Service Environment se používá pro několik věcí mimo tento App Service Environment, může být třeba spravovat DNS na základě názvu na aplikaci. Tato metoda je náročné, protože je nutné přidat každý nový název aplikace do DNS při jeho vytvoření. Z tohoto důvodu doporučujeme používat vyhrazené domény.

## <a name="publish-with-an-ilb-ase"></a>Publikování s ILB App Service Environment ##

Pro každou aplikaci, která je vytvořena jsou dva koncové body. V App Service Environment ILB, máte  *&lt;název aplikace >.&lt; App Service Environment domény ILB >* a  *&lt;název aplikace > .scm.&lt; App Service Environment domény ILB >*. 

Název lokality SCM přejdete ke konzole Kudu názvem **pokročilé portál**, v rámci portálu Azure. Konzola Kudu umožňuje zobrazení proměnné prostředí, prozkoumejte disku, použijte konzolu a mnoho dalšího. Další informace najdete v tématu [konzoly Kudu pro službu Azure App Service][Kudu]. 

V víceklientské služby App Service a externí App Service Environment není jednotné přihlašování mezi portálu Azure a Kudu konzoly. Pro ILB App Service Environment budete muset však použít pro přihlášení ke konzole Kudu vaše přihlašovací údaje pro publikování.

Internetové systémy položek konfigurace, například Githubu a Visual Studio Team Services, nefungují s ILB App Service Environment, protože publikování koncový bod není dostupný na Internetu. Místo toho musíte použít CI systému, který používá model pull, jako je Dropbox.

Publikování koncové body pro aplikace v App Service Environment ILB použijte domény, který byl vytvořený App Service Environment ILB. Tuto doménu se zobrazí v profilu publikování aplikace a v okně portálu aplikace (**přehled** > **Essentials** a také **vlastnosti**). Pokud máte App Service Environment ILB s subdoméně *contoso.net* a aplikaci s názvem *test*, použijte *mytest.contoso.net* pro FTP a *mytest.scm.contoso.net* pro nasazení webu.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Spojte ILB App Service Environment se zařízením firewall webových aplikací ##

Aplikační služba Azure poskytuje mnoho bezpečnostní opatření, které chrání systém. Mohou také pomoci určit, zda se hacker aplikace. Nejlepší ochrany pro webovou aplikaci je spojte hostující platforma, jako je například Azure App Service pomocí brány firewall webových aplikací (firewall webových aplikací). Protože App Service Environment ILB má koncový bod sítě izolované aplikace, je vhodné pro toto použití.

Další informace o tom, jak nakonfigurovat App Service Environment vaší ILB zařízení firewall webových aplikací najdete v tématu [konfigurace brány firewall webových aplikací pomocí služby App Service environment][ASEWAF]. Tento článek ukazuje, jak používat virtuální zařízení Barracuda s vaší App Service Environment. Další možností je použití Azure Application Gateway. Aplikační brána používá pravidla základní OWASP zabezpečit všechny aplikace se umístí za ho. Další informace o Application Gateway najdete v tématu [Úvod do brány firewall Azure webových aplikací][AppGW].

## <a name="get-started"></a>Začínáme ##

* Chcete-li začít pracovat s ASEs, přečtěte si téma [Úvod do služby App Service Environment][Intro].
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
