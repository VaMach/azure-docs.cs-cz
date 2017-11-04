---
title: "Konfigurace clusterů HDInsight připojený k doméně - Azure | Microsoft Docs"
description: "Zjistěte, jak připravit a nakonfigurovat clustery HDInsight připojený k doméně"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: af75d63caca24f389345c964e2dc506a255bec19
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Konfigurace clusterů HDInsight připojený k doméně (Preview)

Zjistěte, jak nastavit cluster Azure HDInsight s Azure Active Directory (Azure AD) a [Apache škálu](http://hortonworks.com/apache/ranger/) využívat výhod silné ověřování a bohaté přístupu podle rolí zásady řízení (RBAC).  HDInsight připojený k doméně se dá nakonfigurovat jenom na clusterech se systémem Linux. Další informace najdete v tématu [clustery HDInsight připojený k doméně zavádět](apache-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie není povoleno v doméně HDInsight.

Tento článek je první kurz řady:

* Vytvoření clusteru HDInsight připojené ke službě Azure AD (prostřednictvím funkce Azure Directory Domain Services) s Apache škálu povolena.
* Vytvořit a použít zásady Hive prostřednictvím Apache škálu a povolit uživatelům (například datových vědců) pro připojení k Hive pomocí rozhraní ODBC nástrojů, například aplikace Excel, Tableau atd. Společnost Microsoft pracuje na přidávání dalších úloh, jako jsou HBase, Spark a Storm, k doméně HDInsight brzy.

Příklad konečné topologie vypadá takto:

![Topologie HDInsight připojený k doméně](./media/apache-domain-joined-configure/hdinsight-domain-joined-topology.png)

Protože Azure AD aktuálně podporuje pouze klasické virtuální sítě (virtuální sítě) a podporuje se jen clustery HDInsight se systémem Linux Azure Resource Manager na základě virtuálních sítí, integrace HDInsight Azure AD vyžaduje dvě virtuální sítě a partnerský vztah mezi nimi. Porovnání informace mezi modely dvě nasazení najdete v tématu [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a stav svých prostředků](../../azure-resource-manager/resource-manager-deployment-model.md). Dvě virtuální sítě musí být ve stejné oblasti jako služba Azure AD DS.

Služba Azure názvy musí být globálně jedinečný. Následující názvy jsou použity v tomto kurzu. Contoso je fiktivní název. Je třeba nahradit *contoso* s jiným názvem při absolvovat kurz. 

**Názvy:**

| Vlastnost | Hodnota |
| --- | --- |
| Virtuální síť Azure AD |contosoaadvnet |
| Skupina prostředků Azure AD virtuální sítě |contosoaadrg |
| Adresář Azure AD |contosoaaddirectory |
| Název domény služby Azure AD |společnosti Contoso (contoso.onmicrosoft.com) |
| Virtuální síť HDInsight |contosohdivnet |
| Skupina prostředků HDInsight virtuální sítě |contosohdirg |
| HDInsight cluster |contosohdicluster |

Tento kurz obsahuje kroky pro konfiguraci clusteru HDInsight připojený k doméně. Každá část obsahuje odkazy na další články s podrobnější informace.

## <a name="prerequisite"></a>Předpoklad:
* Seznamte se s [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) jeho [ceny](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktura.
* Zajistěte, aby vaše předplatné seznam povolených adres pro tuto verzi public preview. Můžete to provést pomocí e-mailu na hdipreview@microsoft.com s vaším ID předplatného.
* Certifikát SSL, který je podepsaný podpisový autoritou pro vaši doménu. Certifikát je požadován konfigurace zabezpečeného LDAP. Nelze použít certifikáty podepsané svým držitelem.

## <a name="procedures"></a>Postupy
1. Vytvoření Azure klasické virtuální sítě pro vaši službu Azure AD.  
2. Vytvoření a konfigurace Azure AD a Azure AD DS.
3. Vytvoření virtuální sítě HDInsight v režimu správy prostředků Azure.
4. Sdílené dvou virtuálních sítí.
5. Vytvoření clusteru HDInsight.

> [!NOTE]
> Tento kurz předpokládá, že nemáte Azure AD. Pokud nemáte, můžete přeskočit část v kroku 2.
> 
> 

## <a name="create-an-azure-virtual-network-classic"></a>Vytvoření virtuální sítě Azure (klasický)
V této části vytvoříte virtuální sítě (klasické) pomocí portálu Azure. V další části povolíte pro vaši službu Azure AD ve virtuální síti Azure AD DS. Další informace o následující postup a použití jiných metod vytvoření virtuální sítě najdete v tématu [vytvoření virtuální sítě (klasické) pomocí portálu Azure](../../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

**Chcete-li vytvořit klasické virtuální sítě**

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com). 
2. Klikněte na tlačítko **nové** > **sítě** > **virtuální sítě**.
3. V **vybrat model nasazení**, vyberte **Classic**a potom klikněte na **vytvořit**.
4. Zadejte nebo vyberte tyto hodnoty:
   
   * **Název**: contosoaadvnet
   * **Adresní prostor**: 10.1.0.0/16
   * **Název podsítě**: Subnet1
   * **Rozsah adres podsítě**: 10.1.0.0/24
   * **Předplatné**: (vybrat odběr, použitý k vytvoření této virtuální sítě.)
   * **ResourceGroup**: contosoaadrg
   * **Umístění**: (vyberte oblast pro váš cluster HDInsight.)
     
     > [!IMPORTANT]
     > Musíte zvolit umístění, které podporuje Azure AD DS. Další informace najdete v tématu [Dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/en-us/regions/services/). 
     > 
     > Klasické virtuální sítě i virtuální síť skupiny prostředků musí být ve stejné oblasti jako služba Azure AD DS.
     > 
     > 
5. Kliknutím na **Vytvořit** vytvořte síť VNet.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Vytvoření a konfiguraci služby Azure AD DS pro vaši službu Azure AD
V této části provedete následující:

1. Vytvořte Azure AD.
2. Vytvořte uživatele Azure AD. Tito uživatelé jsou uživatele domény. První uživatel se používá pro konfiguraci clusteru HDInsight pomocí Azure AD.  Dva uživatelé jsou volitelné pro účely tohoto kurzu. Bude použita v [Hive nakonfigurovat zásady pro clustery služby HDInsight připojený k doméně](apache-domain-joined-run-hive.md) při konfiguraci zásad Apache škálu.
3. Vytvoření skupiny Administrators AAD řadič domény a do skupiny přidat uživatele Azure AD. Tento uživatel slouží k vytvoření organizační jednotky.
4. Povolení služby Azure AD Domain Services (Azure AD DS) pro Azure AD.
5. Nakonfigurujte LDAPS pro Azure AD. Přístup protokolu LDAP (Lightweight Directory) se používá ke čtení z a zapisovat do služby Azure AD.

Pokud byste radši chtěli použít existující Azure AD, můžete přeskočit kroky 1 a 2.

**Chcete-li vytvořit Azure AD**

1. Z [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **nový** > **App Services** > **služby Active Directory**  >  **Directory** > **vytvořit vlastní**. 
2. Zadejte nebo vyberte tyto hodnoty:
   
   * **Název**: contosoaaddirectory
   * **Název domény**: contoso.  Tento název musí být globálně jedinečný.
   * **Země nebo oblast**: vyberte vaši zemi nebo oblast.
3. Klikněte na **Dokončit**.

**Vytvořit uživatele Azure AD**

1. Z [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **služby Active Directory** -> **contosoaaddirectory**. 
2. Klikněte na tlačítko **uživatelé** v hlavní nabídce.
3. Klikněte na tlačítko **přidat uživatele**.
4. Zadejte **uživatelské jméno**a potom klikněte na **Další**. 
5. Konfigurace profilu uživatele; V **Role**, vyberte **globálního správce**; a potom klikněte na **Další**.  Roli globálního správce je potřeba k vytvoření organizační jednotky.
6. Klikněte na tlačítko **vytvořit** získat dočasné heslo.
7. Vytvořit kopii heslo a potom klikněte na **Complete**. Později v tomto kurzu použijete k vytvoření clusteru HDInsight tohoto uživatele globální správce.

Postupujte stejným způsobem, chcete-li vytvořit dva další uživatelé s **uživatele** role, hiveuser1 a hiveuser2. Následující uživatelé se použije v [Hive nakonfigurovat zásady pro clustery služby HDInsight připojený k doméně](apache-domain-joined-run-hive.md).

**Vytvoření skupiny AAD správce řadiče domény a přidat uživatele Azure AD**

1. Z [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **služby Active Directory** > **contosoaaddirectory**. 
2. Klikněte na tlačítko **skupiny** v hlavní nabídce.
3. Klikněte na tlačítko **přidat skupinu** nebo **přidat skupinu**.
4. Zadejte nebo vyberte tyto hodnoty:
   
   * **Název**: Správci AAD řadič domény.  Neměnit název skupiny.
   * **Typ skupiny**: zabezpečení.
5. Klikněte na **Dokončit**.
6. Klikněte na tlačítko **AAD řadič domény správci** otevřete skupinu.
7. Klikněte na tlačítko **přidat členy**.
8. Vyberte první uživatel, který jste vytvořili v předchozím kroku a pak klikněte na tlačítko **Complete**.
9. Opakujte stejný postup, jak vytvořit jinou skupinu s názvem **HiveUsers**, a přidejte dva Hive uživatele do skupiny.

Další informace najdete v tématu [Azure AD Domain Services (Preview) – vytvořit skupinu, správci AAD řadič domény,](../../active-directory-domain-services/active-directory-ds-getting-started.md).

**Pokud chcete povolit služby Azure AD DS pro vaši službu Azure AD**

1. Z [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **služby Active Directory** > **contosoaaddirectory**. 
2. Klikněte na tlačítko **konfigurace** v hlavní nabídce.
3. Přejděte dolů k položce **Domain Services**a nastavte následující hodnoty:
   
   * **Povolit doménové služby pro tento adresář**: Ano.
   * **Název domény DNS služby domain services**: Zobrazí název DNS výchozí adresář Azure. Například contoso.onmicrosoft.com.
   * **Připojit doménové služby k této virtuální síti**: Vyberte klasickou virtuální síť, které jste vytvořili dříve, tj. **contosoaadvnet**.
4. Klikněte na tlačítko **Uložit** v dolní části stránky. Zobrazí se **čekající na vyřízení...**  vedle **povolit doménové služby pro tento adresář**.  
5. Počkejte na **čekající na vyřízení...**  zmizí, a **IP adresu** získá naplněno. Dvě IP adresy budou obsazeny. Toto jsou IP adresy řadiče domény, který vytváří Domain Services. Po odpovídající řadič domény je zřízený a připravena, budou viditelné každou IP adresu. Poznamenejte si dvě IP adresy. Ty budete potřebovat později.

Další informace najdete v tématu [povolit Azure Active Directory Domain Services pomocí webu Azure portal](../../active-directory-domain-services/active-directory-ds-getting-started.md).

**Jak synchronizovat heslo**

Pokud budete používat vlastní doménu, budete muset synchronizovat heslo. V tématu [povolení synchronizace hesel do služby Azure AD domain services pro Azure výhradně cloudový adresář AD](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**Ke konfiguraci LDAPS pro Azure AD**

1. Získejte certifikát SSL, který je podepsaný podpisový autoritou pro vaši doménu. Pokud chcete použít certifikát podepsaný svým držitelem, prosím oslovení hdipreview@microsoft.com pro výjimku.
2. Z [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **služby Active Directory** > **contosoaaddirectory**. 
3. Klikněte na tlačítko **konfigurace** v hlavní nabídce.
4. Přejděte na **služby domény**.
5. Klikněte na tlačítko **konfigurace certifikátu**.
6. Postupujte podle pokynů určete soubor certifikátu a heslo. Zobrazí se **čekající na vyřízení...**  vedle **povolit doménové služby pro tento adresář**.  
7. Počkejte na **čekající na vyřízení...**  zmizí, a **zabezpečení certifikátu LDAP** tu naplněno.  To může trvat až 10 minut nebo déle.

> [!NOTE]
> Pokud některé úlohy na pozadí běží na Azure AD DS, zobrazí chybu při odesílání certifikát – <i>je operace prováděna pro tohoto klienta. Zkuste to prosím znovu později</i>.  V případě, že dojde k této chybě, zkuste to prosím po nějaké době znovu. Druhá IP řadič domény může trvat až 3 hodiny, které se má zřídit.
> 
> 

Další informace najdete v tématu [konfigurace zabezpečení protokolu LDAP (LDAPS) pro Azure AD Domain Services spravované domény](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Vytvoření virtuální sítě Resource Manageru pro HDInsight cluster
V této části vytvoříte o virtuální síť Azure Resource Manager, který se použije pro HDInsight cluster. Další informace o vytváření virtuální sítě Azure pomocí jiných metod najdete v tématu [vytvoření virtuální sítě](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md)

Po vytvoření virtuální sítě, můžete nakonfigurovat virtuální sítě Resource Manageru, abyste mohli používat stejné servery DNS jako virtuální sítě Azure AD. Pokud jste postupovali podle kroků v tomto kurzu k vytvoření klasické virtuální sítě a Azure AD, servery DNS, které jsou 10.1.0.4 a 10.1.0.5.

**K vytvoření virtuální sítě Resource Manageru**

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **nový**, **sítě**a potom **virtuální síť**. 
3. V **vybrat model nasazení**, vyberte **Resource Manager**a potom klikněte na **vytvořit**.
4. Zadejte nebo vyberte tyto hodnoty:
   
   * **Název**: contosohdivnet
   * **Adresní prostor**: 10.2.0.0/16. Zkontrolujte, zda že rozsah adres se nesmí překrývat s rozsahem IP adres klasické virtuální sítě.
   * **Název podsítě**: Subnet1
   * **Rozsah adres podsítě**: 10.2.0.0/24
   * **Předplatné**: (vyberte předplatné Azure.)
   * **Skupina prostředků**: contosohdirg
   * **Umístění**: (Vybrat stejné umístění jako Azure AD VNet, tj. contosoaadvnet.)
5. Klikněte na možnost **Vytvořit**.

**Konfigurace služby DNS pro virtuální sítě Resource Manageru**

1. Z [portál Azure](https://portal.azure.com), klikněte na tlačítko **další služby** -> **virtuální sítě**. Zajistěte, aby klikněte na tlačítko **virtuální sítě (klasické)**.
2. Klikněte na tlačítko **contosohdivnet**.
3. Klikněte na tlačítko **servery DNS** z levé strany nové okno.
4. Klikněte na tlačítko **vlastní**a potom zadejte následující hodnoty:
   
   * 10.1.0.4
   * 10.1.0.5
     
     Tyto IP adresy serveru DNS se musí shodovat na servery DNS ve virtuální síti Azure AD (klasické virtuální sítě).
5. Klikněte na **Uložit**.

## <a name="peer-the-azure-ad-vnet-and-the-hdinsight-vnet"></a>Peer virtuální sítí Azure AD a virtuální sítí HDInsight VNet
**Dva virtuální síť rovnocenných počítačů**

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **další služby** v levé nabídce.
3. Klikněte na tlačítko **virtuální sítě**. Nemáte klikněte na tlačítko **virtuální sítě (klasické)**.
4. Klikněte na tlačítko **contosohdivnet**.  Toto je HDInsight virtuální sítě.
5. Klikněte na tlačítko **partnerských vztahů** v levé nabídce okna.
6. Klikněte na tlačítko **přidat** v hlavní nabídce. Otevře se **přidat partnerský vztah** okno.
7. Na **přidat partnerský vztah** okně nastaven, nebo vyberte tyto hodnoty:
   
   * **Název**: ContosoAADHDIVNetPeering
   * **Virtuální síť modelu nasazení**: Classic
   * **Předplatné**: vyberte název odběru použít pro virtuální sítě classic (Azure AD).
   * **Virtuální síť**: contosoaadvnet.
   * **Povolit přístup k virtuální síti**: (zkontrolujte, zda)
   * **Povolit přesměrovaných přenosů**: (zkontrolujte, zda). Nechte nezaškrtnuté dvou políček.
8. Klikněte na **OK**.

## <a name="create-hdinsight-cluster"></a>Vytvoření clusteru HDInsight
V této části vytvoříte clusteru systémem Linux Hadoop v HDInsight pomocí portálu Azure nebo [šablony Azure Resource Manageru](../../azure-resource-manager/resource-group-template-deploy.md). Další metody vytváření clusterů a Principy nastavení, najdete v tématu [Tvorba clusterů HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Další informace o vytvoření clusterů systému Hadoop v HDInsight pomocí šablony Resource Manageru najdete v tématu [vytvoření Hadoop clusterů v HDInsight pomocí šablony Resource Manageru](../hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**K vytvoření clusteru HDInsight se připojený k doméně pomocí portálu Azure**

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **nový**, **Intelligence + analýzy**a potom **HDInsight**.
3. Z **clusteru HDInsight se nové** okno, zadejte nebo vyberte tyto hodnoty:
   
   * **Název clusteru**: Zadejte nový název clusteru pro cluster HDInsight připojený k doméně.
   * **Předplatné**: Vyberte předplatné Azure použitý k vytvoření tohoto clusteru.
   * **Konfigurace clusteru**:
     
     * **Typ clusteru**: Hadoop. Připojené k doméně HDInsight je aktuálně pouze podporované na Hadoop clusterů.
     * **Operační systém**: Linux.  Připojené k doméně HDInsight je podporována pouze na clusterech HDInsight se systémem Linux.
     * **Verze**: HDI 3.6. Připojené k doméně HDInsight je podporována pouze na verzi clusteru HDInsight 3.6.
     * **Typ clusteru**: PREMIUM
       
       Klikněte na tlačítko **vyberte** a uložte změny.
   * **Přihlašovací údaje**: nakonfigurujte pověření pro uživatele clusteru a uživatel SSH.
   * **Zdroj dat**: Vytvořte nový účet úložiště nebo použijte existující účet úložiště jako výchozí účet úložiště pro HDInsight cluster. Toto umístění musí být stejná jako dvě virtuální sítě.  Umístění je také umístění clusteru HDInsight.
   * **Ceny**: Vyberte počet uzlů pracovního procesu ve vašem clusteru.
   * **Pokročilé konfigurace**: 
     
     * **Připojení k doméně & virtuální sítě a podsítě**: 
       
       * **Nastavení domény**: 
         
         * **Název domény**: contoso.onmicrosoft.com
         * **Uživatelské jméno domény**: Zadejte uživatelské jméno domény. Tato doména musí mít následující oprávnění: připojení počítače k doméně a umístí je do organizační jednotky, které zadáte během vytváření clusteru; Vytvořit objekty služby v rámci organizační jednotka, kterou zadáte během vytváření clusteru; Vytvořte reverzní záznamy DNS. Tento uživatel domény, bude správce tento cluster HDInsight připojený k doméně.
         * **Heslo domény**: Zadejte heslo uživatele domény.
         * **Organizační jednotka**: zadejte rozlišující název organizační jednotky, kterou chcete použít s clusterem HDInsight. Například: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com. Pokud tuto organizační jednotku neexistuje, clusteru HDInsight se pokusí vytvořit tuto organizační jednotku. Ujistěte se, že na organizační jednotku již existuje nebo doménový účet má oprávnění k vytvoření nového. Pokud používáte účet domény, který je součástí AADDC správci, bude mít nezbytná oprávnění k vytváření organizační jednotky.
         * **Adresa URL LDAPS**: ldaps://contoso.onmicrosoft.com:636
         * **Skupina uživatelů přístup**: Určete skupinu zabezpečení, jejichž uživatelé, které chcete synchronizovat do clusteru. Například HiveUsers.
           
           Klikněte na tlačítko **vyberte** a uložte změny.
           
           ![Portál HDInsight připojený k doméně nakonfigurovat nastavení domény](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuální síť**: contosohdivnet
       * **Podsíť**: Subnet1
         
         Klikněte na tlačítko **vyberte** a uložte změny.        
         Klikněte na tlačítko **vyberte** a uložte změny.
   * **Skupina prostředků**: Vyberte skupinu prostředků, použít pro virtuální síť HDInsight (contosohdirg).
4. Klikněte na možnost **Vytvořit**.  

Další možností pro vytvoření clusteru HDInsight se připojený k doméně je pro použití šablony pro správu prostředků Azure. Následující postup ukazuje, jak:

**K vytvoření clusteru HDInsight se připojený k doméně pomocí šablony Správa prostředků**

1. Kliknutím na následující obrázek otevřete šablonu Resource Manageru na portálu Azure. Šablony Resource Manageru se nachází v kontejneru veřejného objektu blob. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>
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
   * **Skupiny uživatelů clusteru DNs**: [\"HiveUsers\"]
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (zadejte uživatelské jméno správce domény)
   * **Skupině**: (zadejte heslo pro uživatele správce domény)
   * **Souhlasím s podmínkami a ujednáními výše uvedených**: (zkontrolujte, zda)
   * **Připnout na řídicí panel**: (zkontrolujte, zda)
3. Klikněte na **Koupit**. Zobrazí se nová dlaždice s názvem **Nasazení šablony**. Vytvoření clusteru trvá přibližně 20 minut. Jakmile je vytvořen cluster, můžete kliknout na okně clusteru na portálu a otevřete jej.

Po dokončení tohoto kurzu můžete cluster odstranit. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány. Postup odstranění clusteru naleznete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](apache-domain-joined-run-hive.md).
* Pro připojení ke clusterům HDInsight připojený k doméně pomocí protokolu SSH, najdete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux, Unix nebo OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

