# MyGCP

My GCP

## My Experience / My GCP projects

- Designed, developed, deployed and managed Microservices with Node.js and Java RESTful APIs in GCP for a large Telecommunication company

  - Built new REST APIs with Node.js and Java to talk with downstream APIs and serve for upstream applications.
  - Leveraged Redis to provide caching and programmed the refresh schedule jobs (bulk deleting hash and set keys).
  - Mocked APIs with Mockoon and nock during the development.
  - Automated the build and test processes with GitHub repo, triggers, schedulers and Cloud Builds.
  - Deployed with Cloud build YAML files, GKE Kubernetes, pods, Helm and Docker file to achieve auto scaling.
  - Implemented CI/CD pipelines with Spinnaker, Jenkins, Groovy and Terraform.
  - Monitored API performance with Google Cloud stack driver dashboard and alert policies.
  - Implemented immutable infrasture with Terraform scripts and pull requests.
  - Conducted performance testing with JMeter.
  - Protected sensitive data with DLP API, Cloud Function and Pub/Sub.
  - Mitigated DDoS threats with Cloud Armor.
  - Identified vulnerabilities in the App Engine with Security Scanner.

- Designed and implemented Cloud functions and CI/CD data pipelines in Google Cloud for a large multinational company

  - Programmed cloud functions in Golang and Python.
  - Configured one cloud function to monitor incoming Pub/Sub messages then save to BigQuery, another cloud function to archive data from BigQuery to GCS bucket in avro format with Cloud Schedule.
  - Created unit tests with mock and integration tests for test coverage;
  - Built a stress testing multi-threading tool for performance benchmark and adjusted Cloud Function configuration.
  - Managed all GCP infrastructure as code with Terraform scripts, parameterized all cloud functions, automated everything and enabled auto-scaling.
  - Designed and built the CI/CD pipelines with Cloud Source Repository, Cloud Build and GCP Container Registry with Docker and YAML; set up the triggers for the test pipelines and prod pipeline; configured multiple test pipelines can run concurrently.
  - Worked on Dataflow pipelines with Apache Beam SDK for Java and Maven.
  - Handed over high quality detailed runbook documentation

- Designed solutions to migrate on premise applications to GCP for a large pharmacy client

  - Discussed with business owners and other vendors to plan the detailed runbook and matrices in both high level and low level.
  - Moved all Informatica applications (MDM, WildFly, EDC, IDQ, AXON) from on premise to Cloud (OpenShift and Google Cloud Platform) and NoSQL BigQuery migration.

- Created many Data Science and Machine Learning POCs with GCP Machine Learning APIs (NLP, Cloud Vision, DialogFlow etc.), Jupyter Notebook and Python.

- Designed, developed, deployed and managed Microservices Java RESTful APIs to OpenShift and GCP for a large telecommunication company
  - Used Stoplight studio to design new APIs (OAS 2 & 3 YAML) and upgrade the old SOAP web services (with WebLogic) to RESTful with Java, Spring Boot and Maven.
  - For a new REST API project, used Kotlin and Gradle to develop the APIs, and Terraform, Docker and Spinnaker to build the pipelines and deploy to GCP.
  - Created the old and new API mapping document for upstream callers; composed the downstream APIs for internal usage; designed API changes to work with external vendors and downstream providers.
  - Utilized Swagger Editor and Swagger Maven plugin to create Java client classes.
  - Implemented Application Gateway BFF pattern to onboard new APIs and used OAuth2 tokens and scopes on enhance the security controls.
  - Built the front end applications with Angular, HTML5 and CSS 3; used code validators to validate all HTML to ensure WCAG 2.0 standard were followed.
  - Managed source file version control with Azure Repos; managed cloud-based identity and access management (IAM) with Azure AD.
  - Used Tekton and Jenkins to build CI/CD OpenShift pipelines with Git Azure DevOps.
  - Managed Microservices Java Spring Boot applications with RedHat OpenShift; Configured OpenShift to auto-scaling (up and down as needed).
  - Created Helm charts to manage Kubernetes resources and deploy applications in OpenShift; Updated and rolled back packages to control new versions.
  - Used Service Discovery and Load Balancing to split traffics for A/B testing, Blue/Green and Canary deployments.
  - Monitored applications with Prometheus graphs and Grafana dashboards.
  - Planned architecture changes for moving everything to Google Cloud Platform, such as migrating Oracle to NoSQL BigQuery.
  - Documented everything in the share drive for knowledge sharing.

Exercises and practice:

- Hands-on labs with Qwiklabs.

## GCP networks

In my GCP projects, there are lot of network / firewall / connectivity issues.

[GCP_Network](GCP_Network.md)

## Dataflow jobs

Java

Python

Cloud Build

YAML

[GCP_Dataflow](GCP_Dataflow.md)

## Eventarc + Cloud Run + Workflow + Secret Manager

These are new tools and solutions for new projects.

### Some use cases

For the text files uploaded to GCP buckets, use Eventarc + Cloud Run + Workflow + OpenSource Clam to scan and verify if they are safe.

For the images uploaded to GCP buckets, use Eventarc + Cloud Run + Workflow + Vision API to scan and verify if they are safe.

## GCE GKE GAE GCF

Google Cloud Platform (GCP) offers multiple services like Google Compute Engine (GCE), Google Kubernetes Engine (formerly Container Engine) (GKE), Google App Engine (GAE) and Google Cloud Functions (GCF).

## GCP Architect

![](GCP-Architect/001.png)

![](GCP-Architect/002.png)

![](GCP-Architect/003.png)

![](GCP-Architect/004.png)

![](GCP-Architect/005.png)

![](GCP-Architect/006.png)

![](GCP-Architect/007.png)

![](GCP-Architect/008.png)

![](GCP-Architect/009.png)

![](GCP-Architect/010.png)

![](GCP-Architect/011.png)

![](GCP-Architect/012.png)

![](GCP-Architect/013.png)

![](GCP-Architect/014.png)

![](GCP-Architect/015.png)

![](GCP-Architect/016.png)

![](GCP-Architect/017.png)

![](GCP-Architect/018.png)

![](GCP-Architect/019.png)

![](GCP-Architect/020.png)

![](GCP-Architect/021.png)

Dataproc vs Dataflow

![](GCP-Architect/022.png)

Kafka vs PubSub

![](GCP-Architect/023.png)

IAM best practice

![](GCP-Architect/024.png)

Cloud KMS

![](GCP-Architect/025.png)

![](GCP-Architect/026.png)

![](GCP-Architect/027.png)

![](GCP-Architect/028.png)

## GCP CI/CD pipeline

![](image/README/gcp_cicd_01.png)

- Developer checks in the source code to a Version Control system such as GitHub
- GitHub triggers a post-commit hook to Cloud Build.
- Cloud Build builds the container image and pushes to Container Registry.
- Cloud Build then notifies Cloud Run to redeploy
- Cloud Run pulls the latest image from the Container Registry and runs it.

## BigQuery

### Dremel

Dremel is a distributed system developed at Google for interactively querying large datasets.

Dremel is the query engine used in Google's BigQuery service.

Dremel is the inspiration for Apache Drill, Apache Impala, and Dremio, an Apache licensed platform that includes a distributed SQL execution engine.

## Best Practice for BQ

![](image/README/best_practice_01.png)

![](image/README/best_practice_02.png)

## Cloud Build

[GCP_CloudBuild](GCP_CloudBuild.md)
