---
title: "Podpora více klientů pro replikaci virtuálních počítačů VMware do Azure (CSP program) | Microsoft Docs"
description: "Popisuje, jak nasadit Azure Site Recovery v prostředí s více klienty k orchestraci replikace, převzetí služeb při selhání a obnovení virtuálních počítačů (VM) místní VMware do Azure prostřednictvím programu CSP pomocí portálu Azure"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: manayar
ms.openlocfilehash: 97edbe67c25036dc1156f0f0ca5431a617d7a004
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>Podpora více klientů ve službě Azure Site Recovery pro replikaci virtuálních počítačů VMware do Azure pomocí zprostředkovatele kryptografických služeb

Azure Site Recovery podporuje víceklientské prostředí odběrů klienta. Podporuje také více klientů pro klienta předplatné, které jsou vytvořeny a spravovat prostřednictvím programu Microsoft Cloud Solution Provider (CSP). Tento článek podrobně popisuje pokyny pro implementaci a správu scénáře víceklientské VMware do Azure. Také vysvětluje vytváření a Správa předplatného klienta prostřednictvím poskytovatele CSP.

V tomto návodu nevykresluje výraznou z ve stávající dokumentaci pro replikaci virtuálních počítačů VMware do Azure. Další informace najdete v tématu [virtuálních počítačů VMware replikovat do Azure pomocí Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Víceklientské prostředí
Existují tři hlavní modely více klientů:

* **Sdílené hostování poskytovatele služeb (HSP)**: partnerovi vlastní fyzické infrastruktuře a používá sdílené prostředky (vCenter, datových center, fyzického úložiště a tak dále) pro hostování virtuálních počítačů více klienty ve stejné infrastruktuře. Partner můžete zadat zotavení po havárii správu jako spravované služby, nebo klienta můžete vlastní zotavení po havárii jako řešení samoobslužné služby.

* **Vyhrazené hostování zprostředkovatele služeb**: partnerovi vlastní fyzické infrastruktuře, ale používá vyhrazených prostředcích (více Vcenter, fyzické datastores a tak dále) pro hostování virtuálních počítačů každého klienta v oddělené infrastruktury. Partner můžete zadat zotavení po havárii správu jako spravované služby, nebo klienta můžete vlastní jako řešení samoobslužné služby.

* **Spravované služby zprostředkovatele (MSP)**: zákazník vlastní fyzické infrastruktury, který je hostitelem virtuálních počítačů a partnerovi poskytuje povolování zotavení po havárii a pro správu.

## <a name="shared-hosting-multi-tenant-guidance"></a>Sdílené hostování víceklientské pokyny
Tato část popisuje scénář sdílené hostování podrobně. Příslušné dva scénáře jsou podmnožinou tohoto scénáře sdílené hostování a používají stejné zásady. Na konci pokyny sdílené hostování jsou popsané rozdíly.

Základní požadavek ve scénáři více klientů, je izolovat různé klienty. Jeden klient by neměl být schopný sledovat, co má atribut hosted jiného klienta. Tento požadavek není důležité, protože je v samoobslužné prostředí, kde může být rozhodující, v prostředí spravovaná partnerem. Tyto pokyny předpokládají, že je vyžadována izolaci klientů.

Architektura je uvedené v následujícím diagramu:

![Sdílené HSP s jeden vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Sdílené hostování scénář se jeden vCenter**

Jak je vidět na předchozím obrázku, každý zákazník má server pro správu samostatné. Tato konfigurace omezuje přístup klientů k virtuálním počítačům konkrétního klienta a umožňuje izolaci klientů. Scénář replikaci virtuálního počítače VMware používá konfigurační server Pokud chcete spravovat účty pro vyhledání virtuálních počítačů a nainstalovat agenty. Nemůžeme se řídí stejnými zásadami pro víceklientské prostředí, a uveďte omezení zjišťování virtuálních počítačů prostřednictvím vCenter řízení přístupu.

Požadavek na data izolace vyžaduje, aby klientům zveřejněny všechny informace o citlivých infrastruktury (například přihlašovací údaje). Z tohoto důvodu doporučujeme, aby všechny součásti serveru pro správu nadále výhradní řídit partnera. Součásti serveru správy jsou:
* Konfigurační server (CS)
* Procesový server (PS)
* Hlavní cílový server (MT) 

PS Škálováním na více systémů je také pod kontrolou partnera.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Každý CS ve scénáři víceklientské používá dva účty

- **účet pro přístup k systému vCenter**: Tento účet použít ke zjištění klientské virtuální počítače. Má oprávnění k přístupu vCenter přiřazen (jak je popsáno v další části). Abyste se vyhnuli přístup náhodných úniků, doporučujeme, partneři zadali tyto přihlašovací údaje, sami v konfiguračním nástroji služby.

- **Účet pro přístup k virtuálnímu počítači**: Tento účet použít na virtuální počítače klientů pomocí automatické nabízené instalace agenta mobility. Je obvykle účet domény, že klient může poskytovat na partnera nebo že, případně partnerovi může spravovat přímo. Pokud klient není chcete sdílet podrobnosti s partnerovi přímo, možné můžete mu zadejte přihlašovací údaje prostřednictvím časově omezenou přístupu k CS nebo s jeho pomocí nainstalovat agenty mobility ručně.

### <a name="requirements-for-a-vcenter-access-account"></a>Požadavky pro účet přístupu k systému vCenter

Jak je uvedeno v předchozí části, musíte nakonfigurovat účet, který má speciální role přiřazené CS. Přiřazení role musí být použít účet přístupu k vCenter pro každý objekt vCenter a nebyl rozšířen do podřízených objektů. Tato konfigurace zajišťuje izolaci klientů, vzhledem k šíření přístup může být v náhodných přístup k ostatním objektům.

![Propagate možnosti podřízené objekty](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

Alternativou je přiřazení role v objektu datacenter a uživatelský účet a šířit je do podřízených objektů. Potom nového účtu *žádný přístup* role pro každý objekt (například virtuální počítače ostatních klientů), která má být dostupná pro konkrétního klienta. Tato konfigurace je náročná a zpřístupňuje ovládacích prvků náhodných přístup, protože každý nový podřízený objekt je také automaticky udělen přístup, který dědí z nadřazeného objektu. Proto doporučujeme použít první metodu.

Postup přístup účtu vCenter je následující:

1. Vytvoření nové role klonováním předem definované *jen pro čtení* role a pak mu pohodlný název (například Azure_Site_Recovery, jak je znázorněno v tomto příkladu).

2. K této roli přiřaďte následující oprávnění:

    * **Úložiště dat**: přidělit prostor, procházet úložiště, operace nízké úrovně souborů, odebrat soubor, soubory aktualizace virtuálního počítače

    * **Sítě**: sítě přiřazení

    * **Prostředek**: používá technologii migrací přiřazení virtuálního počítače do fondu zdrojů, migrací vypnout virtuální počítač, na virtuálním počítači

    * **Úlohy**: vytvoření úlohy, úloha aktualizace

    * **Virtuální počítač**: 
        * Konfigurace > všechny
        * Interakce > odpovědět na otázku, připojení zařízení, nakonfigurovat CD média, konfigurovat disketová média, vypnutí napájení, instalaci nástroje VMware
        * Inventář > vytvořit z existujícího, vytvořit nový, registraci, zrušení registrace
        * Zřizování > Povolit stahování virtuálního počítače, nahrávání souborů virtuálního počítače povolit
        * Správa snímků > odebrat snímky

    ![Dialogové okno Upravit Role](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Přiřadíte úrovně přístupu k účtu vCenter (používá se v klientovi CS) pro různé objekty, následujícím způsobem:

>| Objekt | Role | Poznámky |
>| --- | --- | --- |
>| vCenter | Jen pro čtení | Potřeba pouze pro povolení přístupu vCenter pro správu různé objekty. Toto oprávnění může odebrat, pokud účet nikdy bude potřeba poskytnout klienta nebo použít pro žádné operace správy na serveru vCenter. |
>| Datacentrum | Azure_Site_Recovery |  |
>| Hostitele a cluster hostitelů | Azure_Site_Recovery | Znovu zajišťuje, že přístup na úrovni objektu, tak, aby pouze přístupné hostitelů klientské virtuální počítače před převzetí služeb při selhání a po navrácení služeb po obnovení. |
>| Úložiště dat a úložiště clusteru | Azure_Site_Recovery | Stejné jako předcházející. |
>| Síť | Azure_Site_Recovery |  |
>| Server pro správu | Azure_Site_Recovery | Poskytuje přístup k všechny součásti (CS, PS a MT), pokud jsou některé mimo počítač CS. |
>| Virtuální počítače klienta | Azure_Site_Recovery | Zajišťuje, že všechny nové klientské virtuální počítače konkrétního klienta také získat tento přístup, nebo nebude zjistitelný prostřednictvím portálu Azure. |

Přístup k účtu vCenter je nyní dokončen. Tento krok splní požadavek na minimální oprávnění k dokončení operace navrácení služeb po obnovení. Můžete taky těchto oprávnění k přístupu s existující zásady. Právě upravte vaše stávající oprávnění nastavit, aby zahrnovalo oprávnění role z kroku 2, podrobné dříve.

Chcete-li omezit operace zotavení po havárii, dokud stav převzetí služeb při selhání (to znamená, bez možnosti navrácení služeb po obnovení), postupujte podle předchozích pokynů, s výjimkou: místo přiřazení *Azure_Site_Recovery* pouze přiřazení role účet pro přístup k systému vCenter *jen pro čtení* role k tomuto účtu. Této sadě oprávnění umožňuje replikace virtuálního počítače a převzetí služeb při selhání a navrácení služeb po obnovení není povolen. Všem ostatním v předchozím procesu zůstane, jako je. Každých oprávnění k zajištění izolaci klientů a omezte zjišťování virtuálních počítačů, je stále přidělený pouze na úrovni objekt a není rozšíří do podřízených objektů.

## <a name="other-multi-tenant-environments"></a>Dalších prostředích s více klienty

V předchozí části je popsané, jak nastavit prostředí s více klienty pro sdílené hostování řešení. Další dvě hlavní řešení jsou vyhrazené hostování a spravované služby. Architektura pro těchto řešení je popsána v následujících částech.

### <a name="dedicated-hosting-solution"></a>Vyhrazené řešení v oblasti hostování

Jak je znázorněno v následujícím diagramu, architektury rozdíl ve vyhrazené hostingu řešení je, je pro tohoto klienta pouze nastavili infrastruktury každého klienta. Vzhledem k tomu, že klienti jsou izolované prostřednictvím samostatné Vcenter, poskytovatele hostingu musí stále postupujte podle kroků CSP zadaná pro sdílené hostování, ale není nutné se obávat izolaci klientů. Instalační program zprostředkovatele kryptografických služeb zůstává beze změny.

![Architektura sdílené hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Vyhrazený hostitelský scénář s více Vcenter**

### <a name="managed-service-solution"></a>Řešení spravované služby

Jak je znázorněno v následujícím diagramu, architektury rozdíl v řešení spravované služby je každý klient infrastruktury je také fyzicky oddělená od ostatních klientů infrastruktury. Tento scénář obvykle existuje, když klient vlastní infrastrukturu a chce poskytovatele řešení pro správu zotavení po havárii. Znovu protože klienti jsou fyzicky izolované prostřednictvím různých infrastruktury, je nutné partnera postupujte podle kroků CSP k dispozici pro sdílené hostování ale není nutné se obávat izolaci klientů. Zřizování CSP zůstává beze změny.

![Architektura sdílené hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Spravované služby scénář s více Vcenter**

## <a name="csp-program-overview"></a>Přehled programu zprostředkovatele kryptografických služeb
[CSP program](https://partner.microsoft.com/en-US/cloud-solution-provider) podporovalo lepší společně scénářů, které nabízejí partnery všem cloudovým službám Microsoftu, včetně služeb Office 365, Enterprise Mobility Suite a Microsoft Azure. S CSP našimi partnery vlastní začátku do konce relace se zákazníky a stát kontaktní bod primární relace. Partnery můžete nasadit předplatná Azure pro zákazníky a kombinovat předplatných s vlastní s přidanou hodnotou, přizpůsobené nabídky.

Azure Site Recovery partneři můžou spravovat úplného řešení zotavení po havárii pro zákazníky přímo přes zprostředkovatele kryptografických služeb. Nebo nastavit prostředí Site Recovery a Informujte zákazníky, spravovat jejich vlastní potřeby zotavení po havárii způsobem samoobslužné služby mohou použít CSP. V obou případech partneři jsou spolupráci mezi Site Recovery a zákazníků. Partneři služby vztah se zákazníkem a vyúčtování pro zákazníky za použití Site Recovery.

## <a name="create-and-manage-tenant-accounts"></a>Vytvořit a spravovat účty klientů

### <a name="step-0-prerequisite-check"></a>Krok 0: Kontrola předpokladů

Požadavky virtuálního počítače jsou stejné, jak je popsáno v [dokumentace Azure Site Recovery](site-recovery-vmware-to-azure.md). Kromě těchto požadavků byste měli mít řízení výše uvedených přístupu na místě, než budete pokračovat se správou klientů prostřednictvím poskytovatele CSP. Pro každého klienta vytvořte samostatné management server, který může komunikovat s klientské virtuální počítače a jeho vCenter. Pouze partner má přístupová práva k tomuto serveru.

### <a name="step-1-create-a-tenant-account"></a>Krok 1: Vytvoření účtu klienta

1. Prostřednictvím [Microsoft Partner Center](https://partnercenter.microsoft.com/), přihlaste se ke svému účtu CSP. 
 
2. Na **řídicí panel** nabídce vyberte možnost **zákazníci**.

    ![Odkaz zákazníků Center partnera společnosti Microsoft](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

3. Na stránce, které se otevře, klikněte na **přidat zákazníka** tlačítko.

    ![Tlačítko Přidat zákazníka](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

4. Na **nového zákazníka** stránky, zadejte podrobnosti informace o účtu pro klienta a pak klikněte na tlačítko **Další: odběry**.

    ![Stránka informace o účtu](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

5. Na stránce Výběr předplatných, vyberte **Microsoft Azure** zaškrtávací políčko. Můžete přidat další odběry nyní nebo kdykoli později.

    ![Zaškrtněte políčko předplatné Microsoft Azure](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

6. Na **zkontrolujte** Potvrďte podrobnosti klienta a pak klikněte na tlačítko **odeslání**.

    ![Kontrolní stránku](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)  

    Po vytvoření účtu klienta, se zobrazí potvrzovací stránku, zobrazení podrobností o výchozí účet a heslo pro toto předplatné. 

7. Uložte si informace a změnit heslo později podle potřeby prostřednictvím Azure přihlašovací stránky portálu.  
 
    Tyto informace můžete sdílet s klientem, jako je, nebo můžete vytvořit a sdílet samostatný účet, v případě potřeby.

### <a name="step-2-access-the-tenant-account"></a>Krok 2: Přístup k účtu klienta

Předplatné klienta můžete přistupovat prostřednictvím řídicím panelu Microsoft Partner Center, jak je popsáno v "krok 1: vytvoření účtu klienta." 

1. Přejděte na **zákazníci** stránky a pak klikněte na název účtu klienta.

2. Na **odběry** stránky účtu klienta, můžete sledovat existující odběry účet a přidat další odběry, podle potřeby. Chcete-li spravovat operace zotavení po havárii klienta, vyberte **všechny prostředky (portál Azure)**.

    ![Odkaz všechny prostředky](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)  
    
    Kliknutím na tlačítko **všechny prostředky** uděluje přístup k předplatným Azure klienta. Přístup můžete ověřit kliknutím na odkaz Azure Active Directory v horní pravé části portálu Azure.

    ![Azure Active Directory odkaz](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

Teď můžete provádět všechny operace obnovení lokality pro klienta prostřednictvím portálu Azure a spravovat operace zotavení po havárii. Přístup k předplatnému klienta prostřednictvím zprostředkovatele kryptografických služeb pro zotavení po havárii spravované, postupujte podle procesu bylo popsáno dříve.

### <a name="step-3-deploy-resources-to-the-tenant-subscription"></a>Krok 3: Nasaďte prostředky k předplatnému klienta
1. Na portálu Azure vytvořte skupinu prostředků a pak nasadit trezoru služeb zotavení pro obvyklé procesy. 
 
2. Stáhnout registrační klíč trezoru

3. Zaregistrujte CS pro klienta pomocí registračního klíče trezoru.

4. Zadejte pověření pro dva přístupové účty: účet pro přístup k systému vCenter a virtuální počítač přístup k účtu.

    ![Účty serveru configuration Manager](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>Krok 4: Registrace Site Recovery infrastruktury do trezoru služeb zotavení
1. Na portálu Azure v úložišti, který jste vytvořili dříve, zaregistrujte server vCenter pro CS, který je zaregistrovaný v "krok 3: nasazení prostředků k předplatnému klienta." Použijte účet přístupu k vCenter pro tento účel.
2. Dokončení procesu "Připravit infrastrukturu" pro obnovení lokality podle obvyklé procesu.
3. Virtuální počítače jsou nyní připraveny k replikaci. Ověřte, že se zobrazují jenom virtuální počítače klienta na **vybrat virtuální počítače** okno pod **replikovat** možnost.

    ![Seznam virtuálních počítačů klienta v okně vyberte virtuální počítače](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-to-the-subscription"></a>Krok 5: Přiřadit klientovi přístup k předplatnému

Samoobslužné služby zotavení po havárii, zadejte pro klienta účet podrobnosti, jak je uvedeno v kroku 6 "krok 1: vytvoření účtu klienta" oddílu. Tuto akci proveďte, jakmile partnerovi nastaví infrastruktury zotavení po havárii. Zda typ zotavení po havárii je spravované nebo samoobslužné služby, musí partneři přístup klienta odběry prostřednictvím portálu CSP. Jejich nastavení pro zařízení vlastněná partnera trezoru a zaregistrujte infrastrukturu pro odběry klienta.

Partnery můžete také přidat nového uživatele k předplatnému klienta prostřednictvím portálu CSP následujícím způsobem:

1. Přejděte na stránku odběru klienta CSP a potom vyberte **uživatelé a licence** možnost.

    ![Stránku odběru CSP klienta](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    Nyní můžete vytvořit nového uživatele zadáním příslušné podrobnosti a výběrem oprávnění nebo tím, že nahrajete seznam uživatelů v souboru CSV.

2. Po vytvoření nového uživatele, přejděte zpět na portál Azure a potom na **předplatné** okně vyberte příslušné předplatné.

3. V okně, které se otevře, vyberte **řízení přístupu (IAM)**a potom klikněte na **přidat** přidat uživatele, který má úroveň přístupu relevantní.      
    Uživatelé, které byly vytvořeny prostřednictvím portálu CSP se automaticky zobrazí v okně, které se otevře po kliknutí na tlačítko úroveň přístupu.

    ![Přidání uživatele](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    Pro většinu operací správy *Přispěvatel* role je dostačující. Uživatelé s touto úrovní přístupu může dělat všechno na předplatné s výjimkou Změna úrovně přístupu (pro které *vlastníka*– úroveň přístupu se vyžaduje). Můžete také optimalizovat úrovně přístupu podle potřeby.
