# Spring Boot app to GCP GAE

TOC

- Configure Google Cloud Platform Console and SDK
- Use Cloud SQL to create a MySQL instance
- Configure the application for Spring Cloud GCP
- Deploy the application to App Engine and test it

## Google Cloud Platform Configuration

```dos
gcloud init
gcloud config set project baeldung-spring-boot-bootstrap
gcloud components install app-engine-java
gcloud app create
gcloud sql tiers list
```

Ccan create a MySQL instance and database in Cloud SQL using the Cloud Console or the SDK CLI

```dos
# create instance
gcloud sql instances create \
  baeldung-spring-boot-bootstrap-db \
    --tier=db-f1-micro \
    --region=europe-west2
# create database
gcloud sql databases create \
  baeldung_bootstrap_db \
    --instance=baeldung-spring-boot-bootstrap-db
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

## Application Configuration

Now, let's define the configuration that allows the application to use the cloud-native resources like the database.

Spring Cloud GCP uses `spring-cloud-bootstrap.properties` to determine the application name:

`spring.cloud.appId=baeldung-spring-boot-bootstrap`

We'll use a Spring Profile named gcp for this deployment and we'll need to configure the database connection. Therefore we create `src/main/resources/application-gcp.properties`:

```dos
spring.cloud.gcp.sql.instance-connection-name=\
    baeldung-spring-boot-bootstrap:europe-west2:baeldung-spring-boot-bootstrap-db
spring.cloud.gcp.sql.database-name=baeldung_bootstrap_db
```
