<properties
   pageTitle="Spuštění libovolné aplikace pro Windows v jakémkoliv zařízení s Azure RemoteAppem | Microsoft Azure"
   description="Naučte se sdílet libovolnou aplikaci pro Windows s uživateli pomocí Azure RemoteAppu."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>


# Spuštění libovolné aplikace pro Windows v jakémkoliv zařízení s Azure RemoteAppem

> [AZURE.IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).

Aplikaci pro Windows můžete hned teď spustit kdekoliv a v jakémkoliv zařízení. Neděláme si srandu – stačí použít Azure RemoteApp. Ať se jedná o nějakou vlastní aplikaci napsanou před 10 lety, nebo o aplikaci Office, uživatelé již nemusí vlastnit konkrétní operační systém (např. Windows, XP), aby těchto pár aplikací mohli používat.

S Azure RemoteAppem můžou uživatelé používat svá zařízení Android nebo Apple a mít v nich stejné prostředí jako ve Windows (nebo ve Windows Phone). Toho se dosáhne hostováním aplikací pro Windows v kolekci virtuálních počítačů Windows ve službě Azure – uživatelé k nim budou mít přístup odkudkoliv, kde je připojení k internetu. 

Přečtěte si, jak přesně to provést.

V tomto článku budeme se všemi uživateli sdílet aplikaci Access. Můžete však použít LIBOVOLNOU aplikaci. Pokud lze aplikaci instalovat do počítače se systémem Windows Server 2012 R2, můžete ji sdílet pomocí následujícího postupu. Zkontrolujte [požadavky aplikace](remoteapp-appreqs.md) a ujistěte se, zda bude fungovat.

Poznámka: Protože Access je databázová aplikace a chceme, aby byla užitečná, provedeme několik kroků navíc, které uživatelům umožní přístup ke sdílení dat Accessu. Pokud vaše aplikace není databázová nebo nepotřebujete, aby uživatelé měli přístup ke sdílené složce, můžete tyto kroky kurzu přeskočit

> [AZURE.NOTE] <a name="note"></a>K dokončení tohoto kurzu potřebujete mít účet Azure:
> - Můžete si [otevřít účet Azure zdarma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): Získáte kredity, které můžete použít k vyzkoušení placených služeb Azure. Až je vypotřebujete, můžete si účet nechat a dál používat bezplatné služby Azure, například Websites. Nikdy vám nebudeme účtovat žádné poplatky, pokud si sami nezměníte nastavení a nezačnete používat placené služby.
> - Můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Vaše předplatné MSDN vám každý měsíc dává kredity, které můžete použít k placení za služby Azure.


## Vytvoření kolekce v RemoteAppu

Začněte vytvořením kolekce. Kolekce slouží jako kontejner pro vaše aplikace a uživatele. Každá kolekce je založena na imagi – můžete vytvořit vlastní nebo použít image ve vašem předplatném. Pro účely tohoto kurzu používáme image zkušební verze Office 2013 – obsahuje aplikaci, kterou chceme sdílet.

1. Na portálu Azure přejděte v levém navigační stromu dolů do části RemoteApp. Otevřete danou stránku.
2. Klikněte na tlačítko **Vytvořit kolekci RemoteApp**.
3. Klikněte na tlačítko **Rychlé vytvoření** a zadejte název kolekce.
4. Vyberte oblast, kterou chcete použít k vytvoření kolekce. Pro dosažení co nejlepších výsledků vyberte oblast geograficky co nejblíže k umístění, ze kterého budou uživatelé k aplikaci přistupovat. Například v tomto kurzu se uživatelé budou nacházet v Redmondu ve Washingtonu. Nejbližší oblast Azure je **Západní USA**.
5. Vyberte fakturační plán, který chcete použít. V základním fakturačním plánu jeden velký virtuální počítač Azure používá 16 uživatelů, ve standardním fakturačním plánu používá tento počítač 10 uživatelů. Základní plán obecně dobře funguje pro pracovní postupy zadávání dat. V případě kancelářských aplikací, jako například Office, je vhodnější standardní plán.
6. Nakonec vyberte image Office 2013 Professional. Tento image obsahuje aplikace Office 2013. Jen upozorníme, že tento image je vhodný pouze pro zkušební kolekce a testovací prostředí. Nemůžete ho používat v produkční kolekci.
7. Nyní klikněte na **Vytvořit kolekci RemoteApp**.

![Vytvoření cloudové kolekce v RemoteAppu](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Spustí se vytváření kolekce, což může trvat až hodinu.

Nyní jste připraveni přidat uživatele.

## Sdílení aplikace s uživateli

Po úspěšném vytvoření kolekce je čas na publikování aplikace Access pro uživatele a přidání uživatelů, kteří k ní mají mít přístup.

Pokud jste při vytváření kolekce odešli z uzlu Azure RemoteAppu, je třeba se tam znovu vrátit z domovské stránky Azure.

2. Kliknutím na kolekci, kterou jste dříve vytvořili, získáte přístup k dalším možnostem a ke konfiguraci kolekce.
![Nová cloudová kolekce RemoteApp](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. Na kartě **Publikování** klikněte na **Publikovat** v dolní části obrazovky a pak klikněte na **Publikovat programy v nabídce Start**.
![Publikování aplikace RemoteAppu](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Ze seznamu vyberte aplikace, které chcete publikovat. Pro naše účel jsme zvolili Access. Klikněte na **Dokončit**. Počkejte na dokončení publikování aplikace.
![Publikování aplikace Access v RemoteAppu](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Po dokončení publikování aplikace přejděte na kartu **Uživatelský přístup** a přidejte všechny uživatele, kteří potřebují k aplikaci přístup. Zadejte uživatelská jména (e-mailové adresy) a potom klikněte na **Uložit**.

![Přidání uživatelů do RemoteAppu](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Nyní je čas uživatele o těchto nových aplikacích a o přístupu k nim informovat. Udělejte to tak, že uživatelům pošlete e-mail s odkazem na adresu URL pro stažení klienta vzdálené plochy.
![Adresa URL pro stažení klienta pro RemoteApp](./media/remoteapp-anyapp/ra-anyappurl.png)

## Konfigurace přístupu k Accessu

Některé aplikace po nasazení prostřednictvím RemoteAppu vyžadují další konfiguraci. V případě Accessu vytvoříme ve službě Azure sdílenou složku, ke které bude mít přístup každý uživatel. (Pokud to dělat nechcete, můžete vytvořit [hybridní kolekci](remoteapp-create-hybrid-deployment.md) [namísto naší cloudové kolekce], která uživatelům umožňuje přístup k souborům a informacím v místní síti.) Poté je třeba uživatelům říct, aby namapovali místní jednotku ve svém počítači na systém souborů Azure.

První část, kterou provedete jako správce. Potom máme několik kroků pro vaše uživatele.

1. Začněte publikováním rozhraní příkazového řádku (cmd.exe). Na kartě **Publikování** vyberte možnost **cmd** a potom klikněte na **Publikovat > Publikovat program pomocí cesty**.
2. Zadejte název a cestu aplikace. Pro naše účely použijte jako název „Průzkumník souborů“ a jako cestu „%SYSTEMDRIVE%\windows\explorer.exe“.
![Publikování souboru cmd.exe.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Nyní je třeba vytvořit [účet úložiště](../storage/storage-create-storage-account.md) Azure. Náš jsme pojmenovali „accessstorage“, vy si vyberte jakýkoliv svůj smysluplný název. (Název „accessstorage“ musí být unikátní) ![Náš účet úložiště Azure](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Nyní přejděte zpět na řídicí panel, kde získáte cestu do úložiště (umístění koncového bodu). Za chvilku ji budete potřebovat, takže si ji někam zkopírujte.
![Cesta k účtu úložiště](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Po vytvoření účtu úložiště je nutné zadat primární přístupový klíč. Klikněte na tlačítko **Spravovat přístupové klíče** a poté zkopírujte primární přístupový klíč.
6. Nyní nastavte kontext účtu úložiště a vytvořte novou sdílenou složku pro Access. V okně Windows PowerShell se zvýšenými oprávněními spusťte následující rutiny:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    V případě naší sdílené složky tedy spustíme tyto rutiny:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Nyní jsou na řadě uživatelé. Nejdříve nechte uživatele nainstalovat [klienta RemoteApp](remoteapp-clients.md). Dále uživatelé musí namapovat jednotku ze svého účtu na sdílenou složku Azure, kterou jste vytvořili, a přidat své soubory Access. Udělají to takto:

1. V klientovi RemoteApp přejděte k publikovaným aplikacím. Spusťte program cmd.exe.
2. Spusťte následující příkaz k namapování jednotky z počítače na sdílenou složku:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Pokud nastavíte parametr**/persistent** na hodnotu „yes“, mapovaná jednotka se zachová napříč relacemi.
1. Nyní z RemoteAppu spusťte aplikaci Průzkumník souborů. Do sdílené složky zkopírujte všechny soubory Accessu, které chcete používat ve sdílené aplikaci.
![Umístění souborů Accessu do sdílené složky Azure](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Nakonec otevřete Access a pak otevřete databázi, kterou jste právě nasdíleli. V Accessu by se měla zobrazit vaše data spuštěná z cloudu.
![Skutečná databáze Access spuštěná z cloudu](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Nyní můžete Access používat v libovolném zařízení – stačí si jen nainstalovat klienta RemoteApp.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Další kroky

Teď, když jste zvládli vytvoření kolekce, zkuste vytvořit [kolekci používající Office 365](remoteapp-tutorial-o365anywhere.md). Nebo můžete vytvořit [hybridní kolekci ](remoteapp-create-hybrid-deployment.md), která má přístup k místní síti.

<!--Image references-->
 



<!--HONumber=Sep16_HO3-->


