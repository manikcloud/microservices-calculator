# microservices-calculator / Manik Calculator - A Java Spring Application

## Introduction

This Java Spring-based web application showcases a basic calculator. It is part of a larger ecosystem that involves automated CI/CD pipelines hosted in another repository. This README outlines each directory and file's purpose and provides a brief overview of how to get started with the project.

## Features

- **Calculator Functionality**: Perform basic calculations like addition, subtraction, multiplication, and division.
- **Spring MVC**: Built on the Spring Framework using the MVC architecture.
- **Java**: Contains the Java source code files, including the controller and its tests.
- **JSP**: Utilizes JavaServer Pages (JSP) for creating dynamic views.
- **Maven**: Uses Maven for dependency management and build lifecycle.

## How it Works

1. **Java Controller**: `CalculatorController.java` handles the HTTP request-response lifecycle.
2. **Spring Configuration**: `application-context.xml` contains Spring configurations.
3. **View Layer**: JSP files located under `WEB-INF/views/` serve as the view component.
4. **CSS**: Stylesheets are located in the `webapp` directory as `styles.css`.

## CI/CD Integration 

When code is pushed to this repository, it triggers the CI/CD pipeline hosted in [AzureK8s-CICD-Flow](https://github.com/manikcloud/AzureK8s-CICD-Flow). The pipeline takes care of building the application, running tests, and deploying it to the Kubernetes cluster.

### Linked Repositories

1. **Infrastructure**: The [Azure-k8s-infra-ops](https://github.com/manikcloud/Azure-k8s-infra-ops/) repository contains Terraform scripts to provision the infrastructure components like VNet, AKS, and ACR in Azure.
  
2. **CI/CD Pipeline**: The [AzureK8s-CICD-Flow](https://github.com/manikcloud/AzureK8s-CICD-Flow) repository hosts the CI/CD pipeline, which builds, tests, and deploys this application.

### Development Story

The development process starts with infrastructure setup using [Azure-k8s-infra-ops](https://github.com/manikcloud/Azure-k8s-infra-ops/). Once the infrastructure is provisioned, code pushes to this repository trigger the CI/CD pipeline in [AzureK8s-CICD-Flow](https://github.com/manikcloud/AzureK8s-CICD-Flow).

## Getting Started

### Prerequisites

- Java Development Kit (JDK)
- Maven

### Clone the Repository

```bash
git clone your-repository-link
```
### Build the Project
Navigate to the root directory and run:

```
mvn clean install
```
### Run the Application
```


mvn spring-boot:run
```
### Access the Web Application

Open your browser and go to http://localhost:8080.

### Directory and File Summary
`Dockerfile`: Configuration file to containerize the application.
`pom.xml`: Maven build file with project dependencies and plugins.
`src`: The source code directory.
`main`: Contains the application's main source code.
`java`: Houses the Java classes for the application logic.
`webapp`: Resources for the web layer, including JSPs and CSS.
`test`: Contains the test code for the application.
