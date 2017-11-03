<!--author=alkohli last changed: 07/07/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>Instalace aktualizace z webu Azure Portal

1. Na stránce služby StorSimple vyberte zařízení.

    ![Vyberte zařízení](./media/storsimple-8000-install-update4-via-portal/update1.png)

2. Přejděte na **nastavení zařízení** > **aktualizace zařízení**.

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update4-via-portal/update2.png)

2. Zobrazí se upozornění, pokud jsou k dispozici nové aktualizace. Můžete taky v **aktualizace zařízení** okně klikněte na tlačítko **kontrolovat aktualizace**. Vytvoří se úloha vyhledávání dostupných aktualizací. Po úspěšném dokončení úlohy se zobrazí zpráva.

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update4-via-portal/update3.png)

3. Doporučujeme, abyste si před instalací aktualizace na zařízení prošli poznámky k verzi. Chcete-li použít aktualizace, klikněte na tlačítko **nainstalovat aktualizace**. V **potvrďte pravidelné aktualizace** okno, přečtěte si požadavky pro provedení úkolu před použití aktualizací. Zaškrtněte políčko s informací, že budete chtít aktualizovat zařízení a potom klikněte na o **nainstalovat**.

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update4-via-portal/update4.png)

6. Spustí se sada kontrol požadavků. Mezi tyto kontroly patří:
   
   * **Kontroly stavu kontrolerů** pro ověření, že oba kontrolery zařízení jsou v pořádku a online.
   * **Kontroly stavu hardwarových komponent** pro ověření, že všechny hardwarové komponenty v zařízení StorSimple jsou v pořádku.
   * **Kontroly rozhraní DATA 0** pro ověření, že je na zařízení povolené rozhraní DATA 0. Pokud toto rozhraní není povolené, musíte jej povolit a poté to zkusit znovu.

    Aktualizace je stažen a nainstalován pouze v případě, že jsou všechny kontroly úspěšně dokončeny. Na probíhání kontrol budete upozorněni. Pokud prechecks selže, pak vám bude poskytnuta s příčiny chyby. Vyřešit tyto problémy a poté operaci opakujte. Pokud si s těmito problémy neporadíte sami, bude muset kontaktovat podporu Microsoftu.

7. Po úspěšném dokončení prechecks se vytvoří úloha aktualizace. Po úspěšném vytvoření úlohy aktualizace se zobrazí upozornění.
   
    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update4-via-portal/update6.png)
   
    Aktualizace se potom nainstaluje na vašem zařízení.

9. Dokončení aktualizace trvá několik hodin. Podrobnosti o úloze můžete kdykoli zobrazit výběrem úlohy aktualizace a kliknutím na **Podrobnosti**.

    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update4-via-portal/update8.png)

     Můžete také sledovat průběh úlohy aktualizace z **nastavení zařízení > úlohy**. Na **úlohy** okně uvidíte průběh aktualizace.

     ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update4-via-portal/update7.png)

10. Po dokončení úlohy, přejděte na **nastavení zařízení > aktualizace zařízení**. Nyní je třeba aktualizovat verze softwaru.

    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update4-via-portal/update9.png)

