<properties services="virtual-machines" title="How to Log on to a Virtual Machine Running Windows Server" authors="cynthn" solutions="" manager="timlt" editor="tysonn" />

4. Kliknutím na **Připojit** vytvoříte a stáhnete soubor protokolu Remote Desktop Protocol (.rdp). Kliknutím na **Otevřít** tento soubor použijete.

5. Zobrazí se upozornění, že soubor RDP je od neznámého vydavatele. To je normální. Pokračujte kliknutím na **Připojit** v okně Připojení ke vzdálené ploše.

    ![Snímek obrazovky upozornění na neznámého vydavatele](./media/virtual-machines-log-on-win-server/rdp-warn.png)

6. V okně **Zabezpečení systému Windows** zadejte přihlašovací údaje k účtu virtuálního počítače a potom klikněte na **OK**.

    **Místní účet** – Většinou je to uživatelské jméno a heslo k místnímu účtu, které jste zadávali při vytváření virtuálního počítače. V tomto případě je doménou název virtuálního počítače ve formátu *název_virtuálního_počítače*&#92;*uživatelské_jméno*.  
    
    **Virtuální počítač připojený k doméně** – Pokud virtuální počítač patří do domény, zadejte uživatelské jméno ve formátu *doména*&#92;*uživatelské_jméno*. Účet také musí být členem skupiny Administrators nebo musí mít udělené oprávnění ke vzdálenému přístupu k virtuálnímu počítači.
    
    **Řadič domény** – Pokud je virtuální počítač řadičem domény, zadejte uživatelské jméno a heslo účtu správce domény pro danou doménu.

7.  Kliknutím na **Ano** ověřte identitu virtuálního počítače a dokončete přihlášení.

    ![Snímek obrazovky zobrazující zprávu o ověření identity virtuálního počítače](./media/virtual-machines-log-on-win-server/cert-warning.png)



<!--HONumber=Aug16_HO4-->


