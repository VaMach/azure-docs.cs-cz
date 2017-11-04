<!--author=alkohli last changed:02/22/16-->

#### <a name="to-attach-the-sas-cables"></a>Připojit SAS kabely
1. Určete primární a skříně EBOD. Dva skříně lze identifikovat podle jejich odpovídajících back roviny. Viz následující obrázek pokyny. 
   
    ![Zálohovat roviny primární a EBOD skříně](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Back – primární a EBOD skříně**
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Primární skříň |
   | 2 |EBOD skříň |
2. Vyhledejte sériových čísel na primárním serverem a skříně EBOD. Sériové číslo štítku je opatřen back vymazat z každé skříně. Sériová čísla musí být stejná na obou skříně. [Kontaktujte Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) okamžitě, pokud sériová čísla se neshodují. Viz následující obrázek najít sériová čísla.
   
    ![Zpětná zobrazení skříň zobrazující sériové číslo](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Umístění štítku sériové číslo**
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Vymazat skříni |
3. Použijte zadaný SAS kabely připojení skříni EBOD ke skříni primární následujícím způsobem:
   
   1. Identifikujte čtyři SAS portů na skříni primární a EBOD skříň. SAS porty jsou označeny jako EBOD na primární skříni a odpovídají A portů na skříni EBOD, jak je znázorněno v SAS kabelů obrázku níže.
   2. Použít pro připojení A. portu k portu EBOD zadaný SAS kabely
   3. Port EBOD na řadič 0 musí být připojené k portu A na řadiči EBOD 0. Port EBOD na řadiči 1 musí být připojené k portu A na řadiči EBOD 1. Viz následující obrázek pokyny. 
      
      ![SAS kabelů pro vaše zařízení](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS kabelů**
      
      | Štítek | Popis |
      |:--- |:--- |
      | A |Primární skříň |
      | B |EBOD skříň |
      | 1 |Řadič 0 |
      | 2 |Řadič 1 |
      | 3 |EBOD řadič 0 |
      | 4 |EBOD řadiči 1 |
      | 5, 6 |SAS porty na primární skříň (s popiskem EBOD) |
      | 7, 8 |SAS portů na skříni EBOD (Port A) |

