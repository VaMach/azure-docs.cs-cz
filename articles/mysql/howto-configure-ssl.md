---
title: "Konfigurace připojení SSL k bezpečnému připojování k databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Pokyny, jak správně nakonfigurovat databázi Azure pro MySQL a přidružené aplikace správně používat připojení SSL"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 10/25/2017
ms.openlocfilehash: 83830e4776eaa7c4f10bc14dcefd47c6eaf25997
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Konfigurace připojení SSL v aplikaci pro zabezpečené připojení k databázi Azure pro databázi MySQL
Azure databáze pro databázi MySQL podporuje připojení databáze Azure pro server databáze MySQL pro klientské aplikace pomocí Secure Sockets Layer (SSL). Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.

## <a name="step-1-obtain-ssl-certificate"></a>Krok 1: Získání certifikátu SSL
Stáhněte si certifikát potřebné pro komunikaci pomocí protokolu SSL s Azure databáze MySQL serveru od [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) a uložte soubor certifikátu na místní disk (v tomto kurzu použili jsme c:\ssl).
**Aplikace Microsoft Internet Explorer a Microsoft Edge:** po dokončení stahování, přejmenujte BaltimoreCyberTrustRoot.crt.pem certifikát.

## <a name="step-2-bind-ssl"></a>Krok 2: Vytvoření vazby SSL
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Připojování k serveru pomocí nástroje MySQL Workbench přes protokol SSL
Nakonfigurujte MySQL Workbench pro zabezpečené připojení prostřednictvím protokolu SSL. V dialogu nastavit připojení k nové, přejděte na **SSL** kartě. V **souboru certifikační Autorita protokolu SSL:** pole, zadejte umístění souboru **BaltimoreCyberTrustRoot.crt.pem**. 
![Uložte vlastní dlaždici](./media/howto-configure-ssl/mysql-workbench-ssl.png) pro existující připojení, můžete vytvořit vazbu SSL kliknutím pravým tlačítkem na ikonu připojení a zvolte Upravit. Pak přejděte do **SSL** kartě a vytvořte vazbu soubor certifikátu.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Připojování k serveru pomocí rozhraní příkazového řádku MySQL přes protokol SSL
Další způsob vytvoření vazby certifikátu SSL je pomocí rozhraní příkazového řádku MySQL tím spustíte následující příkaz:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Krok 3: Vynucování připojení SSL v Azure 
### <a name="using-the-azure-portal"></a>Použití webu Azure Portal
Pomocí portálu Azure, Azure databáze pro server databáze MySQL navštěvovat a pak klikněte na tlačítko **zabezpečení připojení**. Přepínací tlačítko použít k povolení nebo zakázání **připojení SSL vynutit** nastavení a potom klikněte na **Uložit**. Microsoft doporučuje, vždy povolit **připojení SSL vynutit** nastavení pro lepší zabezpečení.
![povolit ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Použití Azure CLI
Můžete povolit nebo zakázat **ssl vynucení** parametr pomocí povoleno nebo zakázáno hodnoty v uvedeném pořadí v rozhraní příkazového řádku Azure.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-the-ssl-connection"></a>Krok 4: Ověření připojení protokolem SSL
Spuštění mysql **stav** příkazu ověřte, že jste připojení k serveru databáze MySQL pomocí protokolu SSL:
```dos
mysql> status
```
Zkontrolujte připojení se šifrují kontrolou výstupu, který by měl zobrazit: **SSL: šifer používán je AES256 SHA** 

## <a name="sample-code"></a>Ukázka kódu
Pokud chcete navázat zabezpečené připojení k databázi Azure pro databázi MySQL přes protokol SSL z vaší aplikace, přečtěte následující ukázky kódu.
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)
```
try:
    conn=mysql.connector.connect(user='myadmin@myserver4demo', 
        password='yourpassword', 
        database='quickstartdb', 
        host='myserver4demo.mysql.database.azure.com', 
        ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```
### <a name="python-pymysql"></a>Python (PyMySQL)
```
conn = pymysql.connect(user = 'myadmin@myserver4demo', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'myserver4demo.mysql.database.azure.com', 
        ssl = {'ssl': {'ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```
### <a name="ruby"></a>Ruby
```
client = Mysql2::Client.new(
        :host     => 'myserver4demo.mysql.database.azure.com', 
        :username => 'myadmin@myserver4demo',      
        :password => 'yourpassword',    
        :database => 'quickstartdb',
        :ssl_ca => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
    )
```
### <a name="golang"></a>Golang
```
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/BaltimoreCyberTrustRoot.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool, InsecureSkipVerify: true})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@myserver4demo' , 'yourpassword', 'myserver4demo.mysql.database.azure.com', 'quickstartdb') 
db, _ := sql.Open("mysql", connectionString)
```
### <a name="javajdbc"></a>JAVA(JDBC)
```
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'myserver4demo.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@myserver4demo');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="javamariadb"></a>Java(MariaDB)
```
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'myserver4demo.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@myserver4demo');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

## <a name="next-steps"></a>Další kroky
Přečtěte si následující různé možnosti připojení aplikace [knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
