# Spring Boot app to GCP GAE

TOC

- Configured Google Cloud Platform and the App Engine
- Created a MySQL instance with Cloud SQL
- Configured Spring Cloud GCP for using MySQL
- Deployed our configured Spring Boot application
- Tested and scaled the application

## Google Cloud Platform Configuration

```dos
gcloud init
gcloud config set project sutek-spring-boot-bootstrap
```

We'll install the App Engine support and create an App Engine instance:

```dos
gcloud components install app-engine-java
gcloud app create
```

Our application will need to connect to a MySQL database within the Cloud SQL environment. As Cloud SQL doesn't provide a free tier we'll have to enable billing on the GCP account.

We can check available tiers easily:

`gcloud sql tiers list`

We can create a MySQL instance and database in Cloud SQL using the Cloud Console or the SDK CLI

```dos
# create instance
gcloud sql instances create \
  sutek-spring-boot-bootstrap-db \
    --tier=db-f1-micro \
    --region=europe-west2
# create database
gcloud sql databases create \
  sutek_bootstrap_db \
    --instance=sutek-spring-boot-bootstrap-db
```

## Spring Cloud GCP Dependencies

Our application will need dependencies from the Spring Cloud GCP project for the cloud-native APIs.

For this, let's use a Maven profile named cloud-gcp:

```xml
<profile>
  <id>cloud-gcp</id>
  <dependencies>
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-gcp-starter</artifactId>
      <version>1.0.0.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-gcp-starter-sql-mysql</artifactId>
      <version>1.0.0.RELEASE</version>
    </dependency>
  </dependencies>
```

Then we add the App Engine Maven plugin:

```xml
    <build>
      <plugins>
        <plugin>
          <groupId>com.google.cloud.tools</groupId>
          <artifactId>appengine-maven-plugin</artifactId>
          <version>1.3.2</version>
        </plugin>
      </plugins>
    </build>
</profile>
```

2.2.0

2.4.4

## Application Configuration

Now, let's define the configuration that allows the application to use the cloud-native resources like the database.

Spring Cloud GCP uses `spring-cloud-bootstrap.properties` to determine the application name:

`spring.cloud.appId=sutek-spring-boot-bootstrap`

We'll use a Spring Profile named gcp for this deployment and we'll need to configure the database connection. Therefore we create `src/main/resources/application-gcp.properties`:

```dos
spring.cloud.gcp.sql.instance-connection-name=\
    sutek-spring-boot-bootstrap:europe-west2:sutek-spring-boot-bootstrap-db
spring.cloud.gcp.sql.database-name=sutek_bootstrap_db
```

## Deployment

The Google App Engine provides two Java environments:

- the Standard environment provides Jetty and JDK8
- the Flexible environment provides just JDK8
  
The Flexible environment is the best option for Spring Boot applications.

We require the gcp and mysql Spring profiles to be active, so we provide the `SPRING_PROFILES_ACTIVE environmental` variable to the application by adding it to the deployment configuration in `src/main/appengine/app.yaml`:

```yml
runtime: java
env: flex
runtime_config:
  jdk: openjdk8
env_variables:
  SPRING_PROFILES_ACTIVE: "gcp,mysql"
handlers:
- url: /.*
  script: this field is required, but ignored
manual_scaling: 
  instances: 1
```

Now, let's build and deploy the application using the appengine maven plugin:

`mvn clean package appengine:deploy -P cloud-gcp`

Note: `cloud-gcp` is the Maven profile we created earlier.

After deployment we can view or tail log files:

```dos
# view
gcloud app logs read

# tail
gcloud app logs tail
```

Now, let's verify that our application is working by adding a book:

```dos
http POST https://sutek-spring-boot-bootstrap.appspot.com/api/books \
        title="The Player of Games" author="Iain M. Banks"
```

Expecting the following output:

```dos
HTTP/1.1 201 
{
    "author": "Iain M. Banks",
    "id": 1,
    "title": "The Player of Games"
}
```

## Scaling the Application

The default scaling in App Engine is automatic.

It may be better to start with manual scaling until we understand the runtime behavior, and the associated budgets and costs involved. We can assign resources to the application and configure automatic scaling in `app.yaml`:

```yml
# Application Resources
resources:
  cpu: 2
  memory_gb: 2
  disk_size_gb: 10
  volumes:
  - name: ramdisk1
    volume_type: tmpfs
    size_gb: 0.5
# Automatic Scaling
automatic_scaling: 
  min_num_instances: 1 
  max_num_instances: 4 
  cool_down_period_sec: 180 
  cpu_utilization: 
    target_utilization: 0.6
```
