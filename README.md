# Introduction

This is a simple demo showcasing some of AMQ7 features/concepts such as: Broker, Client, Cluster, Security and Interconnect;

For additional details, please refer to [Additional References](#demo-additional-references) section;

## Environment

- [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [Apache Maven](https://maven.apache.org/)
- [Red Hat AMQ 7.2](https://access.redhat.com/documentation/en-us/red_hat_amq/7.2/)
- [Red Hat AMQ JMS Client](https://access.redhat.com/documentation/en-us/red_hat_amq/7.2/html-single/using_the_amq_jms_client/)

## Demo Script:

1. [Installing AMQ-7.2 Broker](#demo-step-1)
2. [Create a Broker](#demo-step-2)
3. [Explore AMQ-7.2 Console](#demo-step-3)
4. [Configure AMQ-7.2 Broker Settings](#demo-step-4)
5. [Explore AMQ Producer/Consumer Tools](#demo-step-5)
6. [Create Java/JMS and NodeJS Clients](#demo-step-6)

### Installing AMQ-7.2 Broker <a name="demo-step-1"></a>

* Download [Red Hat AMQ 7.2 Broker](https://developers.redhat.com/products/amq/download/) from [Red Hat Developers](https://developers.redhat.com/):

* After downloading AMQ 7.2 broker, uncompress it:

```
unzip amq-broker-7.2.0-bin.zip
```

* Check if the **amq-broker-7.2.0** directory is successfully created and the following directories are within it:

  * *from now on we're going to refer to $AMQ_HOME as the current location of you amq-7.2 broker installation*

```
$ ls -l
total 72
-rw-r--r--@  1 vmartine  staff  22122 Jun 21 04:10 LICENSE
-rw-r--r--@  1 vmartine  staff    179 Jun 21 04:10 NOTICE
-rw-r--r--@  1 vmartine  staff   5391 Jun 21 04:10 README.html
drwxrwxr-x@  5 vmartine  staff    160 Jun 21 04:40 bin
drwxr-xr-x@  3 vmartine  staff     96 Nov  7 17:10 docs
drwxr-xr-x@  3 vmartine  staff     96 Nov  7 17:10 etc
drwxr-xr-x@  7 vmartine  staff    224 Nov  7 17:10 examples
drwxr-xr-x@ 55 vmartine  staff   1760 Nov  7 17:10 lib
drwxrwxr-x@  7 vmartine  staff    224 Jun 21 04:40 schema
drwxr-xr-x@ 16 vmartine  staff    512 Nov  7 17:10 web
```

### Create a Broker <a name="demo-step-2"></a>

* Create a **brokers** directory within *$AMQ_HOME*:
  * *this is step is optional, however it's a good practice since we can easily perform administrative tasks such as cleanup, backup and restore without compromising the default AMQ7 files*

```
cd *$AMQ_HOME*
mkdir brokers
```

* Create a broker (e.g broker01):

```
./bin/artemis create brokers/broker01
```

* Inform both username and passwords of your choice (e.g admin / admin) and set *anonymous access* to **Y**;

* Check if **broker01**  directory has been created within *$AMQ_HOME* and the following directories are present:

```
$ ll -l
total 0
drwxr-xr-x   9 vmartine  staff  288 Nov  7 18:12 .
drwxr-xr-x   3 vmartine  staff   96 Nov  7 17:51 ..
drwxr-xr-x   4 vmartine  staff  128 Nov  7 17:51 bin
drwxr-xr-x   6 vmartine  staff  192 Nov  7 18:12 data
drwxr-xr-x  11 vmartine  staff  352 Nov  7 17:51 etc
drwxr-xr-x   2 vmartine  staff   64 Nov  7 17:51 lib
drwxr-xr-x   3 vmartine  staff   96 Nov  7 18:12 log
drwxr-xr-x   2 vmartine  staff   64 Nov  7 18:12 tmp
```

* Navigate to **broker01** directory and start this instance:

```
cd $AMQ_HOME/brokers/broker01/
./bin/artemis run
```

### Explore AMQ-7.2 Console <a name="demo-step-3"></a>

* By default **AMQ 7.2 Console** is available at port *8161*, therefore in order to access it just open a browser of your choice and try to access *[http://localhost:8161](http://localhost:8161)* using the credentials informed on [Lab 2](#demo-step-2)

### Configure AMQ-7.2 Broker <a name="demo-step-4"></a>

* Navigate to **broker01** directory and review all files within this directory. Specially *broker.xml* and *artemis.profile*;

* Remove all *acceptors* configuration within *broker.xml* except **artemis** which runs on port **61616**:

```
<acceptors>
  <acceptor name="artemis">tcp://0.0.0.0:61616?tcpSendBufferSize=1048576;tcpReceiveBufferSize=1048576;protocols=CORE,AMQP,STOMP,HORNETQ,MQTT,OPENWIRE;useEpoll=true;amqpCredits=1000;amqpLowCredits=300</acceptor>
</acceptors>
```

* Restart **broker01** instance:
```
cd $AMQ_HOME/brokers/broker01/
./bin/artemis run
```

* Please notice that only *acceptor* running on *61616* is available:

```
2018-11-07 21:38:49,511 INFO  [org.apache.activemq.artemis.core.server] AMQ221020: Started NIO Acceptor at 0.0.0.0:61616 for protocols [CORE,MQTT,AMQP,STOMP,HORNETQ,OPENWIRE]
```

* While **broker01** is up and running edit *broker.xml* once again adding a new *address* within *addresses* settings. Example:

```
<addresses>
  ...
  <address name="address.broker01">
    <anycast>
      <queue name="myFirstQueue"/>
      <queue name="mySecondQueue"/>
    </anycast>
  </address>
```

* Finally wait up to 5 seconds and check your broker log for the upcoming info message about a new deployment:

```
2018-11-07 22:06:59,292 INFO  [org.apache.activemq.artemis.core.server] AMQ221003: Deploying queue myFirstQueue on address address.broker01
2018-11-07 22:06:59,302 INFO  [org.apache.activemq.artemis.core.server] AMQ221003: Deploying queue mySecondQueue on address address.broker01
```

### Explore AMQ Producer/Consumer Tools <a name="demo-step-5"></a>

* While **broker01** is up and running, open a new terminal and execute:

```
./bin/artemis producer --destination address.broker01 --message-count 30 --url tcp://localhost:61616
```

* The following output is expected:

```
./bin/artemis producer --destination address.broker01 --message-count 30 --url tcp://localhost:61616
Producer ActiveMQQueue[address.broker01], thread=0 Started to calculate elapsed time ...

Producer ActiveMQQueue[address.broker01], thread=0 Produced: 30 messages
Producer ActiveMQQueue[address.broker01], thread=0 Elapsed time in second : 0 s
Producer ActiveMQQueue[address.broker01], thread=0 Elapsed time in milli second : 97 milli seconds
```

* Access **AMQ Console** *[http://localhost:8161](http://localhost:8161)* and browse: *0.0.0.0* -> *addresses* -> *address.broker01* -> *queues* -> *anycast*

* Since we did not specify a *queue* or *topic* all messages are dispatched and balanced between all locations within our *address* definition, therefore you should see *100* messages on each destination in **Message Count** property;

* Open a new terminal for each *consumer* and execute:

```
cd $AMQ_HOME/brokers/broker01/
./bin/artemis consumer --destination address.broker01 --url tcp://localhost:61616
./bin/artemis consumer --destination myFirstQueue --url tcp://localhost:61616
./bin/artemis consumer --destination mySecondQueue --url tcp://localhost:61616
```

* Plase notice that **Message Count** property for each destination is now *0*;

### Create Java/JMS and NodeJS Clients <a name="demo-step-6"></a>

* Download both **AMQ JMS Client** and **JavaScript Client** from [Red Hat Developers AMQ Download](https://developers.redhat.com/products/amq/download/)

* Uncompress both files in a directory of your choice

* Make sure you have configured your *settings.xml* enabling Red Hat repositories. Example:

```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <profiles>

        <!-- Configure the AMQ7 Maven repository -->
        <profile>
            <id>amq-qpid-jms</id>
            <repositories>
                <repository>
                    <id>amq-qpid-jms</id>
                    <url>file:///path/to/extracted/maven-repository</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>amq7-repository</id>
                    <url>file:///path/to/extracted/maven-repository</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
        <!-- Configure the JBoss GA Maven repository -->
        <profile>
            <id>jboss-ga-repository</id>
            <repositories>
                <repository>
                    <id>jboss-ga-repository</id>
                    <url>https://maven.repository.redhat.com/ga</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>jboss-ga-repository</id>
                    <url>https://maven.repository.redhat.com/ga</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
        <!-- Configure the JBoss Early Access Maven repository -->
        <profile>
            <id>jboss-earlyaccess-repository</id>
            <repositories>
                <repository>
                    <id>jboss-earlyaccess-repository</id>
                    <url>https://maven.repository.redhat.com/earlyaccess/all/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>jboss-earlyaccess-repository</id>
                    <url>https://maven.repository.redhat.com/earlyaccess/all/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
    <activeProfiles>
        <activeProfile>amq-qpid-jms</activeProfile>
        <activeProfile>jboss-ga-repository</activeProfile>
        <activeProfile>jboss-earlyaccess-repository</activeProfile>
    </activeProfiles>

</settings>
```

* From **AMQ JMS Client** directory, go to *examples* folder an run a *clean/install*:

```
cd apache-qpid-jms-0.34.0.redhat-00002/examples
mvn clean install
```

* Review *jndi.properties* and *HelloWorld.java* files for a better comprehension;

* Execute the following command in order to send and consume a  *Hello World Message*

```
cd apache-qpid-jms-0.34.0.redhat-00002/examples
mvn clean install
mvn clean package dependency:copy-dependencies -DincludeScope=runtime -DskipTests
java -cp "target/classes/:target/dependency/*" org.apache.qpid.jms.example.HelloWorld

Hello world!
```

* Access **AMQ Console** and notice that we have a new *queue* under *addresses*;

* From **JavaScript Client** directory, execute:

```
npm install debug
npm install yargs
```

* From *nodejs-rhea-0.2.15-1/node_modules/rhea* directory execute:

```
npm install
npm run browserify
```

* Switch back to *nodejs-rhea-0.2.15-1* folder and create a new directory (e.g example):

```
mkdir example
```

* Create a **send.js** file with the following content:

```
"use strict";

var rhea = require("rhea");
var url = require("url");

if (process.argv.length !== 5) {
    console.error("Usage: send.js <connection-url> <address> <message-body>");
    process.exit(1);
}

var conn_url = url.parse(process.argv[2]);
var address = process.argv[3];
var message_body = process.argv[4];

var container = rhea.create_container();

container.on("sender_open", function (event) {
    console.log("SEND: Opened sender for target address '" +
                event.sender.target.address + "'");
});

container.on("sendable", function (event) {
    var message = {
        "body": message_body
    };

    event.sender.send(message);

    console.log("SEND: Sent message '" + message.body + "'");

    event.sender.close();
    event.connection.close();
});

var opts = {
    host: conn_url.hostname,
    port: conn_url.port || 61616
};

var conn = container.connect(opts);
conn.open_sender(address);
```

* Create a **receive.js** file with the following content:

```
"use strict";

var rhea = require("rhea");
var url = require("url");

if (process.argv.length !== 4 && process.argv.length !== 5) {
    console.error("Usage: receive.js <connection-url> <address> [<message-count>]");
    process.exit(1);
}

var conn_url = url.parse(process.argv[2]);
var address = process.argv[3];
var desired = 0;
var received = 0;

if (process.argv.length === 5) {
    desired = parseInt(process.argv[4]);
}

var container = rhea.create_container();

container.on("receiver_open", function (event) {
    console.log("RECEIVE: Opened receiver for source address '" +
                event.receiver.source.address + "'");
});

container.on("message", function (event) {
    var message = event.message;

    console.log("RECEIVE: Received message '" + message.body + "'");

    received++;

    if (received == desired) {
        event.receiver.close();
        event.connection.close();
    }
});

var opts = {
    host: conn_url.hostname,
    port: conn_url.port || 61616
};

var conn = container.connect(opts);
conn.open_receiver(address);
```

* Open a new terminal and start *receive.js*. Example:

```
node receive.js amqp://localhost node-queue
```

* Open a new terminal and **send** some messages via *send.js*. Example:

```
node send.js amqp://localhost node-queue hello
node send.js amqp://localhost node-queue hello2
node send.js amqp://localhost node-queue hello3
node send.js amqp://localhost node-queue hello4
node send.js amqp://localhost node-queue hello5
```

## Additional References <a name="demo-additional-references">
