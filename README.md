# buzzPost - in-browser notifications in ServiceNow

buzzPost is a real-time notification platform, which provides direct on-screen notifications for ServiceNow users. It is a scoped application, which runs in a browser in a background and listens for incoming messages.

![img](/img/bzp2_sandbox.png)

The application requires some configuration and a message broker (broadcasting services) setup.

## 1. Setup message broker - AWS IoT

The app uses websockets to broadcasts messages to other web browsers. This cannot be done just in ServiceNow, we need to use a third party message broker and the best option is AWS IoT service.

AWS IoT provides industrial grade security/performance, requires zero configuration and it's extremely cheap for messaging purposes.

 1. Get your own AWS IoT instance.
 2. Create a IAM user with *programmatic access* and assign [AWSIoTDataAccess](https://docs.aws.amazon.com/iot/latest/developerguide/iam-policies.html) IAM policy.

For buzzPost configuration you would need:

- AWS IoT endpoint
- Region name
- user Access key ID
- user Secret access key

>If you don't want to spend time for setting up your own AWS IoT service - [send me a message](mailto:support@elinsoftware.com), I'll add you to my IoT instance.

## 2. ServiceNow configuration

### 2.1 Install the application

Download and install `buzzPost2.0 - application.xml` update set.

Download and **XML import** `sys_properties.xml`, that is a list of secure properties for AWS IoT connection.

### 2.2 Activate broadcasting feature in a global scope

Next step is to activate the app broadcasting feature. Scoped application does not allow running scripts in a global background, so you need to add a global UI script manually.

Download and **XML import** `broadcasting script.xml` update set.

### 2.3 Configure the application

The final step is to update the app configuration, navigate to *buzzPost 2.0 / Admin* and set the following:

- Notification listener: **disabled**
- Remote message broker URL: your IoT endpoint
- Message broker region: your AWS region
- Communication key: IoT user Access key ID
- Communication secret: IoT user Secret access key

### 2.4 Reload the browser

Reload your web browser and open any list or form, global script should be injected and you should be connected to the message broker.

Check you browser console for the following messages:

![img](/img/console.png)

The application comes with a very detailed manual how to create your own templates, run tests, create triggers etc.

![img](/img/help.png)

There are two business rules on an Incident table, which come with the app:

![img](/img/br.png)

They are inactive by default. Activate them and assigned users will be notified through the messaging service when an incident assigned.

>Broadcasting service based on ServiceNow event processing, so there may (or may not) be a few seconds delay between the actual event and notification displayed on in a browser.

## 3. How to disable broadcasting service

To disconnect/disable broadcasting service - set *bzp2.init.js* UI script to inactive.