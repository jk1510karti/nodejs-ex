managed-executor-service: Managed Executor Service example
================================================================
Author: Rafael Benevides  
Level: Beginner  
Technologies: EE Concurrency Utilities, JAX-RS, JAX-RS Client API  
Summary: The `managed-executor-service` quickstart demonstrates how Java EE applications can submit tasks for asynchronous execution.  
Target Product: EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts>  


What is it?
-----------

The Managed Executor Service (javax.enterprise.concurrent.ManagedExecutorService) allows Java EE applications to submit tasks for asynchronous execution. It is an extension of Java SE's Executor Service (java.util.concurrent.ExecutorService) adapted to the Java EE platform requirements.

Managed Executor Service instances are managed by the application server, thus Java EE applications are forbidden to invoke any lifecycle related method.

A JAX-RS resource provides access to some operations that are executed asynchronously. 

_Note: This quickstart uses the H2 database included with Red Hat JBoss Enterprise Application Platform 7. It is a lightweight, relational example datasource that is used for examples only. It is not robust or scalable, is not supported, and should NOT be used in a production environment!_

_Note: This quickstart uses a `*-ds.xml` datasource configuration file for convenience and ease of database configuration. These files are deprecated in JBoss EAP and should not be used in a production environment. Instead, you should configure the datasource using the Management CLI or Management Console. Datasource configuration is documented in the [Configuration Guide](https://access.redhat.com/documentation/en/jboss-enterprise-application-platform/) for Red Hat JBoss Enterprise Application Platform._

System requirements
-------------------

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7 or later. 

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.1.1 or later. See [Configure Maven for JBoss EAP 7](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


Start the JBoss EAP Server
-------------------------

1. Open a command line and navigate to the root of the  JBoss EAP directory.
2. The following shows the command line to start the server with the default profile:

        For Linux:   EAP7_HOME/bin/standalone.sh
        For Windows: EAP7_HOME\bin\standalone.bat


Build and Deploy the Quickstart
-------------------------

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command line and navigate to the root directory of this quickstart.
3. Type this command to build and deploy the archive:

        mvn clean package wildfly:deploy
4. This will deploy `target/jboss-managed-executor-service.war` to the running instance of the server.
 


Run the Tests
-------------------------


This quickstart provides tests that shows how the asynchronous tasks are executed. By default, these tests are configured to be skipped as the tests requires that the application to be deployed first. 


1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. Type the following command to run the test goal with the following profile activated:

        mvn clean test -Prest-test

Run tests from JBDS
-----------------------

To be able to run the tests from JBDS, first set the active Maven profile in project properties to be either 'rest-client'.

To run the tests, right click on the project or individual classes and select Run As --> JUnit Test in the context menu.


Investigate the Console Output
------------------------------


    -------------------------------------------------------
     T E S T S
    -------------------------------------------------------
    Running org.jboss.as.quickstarts.managedexecutorservice.test.ProductsRestClientTest
    [timestamp] org.jboss.as.quickstarts.managedexecutorservice.test.ProductsRestClientTest testRestResources
    INFO: creating a new product
    [timestamp] org.jboss.as.quickstarts.managedexecutorservice.test.ProductsRestClientTest testRestResources
    INFO: Product created. Executing a long running task
    [timestamp] org.jboss.as.quickstarts.managedexecutorservice.test.ProductsRestClientTest testRestResources
    INFO: Deleting all products
    Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 4.202 sec - in org.jboss.as.quickstarts.managedexecutorservice.test.ProductsRestClientTest
    
    Results :
    
    Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    
Investigate the Server Console Output
-------------------------------------
Look at the JBoss EAP console or Server log and you should see log messages like the following:

    13:34:07,940 INFO  [ProductResourceRESTService] (default task-51) Will create a new Product on other Thread
    13:34:07,940 INFO  [ProductResourceRESTService] (default task-51) Returning response
    13:34:07,941 INFO  [PersitTask] (EE-ManagedExecutorService-default-Thread-5) Begin transaction
    13:34:07,941 INFO  [PersitTask] (EE-ManagedExecutorService-default-Thread-5) Persisting a new product
    13:34:07,946 INFO  [PersitTask] (EE-ManagedExecutorService-default-Thread-5) Commit transaction
    13:34:08,002 INFO  [ProductResourceRESTService] (default task-52) Submitting a new long running task to be executed
    13:34:08,003 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:08,009 INFO  [LongRunningTask] (EE-ManagedExecutorService-default-Thread-5) Starting a long running task
    13:34:08,010 INFO  [LongRunningTask] (EE-ManagedExecutorService-default-Thread-5) Analysing A Product
    13:34:08,306 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:08,608 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:08,912 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:09,215 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:09,519 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:09,823 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:10,128 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:10,431 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:10,735 INFO  [ProductResourceRESTService] (default task-52) Waiting for the result to be available...
    13:34:11,040 INFO  [ProductResourceRESTService] (default task-52) Result is available. Returning result...56
    13:34:11,082 INFO  [ProductResourceRESTService] (default task-53) Will delete all Products on other Thread
    13:34:11,082 INFO  [ProductResourceRESTService] (default task-53) Returning response
    13:34:11,082 INFO  [DeleteTask] (EE-ManagedExecutorService-default-Thread-5) Begin transaction
    13:34:11,083 INFO  [DeleteTask] (EE-ManagedExecutorService-default-Thread-5) Deleting all products
    13:34:11,092 INFO  [DeleteTask] (EE-ManagedExecutorService-default-Thread-5) Commit transaction. Products deleted: 1

Note that the PersistTask and DeleteTask were executed after ProductResourceRESTService sends a Response. The only exception is for LongRunningTask where ProductResourceRESTService waits for its response.
    

Server Log: Expected warnings and errors
-----------------------------------

_Note:_ You will see the following warnings in the server log. You can ignore these warnings.

    WFLYJCA0091: -ds.xml file deployments are deprecated. Support may be removed in a future version.

    HHH000431: Unable to determine H2 database version, certain features may not work


Undeploy the Archive
--------------------

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. When you are finished testing, type this command to undeploy the archive:

        mvn wildfly:undeploy


Run the Quickstart in Red Hat JBoss Developer Studio or Eclipse
-------------------------------------


You can also start the server and deploy the quickstarts or run the Arquillian tests from Eclipse using JBoss tools. For more information, see [Use JBoss Developer Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JBDS.md#use-jboss-developer-studio-or-eclipse-to-run-the-quickstarts) 

Debug the Application
------------------------------------

If you want to debug the source code of any library in the project, run the following command to pull the source into your local repository. The IDE should then detect it.

    mvn dependency:sources
   

