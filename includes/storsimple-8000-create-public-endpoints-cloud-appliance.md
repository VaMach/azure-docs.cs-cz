<a id="to-create-public-endpoints-on-the-cloud-appliance" class="xliff"></a>

#### Vytvoření veřejných koncových bodů na cloudovém zařízení

1. Přihlaste se k portálu Azure.
2. Přejděte na **Virtuální počítače** a vyberte a klikněte na virtuální počítač, který používáte jako cloudové zařízení.
    
3. Potřebujete vytvořit pravidlo skupiny zabezpečení sítě (NSG) pro řízení toku přenosů do a z virtuálního počítače. Proveďte následující kroky a vytvořte pravidlo skupiny zabezpečení sítě.
    1. Vyberte **Skupina zabezpečení sítě**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. Klikněte na výchozí skupinu zabezpečení sítě, která se zobrazí.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. Vyberte **Příchozí pravidla zabezpečení**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. Kliknutím na **+ Přidat** vytvořte příchozí pravidlo zabezpečení.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        V okně Přidat příchozí pravidlo zabezpečení:

        1. Do pole **Název** zadejte tento název koncového bodu: WinRMHttps.
        
        2. V poli **Priorita** vyberte číslo nižší než 1 000 (což je priorita výchozího pravidla). Čím vyšší hodnota, tím nižší priorita.

        3. Nastavte **Zdroj** na hodnotu **Všechny**.

        4. V poli **Služba** vyberte **WinRM**. **Protokol** je automaticky nastaven na **TCP** a **Rozsah portů** je nastaven na **5986**.

        5. Kliknutím na **OK** vytvořte pravidlo.

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. Posledním krokem je přidružení skupiny zabezpečení sítě k podsíti nebo konkrétnímu síťovému rozhraní. Proveďte následující kroky a přidružte skupinu zabezpečení sítě k podsíti.
    1. Přejděte na **Podsítě**.
    2. Klikněte na **+ Přidružit**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. Vyberte virtuální síť a pak vyberte vhodnou podsíť.
    4. Kliknutím na **OK** vytvořte pravidlo.

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

Po vytvoření pravidla můžete zobrazit jeho podrobnosti, abyste zjistili veřejnou virtuální IP adresu (VIP). Tuto adresu si poznamenejte.


