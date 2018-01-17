---
title: "Kurz pro Azure Security Center – Definování a vyhodnocení zásad zabezpečení | Dokumentace Microsoftu"
description: "Kurz pro Azure Security Center – Definování a vyhodnocení zásad zabezpečení"
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2018
ms.author: yurid
ms.custom: mvc
ms.openlocfilehash: 102e160b706ad8fb8ee12ae4cac8cda51b06a991
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="tutorial-define-and-assess-security-policies"></a>Kurz: Definování a vyhodnocení zásad zabezpečení
Security Center pomáhá zajišťovat dodržování předpisů společnosti nebo soulad se zákonnými požadavky na zabezpečení pomocí zásad zabezpečení, které definují požadovanou konfiguraci úloh. Jakmile určíte zásady pro vaše předplatná Azure a přizpůsobíte je typům úloh nebo míře citlivosti dat, Security Center vám může poskytnout doporučení k zabezpečení pro vaše, výpočetní, síťové, databázové, úložné a aplikační prostředky. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Konfigurovat zásady zabezpečení
> * Vyhodnotit zabezpečení vašich prostředků

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky
Chcete-li si vyzkoušet postupy popsané v tomto kurzu, budete potřebovat službu Security Center v cenové úrovni Standard. Standardní úroveň služby Security Center si můžete zdarma vyzkoušet na 60 dní. Článek Rychlý Start: [Onboarding předplatného Azure na Security Center Standard](security-center-get-started.md) vás provede postupem upgradu na úroveň Standard.

## <a name="configure-security-policy"></a>Konfigurovat zásady zabezpečení
Security Center automaticky vytváří výchozí zásady zabezpečení pro každé z vašich předplatných Azure. Zásady zabezpečení se skládají z doporučení, která můžete zapnout nebo vypnout podle požadavků zabezpečení daného předplatného. Pokud chcete upravit některou výchozí zásadu zabezpečení, musíte být vlastníkem, přispěvatelem nebo správcem zabezpečení daného předplatného.

1. V hlavní nabídce služby Security Center vyberte **Zásady zabezpečení**. Vyberte předplatné, které chcete použít. V části **SOUČÁSTI ZÁSAD** vyberte **Zásady zabezpečení**:

  ![Zásady zabezpečení](./media/tutorial-security-policy/tutorial-security-policy-fig1.png)  

2. Pro každou konfiguraci zabezpečení, kterou chcete sledovat, vyberte **Zapnuto**. Security Center bude nepřetržitě vyhodnocovat konfiguraci vašeho prostředí a pokud nalezne nějakou zranitelnost, vygeneruje doporučení k zabezpečení. Pokud konfiguraci zabezpečení nepovažujete za důležitou, vyberte **Vypnuto**. Například v prostředí pro vývoj/testování nemusíte vyžadovat stejnou úroveň zabezpečení jako v provozním prostředí. Po výběru zásad pro vaše prostředí klikněte na **Uložit**.

Počkejte, až Security Center zásady zpracuje a vygeneruje doporučení. Některé konfigurace, například aktualizace systému a konfigurace operačního systému můžou zabrat až 12 hodin, zatímco skupiny zabezpečení sítě a konfigurace šifrování lze posoudit prakticky okamžitě. Jakmile se doporučení zobrazí v řídicím panelu Security Center, můžete pokračovat k dalšímu kroku.

## <a name="assess-security-of-resources"></a>Vyhodnocení zabezpečení prostředků
1. Podle zapnutých zásad zabezpečení poskytne Security Center sadu doporučení zabezpečení podle aktuální potřeby. Měli byste začít kontrolou doporučení k virtuálním a fyzickým počítačům. Otevřete řídicí panel Security Center, klikněte na **Přehled** a pak na **Compute**.

  ![Compute](./media/tutorial-security-policy/tutorial-security-policy-fig2.png)

  Zkontrolujte jednotlivá doporučení, začněte od těch červených (s vysokou prioritou). Některá z těchto doporučení mají řešení, která můžete implementovat přímo ze služby Security Center, jako [potíže s ochranou koncového bodu](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection). Další doporučení tvoří jen pokyny pro provedení nápravy, například chybějící šifrování disku.

2. Až vyřešíte všechna relevantní výpočetní doporučení, měli byste přejít k síťovým. Na řídicím panelu Security Center klikněte na **Přehled** a pak na **Sítě**.

  ![Sítě](./media/tutorial-security-policy/tutorial-security-policy-fig3.png)

  Stránka doporučení k sítím obsahuje seznam problémů zabezpečení vaší síťové konfigurace, internetových koncových bodů a síťové topologie. Stejně jako u **výpočetních** doporučení můžete některá síťová doporučení napravit přímo, ale jiná nikoli.

3. Až vyřešíte všechna relevantní doporučení k síti, dalším krokem jsou úložiště a data. Otevřete řídicí panel Security Center, klikněte na **Přehled** a pak na **Datové prostředky**.

  ![Datové prostředky](./media/tutorial-security-policy/tutorial-security-policy-fig4.png)

  Stránka **Datové prostředky** obsahuje doporučení týkající se povolení auditování pro servery a databáze Azure SQL, povolení šifrování databází SQL a povolení šifrování vašeho účtu úložiště Azure. Pokud tyto úlohy nepoužíváte, neuvidíte žádné doporučení. Stejně jako u **výpočetních** doporučení můžete některá doporučení k databázím SQL a k úložištím napravit přímo, ale jiná nikoli.

4. Až vyřešíte všechna relevantní doporučení k datům a databázím, dalším krokem jsou aplikace. Na řídicím panelu Security Center klikněte na **Přehled** a pak na **Aplikace**.

  ![Aplikace](./media/tutorial-security-policy/tutorial-security-policy-fig5.png)

  Stránka **Aplikace** obsahuje doporučení pro nasazení brány firewall webových aplikací a obecné pokyny pro posílení zabezpečení aplikací. Pokud nemáte virtuální počítače nebo počítače s webovými aplikacemi používající Internetovou informační službu (IIS), tato doporučení neuvidíte.

## <a name="clean-up-resources"></a>Vyčištění prostředků
Další rychlé starty a kurzy v této kolekci vycházejí z tohoto rychlého startu. Pokud budete chtít pokračovat v práci s následnými kurzy a rychlými starty, ponechte v provozu úroveň Standard a nechte zapnuté automatické zřizování. Pokud neplánujete pokračovat nebo se chcete vrátit na úroveň Free:

1. Vraťte se do hlavní nabídky služby Security Center a vyberte **Zásady zabezpečení**.
2. Vyberte předplatné nebo zásady, které chcete vrátit na úroveň Free. Otevře se okno **Zásady zabezpečení**.
3. V části **SOUČÁSTI ZÁSAD** vyberte **Cenová úroveň**.
4. Výběrem **Free** změníte předplatné z úrovně Standard na úroveň Free.
5. Vyberte **Uložit**.

Pokud chcete vypnout automatické zřizování:

1. Vraťte se do hlavní nabídky služby Security Center a vyberte **Zásady zabezpečení**.
2. Vyberte předplatné, pro které chcete vypnout automatické zřizování.
3. V části **Zásady zabezpečení – shromažďování dat** výběrem možnosti **Vypnuto** u volby **Onboarding** vypnete automatické zřizování.
4. Vyberte **Uložit**.

>[!NOTE]
> Vypnutím automatického zřizování neodeberete agenta Microsoft Monitoring Agent z virtuálních počítačů Azure, na kterých byl agent zřízen. Vypnutí automatického zřizování omezí sledování zabezpečení pro vaše prostředky.
>

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli o základech definování zásad a vyhodnocení zabezpečení vašich úloh pomocí služby Security Center, například:

> [!div class="checklist"]
> * Konfigurace zásad zabezpečení pro zajištění dodržování předpisů společnosti nebo soulad se zákonnými požadavky na zabezpečení
> * Posouzení zabezpečení pro výpočetní, síťové, databázové, úložné a aplikační prostředky

Nyní můžete přejít k dalšímu kurzu, kde se dozvíte, jak používat Security Center k ochraně vašich prostředků.

> [!div class="nextstepaction"]
> [Ochrana vašich prostředků](tutorial-protect-resources.md)
