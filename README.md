https://hub.docker.com/_/microsoft-mssql-server

### Requirements
1. This image requires Docker Engine 1.8+ in any of their supported platforms.
2. At least 2GB of RAM (3.25 GB prior to 2017-CU2). Make sure to assign enough memory to the Docker VM if you're running on Docker for Mac or Windows.
3. Requires the following environment flags
"ACCEPT_EULA=Y"
"SA_PASSWORD=<your_strong_password>"
"MSSQL_PID=<your_product_id | edition_name> (default: Developer)"
A strong system administrator (SA) password: At least 8 characters including uppercase, lowercase letters, base-10 digits and/or non-alphanumeric symbols.

#### Environment Variables
You can use environment variables to configure SQL Server on Linux Containers.
ACCEPT_EULA confirms your acceptance of the End-User Licensing Agreement.
SA_PASSWORD is the database system administrator (userid = 'sa') password used to connect to SQL Server once the container is running. Important note: This password needs to include at least 8 characters of at least three of these four categories: uppercase letters, lowercase letters, numbers and non-alphanumeric symbols.
MSSQL_PID is the Product ID (PID) or Edition that the container will run with. Acceptable values:
Developer : This will run the container using the Developer Edition (this is the default if no MSSQL_PID environment variable is supplied)
Express : This will run the container using the Express Edition
Standard : This will run the container using the Standard Edition
Enterprise : This will run the container using the Enterprise Edition
EnterpriseCore : This will run the container using the Enterprise Edition Core
For a complete list of environment variables that can be used, refer to the documentation here.

### application.properties
https://docs.microsoft.com/ru-ru/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java?source=recommendations&view=sql-server-ver16


### Рабочий application.properties

Примечание: 
1) Базу данных необходимо создавать вручную через консоль CREATE DATABASE library, так как команда "CREATE DATABASE" 
не поддерживается FlyWay 
2) FlyWay создает таблицы в library\dbo\tables 
3) Если необходимо повторно выполнить скрипт FlyWay, то необходимо удалить строку в именем этого скрипта 
в таблице library\dbo\tables\flyway_schema_history 
4) Без параметров "encrypt=true;trustServerCertificate=true;" в spring.datasource.url возникает 
ошибка "com.microsoft.sqlserver.jdbc.SQLServerException: драйвер не смог установить безопасное соединение с SQL Server 
с использованием шифрования Secure Sockets Layer (SSL)" см. https://bit.ly/3LbwUtR

Пример рабочего application.properties:

spring.datasource.url=jdbc:sqlserver://localhost:1433;database=library;encrypt=true;trustServerCertificate=true;
spring.datasource.username=sa
spring.datasource.password=Sa123456
spring.datasource.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriver
spring.jpa.show-sql=true
spring.jpa.hibernate.dialect=org.hibernate.dialect.SQLServer2012Dialect
spring.jpa.hibernate.ddl-auto=none
spring.flyway.enabled=true