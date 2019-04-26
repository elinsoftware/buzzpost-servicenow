# buzzPost - in-browser notifications in ServiceNow

buzzPost is a real-time notification platform, which provides direct on-screen notifications for ServiceNow users.

![img](/img/bzp2_sandbox.png)

It is a scoped application, which runs in a browser in a background and listens for incoming messages.

The application uses websockets to broadcasts messages to other web browsers. This cannot be done just in ServiceNow, we need to use a third party message broker and the best option is AWS IoT service.

## 1. Setup message broker - AWS IoT

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

### 2.2 Activate broadcasting feature in a global scope

Next step is to activate the app broadcasting feature. Scoped application does not allow running scripts in a global background, so you need to add a global UI script manually.

Download and install `broadcasting script.xml` update set.

### 2.3 Configure the application

The final step is to update the app configuration, navigate to *buzzPost 2.0 / Admin* and set the following:

- Remote message broker URL: your IoT endpoint
- Message broker region: your AWS region
- Communication key: IoT user Access key ID
- Communication secret: IoT user Secret access key