<properties
   pageTitle="Konfigurace účtu uživatele Azure AD | Microsoft Azure"
   description="Tento článek popisuje způsob konfigurace přihlašovacích údajů účtu uživatele Azure AD pro runbooky ve službě Azure Automation za účelem ověřování ARM a ASM."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="uživatel azure active directory, správa služby azure, uživatelský účet azure ad" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/12/2016"
   ms.author="magoedte" />


# Ověření runbooku pomocí správy služby Azure a Resource manageru

Tento článek popisuje kroky, které musíte provést při konfiguraci účtu uživatele Azure AD pro runbooky služby Azure Automation, které běží s prostředky ASM (správa služby Azure) a ARM (Azure Resource Manager).  I když toto zůstává podporovanou identitou ověřování pro runbooky založené na ARM, doporučujeme používat nový účet Spustit v Azure jako.       

## Vytvoření nového uživatele Azure Active Directory

1. Přihlaste se k portálu Azure Classic jako správce služby pro předplatné Azure, které chcete spravovat.
2. Vyberte **Active Directory** a potom vyberte název adresáře své organizace.
3. Vyberte kartu **Uživatelé** a potom v oblasti příkazů vyberte **Přidat uživatele**.
4. Na stránce **Informace o uživateli** v části **Typ uživatele** vyberte **Nový uživatel v organizaci**.
5. Zadejte jméno uživatele.  
6. Na stránce Active Directory vyberte název adresáře, který je přidružený k vašemu předplatnému Azure.
7. Na stránce **profilu uživatele** zadejte jeho jméno, příjmení a uživatelské jméno a v seznamu **Role** vyberte uživatele.  **Nepovolujte službu Multi-Factor Authentication**.
8. Poznamenejte si celé jméno uživatele a dočasné heslo.
9. Vyberte **Nastavení > Správci > Přidat**.
10. Zadejte úplné uživatelské jméno vytvořeného uživatele.
11. Vyberte předplatné, které má uživatel spravovat.
12. Odhlaste se ze služby Azure a potom se přihlaste zpět pomocí právě vytvořeného účtu. Zobrazí se výzva ke změně uživatelského hesla.


## Vytvoření účtu Automation na portálu Azure Classic
V této části provedete následující kroky, abyste na webu Azure Portal vytvořili nový účet Azure Automation, který budete spolu s runbooky používat ke správě prostředků v režimu ASM a ARM.  

>[AZURE.NOTE] Účty Automation vytvořené na webu Azure Portal Classic můžete spravovat pomocí webu Azure Portal Classic i pomocí webu Azure Portal nebo pomocí sady rutin. Po vytvoření účtu už nezáleží na způsobu vytváření a správy prostředků v rámci účtu. Pokud máte v úmyslu pokračovat v používání webu Azure Portal Classic, doporučujeme, abyste ho používali k vytváření účtů Automation místo webu Azure Portal.


1. Přihlaste se k portálu Azure Classic jako správce služby pro předplatné Azure, které chcete spravovat.
2. Vyberte **Automation**.
3. Na stránce **Automation** vyberte **Vytvořit účet Automation**.
4. Do pole **Vytvořit účet Automation** zadejte název nového účtu Automation a v rozevíracím seznamu vyberte **Oblast**.  
5. Kliknutím na tlačítko **OK** potvrďte svoje nastavení a vytvořte účet.
6. Účet bude po vytvoření uvedený na stránce **Automation**.
7. Kliknutím na účet přejdete na stránku řídicího panelu.  
8. Na stránce řídicího panelu Automation vyberte **Assety**.
9. V dolní části stránky **Assety** vyberte **Přidat nastavení**.
10. Na stránce **Přidání nastavení** vyberte **Přidat přihlašovací údaje**.
11. Na stránce **Definování přihlašovací údaje** vyberte v rozevíracím seznamu **Typ přihlašovacích údajů** možnost **Přihlašovací údaje Windows PowerShell** a zadejte název přihlašovacích údajů.
12. Na následující stránce **Definování přihlašovacích údajů** zadejte do pole **Uživatelské jméno** dříve vytvořené uživatelské jméno účtu uživatele AD a do pole **Heslo** a **Potvrzení hesla** zadejte heslo. Klikněte na tlačítko **OK** a uložte změny.

## Vytvoření účtu Automation na webu Azure Portal

V této části provedete následující kroky, abyste na webu Azure Portal vytvořili nový účet Azure Automation, který budete spolu s runbooky používat ke správě prostředků v režimu ARM.  

1. Přihlaste se k portálu Azure jako správce služby pro předplatné Azure, které chcete spravovat.
2. Vyberte **Účty Automation**.
3. V okně Účty Automation klikněte na **Přidat**.<br>![Přidání účtu Automation](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
2. V okně **Přidat účet Automation** do pole **Název** zadejte název nového účtu Automation.
5. Pokud máte více než jedno předplatné, zadejte předplatné pro nový účet a také novou nebo existující **skupinu prostředků** a **umístění** datového centra Azure.
3. U možnosti **Vytvořit účet Spustit v Azure jako** vyberte **Ne** a klikněte na tlačítko **Vytvořit** tlačítko.  

    >[AZURE.NOTE] Pokud se rozhodnete nevytvořit účet Spustit v Azure jako tím, že nevyberete možnost **Ne**, zobrazí se v okně **Přidání účtu Automation** zpráva upozornění.  Při vytváření účtu a jeho přiřazování k roli **přispěvatele** v předplatném, nebude mít účet odpovídající identitu ověřování v rámci adresářové služby vašeho předplatného, a proto nebude předplatné obsahovat žádné přístupové prostředky.  Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky ARM.

    ![Přidání upozornění k účtu Automation](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

4. Zatímco Azure vytváří účet Automation, můžete průběh sledovat v nabídce v části **Oznámení**.

Po vytvoření přihlašovacích údajů bude nutné, abyste vytvořili asset přihlašovacích údajů, který přidružíte k účtu Automation pomocí dříve vytvořeného účtu uživatele služby AD.  Nezapomeňte, že jsme účet Automation teprve vytvořili a že ještě není přidružený k identitě ověřování.  Proveďte kroky popsané v [článku Assety přihlašovacích údajů v Azure Automation](../automation/automation-credentials.md#creating-a-new-credential) a zadejte hodnotu **uživatelského jména** ve formátu **doména\uživatel**.

## Použití přihlašovacích údajů v runbooku

Přihlašovací údaje v runbooku můžete získat pomocí aktivity [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) a použít je s [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) k připojení k vašemu předplatnému Azure. Pokud přihlašovací údaje patří správci několika předplatných Azure, potom byste měli použít také [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) a určit to správné. Můžete to vidět níže v ukázce prostředí Windows PowerShell, které se obvykle zobrazuje v horní části většiny runbooků služby Azure Automation.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Tyto řádky byste měli v runbooku opakovat po všech [kontrolních bodech](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints). Pokud je runbook pozastavený a potom se obnoví u dalšího pracovního procesu, je potřeba znovu provést ověření.

## Další kroky
* O různých typech runbooků a krocích k vytvoření vlastních runbooků si můžete přečíst v článku [Typy runbooků v Azure Automation](../automation/automation-runbook-types.md)



<!--HONumber=Sep16_HO3-->


