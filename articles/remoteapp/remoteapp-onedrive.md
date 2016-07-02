<properties
   pageTitle="Integrace OneDrivu pro firmy s Azure RemoteAppem | Microsoft Azure"
   description="Naučte se používat OneDrive pro firmy s Azure RemoteAppem."
   services="remoteapp"
   documentationCenter=""
   authors="pavithir"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="03/31/2016"
   ms.author="elizapo"/>

# Integrace OneDrivu pro firmy s Azure RemoteAppem

OneDrive pro firmy můžete používat jako úložiště souborů pro Azure RemoteApp. OneDrive pro firmy nabízí skvělý způsob, jak mít stále synchronizované soubory mezi všemi vašimi zařízeními a pracovními prostory. [Disk profilu uživatele](remoteapp-upd.md) pro určitého uživatele je místo, kam si mohou uživatelé ukládat své soubory pro aplikace Azure RemoteApp, ale tyto soubory jsou přístupné pouze prostřednictvím Azure RemoteAppu. Na rozdíl od toho OneDrive pro firmy umožňuje uživateli přístup k souborům odkudkoli a kdykoli bez nutnosti přistupovat pomocí Azure RemoteAppu. Tento článek se zabývá podporovanými verzemi OneDrivu pro firmy a různými způsoby, jak mohou správci nastavit OneDrive pro firmy k použití s Azure RemoteAppem.

## Jsou podporovány všechny verze OneDrivu?

Existují dvě verze OneDrivu: OneDrive a OneDrive pro firmy. K použití s Azure RemoteAppem je podporována pouze verze OneDrive pro firmy. Osobní OneDrive sice funguje, ale není oficiálně podporován. Azure RemoteApp (a servery RDSH, Citrix a terminálové servery) také podporují jen nejnovější verze OneDrivu pro firmy (Next Gen Sync Client).

>[AZURE.NOTE]  OneDrive (pro spotřebitele/osobní edice) Azure RemoteApp nepodporuje. Také nejsou podporovány všechny verze OneDrivu pro firmy, protože nebyly certifikovány pro práci v systému Windows Server. I když nový klient (Next Gen Sync Client) i starší verze Groove zdánlivě mohou s Azure RemoteAppem fungovat bez problémů, jak je popsáno v článku [https://support.microsoft.com/cs-cz/kb/2965687](https://support.microsoft.com/kb/2965687), starší synchronizační moduly nebudou plně funkční na serverech Citrix a terminálových serverech (Windows Server).

## Jaké jsou různé možnosti nastavení pro OneDrive pro firmy?

- **Tradiční nastavení synchronizačního modulu OneDrivu pro firmy:** Tato možnost v současnosti (může se změnit) není podporována v nasazeních Azure RemoteAppu, RDSH a Citrixu.
- **„Virtualizace“ OneDrivu pro firmy / přesměrování z místního tlustého klienta do relace:** Pokud synchronizujete OneDrive do složky v klientském zařízení pod určitou jednotkou, můžete zvolit [přesměrování](remoteapp-redirection.md) této jednotky do Azure RemoteAppu. Jednotka by pak měla být stejná ve všech klientech vašich uživatelů a ti by měli mít OneDrive synchronizovaný se složkou pod touto jednotkou. Pokud budou přistupovat k RemoteAppu z jiného klienta, může se jim někdy stát, že tyto soubory nebudou dostupné (náhradním řešením je, že ke svým souborům mají vždy přístup pomocí online verze OneDrivu). 
- **Prezentace OneDrivu pro firmy v prostředí Azure RemoteAppu jako jednotky bez ukládání do mezipaměti a synchronizace souborů:** (namapování adresy URL OneDrivu pro firmy na jednotku ve virtuálním počítači) Je podporováno mapování OneDrivu pro firmy na síťovou jednotku v prostředí RDSH. To lze použít v následujících scénářích: 
    - Pokud jsou pro přístup k Azure RemoteAppu používáni tencí klienti (žádné místní úložiště). Aplikace vyžaduje soubory uložené na OneDrivu pro firmy, ale ty by se měly „tvářit“ jako místní a správce nechce synchronizovat soubory do virtuálního počítače.
    - Pokud je na OneDrivu pro firmy mnoho velkých souborů, které byly vybrány pro synchronizaci. S ohledem na vysokou zátěž synchronizace se může stát, že ne všechny soubory jsou synchronizovány, když je chce uživatel použít. Také pokud celková velikost souborů přesáhne 50 GB, nelze je ukládat do UPD.

Ve výše uvedených scénářích se správci mohou rozhodnout pro možnost mapovat jednotku ve virtuálním počítači na adresu URL OneDrivu pro firmy daného uživatele. Některé možnosti aktivace této funkce:

- Binární soubory Office jsou v imagi a neaktivuje se synchronizační modul OneDrivu pro firmy.
- Binární soubory Office NEJSOU v imagi (tato varianta vyžaduje nainstalovaný balíček Desktopové prostředí). Konkrétně musí být jako součást balíčku Desktopové prostředí nainstalovaná služba WebClient (neboli WebDAV). 

### Instalace balíčku Desktopové prostředí na Windows Server 2012 R2
Postup instalace balíčku Desktopové prostředí: 

1. Ve Správci serveru klikněte na **Spravovat -> Přidat role a funkce**.
2. Klikněte na **Funkce** a potom na **Uživatelská rozhraní a infrastruktura -> Desktopové prostředí**.

### Mapování jednotky na adresu URL OneDrivu pro firmy

Postupujte podle pokynů v následujícím článku podpory: [https://support.microsoft.com/kb/2616712](https://support.microsoft.com/kb/2616712)
 
Důležitým krokem při tomto nastavení je ujistit se, že jste vybrali možnost **Zůstat přihlášeni**.

Hlavní kroky postupu:

1.  Vyhledejte adresu URL pro jednotku. Adresa URL pro mapování jednotky je ta, kterou získáte, když online přejdete do svého domovského adresáře OneDrivu pro firmy. Příklad:
 
    https://microsoft-my.sharepoint.com/personal/alias_microsoft_com/_layouts/15/onedrive.aspx?AjaxDelta=1&isStartPlt1=something
2.  Otevřete adresu URL pomocí prohlížeče v relaci RemoteAppu a před přihlášením k adrese URL pro svůj účet zaškrtněte políčko **Zůstat přihlášeni**.
3.  Spusťte Průzkumníka Windows a namapujete jednotku na výše uvedenou adresu URL. Pokud se adresa URL nepřeloží správně, lze použít kratší formu:
    
    https://microsoft-my.sharepoint.com/personal/alias_microsoft_com. 

    Tím se okamžitě vytvoří mapovaná jednotka – vypadá to takhle:
 
    ![OneDrive pro firmy jako mapovaná síťová jednotka](./media/remoteapp-onedrive/ra-mappeddrive.png)


<!--HONumber=Jun16_HO2-->


