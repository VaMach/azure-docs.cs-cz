---
title: "Konfigurace clusterů HDInsight připojený k doméně pomocí nástroje PowerShell – Azure | Microsoft Docs"
description: "Zjistěte, jak připravit a nakonfigurovat clustery HDInsight připojený k doméně pomocí Azure PowerShell"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a13b2f7a-612d-4800-bc92-7fc0524f3e89
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 9da76bb5f649817cd2f027f3d0eb46d58a996b4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview-using-azure-powershell"></a>Konfigurace clusterů HDInsight připojený k doméně (Preview) pomocí prostředí Azure PowerShell
Zjistěte, jak nastavit cluster Azure HDInsight s Azure Active Directory (Azure AD) a [Apache škálu](http://hortonworks.com/apache/ranger/) pomocí Azure PowerShell. Skript Azure PowerShell zajišťuje, aby konfigurace rychlejší a méně chyba náchylné k chybám. HDInsight připojený k doméně se dá nakonfigurovat jenom na clusterech se systémem Linux. Další informace najdete v tématu [clustery HDInsight připojený k doméně zavádět](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie není povoleno v doméně HDInsight.

Obvyklá konfigurace clusteru připojený k doméně HDInsight zahrnuje následující kroky:

1. Vytvoření Azure klasické virtuální sítě pro vaši službu Azure AD.  
2. Vytvoření a konfigurace Azure AD a Azure AD DS.
3. Přidejte virtuální počítač do klasické virtuální sítě pro vytvoření organizační jednotky. 
4. Vytvořte organizační jednotku pro Azure AD DS.
5. Vytvoření virtuální sítě HDInsight v režimu správy prostředků Azure.
6. Instalace služby Azure AD DS reverzním systému DNS zóny.
7. Sdílené dvou virtuálních sítí.
8. Vytvoření clusteru HDInsight.

Zadaný skript prostředí PowerShell provádí kroky 3 až 7. Musíte přejít do kroku 1 a 2 ručně.  Pokud nechcete použít Azure PowerShell, přečtěte si téma [clustery HDInsight připojený k doméně nakonfigurovat](hdinsight-domain-joined-configure.md). 

Příklad konečné topologie vypadá takto:

![Topologie HDInsight připojený k doméně](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Protože Azure AD aktuálně podporuje pouze klasické virtuální sítě (virtuální sítě) a podporuje se jen clustery HDInsight se systémem Linux Azure Resource Manager na základě virtuálních sítí, integrace HDInsight Azure AD vyžaduje dvě virtuální sítě a partnerský vztah mezi nimi. Porovnání informace mezi modely dvě nasazení najdete v tématu [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a stav svých prostředků](../azure-resource-manager/resource-manager-deployment-model.md). Dvě virtuální sítě musí být ve stejné oblasti jako služba Azure AD DS.

> [!NOTE]
> Tento kurz předpokládá, že nemáte Azure AD. Pokud nemáte, můžete přeskočit část v kroku 2.
> 
> 

## <a name="prerequisites"></a>Požadavky
Následující položky, které chcete absolvovat tento kurz, musíte mít:

* Seznamte se s [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) jeho [ceny](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktura.
* Zajistěte, aby vaše předplatné seznam povolených adres pro tuto verzi public preview. Můžete to provést pomocí e-mailu na hdipreview@microsoft.com s vaším ID předplatného.
* Certifikát SSL, který je podepsaný podpisový autoritou pro vaši doménu. Certifikát je požadován konfigurace zabezpečeného LDAP. Nelze použít certifikáty podepsané svým držitelem.
* Azure Powershell  V tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).

## <a name="create-an-azure-classic-vnet-for-your-azure-ad"></a>Vytvoření Azure klasické virtuální sítě pro vaši službu Azure AD.
Pokyny najdete v tématu [zde](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic).

## <a name="create-and-configure-azure-ad-and-azure-ad-ds"></a>Vytvoření a konfigurace Azure AD a Azure AD DS.
Pokyny najdete v tématu [zde](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).

## <a name="run-the-powershell-script"></a>Spuštění powershellového skriptu
Skript prostředí PowerShell si můžete stáhnout z [Githubu](https://github.com/hdinsight/DomainJoinedHDInsight). Extrahování souboru zip a ukládat soubory lokálně.

**Chcete-li upravit skript prostředí PowerShell**

1. Pomocí Windows PowerShell ISE nebo libovolného textového editoru otevřete run.ps1.
2. Zadejte hodnoty pro následující proměnné:
   
   * **$SubscriptionName** – název předplatného Azure, kde chcete vytvořit HDInsight cluster. Jste již vytvořili klasické virtuální síti v tomto předplatném a bude vytvářet virtuální síť Azure Resource Manageru pro cluster HDInsight v rámci předplatného.
   * **$ClassicVNetName** -klasickou virtuální síť, která obsahuje služba Azure AD DS. Tato virtuální síť musí být ve stejném předplatném, které je uvedené výše. Tato virtuální síť musí být vytvořena pomocí portálu Azure a ne pomocí portálu classic. Pokud budete postupovat podle pokynů v [clustery HDInsight připojený k doméně nakonfigurovat (Preview)](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic), výchozí název je contosoaadvnet.
   * **$ClassicResourceGroupName** – název skupiny Resource Manageru pro klasickou virtuální síť, který je uvedený výše. Například contosoaadrg. 
   * **$ArmResourceGroupName** – název skupiny prostředků v rámci kterého chcete vytvořit HDInsight cluster. Jako $ArmResourceGroupName můžete použít stejnou skupinu prostředků.  Pokud skupina prostředků neexistuje, vytvoří skript skupině prostředků.
   * **$ArmVNetName** -název virtuální sítě Správce prostředků, ve kterém chcete vytvořit HDInsight cluster. Tato virtuální síť se umístí do $ArmResourceGroupName.  Pokud virtuální síti neexistuje, vytvoří skript prostředí PowerShell ji. Když neexistuje, měla by být součástí skupiny prostředků, které zadáváte výše.
   * **$AddressVnetAddressSpace** – síť adresní prostor virtuální sítě Resource Manager. Ujistěte se, že tento adresní prostor je k dispozici. Tento adresní prostor se nesmí překrývat klasickou virtuální síť adresní prostor. Například "10.1.0.0/16"
   * **$ArmVnetSubnetName** -název podsítě virtuální sítě Správce prostředků, ve kterém chcete umístit virtuální počítače clusteru HDInsight. Pokud podsíť neexistuje, vytvoří skript prostředí PowerShell ji. Když neexistuje, měla by být součástí virtuální sítě, které poskytujete výše.
   * **$AddressSubnetAddressSpace** – rozsah adres sítě pro podsíť virtuální sítě Resource Manager. Cluster HDInsight, virtuálních počítačů pro IP adresy bude z této rozsah adres podsítě. Například "10.1.0.0/24".
   * **$ActiveDirectoryDomainName** – název domény Azure AD, že chcete připojit HDInsight clusteru virtuálních počítačů do. Například "contoso.onmicrosoft.com"
   * **$ClusterUsersGroups** – běžný název skupiny zabezpečení ze služby AD, který chcete synchronizovat do clusteru HDInsight. Uživatelé v této skupině zabezpečení, bude moci přihlásit se na řídicí panel clusteru pomocí svých přihlašovacích údajů domény služby active directory. Tyto skupiny zabezpečení musí existovat ve službě active directory. Například "hiveusers" nebo "clusteroperatorusers".
   * **$OrganizationalUnitName** -organizační jednotku v doméně, ve kterém chcete umístit HDInsight clusteru virtuálních počítačů a objekty služby používaný v clusteru. Skript prostředí PowerShell vytvoří tuto organizační jednotku, pokud neexistuje. Například "HDInsightOU".
3. Uložte změny.

**Pro spuštění skriptu**

1. Spustit **prostředí Windows PowerShell** jako správce.
2. Přejděte do složky run.ps1. 
3. Spusťte skript zadáním názvu souboru a stiskněte tlačítko **ENTER**.  Objeví 3 přihlášení dialogová okna:
   
   1. **Přihlaste se k portálu Azure classic** – zadejte své přihlašovací údaje, které používáte pro přihlášení k portálu Azure classic. Musí jste vytvořili Azure AD a Azure AD DS pomocí těchto přihlašovacích údajů.
   2. **Přihlaste se k portálu Azure Resource Manager** – zadejte své přihlašovací údaje, které používáte k přihlášení na portál Azure Resource Manager.
   3. **Uživatelské jméno domény** – zadejte přihlašovací údaje domény uživatelské jméno, které mají být správcem v clusteru HDInsight. Pokud jste vytvořili Azure AD od začátku, vytvořili jste musí tento uživatel použijete tuto dokumentaci. 
      
      > [!IMPORTANT]
      > Zadejte uživatelské jméno v tomto formátu: 
      > 
      > Název_domény\uživatelské_jméno (například contoso.onmicrosoft.com\clusteradmin)
      > 
      > 
      
      Tento uživatel musí mít oprávnění pro 3: připojení počítače k zadané doméně služby Active Directory; Chcete-li vytvořit objekty služby a objekty počítačů v zadané organizační jednotce; Chcete-li přidat reverzní proxy server DNS pravidla a.

Při vytváření zpětné zóny DNS, skript zobrazí výzvu k zadání ID sítě. Toto ID sítě musí být předponu adresy virtuální sítě Resource Manager. Například pokud vaše adresního prostoru podsítě virtuální sítě Resource Manager je 10.2.0.0/24, zadejte 10.2.0.0/24, když nástroj vás vyzve k zadání ID sítě. 

## <a name="create-hdinsight-cluster"></a>Vytvoření clusteru HDInsight
V této části vytvoříte clusteru systémem Linux Hadoop v HDInsight pomocí portálu Azure nebo [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). Další metody vytváření clusterů a Principy nastavení, najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Další informace o vytvoření clusterů systému Hadoop v HDInsight pomocí šablony Resource Manageru najdete v tématu [vytvoření Hadoop clusterů v HDInsight pomocí šablony Resource Manageru](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**K vytvoření clusteru HDInsight se připojený k doméně pomocí portálu Azure**

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **nový**, **Intelligence + analýzy**a potom **HDInsight**.
3. Z **clusteru HDInsight se nové** okno, zadejte nebo vyberte tyto hodnoty:
   
   * **Název clusteru**: Zadejte nový název clusteru pro cluster HDInsight připojený k doméně.
   * **Předplatné**: Vyberte předplatné Azure použitý k vytvoření tohoto clusteru.
   * **Konfigurace clusteru**:
     
     * **Typ clusteru**: Hadoop. Připojené k doméně HDInsight je aktuálně pouze podporované na Hadoop clusterů.
     * **Operační systém**: Linux.  Připojené k doméně HDInsight je podporována pouze na clusterech HDInsight se systémem Linux.
     * **Verze**: Hadoop 2.7.3 (HDI 3.5). Připojené k doméně HDInsight je podporována pouze na verzi clusteru HDInsight 3.5.
     * **Typ clusteru**: PREMIUM
       
       Klikněte na tlačítko **vyberte** a uložte změny.
   * **Přihlašovací údaje**: nakonfigurujte pověření pro uživatele clusteru a uživatel SSH.
   * **Zdroj dat**: Vytvořte nový účet úložiště nebo použijte existující účet úložiště jako výchozí účet úložiště pro HDInsight cluster. Toto umístění musí být stejná jako dvě virtuální sítě.  Umístění je také umístění clusteru HDInsight.
   * **Ceny**: Vyberte počet uzlů pracovního procesu ve vašem clusteru.
   * **Pokročilé konfigurace**: 
     
     * **Připojení k doméně & virtuální sítě a podsítě**: 
       
       * **Nastavení domény**: 
         
         * **Název domény**: contoso.onmicrosoft.com
         * **Uživatelské jméno domény**: Zadejte uživatelské jméno domény. Tato doména musí mít následující oprávnění: připojení počítače k doméně a umístí je do organizační jednotky, které jste nakonfigurovali dříve; Vytvořit objekty služby v rámci organizační jednotky, které jste nakonfigurovali dříve; Vytvořte reverzní záznamy DNS. Tento uživatel domény, bude správce tento cluster HDInsight připojený k doméně.
         * **Heslo domény**: Zadejte heslo uživatele domény.
         * **Organizační jednotka**: zadejte rozlišující název organizační jednotky, který jste nakonfigurovali dříve. Například: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
         * **Adresa URL LDAPS**: ldaps://contoso.onmicrosoft.com:636
         * **Skupina uživatelů přístup**: Zadejte skupina zabezpečení, jejichž uživatelé wan pro synchronizaci do clusteru. Například HiveUsers.
           
           Klikněte na tlačítko **vyberte** a uložte změny.
           
           ![Portál HDInsight připojený k doméně nakonfigurovat nastavení domény](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuální síť**: contosohdivnet
       * **Podsíť**: Subnet1
         
         Klikněte na tlačítko **vyberte** a uložte změny.        
         Klikněte na tlačítko **vyberte** a uložte změny.
   * **Skupina prostředků**: Vyberte skupinu prostředků, použít pro virtuální síť HDInsight (contosohdirg).
4. Klikněte na možnost **Vytvořit**.  

Další možností pro vytvoření clusteru HDInsight se připojený k doméně je pro použití šablony pro správu prostředků Azure. Následující postup ukazuje, jak:

**K vytvoření clusteru HDInsight se připojený k doméně pomocí šablony Správa prostředků**

1. Kliknutím na následující obrázek otevřete šablonu Resource Manageru na portálu Azure. Šablony Resource Manageru se nachází v kontejneru veřejného objektu blob. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure-use-powershell/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Z **parametry** okno, zadejte následující hodnoty:
   
   * **Předplatné**: (vyberte předplatné Azure).
   * **Skupina prostředků**: klikněte na tlačítko **použít existující**, a zadejte stejnou skupinu prostředků, které jste dosud používali.  Například contosohdirg. 
   * **Umístění**: Zadejte umístění skupiny prostředků.
   * **Název clusteru**: Zadejte název pro cluster Hadoop, který chcete vytvořit. Například contosohdicluster.
   * **Typ clusteru**: Vyberte typ clusteru.  Výchozí hodnota je **hadoop**.
   * **Umístění**: Vyberte umístění pro cluster.  Výchozí účet úložiště používá stejné umístění.
   * **Počet uzlů pracovního procesu clusteru**: Vyberte počet uzlů pracovního procesu.
   * **Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je **admin**.
   * **Uživatelské jméno a heslo SSH**: výchozí uživatelské jméno **sshuser**.  Můžete ho změnit. 
   * **Id virtuální sítě**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >
   * **Podsíť virtuální sítě**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >/podsítě/Subnet1
   * **Název domény**: contoso.onmicrosoft.com
   * **Rozlišující název organizační jednotky**: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
   * **Skupiny uživatelů clusteru D Ns**: "\"CN HiveUsers, OU = AADDC Users, DC = =<DomainName>, DC = onmicrosoft, DC = com\""
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (zadejte uživatelské jméno správce domény)
   * **Skupině**: (zadejte heslo pro uživatele správce domény)
   * **Souhlasím s podmínkami a ujednáními výše uvedených**: (zkontrolujte, zda)
   * **Připnout na řídicí panel**: (zkontrolujte, zda)
3. Klikněte na **Koupit**. Zobrazí se nová dlaždice s názvem **Nasazení šablony**. Vytvoření clusteru trvá přibližně 20 minut. Jakmile je vytvořen cluster, můžete kliknout na okně clusteru na portálu a otevřete jej.

Po dokončení tohoto kurzu můžete cluster odstranit. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány. Postup odstranění clusteru naleznete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Další kroky

* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Pro připojení ke clusterům HDInsight připojený k doméně pomocí protokolu SSH, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

