<properties
    pageTitle="Použití řízení přístupu na základě rolí na portálu Azure | Microsoft Azure"
    description="Začněte se správou přístupu pomocí řízení přístupu na základě rolí na portálu Azure. Oprávnění k prostředkům se přiřazují pomocí přiřazení rolí."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/21/2016"
    ms.author="kgremban"/>

# Použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure

Řízení přístupu na základě role v Azure umožňuje přesnou správu přístupu. Pomocí řízení přístupu na základě role v Azure můžete uživatelům poskytnout pouze takovou úroveň přístupu, kterou potřebují k provádění svých úloh. Tento článek vám pomůže nastavit a spustit řízení přístupu na základě role v Azure na portálu Azure. Další informace o tom, jak vám řízení přístupu na základě role v Azure pomůže spravovat přístup uživatelů najdete v článku [Co je řízení přístupu na základě role](role-based-access-control-what-is.md).

## Zobrazení informací o přístupu
Na [portálu Azure](https://portal.azure.com) můžete v hlavním okně sledovat, kdo má přístup k prostředku, skupině prostředků nebo předplatnému. Například byste se mohli chtít podívat, kdo má přístup k jedné ze skupin prostředků:

1. Na navigačním panelu vlevo vyberte **Skupiny prostředků**.  
    ![Skupiny prostředků – ikona](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. V okně **Skupiny prostředků** vyberte název skupiny prostředků.
3. V okně skupiny prostředků vpravo nahoře vyberte možnost **Uživatelé**.  
    ![Uživatelé – ikona](./media/role-based-access-control-configure/users_icon.png)
4. V okně **Uživatelé** se zobrazují všichni uživatelé, skupiny a aplikace, kterým byl udělen přístup ke skupině prostředků.  

    ![Snímek obrazovky s oknem uživatelé – zděděný a přiřazený přístup](./media/role-based-access-control-configure/view-access.png)

Všimněte si, že některým uživatelům byl přístup **přiřazen**, zatímco jiní jej **zdědili**. Přístup je ke skupině prostředků přiřazen přímo, nebo je zděděn od přiřazení nadřazeného předplatného.

> [AZURE.NOTE] Správci a pomocní správci v klasickém modelu předplatného jsou v novém modelu řízení přístupu na základě role v Azure  považováni za vlastníky.


## Přidání přístupu
Přístup se uděluje z prostředku, skupiny prostředků nebo předplatného, které je v rozsahu platnosti přiřazení role.

1. V okně **Uživatelé** vyberte **Přidat**.  
    ![Přidat – ikona](./media/role-based-access-control-configure/add_icon.png)  
2. V okně **Vyberte roli** vyberte roli, kterou chcete přiřadit.
3. Ve svém adresáři vyberte uživatele, skupinu nebo aplikaci, kterým chcete přiřadit přístup. V adresáři můžete vyhledávat pomocí zobrazovaných názvů, e-mailových adres a identifikátorů objektů.  

    ![Snímek obrazovky s oknem Přidat uživatele – vyhledávání](./media/role-based-access-control-configure/grant-access2.png)

4. Výběrem tlačítka **OK** vytvořte přiřazení. V automaticky otevřeném okně **Přidává se uživatel.** se zobrazuje průběh operace.  
    ![Ukazatel průběhu přidávání uživatele – snímek obrazovky](./media/role-based-access-control-configure/addinguser_popup.png)

Po úspěšném přiřazení role se přiřazení zobrazí v okně **Uživatelé**.

## Odebrání přístupu

1. V okně **Uživatelé** vyberte přiřazení role.
2. V okně podrobností přiřazení vyberte **Odebrat**.  
    ![Odebrat – ikona](./media/role-based-access-control-configure/remove_icon.png)
3. Výběrem položky **ano** odebrání potvrďte.  
    ![Snímek obrazovky s oknem Uživatelé – odebrání role](./media/role-based-access-control-configure/remove-access1.png)

Zděděná přiřazení nelze odebrat. Na obrázku níže si všimněte, že je tlačítko odebrání neaktivní. Místo toho je nutné zobrazit podrobnosti **Přiřazený v uzlu**. Poté přejděte k prostředku, který je zde uvedený, a odeberte přiřazení role v něm.

![Snímek obrazovky s oknem Uživatelé – neaktivní tlačítko odebrání u zděděného přístupu](./media/role-based-access-control-configure/remove-access2.png)

## Další nástroje pro správu přístupu
Přiřazování rolí a správu přístupu pomocí řízení přístupu na základě role v Azure lze provádět i v jiných nástrojích než portálu Azure.  Další informace o požadavcích a základních postupech příkazů řízení přístupu na základě role v Azure najdete na následujících odkazech.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Rozhraní příkazového řádku Azure](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

## Další kroky
- [Vytvoření sestavy historie změn přístupu](role-based-access-control-access-change-history-report.md)
- Další informace najdete v článku [Vestavěné role řízení přístupu na základě role v Azure](role-based-access-built-in-roles.md).
- Definujte své [Vlastní role funkce řízení přístupu na základě role v Azure](role-based-access-control-custom-roles.md).



<!--HONumber=Aug16_HO4-->


