Copiar actividad en Data Factory realiza una copia de los datos de un almacén de datos de origen a uno receptor. Data Factory admite los siguientes almacenes de datos. Se pueden escribir datos desde cualquier origen en todos los tipos de receptores. Haga clic en un almacén de datos para obtener información sobre cómo copiar datos a un almacén como origen o destino.

| Categoría | Almacén de datos | Se admite como origen | Se admite como receptor |
|:--- |:--- |:--- |:--- |
| Las tablas de Azure |[Almacenamiento de blobs de Azure](../articles/data-factory/data-factory-azure-blob-connector.md) <br/> [Azure Data Lake Store](../articles/data-factory/data-factory-azure-datalake-connector.md) <br/> [Azure SQL Database](../articles/data-factory/data-factory-azure-sql-connector.md) <br/> [Azure SQL Data Warehouse](../articles/data-factory/data-factory-azure-sql-data-warehouse-connector.md) <br/> [Azure Table Storage](../articles/data-factory/data-factory-azure-table-connector.md) <br/> [Azure DocumentDB](../articles/data-factory/data-factory-azure-documentdb-connector.md) <br/> |✓ <br/> ✓  <br/> ✓  <br/> ✓  <br/> ✓  <br/> ✓  |✓  <br/> ✓  <br/> ✓  <br/> ✓  <br/> ✓  <br/> ✓  |
| Bases de datos |[SQL Server](../articles/data-factory/data-factory-sqlserver-connector.md)\* <br/> [Oracle](../articles/data-factory/data-factory-onprem-oracle-connector.md)\* <br/> [MySQL](../articles/data-factory/data-factory-onprem-mysql-connector.md)\* <br/> [DB2](../articles/data-factory/data-factory-onprem-db2-connector.md)\* <br/> [Teradata](../articles/data-factory/data-factory-onprem-teradata-connector.md)\* <br/> [PostgreSQL](../articles/data-factory/data-factory-onprem-postgresql-connector.md)\* <br/> [Sybase](../articles/data-factory/data-factory-onprem-sybase-connector.md)\* <br/>[Cassandra](../articles/data-factory/data-factory-onprem-cassandra-connector.md)\* <br/>[MongoDB](../articles/data-factory/data-factory-on-premises-mongodb-connector.md)\*<br/>[Amazon Redshift](../articles/data-factory/data-factory-amazon-redshift-connector.md) |✓  <br/> ✓  <br/> ✓  <br/> ✓  <br/> ✓  <br/> ✓ <br/> ✓  <br/> ✓  <br/> ✓  <br/> ✓  |✓  <br/> ✓  <br/> &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;<br/> &nbsp; <br/>&nbsp; |
| Archivo |[Sistema de archivos](../articles/data-factory/data-factory-onprem-file-system-connector.md)\* <br/> [HDFS](../articles/data-factory/data-factory-hdfs-connector.md)\* <br/> [Amazon S3](../articles/data-factory/data-factory-amazon-simple-storage-service-connector.md) <br/> [FTP](../articles/data-factory/data-factory-ftp-connector.md) |✓ <br/> ✓  <br/> ✓  <br/> ✓  |✓  <br/> &nbsp;<br/>&nbsp; |
| Otros |[Salesforce](../articles/data-factory/data-factory-salesforce-connector.md)<br/> [ODBC genérico](../articles/data-factory/data-factory-odbc-connector.md)\* <br/> [OData genérico](../articles/data-factory/data-factory-odata-connector.md) <br/> [Tabla web (tabla de HTML)](../articles/data-factory/data-factory-web-table-connector.md) <br/> [Analista de GE](../articles/data-factory/data-factory-odbc-connector.md#ge-historian-store)* |✓ <br/> ✓  <br/> ✓  <br/> ✓  <br/> ✓  |&nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp; |

> [!NOTE]
> Los almacenes de datos con * pueden ser locales o estar en la IaaS de Azure; además, requieren que instale [Data Management Gateway](../articles/data-factory/data-factory-data-management-gateway.md) en una máquina local o de la IaaS de Azure.
> 
> 



<!--HONumber=Nov16_HO2-->


