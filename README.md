<hr>
<h1 align="center">Azure Workflows (Business Processes Modeled in Software)</h1>
<hr>

Azure offers **MALW**are to implement *Workflows*

- _**M**_ icrosoft Power Automate  
- _**A**_ zure Functions
- _**L**_ ogic Apps 
- _**W**_ ebJobs

All **MALW**are can say **CIAO**:

+ _**C**_ ondition: Tests on input which may cause the exection of actions
+ _**I**_ nput: Data provided to a workflow
+  _**A**_ ction: An operation which may modify data, and cause another action to be performed
+ _**O**_ utput: Data produced by the workflow

<hr></hr>
<h3 align="center">Design first technologies are pretty <b>L</b>a<b>M</b>e, Who wants to draw a flow diagram?</h3>

- ### Logic Apps: 
  - Visual and code based integration of distributed applications
  - Intended for users with some technical knowledge
  - Implements 200+ ***Connectors*** which allow the logic app to integrate with many pre-existing technologies
  - Can create custom connectors for REST apis
  
- ### Microsoft Power Automate: 
  - Intended for users with little to no technical knoweldge. They need to be 'automated'.
  - Power automate apps can only created through Microsofts GUI
  - Azure provides testing and production enviroments
  - It's has 4 types of flows created for **BAS**e**B**all fans
    - _**B**_ utton: A button triggers a task.
    - _**A**_ utomated: a flow that is started by some *event*
    - _**S**_ cheduled: A flow that is triggered by a set time
    - _**B**_ usiness process: A flow that models a business procedure. e.g 'a customer complaint system'
  
<hr></hr>
<h3 align="center">Code first technologies are <b>AW</b>esome</h3>

- ### Azure Functions: 
  - Runs simple codes on event triggers
  - Support for a very large amount of programming languages
  - Has many built in integrations simmilar to logic app connectors.

- ### WebJobs: 
  - Apart of the ***Azure App Service*** platform, they run in the *cloud<sup>TM</sup>*
  - Typicaly written in languages built for scripting
  - Webjobs perform two types of tasks: 
    - Continous: Are always running, 'when a new photo is uploaded do x'. 
    - Triggered: Are not always running, 'when I click this button'. 
  
- ### Why would you ever choose WebJobs? 
  - For some reason you really want your code to be managed with an **existing app service**
  - You need control over the **JobHost** class to modify pre-existing event listeners.
  - WebJobs **Cannot**:
    - Scale processing automatically
    - Be developed in a broswer 
    - Utilize 'pay per use' pricing, you must pay for an entire VM.
    - Integrate with logic apps

![Very Useful diagram](https://docs.microsoft.com/en-us/learn/modules/choose-azure-service-to-integrate-and-automate-business-processes/media/3-service-choice-flow-diagram.png)


<hr></hr>
<h1 align="center">Azure Functions</h1>
<hr>

### What is """"SERVERLESS"""" compute? 

- Computation done on a non-managed infrastructure. Azure functions are consider a "Function as a Service (FAAS), and are managed by Azure servers. They are thus  "Serverless functions".

- Serverless compute allows intrinsic scalability. Azure's instrastruture for Azure functions allows for scaling up and scaling down the infrastructure to handle the precise demand, preventing over allocation of hardware.

### Azure Function Pros

- Only run in response to some event, called triggers.
- Provide the ability to re-deploy an azure function to a non-serveless enviroment.
- Can minimize cost of cloud computation.
  
### Azure Functions Cons 

- Functions instances are created and destroyed on demand, any state must be stored/retrieved from within the function to an assosiated storage service.
- Functions can only run for a maximum of **10 minutes (default of 5)** . **Only 2.5 minutes for a HTTP in/out binding**. (*Azure now provides a seperate service called "Durable Functions" which allow for long running stateful functions without timeout*)
- High Frequency function calls may be less cost effective than hosting the service as a VM. i.e Do not treat azure functions as a HTTP server.
- One function instance is created every **10 seconds**, to up to **200 total instances**, each instance can handle concurrent exeuctions. State/Acccess within functions will have handle concurrency concerns.

<hr></hr>
<h3 align="center">Azure Function Resource Allocation</h3>

### Azure Resource Hierachy:
    Azure Subscription
    |
    |--> Resource Group 
        |
        |--> Function App ----ref-----> | Storage Account (Blobs, Queues, and Tables)
            |
            |--> Azure Function 


### Deployment Details: 
-   Function app deploys with a specified runtime stack
-   Functions apps are hosted within a specified region
-   Function apps are the execution context for an Azure function
-   Plan Types 
    -   Consumption: Charged on a per-use basis, provides automatic scaling
    -   **App Service: runs the function on a provided VM which is managed by you, this avoids timeouts for functions** 
    -   Premium: More performant and available version of the consumption plan
- To use azure's application insights you must enable the service upon deployment
- Each function will be allocated it's own URL to be accessed, these cannot be static
- Each function app requires a globaly unique name
  
<hr></hr>
<h3 align="center">Azure Function Implementation</h3>

### Bindings: 
    {
        "bindings": [
            {
            "name": "order",
            "type": "queueTrigger",
            "direction": "in",
            "queueName": "myqueue-items",
            "connection": "MY_STORAGE_ACCT_APP_SETTING"
            },
            {
            "name": "$return",
            "type": "table",
            "direction": "out",
            "tableName": "outTable",
            "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
            }
        ]
    }

- Bindings declare the ingress and egress of data to and from the azure function.
- Event triggers are input bindings which initiate the exeuction of the function.
- There are a large number of built in output and input bindings to integrate with commonly used services.
- **You can have any number of input and output bindings**


### Debugging:
- If application insight integration is enabled on the function app, common loging functions such as "console.log()" can be viewed using the "Monitor" dashboard on the azure portal. Errors and warnings caused via the compilation or running of the code will also be displayed with the logs.

### Authorization:
- The HTTP triggers have built in "authorization level" security. There are three levels built in. I think you could see them from **AFA**r.
  - _**A**_ dmin: Authorised by 1 global master key
  - _**F**_ unction: Authorised by many function-specific API keys
  - _**A**_ nonymous: No key is needed to authorise
- Keys are provided to the function URL as header content of the name "code" or "x-function-key".









  
























