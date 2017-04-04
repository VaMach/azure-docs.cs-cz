Aktivita kopírování ve službě Data Factory kopíruje data ze zdrojového úložiště dat do úložiště dat jímky. Data Factory podporuje následující typy úložišť dat. Data z libovolného zdroje lze zapsat do libovolné jímky. Kliknutím na úložiště dat se dozvíte, jak kopírovat data z a do daného úložiště.

| Kategorie | Úložiště dat | Podporované jako zdroj | Podporované jako jímka |
|:--- |:--- |:--- |:--- |
| **Azure** |[Azure Blob Storage](../articles/data-factory/data-factory-azure-blob-connector.md) |✓ |✓ |
| &nbsp; |[Azure Data Lake Store](../articles/data-factory/data-factory-azure-datalake-connector.md) |✓ |✓ |
| &nbsp; |[Azure DocumentDB](../articles/data-factory/data-factory-azure-documentdb-connector.md) |✓ |✓ |
| &nbsp; |[Azure SQL Database](../articles/data-factory/data-factory-azure-sql-connector.md) |✓ |✓ |
| &nbsp; |[Azure SQL Data Warehouse](../articles/data-factory/data-factory-azure-sql-data-warehouse-connector.md) |✓ |✓ |
| &nbsp; |[Index Azure Search](../articles/data-factory/data-factory-azure-search-connector.md) | |✓ |
| &nbsp; |[Azure Table storage](../articles/data-factory/data-factory-azure-table-connector.md) |✓ |✓ |
| **Databáze** |[Amazon Redshift](../articles/data-factory/data-factory-amazon-redshift-connector.md) |✓ | |
| &nbsp; |[DB2](../articles/data-factory/data-factory-onprem-db2-connector.md)* |✓ | |
| &nbsp; |[MySQL](../articles/data-factory/data-factory-onprem-mysql-connector.md)* |✓ | |
| &nbsp; |[Oracle](../articles/data-factory/data-factory-onprem-oracle-connector.md)* |✓ |✓ |
| &nbsp; |[PostgreSQL](../articles/data-factory/data-factory-onprem-postgresql-connector.md)* |✓ | |
| &nbsp; |[SAP Business Warehouse](../articles/data-factory/data-factory-sap-business-warehouse-connector.md)* |✓ | |
| &nbsp; |[SAP HANA](../articles/data-factory/data-factory-sap-hana-connector.md)* |✓ | |
| &nbsp; |[SQL Server](../articles/data-factory/data-factory-sqlserver-connector.md)* |✓ |✓ |
| &nbsp; |[Sybase](../articles/data-factory/data-factory-onprem-sybase-connector.md)* |✓ | |
| &nbsp; |[Teradata](../articles/data-factory/data-factory-onprem-teradata-connector.md)* |✓ | |
| **NoSQL** |[Cassandra](../articles/data-factory/data-factory-onprem-cassandra-connector.md)* |✓ | |
| &nbsp; |[MongoDB](../articles/data-factory/data-factory-on-premises-mongodb-connector.md)* |✓ | |
| **File** |[Amazon S3](../articles/data-factory/data-factory-amazon-simple-storage-service-connector.md) |✓ | |
| &nbsp; |[Systém souborů](../articles/data-factory/data-factory-onprem-file-system-connector.md)* |✓ |✓ |
| &nbsp; |[FTP](../articles/data-factory/data-factory-ftp-connector.md) |✓ | |
| &nbsp; |[HDFS](../articles/data-factory/data-factory-hdfs-connector.md)* |✓ | |
| &nbsp; |[SFTP](../articles/data-factory/data-factory-sftp-connector.md) |✓ | |
| **Ostatní** |[Obecná rozhraní HTTP](../articles/data-factory/data-factory-http-connector.md) |✓ | |
| &nbsp; |[Obecná OData](../articles/data-factory/data-factory-odata-connector.md) |✓ | |
| &nbsp; |[Obecná rozhraní ODBC](../articles/data-factory/data-factory-odbc-connector.md)* |✓ | |
| &nbsp; |[Salesforce](../articles/data-factory/data-factory-salesforce-connector.md) |✓ | |
| &nbsp; |[Webová tabulka (tabulka z HTML)](../articles/data-factory/data-factory-web-table-connector.md) |✓ | |
| &nbsp; |[GE Historian](../articles/data-factory/data-factory-odbc-connector.md#ge-historian-store)* |✓ | | |

> [!NOTE]
> Úložiště dat s * mohou být místní nebo v Azure IaaS a vyžadují nainstalování [Brány správy dat](../articles/data-factory/data-factory-data-management-gateway.md) na místním počítači nebo na počítači Azure IaaS.
>
>
