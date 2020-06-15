* Install Java 8  
  * Install old version of Java using https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html
  * Note for Catalina MacOs https://www.oracle.com/technetwork/java/javase/using-jdk-jre-macos-catalina-5781620.html  
  or  
  * Install using brew cask `brew cask install adoptopenjdk/openjdk/adoptopenjdk8`
* Install IntelliJ IDEA
* Install Gradle 5.4.1 using sdk  
`sdk install gradle 5.4.1`
* Follow the tutorial here: https://docs.corda.net/docs/corda-os/4.4/tutorial-cordapp.html
  * In case `workflows-kotlin/build/nodes/runnodes` does not start terminal windows, try navigating into one of the nodes' folders, and starting the node directly using  `java -jar corda.jar`
  * Only when 4 terminal nodes are up, `./gradlew runPartyXServer` will work
  * Once `Started ServerKt in xx.xxx seconds` displays, the Spring Boot server is ready
* Access directly H2 database file
  * Download H2 Console https://docs.corda.net/docs/corda-os/4.4/node-database-access-h2.html#connecting-to-the-database
  * Check `build/nodes/.cache/apps/net.corda.node.Corda_4.4/reference.conf` file for data source URL, username and password. It looks like this: `jdbc:h2:file:"${baseDirectory}"/persistence`
  * Remember to close node connection to access the file
* Access H2 database while node is running
  * Config `h2Port` in `build.gradle`
  * Database connection URL will appear in the node portal
  * Change H2 setting to Generic H2 (Server)
```
build.gradle
task deployNodes(type: net.corda.plugins.Cordform, dependsOn: ['jar']) {
    node {
        name "O=Notary,L=London,C=GB"
        h2Port 10020
```
```
Database connection url is              : jdbc:h2:tcp://localhost:10021/node
```
* To connect with PostgreSQL, download Java postgresql driver https://jdbc.postgresql.org/download.html and add path to `jarDirs`
```
build.gradle
extraConfig = [
    'dataSourceProperties.dataSourceClassName':'org.postgresql.ds.PGSimpleDataSource',
    'dataSourceProperties.dataSource.url':'jdbc:postgresql://localhost:5432/propine_development?currentSchema=corda_alice',
    'dataSourceProperties.dataSource.password':'corda_alice',
    'dataSourceProperties.dataSource.user':'corda_alice',
    'database.transactionIsolationLevel':'READ_COMMITTED',
    'database.initialiseSchema':true,
    'jarDirs' : [ '/Library/Java/postgres' ]
]
```
* If you want to add dependencies, you need to add to `build.gradle` respectively. E.g. if you want to use lib in workflow, you need to add to the `build.gradle` file under workflow directory