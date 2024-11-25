---
title: "Elastic Observability 101"
seoDescription: "Learn the basics of Elastic Observability and discover how it helps monitor and improve your system's performance. Perfect for beginners exploring Elastic"
datePublished: Mon Nov 25 2024 02:55:20 GMT+0000 (Coordinated Universal Time)
cuid: cm3wfqtkv000209ky5sbp37eu
slug: elastic-observability-101
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732499636433/6d2dba6b-dd8a-44e3-881d-be7c36efa6cb.webp
tags: kubernetes, elasticsearch, elastic, observability

---

# Introduction

Have you ever wondered how a pilot can navigate an airplane through dark skies and storms, carrying hundreds of passengers, and still reach their destination after hours of flight? The answer lies in the tools provided by the airplane manufacturer. These tools allow the pilot to continuously monitor the health of all systems on board, ensuring that everything is functioning properly even when human vision is limited, making the airplane **observable**! This concept of constant monitoring and insight is what we call 'observability.' But what exactly is observability, and how does it apply to modern IT technologies?"

## What is Observability?

You may have come across this buzzword before or you’ve just heard of it when reading the introduction of this blog. Making a system observable means any human or machine based on some signals can infer knowledge from and therefore take a decision to tackle an issue.

In modern technologies like cloud and distributed systems, applications and infrastructure are getting complex and big enough to make *classic* *monitoring* a difficult task. With observability, you make your system observable and easy for engineers/SRE to understand and troubleshoot issues effectively. Observability goes beyond monitoring by focusing not just on what’s happening, but also why it’s happening, making it possible to trace root causes, detect anomalies, and maintain performance even in complex, cloud-native and distributed environments.

## Observability foundations

As described in the Observability [white paper](https://github.com/cncf/tag-observability/blob/whitepaper-v1.0.0/whitepaper.md), observability has 3 foundations, which, based on the paper, are a good start. The 3 foundations are :

* Metrics: can be a measure like CPU, or numeric representation like HTTP code
    
* Logs: are text-based information, and can be system logs, application logs, etc.
    
* Traces: are transactions and are distributed, they represent what applications, services, and databases are being called when executing the parent request.
    

## Observability in Elastic

Elastic Observability has implemented the previous concepts and made it easy for engineers to make their system observable, by providing a wide range of elastic products, integrations, and tools to achieve it.

Elastic stack is known as a search company to centralize logs from operational systems, if you are new to Elastic you can check my previous blogs!

# Elastic Observability tools

Elastic has made many tools for different usage to make a system observable based on a framework called beats. Beats have many tools such as:

* metricbeat: a lightweight shipper tool to collects metrics from different systems/services and support many [modules](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-modules.html).
    
* filebeat: a lightweight shipper tool to collects logs from differents systems/services, and supports many [modules](https://www.elastic.co/guide/en/beats/filebeat/master/filebeat-modules.html).
    
* heartbeat: a lightweight daemon installed in a server to periodically check the status of your endpoints/services, and support the most used [monitor types](https://www.elastic.co/guide/en/beats/heartbeat/8.15/configuration-heartbeat-options.html) (TCP, ICMP and HTTP)
    
* winlogbeat, and more
    

For all the beat framework check this link: [https://www.elastic.co/guide/en/beats/libbeat/current/beats-reference.html](https://www.elastic.co/guide/en/beats/libbeat/current/beats-reference.html)

Perhaps you may say: there are many tools out there it’s complex! Don’t worry Elastic got your back, by making all the beats packaged in one tool called Elastic Agent.

## Elastic Agent

Elastic agent when deployed you have all the 4 beats at once:

* metricbeat
    
* filebeat
    
* heartbeat
    
* auditbeat
    

## but wait.. Elastic agent needs Fleet

If the word “Fleet” is new to you, maybe you don’t understand why you need it in elastic, we need it to manage our elastic agents centrally. Imagine you have hundreds of nodes that need to be monitored. For each node, you would need to manually deploy an Elastic Agent and configure the necessary integrations. Sounds overwhelming, right? The amount of manual work required would be enormous! That’s where Fleet comes in.

Elastic introduced this concept of Fleet to make it easy for us to maintain, upgrade and manage elastic agents across your environment. With Fleet, you can handle everything from a single interface, saving time, reducing complexity, and ensuring consistency across all your nodes.

The following is the overview architecture for Fleet server:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731457042387/76f4cc2f-e89f-416e-a16e-1e3078d8b7d1.png align="center")

Here are the following points to remember for elastic agent and Fleet server:

* A policy can contain many integrations (ex: Postgresql integration, system integration, etc.)
    
* An elastic agent can be only enrolled with a **single policy**
    
* each elastic agent is enrolled to a policy using an enrollment token
    
* Elastic agent checks with Fleet server for updates
    
* There is a difference between “Fleet” and “Fleet server”:
    
    * Fleet: is a web-based UI in Kibana for [centrally managing Elastic Agents](https://www.elastic.co/guide/en/fleet/current/manage-agents-in-fleet.html)
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731457462182/3c2aded5-e0f1-4041-bf64-c4974c0b7d44.png align="center")
        
    * Fleet server: is the elastic component that connects elastic agents to Fleet
        

And for elastic agents, there are two types of deployment:

* **Fleet-managed Elastic Agent (recommended):** With this approach, you install Elastic Agent and use Fleet in Kibana to define, configure, and manage your agents in a central location.
    
* **Elastic Agent in standalone mode (advanced users):** With this approach, you install Elastic Agent and manually configure the agent locally on the system where it’s installed. You are responsible for managing and upgrading the agents. This approach is reserved for advanced users only.
    
* **Elastic Agent in a containerized environment:** run Elastic Agent inside of a container — either with Fleet Server or standalone
    

More information can be found in the doc: [https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html](https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html)

### Create your Fleet Server

1. To create your Fleet Server, go to “Fleet” app from “Management” menu and click **“Add Fleet Server”**
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731458261852/68e18782-f7c0-497d-a985-0bd4d65a194f.png align="center")
    
2. enter the name, the host and click **“Generate Fleet Server Policy”**, we will be using the quickstart for this demo
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731458485922/d1abcbee-cd51-4a95-b4c3-50d9da4145f8.png align="center")
    
    this method generates a fleet server policy and an enrollment token.
    
3. after a few seconds, the fleet server policy is configured:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731458732722/247bcb30-9aff-4096-855a-34255861da4c.png align="center")
    
    4. in the second step we have the code to execute to deploy the elastic agent:
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731458979099/edbd63c4-7087-4482-8df2-6b6740e7fc7a.png align="center")
        
        we need to run each command to deploy it:
        
    
    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.14.3-linux-x86_64.tar.gz
    tar xzvf elastic-agent-8.14.3-linux-x86_64.tar.gz
    cd elastic-agent-8.14.3-linux-x86_64
    sudo ./elastic-agent install \
      --fleet-server-es=https://10.0.0.100:9200 \
      --fleet-server-service-token=USE_YOUR_TOKEN \
      --fleet-server-policy=fleet-server-policy \
      --fleet-server-es-ca-trusted-fingerprint=10e02827f48c1c97...0fc567 \
      --fleet-server-port=8220
    ```
    
    The response from the terminal is:
    
    ```bash
     $> sudo ./elastic-agent install \
      --fleet-server-es=https://10.0.0.100:9200 \
      --fleet-server-service-token=AAEAAWVsYXN0aWMvZmxlZXQtc2VydmVyL3Rva2VuLTE3MzE0NTg0OTE3NTg6cDJ1bFpiWEtTcEtnVmlZd2U3VVg1QQ \
      --fleet-server-policy=fleet-server-policy \
      --fleet-server-es-ca-trusted-fingerprint=10e02827f48c1c9775b881dda9bfe468ac2463422648815a6fdcd5958b0fc567 \
      --fleet-server-port=8220
    Elastic Agent will be installed at /opt/Elastic/Agent and will run as a service. Do you want to continue? [Y/n]:Y
    [ ===] Service Started  [11s] Elastic Agent successfully installed, starting enrollment.
    [    ] Waiting For Enroll...  [14s] {"log.level":"info","@timestamp":"2024-11-13T00:54:25.987Z","log.origin":{"file.name":"cmd/enroll_cmd.go","file.line":434},"message":"Generating self-signed certificate for Fleet Server","ecs.version":"1.6.0"}
    [====] Waiting For Enroll...  [20s] {"log.level":"info","@timestamp":"2024-11-13T00:54:32.040Z","log.origin":{"file.name":"cmd/enroll_cmd.go","file.line":480},"message":"Restarting agent daemon, attempt 0","ecs.version":"1.6.0"}
    [    ] Waiting For Enroll...  [22s] {"log.level":"info","@timestamp":"2024-11-13T00:54:34.075Z","log.origin":{"file.name":"cmd/enroll_cmd.go","file.line":865},"message":"Fleet Server - Starting","ecs.version":"1.6.0"}
    [   =] Waiting For Enroll...  [26s] {"log.level":"info","@timestamp":"2024-11-13T00:54:38.080Z","log.origin":{"file.name":"cmd/enroll_cmd.go","file.line":846},"message":"Fleet Server - Running on policy with Fleet Server integration: fleet-server-policy; missing config fleet.agent.id (expected during bootstrap process)","ecs.version":"1.6.0"}
    [   =] Waiting For Enroll...  [27s] {"log.level":"info","@timestamp":"2024-11-13T00:54:38.869Z","log.origin":{"file.name":"cmd/enroll_cmd.go","file.line":517},"message":"Starting enrollment to URL: https://node-0:8220/","ecs.version":"1.6.0"}
    [=== ] Waiting For Enroll...  [31s] {"log.level":"info","@timestamp":"2024-11-13T00:54:43.295Z","log.origin":{"file.name":"cmd/enroll_cmd.go","file.line":480},"message":"Restarting agent daemon, attempt 0","ecs.version":"1.6.0"}
    {"log.level":"info","@timestamp":"2024-11-13T00:54:43.298Z","log.origin":{"file.name":"cmd/enroll_cmd.go","file.line":298},"message":"Successfully triggered restart on running Elastic Agent.","ecs.version":"1.6.0"}
    Successfully enrolled the Elastic Agent.
    [=== ] Done  [31s]
    Elastic Agent has been successfully installed.
    ```
    
    The output says that fleet server is installed and an elastic-agent is deployed with the policy generated early from Kibana UI. If you run `top` in your terminal, you’ll see a process named `fleet-server` is running:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731459912156/f4005c9a-2081-4631-8c2e-e1e023ccb8ac.png align="center")
    
    and to check the status of the elastic-agenton Debian, run `systemctl status elastic-agent`:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731459987532/f9216bea-481c-493f-b524-7056a9e6db5e.png align="center")
    
4. In Kibana we have the confirmation Fleet server is connected, click on **“Continue enrolling Elastic Agent“** if you want to enroll new elastic-agents. For this demo we previously deployed one with Fleet server, close this menu if you don’t want to enroll a new elastic-agent.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731460967709/c75d6425-c9b4-40a9-af0f-de8c803eacac.png align="center")
    
5. From Fleet UI, the agent is now visible and in a healthy state:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731461043598/19659e63-f8a4-4fa3-80ce-284a2d6593a7.png align="center")
    
6. Click on the host, In this case “node-0” to confirm that system metrics are properly collected, the dot should be green and the logs should not contain any error message:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731461141940/d11d1f99-d321-48f8-ba65-36a42512a860.png align="center")
    
    To verify whether system metrics are properly collected, Click on **“Metrics”** to read the logs, it should not contain any error message:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731461245244/fbc7b1c0-3974-4a78-a48b-25a396f40995.png align="center")
    
    To visualize all the nodes, open “Hosts” under **Infrastructure** in “Observability” app:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731461393234/e1141966-2950-4f2a-b831-6e29b5737859.png align="center")
    
    Until now, with Elastic Agent we collected metrics and logs using system integration. If you want to browse all integrations provided by elastic, go to [https://localhost:5601/app/integrations/browse](http://localhost:5600/app/integrations/browse) where you can search for a specific one and add it to your policy:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731462499534/d32d2668-f840-48f7-a670-b166cc77768f.png align="center")
    

## Traces

For traces in the Elastic observability, we are talking about APM (Application Performance Monitoring). It is done by using APM agents to collect traces about your application and send them to elasticsearch. Here APM agents are not the same as Elastic Agents. Remember: Elastic agents are used to collect logs, metrics from various sources like VMs, Databases, etc. and APM agents are used to collect traces from applications: your e-commerce website, your Flask API, etc.

APM agents support many programming languages:

* Android
    
* iOS
    
* Java
    
* Python
    
* and many more
    

For all APM agents check the documentation: [https://www.elastic.co/guide/en/apm/agent/index.html](https://www.elastic.co/guide/en/apm/agent/index.html)

APM agents in elastic are used for:

* catching application errors
    
* improve code quality by using distributed tracing
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732408352164/b3ac4b7d-bd85-4450-b237-fa01daeb2f47.png align="center")

* learn more about your application by knowing its overall performance & impact
    
* visualize the application dependency with Service map
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732408638364/34f8f9e9-ddc5-4d4a-ab9c-e7a55a127155.png align="center")

* and Elastic observability support OpenTelemetry integration
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732408748519/89131a52-1ff2-42d9-930d-276c11e62159.png align="center")

* support RUM (Real User Monitoring) for frontend applications
    
* and many more..
    

### How APM works in Elastic?

By now, you understand that traces in Elastic require APM Agents to collect data (also known as transactions) and Elasticsearch to store the data. However, there’s an essential component that bridges these two: the APM Server.

The APM Server acts as the intermediary where APM data transits, is parsed into the Elastic Common Schema (ECS), and is then sent to Elasticsearch for storage and analysis. This additional component plays a crucial role in ensuring that your tracing data is properly structured and ready for use in Elastic Observability.

Having a component like APM server will achieve the following:

* will help to have a light-weight APM agent
    
* It enables scaling the server as it is an independent component.
    
* Allows collecting information from the browser for RUM (Real User Monitoring), preventing the browser from interacting directly with Elasticsearch for security reasons.
    
* Controls the flow of data sent to Elasticsearch.
    
* Fault tolerance: if one or more Elasticsearch nodes fail, the APM server has a buffer to store data until the Elasticsearch nodes are available.
    
* Acts as an intermediary for source mapping for JavaScript in the browser.
    

The code used for APM is available in my github repo: [https://github.com/mjrlgue/elastic-observability-demo](https://github.com/mjrlgue/elastic-observability-demo)

#### How to deploy APM server

There are two ways:

* Using Fleet-managed APM Server
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732409649881/43999b82-ddc8-4c50-bc8f-60ee2864280d.png align="center")
    
* APM server binary
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732409685601/f894d4e3-ebda-490a-b62b-be4c4bff829a.png align="center")
    
    For more information: [https://www.elastic.co/guide/en/observability/current/apm-getting-started-apm-server.html](https://www.elastic.co/guide/en/observability/current/apm-getting-started-apm-server.html)
    

In this post I’ll be using APM server binary using ECK, if you are not familiar with it check my blog: [https://mar1.hashnode.dev/kubernetes-operators-explained-with-elastic](https://mar1.hashnode.dev/kubernetes-operators-explained-with-elastic).

For the application, I’ll be using these two app I found on github:

* the frontend app using NodeJs: [https://github.com/adamquan/carfront](https://github.com/adamquan/carfront)
    
* the backend app using Java (Spring boot): [https://github.com/adamquan/cardatabase](https://github.com/adamquan/cardatabase)
    

Since I’m using ECK in a kubernetes environment, I had to Dockerize both apps:

* carfront Dockerfile
    

```bash
FROM node:16
# set working direction
WORKDIR /app
# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH
# install application dependencies
COPY package.json ./
COPY package-lock.json ./
# RUN npm i
RUN npm ci
# add app
COPY . ./
# generate static js
RUN npm run build
# install serve, use node:10.6.0 to install server
RUN npm install -g serve #use node
# start app
# CMD ["npm", "start"]
# you can use CMD serve -s --port 8081 build to change default 5000 port
#CMD serve -s build
CMD ["serve", "-s", "build", "--listen", "5000"]
EXPOSE 5000
```

* cardatabase Dockerfile
    
    * Before building the docker image, run this: `mvn -DskipTests=true package` to build an executable jar for the java app. The jar will be created under the folder `target`
        

```bash
FROM openjdk:8u322-jdk
ENV APP_HOME=/usr/app

WORKDIR $APP_HOME

COPY target/cardatabase*.jar app.jar
# COPY target/elastic-apm-agent-latest-*.jar apm-agent.jar
COPY target/elastic-apm-agent-1.23.0.jar apm-agent.jar


CMD java -javaagent:apm-agent.jar \
     -Delastic.apm.service_name=${SERVICE_NAME} \
     -Delastic.apm.server_urls=${APM_URL} \
     -Delastic.apm.secret_token=${SECRET_TOKEN} \
     -Delastic.apm.environment=${ENVIRONMENT} \
     -Delastic.apm.global_labels=project=demo \
     -Delastic.apm.application_packages=${APPLICATION_PACKAGES} \
     -Delastic.apm.verify_server_cert=false \
     -jar app.jar
```

From Dockerfile for Java app, you can see I’m attaching the elastic-apm-agent jar to the application, this is the manual setup for Java, you can find other setups in here: [https://www.elastic.co/guide/en/apm/agent/java/current/setup.html](https://www.elastic.co/guide/en/apm/agent/java/current/setup.html).

And we need to pass some properties to the Java command, like the `-Delastic.apm.service_name` property that will be used to filter all errors and transactions for a given application in Elastic. To learn more about each property used here and others check the documentation: [https://www.elastic.co/guide/en/apm/agent/java/current/config-core.html](https://www.elastic.co/guide/en/apm/agent/java/current/config-core.html)

Before building the two docker images, get the secret token of your APM Server under the secret name `apm-server-quickstart-apm-token` and update the key `secretToken` in the file `carfront/src/rum.js`:

```javascript
import { init as initApm } from '@elastic/apm-rum'
var apm = initApm({
  // Set required service name (allowed characters: a-z, A-Z, 0-9, -, _, and space)
  serviceName: 'carfront',
  // Set the version of your application
  // Used on the APM Server to find the right sourcemap
  serviceVersion: '0.90',
  environment: 'demo',
  secretToken: 'Dqdv74xXZS31QTc3VC22s577',
  // Set custom APM Server URL (default: http://localhost:8200)
  // serverUrl: 'https://apm-server-quickstart-apm-http.eck.svc:8200',
  serverUrl: 'https://localhost:8200',
  // ...
})

export default apm;
```

The rum.js file is responsible for collecting RUM data when any user is accessing our web app. Like: Page information (URLs visited and referrer), Network connection information, JavaScript errors, etc. To know more about RUM check the documentation: [https://www.elastic.co/guide/en/apm/agent/rum-js/current/intro.html](https://www.elastic.co/guide/en/apm/agent/rum-js/current/intro.html)

Now build your two apps with: `docker build -t carfront:latest .` and `docker build -t cardatabase:latest .`

Then deploy your APM Server, carfront and cardatabase apps:

```bash
kubectl apply -f .\apmserver.yml
kubectl apply -f .\cardatabase.yml
kubectl apply -f .\carfront.yml
```

Don’t forget to port-forward all required ports (APM Server, carfront & cardatabase apps):

```bash
kubectl port-forward -n eck service/apm-server-quickstart-apm-http 8200:8200
kubectl port-forward -n carapp service/cardatabase-service 8080:8080
kubectl port-forward -n carapp service/carfront-service 5000:5000
```

**Tip: Since we are using a self-signed certificate, before accessing your carfront URL, access first to the APM Server on** **https://localhost:8200** **and accept from your browser the warning message. Doing this will allow the carfront web app to send RUM data to APM Server, otherwise, you’ll get some HTTP error message in the console preventing sending data to** **https://localhost:8200**\*\*.\*\*

When accessing the carfront app on **http://localhost:5000**, we can see both apps showing in the APM UI: [https://localhost:5601/app/apm/services](https://localhost:5601/app/apm/services) :

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732489817832/9292a40c-4cae-47cc-a174-4d4abdbad646.png align="center")

When clicking on “cardatabase”, we can see Elastic APM recognize the language the app uses and the environment (Java and Kubernetes):

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732490124561/d5c7f0ef-561d-46c8-b719-58c781e694ea.png align="center")

If we go to **Transactions** tab and click on one transaction, we can see the dependency and the request made between the backend and H2 database:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732490462857/34286707-692b-4f98-9605-39ec7158afa0.png align="center")

For the carfront, one transaction shows the dependencies between the backend and H2 database and we can see how much time it took for the javascript files to load:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732491831635/62186206-69a5-4bf7-98ed-92f900691d95.png align="center")

#### Service map

A cool feature is the service map where you can see a high overview of all your applications and see their dependencies:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732498992423/4bd92e7b-afd7-41eb-91c5-913b38bf69d4.png align="center")

#### Code Debugging

* Elastic APM helps to debug the code in case of an error. If we add a car and set the price using a comma:
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732492843225/9c831371-dd3c-49e9-9b8f-569e6b48910f.png align="center")

In the **Errors** tab for cardatabase service, we can see explicitly the error message:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732493026926/2079f9f1-58bd-454d-8ff4-d8ccf3bacb92.png align="center")

* For the carfront app, If we click on the “ERROR” button and check in carfront service what kind of error and the root cause, we can’t understand much:
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732493244347/f7455b08-cfb4-4dbf-80b7-b49bd1f321c9.png align="center")

To see the code, we have to add source maps: [https://www.elastic.co/guide/en/apm/agent/rum-js/4.x/sourcemap.html](https://www.elastic.co/guide/en/apm/agent/rum-js/4.x/sourcemap.html)

For that, we need to upload js file located in `/app/build/static/js` from inside the carfront pod, and run the following commands using Kibana endpoint: (adapt them depending on the file names):

```bash
curl -k -v -X POST "https://quickstart-kb-http.eck.svc:5601/api/apm/sourcemaps" \
-H "kbn-xsrf: true" \
-u elastic:0by0g96zH26FteKJc4V1k7s4 \
-F "service_name=carfront" \
-F "service_version=0.90" \
-F "bundle_filepath=http://localhost:5000/static/js/2.d9554e2d.chunk.js" \
-F "sourcemap=@./2.d9554e2d.chunk.js.map"

curl -k -v -X POST "https://quickstart-kb-http.eck.svc:5601/api/apm/sourcemaps" \
-H "kbn-xsrf: true" \
-u elastic:0by0g96zH26FteKJc4V1k7s4 \
-F "service_name=carfront" \
-F "service_version=0.90" \
-F "bundle_filepath=http://localhost:5000/static/js/main.09b5f190.chunk.js" \
-F "sourcemap=@./main.09b5f190.chunk.js.map"

curl -k -v -X POST "https://quickstart-kb-http.eck.svc:5601/api/apm/sourcemaps" \
-H "kbn-xsrf: true" \
-u elastic:0by0g96zH26FteKJc4V1k7s4 \
-F "service_name=carfront" \
-F "service_version=0.90" \
-F "bundle_filepath=http://localhost:5000/static/js/runtime~main.fdfcfda2.js" \
-F "sourcemap=@./runtime~main.fdfcfda2.js.map"
```

For each curl command, you need to get the following message at the end:

```bash
..."created":"2024-11-25T00:36:52.422Z","id":"apm:carfront-0.90-f7e4
7b03e6a4f4ce63a00a62c4a4cddf636351c1c0656c397ebc5c69d4109194","compressionAlgorithm":"zlib","decodedSha256":"f7e47b03e6a4f4ce63a00a62c4a4cddf636351c1c0656c397ebc5c69d4
* Connection #0 to host quickstart-kb-http.eck.svc left intact
```

The host `quickstart-kb-http.eck.svc` is the Kibana host from kubernetes services.

If we click again on the **Error** button in the carfront UI and check the error message from Elastic, we will see the exact javascript code that triggers and the javascript file responsible for raising the error:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732496510612/4de3d8c4-b535-4c20-bbef-97e435074534.png align="center")

and clicking on the error message will reveal the javascript code and the exact line that raised the error.. which is cool :D

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732496585666/aad5f55e-da31-440b-afa8-1448ab05a747.png align="center")

#### User Experience

With RUM you can see what’s happening on the user side (how much time it takes for web page to load, views, etc.) Go to [https://localhost:5601/app/ux](https://localhost:5601/app/ux) and a dashboard will open showing some stats about your web page and visitors:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732501182238/8fe0dd07-6a46-4539-80f1-784bc81a3591.png align="center")

### Monitoring endpoints

Elastic offers tools to monitor endpoints using known protocols (HTTP, TCP, ICMP) to check whether a service is up or down. The tool used here is [Heartbeat](https://www.elastic.co/guide/en/beats/heartbeat/current/index.html).

I defined some internal and external endpoints to monitor them, go to [https://localhost:5601/app/uptime](https://localhost:5601/app/uptime) to see them:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732497784943/dbd314d0-c7f7-443e-b04a-c6c049fcc205.png align="center")

There is another tool integrated in Elastic agents called Synthetic to make lightweight checks: [https://www.elastic.co/guide/en/observability/current/monitor-uptime-synthetics.html](https://www.elastic.co/guide/en/observability/current/monitor-uptime-synthetics.html).

## Alerting

Catching errors and knowing what’s happening in your infrastructure and your application is good, but notifying your team about them is better. Elastic offers an alerting system to define alerts with many types:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732496779956/f12ea3aa-53e2-43e1-a522-bcc769e7f91a.png align="center")

and also provides connectors (like Slack, email, etc.) to automatically send the alert payload to external tools:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732498046757/43c71911-dd71-46ee-bd06-998c97c32890.png align="center")

### SLOs

Service-level objectives (SLOs) allow you to measure your service performance based on factors like: availability, response time, etc. We can create an SLO for our monitored services in heartbeat to measure their performance using an SLO of type “Custom Query”:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732498385802/4cf87014-a65f-4e4a-8378-9d0f7107fc14.png align="center")

Here we used the “Group by” field and set it to the field “monitor.name” to create an SLO for each monitor instance. The results are 6 SLO created:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732498545506/6c31df11-b80b-4915-b448-ff7152bc88db.png align="center")

For the SLO “cardatabaseWillFail”, you can see a warning icon indicating there is an alert, clicking on it will open up the Alert UI indicating that our SLO didn’t reach the 99% target:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732498714840/fcadbb79-58ba-49be-a492-0f53025a58c1.png align="center")

SLO documentation: [https://www.elastic.co/guide/en/observability/current/slo.html](https://www.elastic.co/guide/en/observability/current/slo.html)

# Conclusion

Elastic Observability provides a wide range of features and tools to make your systems easier to monitor and understand. In this post, I covered just the basics, but there's so much more to explore, feel free to dive into other aspects and discover its full potential.

*Cover image generated with AI!*