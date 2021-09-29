# Recipe-Webservice
ABN Amro Assessment for Recipe Web Service

## Prerequisites
* [JDK 1.8](https://www.oracle.com/in/java/technologies/javase/javase-jdk8-downloads.html)
* [Apache Maven](https://maven.apache.org/)
* [MySQL](https://www.mysql.com/)
* [Git](https://git-scm.com/)

## Steps to build Web Service
* Download code zip / `git clone https://github.com/diptichavan/Recipe-Webservice.git
* Move to `ABN-Amro-Assessment` and run maven build command `mvn clean package`
* To build by skipping unit tests run maven command `mvn clean package -DskipTests`
* On successfull build completion, one should have web service jar in `target` directory named as `Recipes-Service-1.0.jar`

## Steps to execute Web Service
* **Execution on Development profile with Embedded H2 Database**
  - In Development Mode, by default web service uses [Embedded H2 database](https://spring.io/guides/gs/accessing-data-jpa/) for persisting and retrieving recipes details.
  - Command to execute: 
   ```
        java -jar target/Recipes-Service-1.0.jar --spring.profiles.active=dev --logging.level.root=INFO
   ```
  - On successfull start, one should notice log message on console `Tomcat started on port(s): 9000 (http)` and have web service listening for web requests at port 9000
* **Execution on Development profile with MySQL Database**
  - In Development mode, one can also execute web service against local [MySQL Service](https://www.mysql.com/) for persisting and retrieving recipes details.
  - Specify required [MySQL Service](https://spring.io/guides/gs/accessing-data-mysql/) configuraiton parameters in `application.properties` file as given below:
    -  server.port=9000
    -  spring.profiles.active=dev
    -  spring.jpa.hibernate.ddl-auto=update
    -  spring.datasource.url=jdbc:mysql://${MYSQL_HOST:localhost}:3306/db_name
    -  spring.datasource.username=mysql-username
    -  spring.datasource.password=mysql-user-password
    -  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  - Web service needs database table with name `RECIPES` to be present in configured MySQL Database. Use below given table schema to create one before execution
    - CREATE TABLE recipes(id INT PRIMARY KEY, name VARCHAR(50), type VARCHAR(4),cdatetime TIMESTAMP, capacity INT, ingredients TEXT, instructions TEXT);  
  - Make sure MySQL Service is running on locallhost and listening at default port 3306
  - Command to execute with `mysql` profile: 
  ```
  java -jar target/Recipes-Service-1.0.jar --spring.config.location=src/main/resources/application-mysql.properties --logging.level.root=INFO
  ```
  - On successfull start, one should notice log message on console `Tomcat started on port(s): 9000 (http)` and have web service listening for web requests at port 9000
* **Execution on Production Profile with MySQL Database**
  - In order to execute on Production, set the required configuration parameters in application.properties file
    -  server.port=required-port-number
    -  spring.profiles.active=prod
    -  spring.jpa.hibernate.ddl-auto=update
    -  spring.datasource.url=jdbc:mysql://${MYSQL_HOST:hostName}:port-Number/db_name
    -  spring.datasource.username=mysql-username
    -  spring.datasource.password=mysql-user-password
    -  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  - Web service needs database table with name `RECIPES` to be present in configured MySQL Database. Use below given table schema to create one before execution
    - CREATE TABLE recipes(id INT PRIMARY KEY, name VARCHAR(50), type VARCHAR(4),cdatetime TIMESTAMP, capacity INT, ingredients TEXT, instructions TEXT);    
  - Command to execute with custom application.properties file: 
  ```
  java -jar target/Recipes-Service-1.0.jar --spring.config.location=application.properties
  ```
  - On successfull start, one should have web service listening for web requests at specified port in `application.properties` file interacting with configured production grade MySQL Service for persistence and retrieval of recipes details. 
