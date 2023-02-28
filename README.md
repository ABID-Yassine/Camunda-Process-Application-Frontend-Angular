
## CAMUNDA - SPRING BOOT - ANGULAR

![Orchestration](https://insatunisia.github.io/TP-eServices/img/orchestration.png)

### Lab objectives

Creation of a business process (Business Process) using Camunda.

### Tools and Versions

-   [Camunda](https://camunda.org/download/) Version: 7.7.0
-   [Java](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html)  Version 1.8.0_121 (7+ needed).
-   [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Version Ultimate 2016.1 (or any other IDE of your choice)
-   [Camunda Modeler](https://camunda.org/download/modeler/) Version 1.10.0

### Camunda[![Camunda website](https://insatunisia.github.io/TP-eServices/img/website.png)](https://camunda.org/)

Camunda is an open source platform for business process management. It is a Java framework that supports **BPMN** for process automation, **CMMN** for Case Management, and **DMN** for Business Decision Management.

### BPMN 2.0  [![BPMN Website](https://insatunisia.github.io/TP-eServices/img/website.png)](http://www.bpmn.org/)

BPMN 2.0 (Business Process Modeling Notation) is a standard developed by the Object Management Group ( **OMG** ) to provide a notation that is easily understood by all business users: business analysts, developers implementing the technologies executing these processes, and people managing and overseeing these processes. BPMN makes it possible to establish a bridge minimizing the gap between the designs of the processes and their implementations.

In its first version, the BPMN specification provided only graphical notation, and quickly became famous among business analysts. It defined how concepts such as human tasks and executable scripts could be visualized in a standard way, independent of a particular manufacturer.

This second version extends this standard by including execution semantics and a common exchange format. This means that BPMN 2.0 process models can be exchanged between different graphical editors, and run on any engine compatible with BPMN 2.0, such as Camunda and Activiti.

### Installation

To install the environment necessary for this lab, follow these steps:

-   Télécharger [Camunda](https://camunda.org/download/) (Tomcat Distribution), [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) and [Camunda Modeler](https://camunda.org/download/modeler/) .
-   Unzip the downloaded Camunda .zip file, and run _start-camunda.sh_ (for Unix-based systems) or _start-camunda.bat_ (for Windows systems).
-   Open the application server home page in your favorite browser.
-   Launch the Camunda Modeler.

### First Camunda BPMN Project: Helloworld

#### Project Creation and Dependencies

You will now create a new Java project to define the behavior of your process.

-   Open IntelliJ and create a new Maven project (without archetype).
-   You can choose the following settings:
    
    -   Group Id:  _tn.insat.eservices.tp2_
    -   Artifact Id:  _Helloworld_
    -   Project Name:  _HelloworldCamunda_
-   In the _pom.xml_ file , indicate that the application will be deployed later as a _war_ file . To do this, add the following line, just after the version:
    

`<packaging>war</packaging>` 

-   Add the necessary dependencies to Camunda in your project. To do this, insert the following lines in your _pom.xml file_

`<dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.camunda.bpm</groupId>
        <artifactId>camunda-bom</artifactId>
        <version>7.7.0</version>
        <scope>import</scope>
        <type>pom</type>
      </dependency>
    </dependencies>
  </dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.camunda.bpm</groupId>
      <artifactId>camunda-engine</artifactId>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.0.1</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>2.3</version>
        <configuration>
          <failOnMissingWebXml>false</failOnMissingWebXml>
        </configuration>
      </plugin>
    </plugins>
  </build>` 

-   Make a build of your project. To do this, create a new Maven-like configuration, which you will call _maven-install_ for example, and you will write in the **Command Line** part : _install_ , as follows:

![Maven Install](https://insatunisia.github.io/TP-eServices/img/tp2/maven-install.png)

-   Launch the build and check that your packages have been installed.

#### Creating the main class for the process

The next step is to build a class for the process. This class represents the interface between your application and the Camunda process engine.

`package tn.insat.eservices.tp2.helloworld;
import org.camunda.bpm.application.ProcessApplication;
import org.camunda.bpm.application.impl.ServletProcessApplication;
@ProcessApplication("Helloworld App")
public class HelloworldApplication extends ServletProcessApplication {
    // empty implementation
}` 

_Then add the processes.xml_ file under the _src/main/resources/META-INF_ directory . This file allows us to provide a configuration for the deployment of this process in the process engine.

`<?xml version="1.0" encoding="UTF-8" ?>
<process-application
        xmlns="http://www.camunda.org/schema/1.0/ProcessApplication"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <process-archive name="helloworld">
        <process-engine>default</process-engine>
        <properties>
            <property name="isDeleteUponUndeploy">false</property>
            <property name="isScanForProcessDefinitions">true</property>
        </properties>
    </process-archive>
</process-application>` 

From this point, we will start modeling the process.

#### Modeling a BPMN 2.0 process

The modeling of the process will be done using the _Camunda Modeler_ . For that:

-   Start the application, and create a new BPMN diagram by clicking on _File > New File > BPMN Diagram_ .

![First Camunda Project](https://insatunisia.github.io/TP-eServices/img/tp2/camunda-first-proj.png)

-   Double-click on the starting event (the small circle) to modify its name. We'll call it _Say Hello_ .
-   Click on the starting event, choose the rectangle (representing an activity) from the pop-up menu that appears, and drag it to a suitable location. We'll call the new inserted event _Add Hello_ .

![](https://insatunisia.github.io/TP-eServices/img/tp2/menu-contextuel.png)

-   This task will be the one where the user will indicate his name for the eternal **Hello <name>!** . For this, we must indicate that the processing done here will be carried out by a human. To do this, by clicking on the created activity, click in the popup menu on the adjustable wrench, then choose _User Task_ .

![](https://insatunisia.github.io/TP-eServices/img/tp2/user-task.png)

-   Add an end task to the process.

We finally obtain the following diagram:

![](https://insatunisia.github.io/TP-eServices/img/tp2/hw-process.png)

#### Process Setup

-   To configure the _Add Bonjour_ user task , click on it, and fill in the properties panel (on the right). Define the user for whom this activity will be intended. To do this, in the _Assignee_ field , insert _john_ . John is a predefined user on the Camunda server. Later, you can define your own list of users and roles.
-   To configure the entire process, click on an empty spot on the canvas. In the properties panel, specify the following fields:
    -   Id:  _helloworld_
    -   Name:  _Helloworld_
    -   Executable:  _true_

You will get the following result:

![First Process Completed](https://insatunisia.github.io/TP-eServices/img/tp2/process-defined.png)

-   Save the diagram under the _src/main/resources_ directory of the project you created. You will call it _helloworld.bpmn_

#### Process deployment

-   Configure the application to be deployed on the integrated tomcat server in your Camunda installation. For this, in IntelliJ:
    
    -   Go to menu _File > Project Structure..._
    -   Click on _Artifacts_ .
    -   Define the type of archive to deploy: _Web Application: Archive_
    -   Set the Output Directory to the _webapps_ directory , usually located under the _$CAMUNDA_HOME/server/apache-tomcat-<version>/webapps_ directory .
    -   At the bottom of the window, you will find a _Create Manifest_ button . Clicking on it will create the Manifest file responsible for the deployment.
    -   You will get the following result:

![Deployment configuration](https://insatunisia.github.io/TP-eServices/img/tp2/deploy.png)

-   Make a _make_ of the project. To do this, go to the menu _Build > Make Project_ or click on ![make](https://insatunisia.github.io/TP-eServices/img/tp2/make.png). Normally, a new _helloworld-1.0-SNAPSHOT.war_ file will be created in the _webapps_ directory of the server.

To verify that the process has been successfully deployed on the Tomcat server, consult the log file located under _$CAMUNDA_HOME/server/apache-tomcat-<version>/logs_ and open the _catalina.out_ file .

Tip

The best way to constantly check the log file on Linux-like systems is to open a terminal, and type `tail -f catalina.out`.

The file should contain the following lines:

![BPMN deployed!](https://insatunisia.github.io/TP-eServices/img/tp2/bpm-deployed.png)

#### Checking the deployment with Cockpit

_Camunda offers the Cockpit_ tool to inspect running and completed processes, and manage various incidents. To do this, if your Camunda server is well launched, you will be able to view your processes in the browser, by typing: `http://localhost:8080/camunda/app/cockpit`. Identify yourself as an administrator by typing the credentials: `demo/demo`. Click on the number under _Process Definitions_ (it should be **2** in your case), you should find your process, with a _checked_ state .

![Cockpit](https://insatunisia.github.io/TP-eServices/img/tp2/cockpit.png)

#### Starting the process

-   Go to the _Camunda Tasklist_ ( `http://localhost:8080/camunda/app/tasklist`), then start the process by clicking on the _Start Process_ button (top right).
-   Click on your _Helloworld_ process .
-   Add as many variables as necessary in the generic form. In our case, we are going to add a _name_ variable of character string type. To do this, click on _Add a variable_ and fill in as follows (put your name of course, not mine ! 

-   Refreshing the Cockpit now, you will find that the process has moved to the _Running_ state .

#### Configuring permissions

To allow the user John to view and launch the _Helloworld_ process , it will be necessary to add the authorizations to him. For that:

-   Go to _Camunda Admin_ ( `http://localhost:8080/camunda/app/admin/default/#/authorization?resource=0`).
-   Add a new permission in the _Process Definition_ part , to allow John to manipulate the _Helloworld_ process definition .

![Add Permission - Process Definition](https://insatunisia.github.io/TP-eServices/img/tp2/def-perm.png)

-   _In the Process Instance_ part , add the permission to create a process instance to John.

![Add Permission - Process Instance](https://insatunisia.github.io/TP-eServices/img/tp2/inst-perm.png)

-   Authenticate as John, using ( _john/john_ ), preferably on another browser. This will allow you to view the Helloworld process as seen by John. He can thus add the variables of his choice, and complete the process.

![Run the process](https://insatunisia.github.io/TP-eServices/img/tp2/john-run-process.png)

#### Creating a custom form

To create your own form, with input variables that can be manipulated by the service, follow these steps:

-   _Go back to IntelliJ, and create a dis-bonjour.html_ file under the _src/main/webapp/forms_ directory . Add the following content:

`<form name="disBonjour">
    <div class="form-group">
        <label for="nom">Nom</label>
        <input class="form-control"
               cam-variable-type="String"
               cam-variable-name="nom"
               name="nom" />
    </div>
</form>` 

-   Open the process with the Modeler, and click on the start event. In the properties panel, choose the _Forms_ tab and insert `embedded:app:forms/dis-bonjour.html`in the _Key_ field . This indicates that we want to use an embedded form in the Tasklist, and that it will be loaded from the application.
-   Save, and refresh the project in IntelliJ.
-   Similarly, we are going to create the form that will allow John to say Hello. We'll call it _hello.html_ .

`<form name="bonjour">
    <div class="form-group">
        <label for="salutation">Salutation</label>
        <input class="form-control"
               cam-variable-type="String"
               cam-variable-name="salutation"
               name="salutation" />
    </div>
    <div class="form-group">
        <label for="nom">Nom</label>
        <input class="form-control"
               cam-variable-type="String"
               cam-variable-name="nom"
               name="nom"
               readonly="true" />
    </div>
</form>` 

-   _Assign this form to the Add Bonjour_ task in the same way as before.
-   Save everything and re-deploy the project.
-   Now start the process. Enter your name in the _Name_ field .

![Run Process2]

-   Identify yourself as John again, you will find the second form:

![Add Hello!](https://insatunisia.github.io/TP-eServices/img/tp2/add-hello-2.png)

For now, when clicking on complete, nothing happens, because we have not indicated anywhere what must be done following the entry of "Hello" by John. This will be done through a _Service Task_ .

#### Adding a Java Service Task

To define the behavior of your service, follow these steps:

-   Use the Modeler to add a service task just after the user task. To do this, select an activity in the left palette, and drag it between the user task and the end event. With the adjustable wrench , select the _Service Task_![spanner](https://insatunisia.github.io/TP-eServices/img/tp2/clef-molette.png) option . Call the _Say Hello_ service . You will get the following result:

![New Process with Service Task](https://insatunisia.github.io/TP-eServices/img/tp2/service-task.png)

-   Now add the Service Task implementation. For this, add a class in the IntelliJ project called _ProcessRequestDelegate_ which implements the _JavaDelegate_ interface , as follows:

`package tn.insat.eservices.tp2.helloworld;
import java.util.logging.Logger;
import org.camunda.bpm.engine.delegate.DelegateExecution;
import org.camunda.bpm.engine.delegate.JavaDelegate;
public class ProcessRequestDelegate implements JavaDelegate {
    private final static Logger LOGGER = Logger.getLogger("Hello-Greetings");
    public void execute(DelegateExecution execution) throws Exception {
        LOGGER.info("Hey! " + execution.getVariable("salutation")
                    + " " + execution.getVariable("nom") + "!");
    }
}` 

-   Use the properties panel to reference the class in the process. To do this, click on the Service Task, and define its implementation by the Java Class: `tn.insat.eservices.tp2.helloworld.ProcessRequestDelegate`.
-   Deploy your application, and observe the result. It will be displayed in your Tomcat server log (catalina.out), as follows:

![Hello Lilia!](https://insatunisia.github.io/TP-eServices/img/tp2/bonjour.png)

### Calling a REST Web Service

Thanks to connectors, Camunda can integrate REST or SOAP web services. For this, we are going to use a classic weather web service. In his form, John will enter the name of a city, and the process should return the current temperature in that city, in addition to the usual Hello.

-   _Start by adjusting the hello.html_ form , adding another text field: _city_ after the _name_ field .
-   Return to the Modeler, and add a Service Task, which we will call _Consult Weather_ , between _Add Hello_ and _Say Hello_ .
-   In this service, indicate that the implementation type is _Connector_ , and move to the _Connector_ tab to configure it.
-   Give the following parameters to your connector;
    
    -   **Id**:  _http-connector_
    -   **Input** : The inputs will take all the information needed to send the REST request to the _OpenWeatherMap_ web service . This service takes the city as a parameter, which will be inserted in our case from the previous form, in the _city_ field .
    
    Name
    
    Type
    
    Value
    
    url
    
    Script / JavaScript / Inline Script
    
    `var ville=execution.getVariable("ville"); 'http://api.openweathermap.org/data/2.5/weather?APPID=17db59488cadcad345211c36304a9266&q='+ville;`
    
    method
    
    Text
    
    GET
    
    headers
    
    Map
    
    `key: accept, value:application/json - key:content-type, value:application/json`
    
    -   **Output** : The service used returns a json document that looks like the following:

`{
  coord: {
    lon: 10.17,
    lat: 36.82
  },
  weather: [
    {
      id: 801,
      main: "Clouds",
      description: "few clouds",
      icon: "02d"
    }
  ],
  base: "stations",
  main: {
    temp: 299.87,
    pressure: 1018,
    humidity: 39,
    temp_min: 299.15,
    temp_max: 301.15
  },
  visibility: 10000,
  wind: {
    speed: 3.6,
    deg: 40
  },
  clouds: {
    all: 20
  },
  dt: 1506864600,
  sys: {
    type: 1,
    id: 6318,
    message: 0.0039,
    country: "TN",
    sunrise: 1506834907,
    sunset: 1506877308
  },
  id: 2464470,
  name: "Tunis",
  cod: 200
}` 

If the goal is to return the temperature value, one must navigate to the _main_ element and then to its _temp_ child . The output of our service will therefore have the following form:

Name

Type

Value

WsResponse

Script / JavaScript / Inline Script

`S(response).prop("main").prop("temp").numberValue();`

Tip

Always test your REST web service on browser before using it in any application!

-   Now, add the exploit code for this service in the _ProcessRequestDelegate_ class , to tell it to display the request result:

`public class ProcessRequestDelegate implements JavaDelegate {
    private final static Logger LOGGER = Logger.getLogger("Hello-Greetings");
    public void execute(DelegateExecution execution) throws Exception {
        LOGGER.info("Hey! " + execution.getVariable("salutation") + " "
                + execution.getVariable("nom")
                + "! La température aujourd'hui à "
                + execution.getVariable("ville")
                + " est de "
                + execution.getVariable("WsResponse")+"!");
    }
}` 

-   Save everything then deploy the service. Running it you get the following output:
    
    -   The demo user enters his name:![Name entry](https://insatunisia.github.io/TP-eServices/img/tp2/start-process-2.png)
        
    -   User john adds the salutation and the city:![Added name and city](https://insatunisia.github.io/TP-eServices/img/tp2/say-hello-3.png)
        
    -   The process displays this result on the log:![Result](https://insatunisia.github.io/TP-eServices/img/tp2/resultat-3.png)
