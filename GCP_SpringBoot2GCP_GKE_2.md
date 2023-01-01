# Spring Boot app to GCP GKE

<!-- https://codelabs.developers.google.com/codelabs/cloud-springboot-kubernetes#0 -->

If you're interested in running Kubernetes on your local machine, such as a development laptop, then look into Minikube, which offers a simple setup of a single-node Kubernetes cluster for development and testing purposes.

TOC

## Setup and requirements

Run the following command in Cloud Shell to confirm that you are authenticated:

`gcloud auth list`

## Get source code

```dos
$ git clone https://github.com/spring-guides/gs-spring-boot.git
$ cd gs-spring-boot/complete
```

## Locally run the app

You can start the Spring Boot app normally with the Spring Boot plugin.

`$ ./mvnw -DskipTests spring-boot:run`

After the app starts, click on Web Preview 396bfd51f55afb5d.pngin the Cloud Shell toolbar and select Preview on port 8080.

![1672585047968](image/GCP_SpringBoot2GCP_GKE_2/1672585047968.png)

A tab in your browser opens and connects to the server you just started.

![1672585052595](image/GCP_SpringBoot2GCP_GKE_2/1672585052595.png)




