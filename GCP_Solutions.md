# My GCP Solutions

## How to migrate from IIB (ibm solution for integration) to GCP in Java?

Migrating from IBM Integration Bus (IIB) to Google Cloud Platform (GCP) in Java involves several steps and considerations. Here's a high-level overview of the process:

1. **Assessment and Planning:**
   - Assess your existing integration architecture implemented in IIB.
   - Identify the components, interfaces, and dependencies of your integration solutions.
   - Define your migration goals, such as cost reduction, scalability improvement, or adopting cloud-native technologies.

2. **Selecting GCP Services:**
   - Identify GCP services that align with your integration requirements. For example:
     - Google Cloud Pub/Sub for messaging and event-driven architectures.
     - Google Cloud Functions for serverless computing.
     - Google Cloud Dataflow for batch and stream processing.
     - Google Cloud Storage for file storage.
     - Google Kubernetes Engine (GKE) for containerized deployments.
     - Google Cloud SQL or Cloud Spanner for databases.
   - Evaluate these services based on their features, performance, scalability, and pricing.

3. **Application Refactoring:**
   - Rewrite or refactor your integration solutions in Java to utilize GCP services.
   - Modify your code to interact with GCP services using appropriate SDKs or client libraries provided by Google.
   - Ensure compatibility with the chosen GCP services and adjust the architecture as needed.

4. **Data Migration:**
   - Migrate your existing data from on-premises or existing data stores to GCP storage solutions, such as Cloud Storage, Cloud SQL, or BigQuery.
   - Develop data migration scripts or utilize GCP's data transfer tools to streamline the migration process.

5. **Testing and Validation:**
   - Conduct thorough testing of your migrated applications and integration solutions on GCP.
   - Validate the functionality, performance, and scalability of your applications in the GCP environment.
   - Perform integration testing to ensure seamless communication between different components and services.

6. **Deployment and Monitoring:**
   - Deploy your Java applications and integration solutions on GCP infrastructure, leveraging services like GKE or App Engine.
   - Set up monitoring, logging, and alerting using Google Cloud Monitoring, Logging, and Error Reporting to monitor the health and performance of your applications.

7. **Training and Support:**
   - Provide training to your development and operations teams on GCP services and best practices for managing applications in the cloud.
   - Leverage Google Cloud's support resources, documentation, and community forums for ongoing assistance and troubleshooting.

8. **Post-Migration Optimization:**
   - Optimize your applications and integration solutions for performance, cost, and security in the GCP environment.
   - Continuously monitor and analyze your workloads to identify opportunities for further optimization and enhancement.

Throughout the migration process, it's essential to engage stakeholders, maintain clear communication, and prioritize security and compliance requirements. Additionally, consider leveraging professional services or consulting partners with expertise in both IIB migration and GCP to ensure a smooth and successful transition.

## How to depoply a Spring Boot app to GCP

There are 3 ways to deploy and run an Spring Boot app on GCP.

### Spring Boot app to GCP GKE [Recommended]

Google Kubernetes Engine is formerly known as Container Engine. It takes care of provisioning and maintaining the Cluster Virtual Machines, Scaling our applications.

#### Steps to deploy a Spring Boot app to GCP GKE

[Another Guide for Spring Boot 2 GCP GKE](GCP_SpringBoot2GCP_GKE_2.md)

1. Create a Kubernetes Cluster

2. Open the Google Cloud Shell

3. Clone the Spring App project in Cloud shell

`$ git clone https://github.com/infiprotonblog/hello-world-spring-boot-app.git`

4. change the directory to Spring App

`$ cd spring-boot-app/springapp/`

5. Build the Docker-image for Spring App

`$ docker build -t <user-name>/<tag-name> <location-of-dockerfile>`

`$ docker build -t sunilbvb/spring-app .`

6. Check the images, we just built

`$ docker images`

![1672580331557](image/GCP_Solutions/1672580331557.png)

7. Run the Spring App Docker image locally in the Cloud shell

`$ docker run -d -p <port>:<port> <image-name>`

`$ docker run -d -p 8080:8080 spring-app`

8. Check the App is running by clicking on Web Preview on the port 8080

![1672580339614](image/GCP_Solutions/1672580339614.png)

9. Push the Spring App docker image to Google's Container Registry

First, setup the PROJECT_ID of your project as below

`$ export PROJECT_ID=<your-project-id>`

`$ export PROJECT_ID=learn1233`

Retag the Spring App docker image according to Google Container registry

`$ docker tag <old-image-name> <new-image-name>`

`$ docker tag spring-app gcr.io/${PROJECT_ID}/spring-app`

Now push the Spring App docker image to the Google Container registry

`$ docker push gcr.io/${PROJECT_ID}/<image-name>`

`$ docker push gcr.io/${PROJECT_ID}/spring-app`

![1672580471780](image/GCP_Solutions/1672580471780.png)

10. Deploy Spring App to Kubernetes

Click on the Connect button of Spring App Cluster to connect to the Kubernetes cluster

![1672580525768](image/GCP_Solutions/1672580525768.png)

Create a Deployment for Spring App docker image

`$ kubectl create deployment hello-spring-app --image=gcr.io/${PROJECT_ID}/spring-app`

To View the Deployment that we just created

```dos
$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
hello-spring-app   1/1     1            1           14s
```

To View, the Spring App Pod instances created by the deployment

```dos
$ kubectl get pods
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
hello-spring-app-5c7598bf68-sd6q6   1/1     Running         0       58s
```

11. Allow External Traffic

We have a Container running under the control of Kubernetes, Now it's time to make it accessible to the outside world

`$ kubectl expose deployment <deployment-name> --name=<service-name> --type=LoadBalancer --port 8080 --target-port 8080`

`$ kubectl expose deployment hello-spring-app --name=hello-spring-app-service --type=LoadBalancer --port 8080 --target-port 8080`

12. Check the Service that we just created

```dos
$ kubectl get services
NAME                         TYPE       CLUSTER-IP   EXTERNAL-IP
hello-spring-app-service  LoadBalancer  10.4.1.232  35.192.217.243
kubernetes                ClusterIP     10.4.0.1     <none>

 PORT(S)          AGE
8080:30571/TCP    102s
443/TCP           80m
```

13. Verify that Spring Boot App is running

`$ http://35.192.217.243:8080/hello`

![1672580664907](image/GCP_Solutions/1672580664907.png)

#### Pros of using GCP GKE to deploy a Spring Boot app

GKE provides integration with containers that allow a custom runtime and greater control over to Cluster configuration

Easy ability to RollBack to previous Versions of application

GKE provides a Consistent and Repeatable deployment framework

GKE provides Version Management support to accomplish with Docker Containers and Google Container registry

#### Cons of using GCP GKE to deploy a Spring Boot app

Traffic splitting and management should do it by ourself

Understanding the Complex concepts in Kubernetes such as POD, deployment, networking, etc

For smaller applications, it reduces the productivity

### Spring Boot app to GCP GAE [Not recommended]

[Another Guide for Spring Boot 2 GCP GAE](GCP_SpringBoot2GCP_GAE_2.md)

Google App Engine is a Platform as a Service and cloud computing platform for developing and hosting web applications.

Google App Engine supports Standard and Flexible environment, and the Flexible environment is the best option for deploying the Spring Boot application.

With the AppEngine Flex Environment, we are not restricted to the Jetty web framework to run the web applications. So there is no need to tweak Spring Boot dependencies to run in the GAE any more.

#### Steps to deploy a Spring Boot app to GCP GAE

1. Go to Google App Engine, Click on the Create Application button, select Region, and click Create an app

2. Select the Programming Language on which we are deploying the App and select Environment as Flexible

3. Deploy to App Engine using Google's Cloud Shell

4. Clone the Project

`$ git clone https://github.com/infiprotonblog/hello-world-spring-boot-app.git`

5. Change the directory to Spring App

`$ cd hello-world-spring-boot-app/springapp/`

6. Create an appengine folder in the main directory: src/main/appengine/

`$ mkdir src/main/appengine/`

7. Create an app.yaml file inside the appengine folder

`$ vim app.yml`

or nano, vi etc.

8. Add the below contents to it

```yml
#app.yml file src/main/appengine

runtime: java
env: flex

env_variables:
SPRING_PROFILES_ACTIVE: "cloud-gcp"

handlers:  
- url: /.*
script: this field is required, but ignored
secure: always
manual_scaling:
instances: 1

resources:
cpu: 2
memory_gb: 2.3
disk_size_gb: 10
```

9. Set the configuration to the Google App Engine project

`$ gcloud init`

10. Change the directory to the appengine, so we can deploy the App using the app.yml file

`$ cd src/main/appengine/`

11. Deploy to Google App Engine using the below command

`$ gcloud app deploy`

12. Verify that the Application is running

`$ https://learn1233.uc.r.appspot.com/hello`

![1672580119659](image/GCP_Solutions/1672580119659.png)

#### Pros of using GCP GAE to deploy a Spring Boot app

Project running on GAE can scale down to zero instance, if no requests are coming in, this is useful at the development stage for developers

GAE suited for Large Corporations as it automates everything

Fastest Autoscaling Capabilities

Management Complexity reduces as Developer only need to focus on Application and not to worry about managing VM's etc

#### Cons of using GCP GAE to deploy a Spring Boot app

GAE is more expensive

GAE provides less control over to App Engine

GAE restriction is, we can create applications in limited Programming Languages such as Python, Java, Go, PHP, NodeJs, etc

GAE has no Network Configuration

### Spring Boot app to GCP GCE (vm) [Not recommended]

Google Compute Engine is Infrastructure  as a Service (IaaS), which allows us to Create VM, allocate Memory, CPU, and Storage (SSD, HDD).

#### Steps to deploy a Spring Boot app to GCP GCE (vm)

1. Create a new Virtual Machine

Set the Ubuntu OS with the N1 Standard Machine type with 4 GB of memory.

![1672579507965](image/GCP_Solutions/1672579507965.png)

2. SSH into the VM by clicking on the SSH button

3. Update the Ubuntu software packages

`$ sudo apt-get update`

4. Installing the Java 8 on Ubuntu VM

`$ sudo apt-get install openjdk-8-jdk`

5. Now, Clone the Spring App from Github

`$ git clone <https://github.com/infiprotonblog/hello-world-spring-boot-app.git>`

6. Change the directory to Spring App

`$ cd hello-world-spring-boot-app/springapp/`

7. Grant the permission to ./mvnw ( maven wrapper ) to run on the VM

`$ chmod +x ./mvnw`

8. Building the Spring Boot App

`$ ./mvnw package`

9. Change the directory to the target

`$ cd target`

10. Now, Run the Spring Boot App as the background process in the VM

`$  nohup java -jar springapp1-0.0.1-SNAPSHOT.war &`

11. Expose 8080 port on the VM to make the Spring App accessible to the Internet

- Select View Network details

![1672579522543](image/GCP_Solutions/1672579522543.png)

- Go to Firewall Settings

![1672579545958](image/GCP_Solutions/1672579545958.png)

- Go to default-http-server and click on the edit button

- Add the 8080 port to the TCP and click the Save button to make the changes

![1672579554844](image/GCP_Solutions/1672579554844.png)

12. Verify that Spring Boot App is running

#### Pros of using GCP GCE (vm) to deploy a Spring Boot app

GCE is popular with small business applications

Cost less compared to GAE

GCE provides the full infrastructure in the form of VM that we have control over it

GCE allows us to use container optimized OS to run docker containers

#### Cons of using GCP GCE (vm) to deploy a Spring Boot app

VM requires at least one instance to run constantly

Need to manage Application and Virtual Machine regularly

Auto Scaling is Slower than Google App Engine

High Expensive as we need someone to manage VM and install everything for running Application

### How to connect the Google Cloud SQL into your Spring Boot application

Cloud SQL is a fully-managed database service that helps you set up, maintain, manage, and administer your relational databases on Google Cloud Platform.

You can use Cloud SQL with MySQL, PostgreSQL, or SQL Server.

Add the following properties into your application.properties file.

JDBC URL Format

`jdbc:mysql://google/<DATABASE-NAME>?cloudSqlInstance = <GOOGLE_CLOUD_SQL_INSTANCE_NAME> &socketFactory = com.google.cloud.sql.mysql.SocketFactory&user = <USERNAME>&password = <PASSWORD>`

Note − The Spring Boot application and Google Cloud SQL should be in same GCP project.

`application.properties` file:

```dos
spring.dbProductService.driverClassName = com.mysql.jdbc.Driver
spring.dbProductService.url = jdbc:mysql://google/PRODUCTSERVICE?cloudSqlInstance = springboot-gcp-cloudsql:asia-northeast1:springboot-gcp-cloudsql-instance&socketFactory = com.google.cloud.sql.mysql.SocketFactory&user = root&password = rootspring.dbProductService.username = root

spring.dbProductService.password = root
spring.dbProductService.testOnBorrow = true
spring.dbProductService.testWhileIdle = true
spring.dbProductService.timeBetweenEvictionRunsMillis = 60000
spring.dbProductService.minEvictableIdleTimeMillis = 30000
spring.dbProductService.validationQuery = SELECT 1
spring.dbProductService.max-active = 15
spring.dbProductService.max-idle = 10
spring.dbProductService.max-wait = 8000
```

`application.yml` file:

```yml
spring:
   datasource: 
      driverClassName: com.mysql.jdbc.Driver
      url: "jdbc:mysql://google/PRODUCTSERVICE?cloudSqlInstance=springboot-gcp-cloudsql:asia-northeast1:springboot-gcp-cloudsql-instance&socketFactory=com.google.cloud.sql.mysql.SocketFactory&user=root&password=root"
      password: "root"
      username: "root"
      testOnBorrow: true
      testWhileIdle: true
      validationQuery: SELECT 1
      
      max-active: 15
      max-idle: 10
      max-wait: 8000
```

### Jib — build Java Docker images better

Jib, an open-source Java containerizer from Google that lets Java developers build containers using the Java tools they know. Jib is a fast and simple container image builder that handles all the steps of packaging your application into a container image. It does not require you to write a Dockerfile or have docker installed, and it is directly integrated into Maven and Gradle—just add the plugin to your build and you'll have your Java application containerized in no time.

Docker build flow:

![1672582196045](image/GCP_Solutions/1672582196045.png)

Jib build flow:

![1672582199966](image/GCP_Solutions/1672582199966.png)

How to use Jib to containerize your application

Jib is available as plugins for Maven and Gradle and requires minimal configuration. Simply add the plugin to your build definition and configure the target image. If you are building to a private registry, make sure to configure Jib with credentials for your registry. The easiest way to do this is to use credential helpers like docker-credential-gcr. Jib also provides additional rules for building an image to a Docker daemon if you need it.

Jib on Maven

```xml
<plugin>
  <groupId>com.google.cloud.tools</groupId>
  <artifactId>jib-maven-plugin</artifactId>
  <version>0.9.0</version>
  <configuration>
    <to>
      <image>gcr.io/my-project/image-built-with-jib</image>
    </to>
  </configuration>
</plugin>
```

```dos
# Builds to a container image registry.
$ mvn compile jib:build
# Builds to a Docker daemon.
$ mvn compile jib:dockerBuild
```

Jib on Gradle

```dos
plugins {
  id 'com.google.cloud.tools.jib' version '0.9.0'
}
jib.to.image = 'gcr.io/my-project/image-built-with-jib'
```

```dos
# Builds to a container image registry.
$ gradle jib
# Builds to a Docker daemon.
$ gradle jibDockerBuild
```
