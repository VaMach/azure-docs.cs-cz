<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a>Chcete-li zapojení kabeláže zařízení pro napájení
> [!NOTE]
> Obě skříně zařízení StorSimple zahrnují redundantní PCMs. Pro každou skříň musí být nainstalované PCMs a připojené k jiné power zdroje k zajištění vysoké dostupnosti.
> 
> 

1. Zajistěte, aby napájení přepínače na všechny PCMs byla v pozici OFF.
2. Na primární skříň připojte k obou PCMs napájecích kabelů. Napájecích kabelů jsou identifikovány červeně v diagramu kabeláže napájení.
3. Ujistěte se, že dvě PCMs na primární skříni používají zdrojů energie samostatné.
4. K zapnutí jednotek pro distribuci rack připojte napájecích kabelů, jak je znázorněno v power kabelů diagram.
5. Opakujte kroky 2 až 4 pro EBOD skříň.
6. Zapněte skříni EBOD překlopení vypínač na každý PCM na pozici ON.
7. Ověřte, jestli je zapnutá skříni EBOD kontrolou, že jsou zelená LED na zadní straně řadičem EBOD povolena ON.
8. Zapněte primární skříň překlopení každý přepínač PCM na pozici ON.
9. Ověřte, že systém je na zajištěním řadiče zařízení, které jste zapnuli LED.
10. Ujistěte se, že je spojení mezi řadičem EBOD a řadiče zařízení aktivní pomocí ověření, že jsou čtyři LED vedle SAS portů na řadiči EBOD zelená.
    
    > [!IMPORTANT]
    > K zajištění vysoké dostupnosti pro váš systém, doporučujeme, budete striktně dodržovat power kabelů schéma vidět v následujícím diagramu.
    > 
    > 
    
    ![Zapojení kabeláže zařízení 4U pro napájení](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Kabeláž napájení**
    
    | Štítek | Popis |
    |:--- |:--- |
    | 1 |Primární skříň |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |Řadič 0 |
    | 5 |Řadič 1 |
    | 6 |EBOD řadič 0 |
    | 7 |EBOD řadiči 1 |
    | 8 |EBOD skříň |
    | 9 |Jednotky PDU |

