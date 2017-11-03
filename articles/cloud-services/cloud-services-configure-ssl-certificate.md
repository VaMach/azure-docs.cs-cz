---
title: "Konfigurace protokolu SSL pro cloudové služby (klasické) | Microsoft Docs"
description: "Zjistěte, jak zadat koncový bod HTTPS pro webovou roli a jak nahrát certifikát SSL pro zabezpečení vaší aplikace."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: 7df2371f64f0d8a9fabc0df37c17d4dfbc47cd7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>Konfigurace protokolu SSL pro všechny aplikace v Azure
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-configure-ssl-certificate-portal.md)
> * [Portál Azure Classic](cloud-services-configure-ssl-certificate.md)
> 
> 

Šifrování SSL (Secure Socket Layer) je nejčastěji používanou metodou zabezpečení dat posílaných přes internet. Článek o tomto běžném postupu probírá, jak zadat koncový bod HTTPS pro webovou roli a jak nahrát certifikát SSL pro zabezpečení aplikace.

> [!NOTE]
> Postupy v této úloze platí pro Azure Cloud Services; Aplikační služby, najdete v části [to](../app-service/app-service-web-tutorial-custom-ssl.md) článku.
> 
> 

Tato úloha používá produkčním nasazení. Informace o používání pracovní nasazení najdete na konci tohoto tématu.

Čtení [to](cloud-services-how-to-create-deploy.md) nejprve článek, pokud jste ještě nevytvořili cloudové služby.

## <a name="step-1-get-an-ssl-certificate"></a>Krok 1: Získání certifikátu protokolu SSL
Konfigurace protokolu SSL pro aplikaci, musíte nejdřív získat certifikát SSL, která byla podepsána pomocí certifikační autoritou (CA), důvěryhodná třetí straně, která vystavuje certifikáty pro tento účel. Pokud není již nemáte, budete muset získat jeden ze společnosti, která prodává certifikáty SSL.

Certifikát musí splňovat následující požadavky na certifikáty SSL v Azure:

* Certifikát musí obsahovat privátní klíč.
* Certifikát se musí vytvořit pro výměnu klíčů, exportovat do souboru Personal Information Exchange (.pfx).
* Název subjektu certifikátu musí odpovídat domény používá pro přístup ke cloudové službě. Nelze získat certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net. Musíte získat vlastní název domény má použít při přístupu ke službě. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, používá pro přístup k aplikaci. Například, pokud je váš vlastní název domény **contoso.com** by vyžádání certifikátu z certifikační Autority pro ***. contoso.com** nebo **www.contoso.com**.
* Certifikát musí používat minimálně 2048bitové šifrování.

Pro účely testování můžete [vytvořit](cloud-services-certs-create.md) a použít certifikát podepsaný svým držitelem. Certifikát podepsaný svým držitelem není ověřen pomocí certifikační Autority a cloudapp.net domény můžete použít jako adresu URL webu. Například následující úkol používá certifikát podepsaný svým držitelem, ve kterém je běžný název (CN) použitý v certifikátu **sslexample.cloudapp.net**.

V dalším kroku musí zahrnovat informace o certifikátu v definici služby a služby konfigurační soubory.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Krok 2: Upravte soubory definice a konfigurace služby
Vaše aplikace musí být nakonfigurována pro použití certifikátu a koncový bod HTTPS musí být přidán. Definice služby a služby konfigurační soubory v důsledku toho musí aktualizovat.

1. Ve vašem vývojovém prostředí, otevřete soubor definice služby (CSDEF), přidejte **certifikáty** části v rámci **WebRole** části a uveďte tyto informace o certifikátu (a zprostředkující certifikáty):
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                        storeLocation="LocalMachine" 
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by the CA root, you
            must include all the intermediate certificates
            here. You must list them here, even if they are
            not bound to any endpoints. Failing to list any of
            the intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```
   
   **Certifikáty** oddíl definuje název naše certifikát, její umístění a název úložiště, kde se nachází.
   
   Oprávnění (`permisionLevel` atribut) můžete nastavit na jedno z následujících hodnot:
   
   | Hodnota oprávnění | Popis |
   | --- | --- |
   | limitedOrElevated |**(Výchozí)**  Všechny procesy role můžete přístup k privátnímu klíči. |
   | zvýšené |Jenom procesy se zvýšenými oprávněními, můžete přístup k privátnímu klíči. |
2. V souboru definice služby, přidejte **InputEndpoint** v rámci **koncové body** části Povolit protokol HTTPS:
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. V souboru definice služby, přidejte **vazby** v prvku **lokality** části. Tato část přidá vazbu HTTPS k mapování koncový bod na váš web:
   
    ```xml   
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```
   
   Byly dokončeny všechny požadované změny pro soubor definice služby, ale potřebujete přidat informace o certifikátu do konfiguračního souboru služby.
4. V konfiguračním souboru služby (CSCFG), ServiceConfiguration.Cloud.cscfg, přidejte **certifikáty** části v rámci **Role** části nahraďte ukázkové hodnoty kryptografického otisku vidíte níže se u vašeho certifikátu:
   
    ```xml   
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

(V předchozím příkladu používá **sha1** pro algoritmus kryptografický otisk. Zadejte hodnotu vhodnou pro váš certifikát kryptografický algoritmus.)

Teď, když byly aktualizovány definice služby a služby konfigurační soubory, balíček nasazení nahrát do Azure. Pokud používáte **cspack**, nepoužívejte **/generateConfigurationFile** příznak, jako který přepíše informací o certifikátu, můžete vložit.

## <a name="step-3-upload-a-certificate"></a>Krok 3: Nahrát na server certifikát
Balíček pro nasazení byl aktualizován na použití certifikátu a byl přidán koncový bod HTTPS. Nyní můžete nahrávat certifikátů a balíčků do Azure pomocí portálu Azure classic.

1. Přihlaste se k [portál Azure classic][Azure classic portal]. 
2. Klikněte na tlačítko **cloudové služby** v levém navigačním podokně.
3. Klikněte na požadovanou cloudové služby.
4. Klikněte **certifikáty** kartě.
   
    ![Klikněte na kartu certifikáty](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Klikněte na tlačítko **Nahrát**.
   
    ![Odeslat](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Zadejte **soubor**, **heslo**, pak klikněte na tlačítko **Complete** (na značku zaškrtnutí).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Krok 4: Připojení k instanci role pomocí protokolu HTTPS
Teď, když vaše nasazení je spuštěná v Azure, můžete připojit se pomocí protokolu HTTPS.

1. Na portálu Azure classic, vyberte nasazení a pak klikněte na odkaz v části **adresa URL webu**.
   
   ![Určení adresy URL webu][2]
2. Ve webovém prohlížeči, upravte odkaz použít **https** místo **http**a potom navštivte stránku.
   
   > [!NOTE]
   > Pokud používáte certifikát podepsaný svým držitelem, když přejdete na koncový bod HTTPS, který je spojen s certifikát podepsaný svým držitelem může se zobrazit chyba certifikátu v prohlížeči. Používá certifikát podepsaný důvěryhodnou certifikační autoritou eliminuje tento problém; do té doby můžete chybu ignorovat. (Další možností je přidání certifikátu podepsaného svým držitelem do úložiště certifikátů důvěryhodné certifikační autority uživatele.)
   > 
   > 
   
   ![SSL příklad webové stránky][3]

Pokud chcete používat protokol SSL pro pracovní nasazení místo produkční nasazení, musíte nejdřív určit adresu URL pro pracovní nasazení použít. Nasazení cloudové služby na pracovní prostředí bez zahrnutí certifikát nebo všech informací o certifikátu. Po nasazení můžete určit adresu URL na základě GUID, který je uveden v portálu Azure classic na **adresa URL webu** pole. Vytvoření certifikátu pomocí běžný název (CN) rovnou na adresu URL, na základě GUID (například **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Pomocí portálu Azure classic se přidat certifikát do dvoufázové instalace cloudové služby. Potom CSDEF a CSCFG souborům přidat informace o certifikátu, znovu zabalte aplikaci a aktualizaci dvoufázové nasazení tak, aby pomocí nového balíčku.

## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* [Správa služby cloud](cloud-services-how-to-manage.md).

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
