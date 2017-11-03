---
title: "Virtuální počítače Azure Security Center a systému Windows v Azure | Microsoft Docs"
description: "Další informace o zabezpečení pro virtuální počítač Azure Windows s Azure Security Center."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: adb00e28b0b204858a763f83836ee2ac96f8f9e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Sledování zabezpečení virtuálního počítače pomocí Azure Security Center

Azure Security Center můžete získat přehled o Azure prostředku postupy zabezpečení. Security Center nabízí integrované zabezpečení monitorování. Může zjistit hrozeb, které jinak může nevšimli. V tomto kurzu se dozvíte o Azure Security Center a postup:
 
> [!div class="checklist"]
> * Nastavení shromažďování dat
> * Nastavte zásady zabezpečení
> * Zobrazení a opravte problémy s konfigurací stavu
> * Zkontrolujte zjištěnými hrozbami  

## <a name="security-center-overview"></a>Security Center – přehled

Security Center identifikuje potenciální potíže s konfigurací virtuálního počítače (VM) a cílem bezpečnostní hrozby. To může zahrnovat virtuální počítače, které chybí skupin zabezpečení sítě, nezašifrované disků a útoku hrubou silou protokol RDP (Remote Desktop). Informace se zobrazí na řídicím panelu Security Center v grafy snadno čitelné.

Chcete-li přístup k řídicímu panelu Security Center na portálu Azure, v nabídce vyberte **Security Center**. Na řídicím panelu můžete zobrazit stav zabezpečení prostředí Azure, najít a počet aktuální doporučení a zobrazit aktuální stav výstrahy hrozeb. Můžete rozbalit každý vysoké úrovně grafu pro zobrazení dalších podrobností.

![Řídicí panel Security Center](./media/tutorial-azure-security/asc-dash.png)

Security Center překročí data zjišťování poskytnout doporučení pro problémy, které zjistí. Například pokud virtuální počítač byl nasazen bez skupiny zabezpečení služby připojené síti, Security Center zobrazuje doporučení, s nápravy kroky, které můžete provést. Získáte automatizovanou nápravu, aniž byste museli opustit kontext služby Security Center.  

![Doporučení](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Nastavení shromažďování dat

Než do zabezpečení konfigurací virtuálních počítačů můžete získat viditelnost, budete muset nastavit shromažďování dat Security Center. To zahrnuje zapnutí shromažďování dat a vytvoření účtu úložiště Azure pro uložení shromážděná data. 

1. Na řídicím panelu Security Center klikněte na tlačítko **zásady zabezpečení**a potom vyberte své předplatné. 
2. Pro **shromažďování dat**, vyberte **na**.
3. Chcete-li vytvořit účet úložiště, vyberte **zvolte účet úložiště**. Pak vyberte **OK**.
4. Na **zásady zabezpečení** vyberte **Uložit**. 

Agenta pro sběr dat Security Center je nainstalován na všech virtuálních počítačů a shromažďování dat začíná. 

## <a name="set-up-a-security-policy"></a>Nastavte zásady zabezpečení

Zásady zabezpečení se používá k definování položky, pro které Security Center shromažďuje data a díky doporučení. Můžete použít jiné bezpečnostní zásady pro různé skupiny prostředků Azure. I když ve výchozím nastavení jsou prostředky Azure porovnán s všechny položky zásad, můžete vypnout jednotlivé zásady položky pro všechny prostředky Azure nebo pro skupinu prostředků. Podrobné informace o zásadách zabezpečení Security Center najdete v tématu [nastavení zásad zabezpečení v Azure Security Center](../../security-center/security-center-policies.md). 

Nastavení zásad zabezpečení pro všechny prostředky Azure:

1. Na řídicím panelu Security Center, vyberte **zásady zabezpečení**a potom vyberte své předplatné.
2. Vyberte **zásada Zabránění**.
3. Zapněte nebo vypněte zásady položky, které chcete použít pro všechny prostředky Azure.
4. Jakmile budete hotovi, vyberte nastavení, vyberte **OK**.
5. Na **zásady zabezpečení** vyberte **Uložit**. 

Nastavení zásad pro určité skupiny zdrojů:

1. Na řídicím panelu Security Center, vyberte **zásady zabezpečení**a pak vyberte skupinu prostředků.
2. Vyberte **zásada Zabránění**.
3. Zapněte nebo vypněte zásady položky, které chcete použít ke skupině prostředků.
4. V části **DĚDIČNOSTI**, vyberte **jedinečný**.
5. Jakmile budete hotovi, vyberte nastavení, vyberte **OK**.
6. Na **zásady zabezpečení** vyberte **Uložit**.  

Také můžete vypnout shromažďování dat pro určité skupiny zdrojů na této stránce.

V následujícím příkladu, byl vytvořen jedinečné zásady pro skupinu prostředků s názvem *myResoureGroup*. V této zásadě jsou vypnuté disku šifrování a webové aplikace brány firewall doporučení.

![Jedinečné zásady](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Zobrazit stav konfigurace virtuálního počítače

Po zapnutá shromažďování dat a nastavit zásadu zabezpečení, Security Center začne poskytovat výstrahy a doporučení. Při nasazování virtuálních počítačů, agenta pro sběr dat je nainstalována. Security Center je pak naplněný daty pro nové virtuální počítače. Podrobné informace o stavu konfigurace virtuálních počítačů najdete v tématu [chránit virtuální počítače ve službě Security Center](../../security-center/security-center-virtual-machine-recommendations.md). 

Jak se data shromažďují, je agregován stav prostředku pro každý virtuální počítač a souvisejících prostředků Azure. Informace jsou zobrazeny v diagramu snadno čitelné. 

Chcete-li zobrazit stav prostředku:

1.  Na zabezpečení Center řídicího panelu, v části **stav zabezpečení prostředků**, vyberte **výpočetní**. 
2.  Na **výpočetní** vyberte **virtuální počítače**. Toto zobrazení obsahuje souhrn stavu konfigurace pro všechny virtuální počítače.

![Výpočetní stavu](./media/tutorial-azure-security/compute-health.png)

Pokud chcete zobrazit všechna doporučení pro virtuální počítač, vyberte virtuální počítač. Doporučení a náprava jsou podrobněji v další části tohoto kurzu.

## <a name="remediate-configuration-issues"></a>Opravit problémy s konfigurací

Po zahájení Security Center k naplnění dat konfigurace doporučení jsou provedená na základě zásad zabezpečení, kterou vytvoříte. Například pokud virtuální počítač vytvořený bez skupinu zabezpečení sítě spojenou doporučení přišla k jeho vytvoření. 

Chcete-li zobrazit seznam všech doporučení: 

1. Na řídicím panelu Security Center, vyberte **doporučení**.
2. Vyberte konkrétní doporučení. Zobrazí se seznam všech prostředků, pro kterou platí doporučení.
3. Chcete-li použít doporučení, vyberte konkrétní prostředku. 
4. Postupujte podle pokynů pro nápravu kroky. 

V mnoha případech Security Center nabízí řešitelné kroky, které můžete provést k vyřešení doporučení, aniž byste museli opustit Security Center. V následujícím příkladu Security Center zjišťuje skupinu zabezpečení sítě, který má neomezený příchozího pravidla. Na stránce doporučení můžete vybrat **upravit příchozí pravidla** tlačítko. Zobrazí se uživatelské rozhraní, které je potřeba upravit pravidlo. 

![Doporučení](./media/tutorial-azure-security/remediation.png)

Podle doporučení opraví, jsou označeny jako vyřešené. 

## <a name="view-detected-threats"></a>Zobrazit zjištěnými hrozbami

Kromě doporučené konfigurace prostředků Security Center zobrazuje výstrahy detekce hrozeb. Funkce výstrahy zabezpečení agreguje data shromážděná z jednotlivých virtuálních počítačů Azure síťové protokoly a připojených partnerských řešení ke zjištění ohrožení zabezpečení proti prostředků Azure. Podrobné informace o možnostech detekce hrozeb Security Center najdete v tématu [funkce zjišťování služby Azure Security Center](../../security-center/security-center-detection-capabilities.md).

Funkce výstrahy zabezpečení vyžaduje Security Center cenová úroveň zvýšit z *volné* k *standardní*. 30denní **bezplatnou zkušební verzi** je k dispozici, když přesouváte vyšší cenová úroveň. 

Chcete-li změnit cenovou úroveň:  

1. Na řídicím panelu Security Center klikněte na tlačítko **zásady zabezpečení**a potom vyberte své předplatné.
2. Vyberte **cenová úroveň**.
3. Vyberte novou vrstvu a pak vyberte **vyberte**.
4. Na **zásady zabezpečení** vyberte **Uložit**. 

Po změně cenové úrovně, začne grafu výstrahy zabezpečení k naplnění jako zjištění ohrožení zabezpečení.

![Výstrahy zabezpečení](./media/tutorial-azure-security/security-alerts.png)

Vyberte příslušnou výstrahu a zobrazit informace. Například se zobrazí popis ohrožení, čas detekce, všechny pokusy hrozeb a doporučené nápravy. V následujícím příkladu byla zjištěna útoku hrubou silou RDP s 294 neúspěšných pokusů protokolu RDP. Doporučené řešení je k dispozici.

![Útok protokolu RDP](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu nastavení Azure Security Center a poté zkontrolovat virtuální počítače ve službě Security Center. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Nastavení shromažďování dat
> * Nastavte zásady zabezpečení
> * Zobrazení a opravte problémy s konfigurací stavu
> * Zkontrolujte zjištěnými hrozbami

Přechodu na v dalším kurzu se dozvíte, jak vytvořit kanál CI/CD s Visual Studio Team Services a virtuální počítač s Windows služby IIS.

> [!div class="nextstepaction"]
> [Visual Studio Team Services CI/CD kanálu](./tutorial-vsts-iis-cicd.md)
