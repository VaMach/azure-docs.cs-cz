---
title: "Nastavit zotavení po havárii pro virtuální počítače Azure sekundární oblasti Azure s Azure Site Recovery (Preview)"
description: "Zjistěte, jak nastavit zotavení po havárii pro virtuální počítače Azure do jiné oblasti Azure, pomocí služby Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 2608e0e0c87df1e7c6d034cf0977ed0e16b128cf
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region-preview"></a>Nastavit zotavení po havárii pro virtuální počítače Azure sekundární oblasti Azure (Preview)

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategie zotavení po havárii správy a Orchestrace replikace, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

V tomto kurzu se dozvíte, jak nastavit zotavení po havárii sekundární oblasti Azure pro virtuální počítače Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření trezoru Služeb zotavení
> * Ověřte nastavení cílového prostředku
> * Nastavit odchozí přístup pro virtuální počítače
> * Povolení replikace pro virtuální počítač

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

- Ujistěte se, že rozumíte [scénář architektura a komponenty](concepts-azure-to-azure-architecture.md).
- Zkontrolujte [podporu požadavků](site-recovery-support-matrix-azure-to-azure.md) pro všechny součásti.

## <a name="create-a-vault"></a>Vytvoření trezoru

Vytvořte trezor nejprve v libovolné oblasti, s výjimkou oblasti zdroje.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klikněte na **Nový** > **Monitorování a správa** > **Backup a Site Recovery**.
3. Do pole **Název** zadejte popisný název pro identifikaci trezoru. Pokud máte více než jedno předplatné, vyberte příslušný.
4. Vytvořte skupinu prostředků nebo vyberte nějaký existující. Zadejte oblast Azure. Informace o tom, které oblasti jsou podporované, najdete v části s geografickou dostupností v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Chcete-li rychle získat přístup k úložišti z řídicího panelu, klikněte na tlačítko **připnout na řídicí panel** a pak klikněte na **vytvořit**.

   ![Nový trezor](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   Nový trezor se přidá do **řídicí panel** pod **všechny prostředky**a v hlavním **trezory služeb zotavení** stránky.

## <a name="verify-target-resources"></a>Zkontrolujte cílové prostředky

1. Ověřte, že vaše předplatné Azure umožňuje vytvářet virtuální počítače v cílová oblast pro obnovení po havárii. Obraťte se na podporu, aby umožnil požadované kvótu.

2. Zajistěte, aby že vaše předplatné nemá dostatek prostředků pro podporu virtuálních počítačů s velikostí, které odpovídají zdrojové virtuální počítače. Site Recovery vybere stejnou velikost nebo nejbližší možné velikost pro cílový počítač.

## <a name="configure-outbound-network-connectivity"></a>Konfigurace odchozího síťového připojení

Site Recovery k fungovat podle očekávání budete muset provést některé změny v odchozí síťové připojení z virtuálních počítačů, které chcete replikovat.

- Site Recovery nepodporuje použití proxy služby ověřování pro připojení k síti ovládacího prvku.
- Pokud máte proxy služby ověřování, nelze povolit replikaci.

### <a name="outbound-connectivity-for-urls"></a>Odchozí připojení pro adresy URL

Pokud používáte proxy server brány firewall založená na adresu URL k řízení odchozí připojení, povolí přístup k následujícím adresám URL používá Site Recovery.

| **ADRESA URL** | **Podrobnosti** |
| ------- | ----------- |
| *.blob.core.windows.net | Umožňuje zápis z virtuálního počítače k účtu úložiště mezipaměti v oblasti zdroj dat. |
| Login.microsoftonline.com | Poskytuje autorizaci a ověřování adresy URL služby Site Recovery. |
| *.hypervrecoverymanager.windowsazure.com | Umožňuje virtuálnímu počítači komunikovat se službou Site Recovery. |
| *. servicebus.windows.net | Umožňuje virtuálních počítačů k zápisu monitorování Site Recovery a diagnostická data. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Odchozí připojení pro rozsahy IP adres

Při použití jakékoli založenou na protokolu IP brány firewall, proxy serveru nebo pravidla NSG k řízení odchozí připojení, musí být seznam povolených adres následující rozsahy IP adres. Seznam rozsahů stáhněte z následujících odkazů:

  - [Rozsahy Datacenter IP Microsoft Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Rozsahy Azure Datacenter IP v Německu s Windows](http://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Windows Azure Datacenter IP rozsahy v Číně](http://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Adresy URL a rozsahy IP adres pro Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Koncový bod služby Site Recovery IP adres](https://aka.ms/site-recovery-public-ips)

Využijte tyto seznamy pro konfiguraci ovládacích prvků přístupu k síti ve vaší síti. Můžete to použít [skriptu](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) k vytvoření požadované pravidla NSG.

## <a name="verify-azure-vm-certificates"></a>Ověření certifikátů virtuálního počítače Azure

Zkontrolujte, zda všechny nejnovější kořenové certifikáty jsou k dispozici v systému Windows nebo je chcete replikovat virtuální počítače s Linuxem. Pokud nejsou nejnovější kořenové certifikáty, virtuální počítač nelze zaregistrovaný Site Recovery náležitý omezení zabezpečení.

- Pro virtuální počítače Windows nainstalujte všechny nejnovější aktualizace systému Windows na virtuální počítač tak, aby byly všechny důvěryhodných kořenových certifikátů na počítači. V odpojeném prostředí postupujte podle standardní Windows Update a procesy aktualizace certifikátů pro vaši organizaci.

- Pro virtuální počítače s Linuxem využijte tohoto vodítka poskytované vaší Linux distributora, chcete-li získat nejnovější důvěryhodných kořenových certifikátů a seznam odvolaných certifikátů ve virtuálním počítači.

## <a name="set-permissions-on-the-account"></a>Nastavení oprávnění pro účet

Azure Site Recovery poskytuje tři předdefinovaných rolí k řízení operace správy Site Recovery.

- **Lokality obnovení Přispěvatel** – tato role má všechna oprávnění vyžadovaná ke správě operací Azure Site Recovery v trezoru služeb zotavení. Uživatel k této roli, ale nelze vytvořit nebo odstranit trezor služeb zotavení nebo přiřadit přístupová práva ostatním uživatelům. Tato role je nejvhodnější pro správce obnovení po havárii, kteří můžete povolit a spravovat zotavení po havárii aplikace nebo celé organizace.

- **Operátor obnovení lokality** – tato role nemá oprávnění ke spuštění a převzetí služeb při selhání a navrácení služeb po obnovení operations manager. K této roli uživatele nelze povolit nebo zakázat replikaci, vytvořit nebo odstranit trezory, zaregistrujte novou infrastrukturu nebo přiřadit přístupová práva jiným uživatelům. Tato role je nejvhodnější pro operátor obnovení po havárii, kdo může převzít virtuální počítače nebo aplikace při pokyn vlastníci aplikace a správci IT. Po vyřešení po havárii, operátor zotavení po Havárii můžete znovu aktivujte ochranu a navrácení služeb po obnovení virtuálních počítačů.

- **Lokality obnovení čtečky** – tato role nemá oprávnění k zobrazení všech operací správy Site Recovery. Tato role je nejvhodnější pro monitorování vedení IT, který můžete sledovat aktuální stav ochrany a zvýšení lístky žádostí o podporu.

Další informace na [předdefinované role Azure RBAC](../active-directory/role-based-access-built-in-roles.md)

## <a name="enable-replication"></a>Povolení replikace

### <a name="select-the-source"></a>Vyberte zdroj

1. V trezory služeb zotavení, klikněte na název trezoru > **+ replikovat**.
2. V **zdroj**, vyberte **Azure - PREVIEW**.
3. V **umístění zdroje**, vyberte zdroj oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.
4. Vyberte **modelu nasazení virtuálního počítače Azure** pro virtuální počítače: **Resource Manager** nebo **Classic**.
5. Vyberte **zdrojové skupiny prostředků** pro správce prostředků virtuálních počítačů, nebo **Cloudová služba** pro klasické virtuální počítače.
6. Kliknutím na **OK** uložte nastavení.

### <a name="select-the-vms"></a>Vyberte virtuální počítače

Site Recovery načte seznam virtuálních počítačů spojené s předplatného a skupiny nebo cloudové služby prostředků.

1. V **virtuální počítače**, vyberte virtuální počítače, které chcete replikovat.
2. Klikněte na **OK**.

### <a name="configure-replication-settings"></a>Konfigurace nastavení replikace

Site Recovery vytvoří výchozí nastavení a zásady replikace pro cílová oblast. Můžete změnit nastavení podle svých požadavků.

1. Klikněte na tlačítko **nastavení** zobrazíte nastavení cíle.
2. Chcete-li přepsat výchozí nastavení cíl, klikněte na tlačítko **přizpůsobit**. 

![Konfigurace nastavení](./media/azure-to-azure-tutorial-enable-replication/settings.png)


- **Cílové umístění**: cílová oblast pro obnovení po havárii. Doporučujeme vám, že cílové umístění odpovídá umístění trezoru Site Recovery.

- **Cílová skupina prostředků**: skupinu prostředků cílová oblast, která obsahuje virtuální počítače Azure po převzetí služeb při selhání. Ve výchozím nastavení Site Recovery vytvoří novou skupinu prostředků v cílové oblasti s příponou "Automatické obnovení systému".

- **Cílová virtuální síť**: síť v oblasti cíl, že jsou virtuální počítače umístěné po převzetí služeb při selhání.
  Ve výchozím nastavení Site Recovery vytvoří nové virtuální sítě (a podsítě) v oblasti cíl s příponou "Automatické obnovení systému".

- **Účty úložiště mezipaměti**: Site Recovery používá účet úložiště v oblasti zdroje. Změny zdrojové virtuální počítače odešlou k tomuto účtu před replikace do cílového umístění.

- **Cíl účty úložiště**: ve výchozím nastavení, Site Recovery vytvoří nový účet úložiště v cílová oblast pro zrcadlení zdrojový účet úložiště virtuálního počítače.

- **Cílové skupiny dostupnosti**: ve výchozím nastavení vytvoří Site Recovery novou skupinou dostupnosti ve cílová oblast s příponou "Automatické obnovení systému". Skupiny dostupnosti můžete přidat pouze pokud virtuální počítače jsou součástí sady v oblasti zdroje.

- **Název zásady replikace**: název zásady.

- **Uchování bodu obnovení**: ve výchozím nastavení, Site Recovery zachová body obnovení po dobu 24 hodin. Můžete nakonfigurovat hodnotu v rozmezí 1 až 72 hodin.

- **Frekvence snímkování konzistentní aplikace vzhledem**: ve výchozím nastavení, Site Recovery vytváří konzistentní snímek každé 4 hodiny. Můžete nakonfigurovat libovolnou hodnotu mezi 1 a 12 hodin. Snímky konzistentní s aplikací je v okamžiku snímek dat aplikací ve virtuálním počítači. Služby Stínová kopie svazku (VSS) zajišťuje, že aplikace ve virtuálním počítači budou při pořízení snímku v konzistentním stavu.

### <a name="track-replication-status"></a>Sledovat stav replikace

1. V **nastavení**, klikněte na tlačítko **aktualizovat** a získat nejnovější stav.

2. Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.

3. V **nastavení** > **replikované položky**, můžete zobrazit stav virtuálních počítačů a probíhá počáteční replikace. Klikněte na virtuální počítač můžete rozbalit jeho nastavení.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali zotavení po havárii pro virtuální počítač Azure. Dalším krokem je konfigurace.

> [!div class="nextstepaction"]
> [Spuštění postupu zotavení po havárii](azure-to-azure-tutorial-dr-drill.md)
