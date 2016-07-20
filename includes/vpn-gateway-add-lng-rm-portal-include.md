1. Na portálu Azure klikněte na **Nový** **>** **Sítě** **>** **Brána místní sítě**.

    ![Vytvoření brány místní sítě](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. V okně **Vytvoření brány místní sítě** zadejte **Název** objektu brány místní sítě.
 
3. Do pole **IP adresa** zadejte IP adresu brány. Jedná se o IP adresu externího zařízení VPN, ke kterému se chcete připojit. IP adresa nemůže být za serverem NAT a musí být dostupná pro Azure.

4. Údaj v části **Adresní prostor** odkazuje na rozsahy adres ve vaší (obvykle místní) síti. Můžete přidat více různých rozsahů adres. Rozsahy, které zadáte v této části, nesmí překrývat žádné z rozsahů adresních prostorů, které používáte pro jakékoliv virtuální sítě, se kterými budete komunikovat prostřednictvím brány.  Bude nutné provést koordinaci s vaší místní konfigurací a také s adresními prostory vaší virtuální sítě Azure.
 
5. V rozevíracím seznamu **Předplatné** zkontrolujte, jestli se zobrazuje správné předplatné.

6. V rozevíracím seznamu **Skupina prostředků** vyberte skupinu prostředků, kterou chcete použít. Můžete vytvořit novou skupinu prostředků, nebo vybrat skupinu prostředků, kterou jste už vytvořili. Pokud chcete vytvořit novou skupinu prostředků, zadejte do pole její název. Pokud chcete vybrat skupinu prostředků, kterou jste už vytvořili, kliknutím na **Skupina prostředků** otevřete okno **Skupina prostředků** a potom vyberte skupinu prostředků, kterou chcete použít.

7. Pokud vytváříte novou bránu místní sítě, v rozevíracím seznamu **Umístění** můžete vybrat stejné umístění, jaké má brána virtuální sítě. Není to však nutné. Brána místní sítě může být v jiném umístění. 

8. Pokud chcete mít k této bráně místní sítě snadný přístup z řídicího panelu, nechte zaškrtnuté políčko Připnout na řídicí panel.

9. Kliknutím na **Vytvořit** vytvořte bránu místní sítě. Na řídicím panelu se zobrazí dlaždice nasazování brány místní sítě.

10. Po vytvoření brány místní sítě se brána otevře na portálu, abyste si ji mohli prohlédnout.

    



<!--HONumber=Jun16_HO2-->


