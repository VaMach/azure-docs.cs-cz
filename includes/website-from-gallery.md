Azure Marketplace nabízí širokou škálu oblíbených webových aplikací vyvinutých společností Microsoft, jinými společnostmi a iniciativami v oblasti softwaru Open Source. Webové aplikace, které jsou vytvořené prostřednictvím Azure Marketplace, nevyžadují instalaci žádného softwaru kromě prohlížeče použitého pro připojení k [portálu Azure Preview](http://go.microsoft.com/fwlink/?LinkId=529715). 

V tomto kurzu se naučíte:

* Jak vytvořit novou webovou aplikaci prostřednictvím Azure Marketplace.
* Jak tuto webovou aplikaci nasadit pomocí portálu Azure Preview.

Vytvoříte blog WordPress, který využívá výchozí šablonu. Na následujícím obrázku je vidět hotová aplikace:

![Blog Wordpress][13]

> [!NOTE]
> Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="create-a-web-app-in-the-portal"></a>Vytvoření webové aplikace na portálu
1. Přihlaste se k portálu Azure Preview.
2. Otevřete Azure Marketplace, a to buď kliknutím na ikonu **Marketplace**:
   
    ![Ikona Marketplace][marketplace]
   
    nebo kliknutím na ikonu **Nová** v pravém horním rohu řídicího panelu a následným výběrem **Marketplace** v dolní části seznamu.
   
    ![Vytvořit nový][5]
3. Vyberte **Web + mobilní zařízení**. Vyhledejte **WordPress** a klikněte na ikonu **WordPress**.
   
    ![WordPress ze seznamu][7]
4. Přečtěte si popis aplikace WordPress a potom vyberte **Vytvořit**.
5. Klikněte na **Webová aplikace** zadejte požadované hodnoty pro konfiguraci vaší webové aplikace.
   
    ![Konfigurace aplikace][8]
6. Klikněte na **Databáze** a potom zadejte požadované hodnoty pro konfiguraci databáze MySQL. 
   
    ![Konfigurace databáze][database]
7. Zadejte název nové skupiny prostředků.
   
    ![Nastavení skupiny prostředků][groupname]
8. V případě potřeby klikněte na **PŘEDPLATNÉ** a určete, které předplatné se má použít. 
9. Až dokončíte definici webové aplikace, klikněte na **Vytvořit** a počkejte, než se nová webová aplikace vytvoří.
   
   Po vytvoření aplikace se zobrazí skupina prostředků obsahující webovou aplikaci a databázi.
   
   ![zobrazení skupiny][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Spuštění a správa webové aplikace WordPress
1. Kliknutím na novou webovou aplikaci zobrazíte podrobné údaje o této aplikaci.
   
    ![řídicí panel spuštění][10]
2. Na stránce **Základy ** klikněte na **Procházet** nebo na odkaz pod **adresou URL**. Otevře se úvodní stránka webové aplikace.
   
    ![adresa URL webu][browse]
3. Pokud jste nenainstalovali WordPress, zadejte správné konfigurační informace, které WordPress vyžaduje, a klikněte na **Instalovat WordPress**. Konfigurace se dokončí a otevře se přihlašovací stránka webové aplikace.
4. Klikněte na **Přihlásit** a zadejte svoje přihlašovací údaje.  
5. Teď máte novou webovou aplikaci WordPress, která vypadá podobně jako webová aplikace uvedená níž.    
   
    ![váš web WordPress][13]

[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png


<!--HONumber=Jan17_HO3-->


