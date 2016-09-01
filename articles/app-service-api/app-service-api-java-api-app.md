<properties
    pageTitle="Sestavení a nasazení aplikace API Java v Azure App Service"
    description="Zjistěte, jak vytvořit balíček aplikace API Java a jak ho nasadit v Azure App Service."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="rachelap"/>

# Sestavení a nasazení aplikace API Java v Azure App Service

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

V tomto kurzu se dozvíte, jak vytvořit aplikaci Java a jak ji pomocí [Git] nasadit v Azure App Service API Apps. Pokyny v tomto kurzu platí pro všechny operační systémy, které podporují Javu. Kód v tomto kurzu je sestaven pomocí [Maven]. Projekt [Jax-RS] vytvoří službu RESTful a vygeneruje se na základě specifikace metadat [Swagger] v [editoru Swagger].

## Požadavky

1. [Sada pro vývojáře Java 8] (nebo novější)
1. Nástroj [Maven] nainstalovaný na počítači pro vývoj
1. Nástroj [Git] nainstalovaný na počítači pro vývoj
1. Placená nebo [bezplatná zkušební verze] předplatného [Microsoft Azure]
1. Testovací aplikace HTTP, například [Postman]

## Automatické generování (scaffold) rozhraní API pomocí editoru Swagger.IO

Pomocí online editoru swagger.io můžete zadat kód YAML nebo JSON pro Swagger představující strukturu rozhraní API. Jakmile budete mít hotový návrh svrchní oblasti rozhraní API, můžete kód exportovat do různých platforem a rozhraní. V další části se automaticky vygenerovaný kód upraví tak, aby zahrnoval imitované funkce. 

Tato ukázka začíná textem ve formátu JSON pro Swagger. Tento text vložíte do editoru swagger.io, ve kterém pak vygenerujete kód s využitím JAX-RS pro přístup ke koncovému bodu rozhraní REST API. Potom automaticky generovaný kód upravíte tak, aby vracel imitovaná data simulující sestavení REST API vytvořené na mechanismu trvalosti dat.  

1. Zkopírujte následující kód JSON pro Swagger do schránky:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Přejděte do [Online editor Swagger]. V editoru klikněte na položku nabídky **File (Soubor) -> Paste JSON (Vložit JSON)**.

    ![Položka nabídky Paste JSON (Vložit JSON)][paste-json]

1. Vložte kód JSON pro Swagger rozhraní API seznamu kontaktů, který jste předtím zkopírovali. 

    ![Vložení kódu JSON do Swaggeru][pasted-swagger]

1. Zobrazte stránky dokumentace a souhrn rozhraní API vykreslený v editoru. 

    ![Zobrazení dokumentů generovaných ve Swaggeru][view-swagger-generated-docs]

1. Vyberte možnost nabídky **Generate Server (Generovat server) -> JAX-RS** k automatickému vygenerování kódu na straně serveru, který později upravíte přidáním imitované implementace. 

    ![Generování položky nabídky kódu][generate-code-menu-item]

    Po vygenerování kódu se stáhne soubor ZIP. Tento soubor obsahuje kód automaticky vytvořený (scaffold) generátorem kódu Swagger a všechny přidružené skripty sestavení. Rozbalte celou knihovnu do adresáře na pracovní stanici vývoje. 

## Úprava kódu k přidání implementace rozhraní API

V této části nahradíte implementaci kódu na straně serveru generovaného ve Swaggeru vlastním kódem. Nový kód vrátí entity kontaktů volajícímu klientovi jako objekt ArrayList. 

1. Otevřete soubor modelu *Contact.java*, který najdete ve složce *src/gen/java/io/swagger/model*, v editoru [Visual Studio Code] nebo vašem oblíbeném textovém editoru. 

    ![Otevření souboru modelu kontaktů][open-contact-model-file]

1. Přidejte do třídy **Contact** následující konstruktor. 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Otevřete soubor implementace služby *ContactsApiServiceImpl.java*, který najdete ve složce *src/main/java/io/swagger/api/impl*, v editoru [Visual Studio Code] nebo svém oblíbeném textovém editoru.

    ![Otevření souboru kódu služby kontaktů][open-contact-service-code-file]

1. Přepsáním kódu v souboru tímto novým kódem přidejte do kódu služby imitovanou implementaci. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Otevřete příkazový řádek a změňte adresář na kořenovou složku vaší aplikace.

1. Spuštěním následujícího příkazu Mavenu vytvořte kód a spusťte ho místně pomocí aplikačního serveru Jetty. 

        mvn package jetty:run

1. Mělo by se zobrazit příkazové okno, ve kterém bude vidět, že server Jetty tento kód spustil na portu 8080. 

    ![Otevření souboru kódu služby kontaktů][run-jetty-war]

1. Pomocí aplikace [Postman] vytvořte žádost na metodu API get all contacts umístěnou na adrese http://localhost:8080/api/contacts.

    ![Volání rozhraní API kontaktů][calling-contacts-api]

1. Pomocí aplikace [Postman] vytvořte žádost na metodu API get specific contact umístěnou na adrese http://localhost:8080/api/contacts/2.

    ![Volání rozhraní API kontaktů][calling-specific-contact-api]

1. Nakonec sestavte soubor Java WAR (webový archiv) tak, že na konzole spustíte následující příkaz Mavenu. 

        mvn package war:war

1. Soubor WAR se po sestavení umístí do složky **target**. Přejděte do složky **target** a přejmenujte soubor WAR na **ROOT.war**. (Zkontrolujte, že použití velkých a malých písmen odpovídá tomuto formátu.)

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Nakonec spuštěním následujících příkazů z kořenové složky vaší aplikace vytvořte složku **deploy**, která bude použita k nasazení souboru WAR na Azure. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## Publikování výstupu v Azure App Service

V této části se dozvíte, jak vytvořit novou aplikaci API pomocí Portálu Azure, jak tuto aplikaci API připravit na hostování aplikací Java a jak nasadit nově vytvořený soubor WAR v Azure App Service, abyste mohli novou aplikaci API spustit. 

1. Kliknutím na položku nabídky **Nový -> Web + mobilní zařízení -> Aplikace API**, zadáním podrobností o aplikaci a kliknutím na **Vytvořit** vytvořte na [Azure Portal] novou aplikaci API.

    ![Vytvoření nové aplikace API][create-api-app]

1. Po vytvoření aplikace API otevřete v této aplikaci okno **Nastavení** a klikněte na položku nabídky **Nastavení aplikace**. Z dostupných možností vyberte nejnovější verzi Javy, potom v nabídce **Webový kontejner** vyberte nejnovější verzi serveru Tomcat a klikněte na **Uložit**.

    ![Nastavení Javy v okně aplikace API][set-up-java]

1. Klikněte na položku nabídky nastavení **Přihlašovací údaje pro nasazení** a zadejte uživatelské jméno a heslo, které chcete použít při publikování souborů ve vaší aplikaci API. 

    ![Nastavení přihlašovacích údajů nasazení][deployment-credentials]

1. Klikněte na položku nabídky nastavení **Zdroj nasazení**. Potom klikněte na tlačítko **Vybrat zdroj**, vyberte možnost **Místní úložiště Git** a klikněte na **OK**. Tím se vytvoří úložiště Git spuštěné v Azure, které bude přidružené k vaší aplikaci API. Pokaždé, když potvrdíte nějaký kód do *hlavní* větve úložiště Git, se tento kód publikuje v živé, běžící instanci aplikace API. 

    ![Nastavení nového místního úložiště Git][select-git-repo]

1. Zkopírujte adresu URL nového úložiště Git do schránky. Uložte ji, protože ji budete zanedlouho potřebovat. 

    ![Nastavení nového úložiště Git pro vaši aplikaci][copy-git-repo-url]

1. Proveďte operaci Git push se souborem WAR do online úložiště. To provedete tak, že přejdete do složky **deploy**, kterou jste dříve vytvořili, abyste mohli snadno potvrdit kód do úložiště spuštěného v App Service. Až přejdete v okně konzoly do složky, kde je umístěna složka webových aplikací, zahajte proces a spusťte nasazení provedením následujícího příkazu Git. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Jakmile odešlete žádost **push**, budete vyzváni k zadání hesla, které jste předtím vytvořili pro přihlašovací údaje nasazení. Po zadání přihlašovacích údajů by mělo být na portálu vidět, že aktualizace je nasazená.

1. Pokud znovu použijete aplikaci Postman k volání nově nasazené aplikace API spuštěné v Azure App Service, zjistíte, že její chování je konzistentní a že vrací kontaktní údaje podle očekávání při použití jednoduchých změn kódu Java automaticky vygenerovaného v editoru Swagger.io. 

    ![Použití živého REST API kontaktů Java v Azure][postman-calling-azure-contacts]

## Další kroky

V tomto článku jste úspěšně použili soubor dat JSON pro Swagger a automaticky generovaný kód Java získaný v editoru Swagger.io. Jednoduchými změnami kódu a procesem nasazení v Gitu jste získali funkční aplikaci API napsanou v jazyce Java. V dalším kurzu se dozvíte, jak [využívat aplikace API z klientů JavaScript pomocí CORS][CORS pro rozhraní API v App Service]. Další kurzy v této sérii vás seznámí s postupem implementace ověřování a autorizace.

Pokud chcete pracovat s tímto příkladem, zjistěte si další informace o [Sada SDK úložiště pro jazyk Java], abyste objekty blob JSON mohli nastavit jako trvalé. Nebo můžete pomocí [Sada SDK Document DB Java] ukládat kontaktní údaje v Azure DocumentDB. 

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].

<!-- URL List -->

[CORS pro rozhraní API v App Service]: app-service-api-cors-consume-javascript.md
[Azure Portal]: https://portal.azure.com/
[Sada SDK Document DB Java]: ../documentdb/documentdb-java-application.md
[bezplatná zkušební verze]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Středisko pro vývojáře Java]: /develop/java/
[Sada pro vývojáře Java 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online editor Swagger]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Sada SDK úložiště pro jazyk Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[editoru Swagger]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png



<!---HONumber=Aug16_HO4-->


