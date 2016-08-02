<properties
   pageTitle="Konfigurace služby Azure Automation"
   description="Popisuje kroky, které je třeba provést při konfiguraci služby Azure Automation pro počáteční použití."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="magoedte;bwren" />

# Konfigurace služby Azure Automation

Tento článek popisuje akce, které musíte provést, abyste službu Azure Automation mohli začít používat.

## Účty Automation

Při prvním spuštění služby Azure Automation vytvořte alespoň jeden účet Automation. Účty Automation umožňují izolovat vaše prostředky služby Automation (runbooky, assety, konfigurace) od prostředků služby Automation, které jsou obsažené v jiných účtech Automation. Účty Automation můžete použít k rozdělení prostředků služby Automation do samostatných logických prostředí. Můžete například jeden účet použít pro vývoj a druhý k produkci.

Prostředky služby Automation jednotlivých účtů Automation jsou přidružené k jedné oblasti Azure, ale účty Automation můžou spravovat služby Azure v libovolné oblasti. Hlavním důvodem k vytvoření účtů Automation v různých oblastech by byla situace, kdy máte zásady, které vyžadují izolaci dat a prostředků v určité oblasti.

>[AZURE.NOTE] Účty Automation a v nich obsažené prostředky, které jsou vytvořené pomocí portálu Azure, nemůžete otevírat pomocí portálu Azure Classic. Pokud chcete tyto účty nebo jejich prostředky spravovat pomocí rozhraní Windows PowerShell, použijte moduly Azure Resource Manageru. 
>
>Účty Automation vytvořené pomocí portálu Azure Classic můžete spravovat buď pomocí portálu nebo pomocí rutin. Po vytvoření účtu už nezáleží na způsobu vytváření a správy prostředků v rámci účtu. Pokud máte v úmyslu pokračovat v používání portálu Azure Classic, doporučujeme, abyste ho používali k vytváření účtů Automation místo portálu Azure.


Pokud by se vyskytl problém s vaším účtem Azure, například zpožděná platba, účet Automation může být pozastavený. V takovém případě nebudete mít na účet přístup, úlohy budou pozastavené a všechny plány budou zakázané. Účet si budete moct zobrazit, ale neuvidíte na něm žádné prostředky. Jakmile problém vyřešíte a účet Automation bude povolený, povolte opět plány a restartujte všechny pozastavené runbooky.


## Konfigurace ověřování k prostředkům Azure

Když k prostředkům Azure přistupujete pomocí [rutin Azure](http://msdn.microsoft.com/library/azure/jj554330.aspx), musíte zadat ověření vašeho předplatného Azure. Ve službě Azure Automation se to provádí pomocí organizačního účtu v Azure Active Directory, který pro svoje předplatné nakonfigurujete jako správce. Potom můžete vytvořit [přihlašovací údaje](http://msdn.microsoft.com/library/dn940015.aspx) tohoto uživatelského účtu a použít je s účtem [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) ve svém runbooku.

>[AZURE.NOTE] Účty Microsoft, dřív označované jako LiveID, se ve službě Azure Automation nedají použít.

## Vytvoření nového uživatele Azure Active Directory ke správě předplatného Azure

1. Přihlaste se k portálu Azure Classic jako správce služby pro předplatné Azure, které chcete spravovat.
2. Výběr **Active Directory**
3. Vyberte název adresáře, který je přidružený k vašemu předplatnému Azure. V případě potřeby můžete toto přidružení změnit: **Nastavení > Předplatné > Upravit adresář**.
4. [Vytvořte nového uživatele služby Active Directory](http://msdn.microsoft.com/library/azure/hh967632.aspx).  U možnosti **Typ uživatele** vyberte **Nový uživatel v organizaci**. Nepovolujte možnost **Povolit vícefaktorové ověřování**.
5. Poznamenejte si celé jméno uživatele a dočasné heslo.
7. Vyberte **Nastavení > Správci > Přidat**.
8. Zadejte úplné uživatelské jméno vytvořeného uživatele.
9. Vyberte předplatné, které má uživatel spravovat.
10. Odhlaste se ze služby Azure a potom se přihlaste zpět pomocí právě vytvořeného účtu. Zobrazí se výzva ke změně uživatelského hesla.
11. Vytvořte nový [asset přihlašovacích údajů Azure Automation](automation-credentials.md) pro uživatelský účet, který jste vytvořili.  **Typ přihlašovacích údajů** by měl být **Přihlašovací údaje Windows PowerShell**.

## Vytvoření účtu Automation

Účet Automation je kontejnerem pro vaše prostředky Azure Automation. Nabízí možnost oddělení vašich prostředí nebo dalšího uspořádání pracovních postupů. Pokud už máte účet Automation vytvořený, můžete tento krok přeskočit.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).

2. Klikněte na **Nový** > **Správa** > **Účet Automation**

3. V okně **Přidat účet Automation** nakonfigurujte podrobnosti svého účtu Automation. 

>[AZURE.NOTE] Když vytvoříte účet Automation pomocí portálu Azure, nebude se svými přidruženými prostředky přenesen zpět na portál Azure Classic. 

Níže je uvedený seznam parametrů ke konfiguraci:

|Parametr            |Popis |
|:---|:---|
| Název | Název účtu Automation (musí mít jedinečnou hodnotu). |
| Skupina prostředků | Skupiny prostředků usnadňují prohlížení a správu souvisejících prostředků Azure. Na portálu Azure můžete pro svůj účet Automation vybrat existující skupinu prostředků nebo vytvořit novou, zatímco na portálu Azure Classic budou všechny účty Automation umístěné do výchozí skupiny prostředků. |
| Předplatné | V seznamu dostupných předplatných si vyberte požadované předplatné. |
| Oblast | Oblast určuje místo, kde budou prostředky služby Automation na účtu uložené. V seznamu si můžete vybrat libovolnou oblast, funkčnost účtu to nijak neovlivní. Runbooky se ale budou spouštět rychleji, když bude oblast účtu poblíž místa, kde jsou uložené vaše ostatní prostředky služby Azure. |
| Možnosti účtu | Tato možnost vám umožňuje zvolit, jaké prostředky vytvoříte na novém účtu Automation. Výběrem **Ano** vytvoříte runbook kurzu. |

![Vytvoření účtu](media/automation-configuration/automation-01-create-automation-account.png)

>[AZURE.NOTE] Když účet Automation (vytvořený pomocí portálu Azure Classic) [přesunete do jiné skupiny prostředků](../resource-group-move-resources.md) pomocí portálu Azure, účet Automation už nebude na portálu Azure Classic dostupný.



## Použití přihlašovacích údajů v runbooku

Přihlašovací údaje v runbooku můžete získat pomocí aktivity [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) a použít je s [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) k připojení k vašemu předplatnému Azure. Pokud přihlašovací údaje patří správci několika předplatných Azure, potom byste měli použít také [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) a určit to správné. Můžete to vidět níže v ukázce prostředí Windows PowerShell, které se obvykle zobrazuje v horní části většiny runbooků služby Azure Automation.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Tyto řádky byste měli v runbooku opakovat po všech [kontrolních bodech](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints). Pokud je runbook pozastavený a potom se obnoví u dalšího pracovního procesu, je potřeba znovu provést ověření.

## Související články
- [Azure Automation: Ověřování na Azure pomocí Azure Active Directory](https://azure.microsoft.com/blog/2014/08/27/azure-automation-authenticating-to-azure-using-azure-active-directory/)
 



<!--HONumber=Jun16_HO2-->


