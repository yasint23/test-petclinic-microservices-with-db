# Project 503: Microservices CI/CD Pipeline

## Description

This project aims to create full CI/CD Pipeline for microservice based applications using [Spring Petclinic Microservices Application](https://github.com/spring-petclinic/spring-petclinic-microservices). Jenkins Server deployed on Elastic Compute Cloud (EC2) Instance is used as CI/CD Server to build pipelines.

## DevOps Pipelines

### Development Diagram

![Development Diagram](./project-503-dev-diagram.png)

### Pipelines Configurations

![Pipelines to be configured](./project-503-pipelines.png)

![Project Structure](./project_structure.png)

## Flow of Tasks for Project Realization

| Epic | Task  | Task #  | Task Definition   | Branch  |
| ---   | :---  | ---                  | :---              | :---    |
| Local Development Environment | Prepare Development Server Manually on EC2 Instance| MSP-1 | Prepare development server manually on Amazon Linux 2 for developers, enabled with `Docker` , `Docker-Compose` , `Java 11` , `Git` .  |
| Local Development Environment | Prepare GitHub Repository for the Project | MSP-2-1 | Clone the Petclinic app from the Clarusway repository [Petclinic Microservices Application](https://github.com/clarusway/petclinic-microservices.git) |
| Local Development Environment | Prepare GitHub Repository for the Project | MSP-2-2 | Prepare base branches namely `main` , `dev` , `release` for DevOps cycle. |
| Local Development Environment | Check the Maven Build Setup on Dev Branch | MSP-3 | Check the Maven builds for `test` , `package` , and `install` phases on `dev` branch |
| Local Development Environment | Prepare a Script for Packaging the Application | MSP-4 |  Prepare a script to package the application with Maven wrapper | feature/msp-4 |
| Local Development Environment | Prepare Development Server Cloudformation Template | MSP-5 |  Prepare development server script with Cloudformation template for developers, enabled with `Docker` , `Docker-Compose` , `Java 11` , `Git` . | feature/msp-5 |
| Local Development Build | Prepare Dockerfiles for Microservices | MSP-6 | | Prepare Dockerfiles for each microservices. | feature/msp-6 |
| Local Development Environment | Prepare Script for Building Docker Images | MSP-7 |  Prepare a script to package and build the docker images for all microservices. | feature/msp-7 |
| Local Development Build | Create Docker Compose File for Local Development | MSP-8-1 |  Prepare docker compose file to deploy the application locally. | feature/msp-8 |
| Local Development Build | Create Docker Compose File for Local Development | MSP-8-2 |  Prepare a script to test the deployment of the app locally. | feature/msp-8 |
| Testing Environment Setup | Implement Unit Tests | MSP-9-1  | Implement 3 Unit Tests locally. | feature/msp-9 |
| Testing Environment Setup | Setup Code Coverage Tool | MSP-9-2  | Update POM file for Code Coverage Report. | feature/msp-9 |
| Testing Environment Setup | Implement Code Coverage | MSP-9-3  | Generate Code Coverage Report manually. | feature/msp-9 |
| Testing Environment Setup | Prepare Selenium Tests | MSP-10-1  | Prepare 3 Selenium Jobs for QA Automation Tests. | feature/msp-10 |
| Testing Environment Setup | Implement Selenium Tests | MSP-10-2  | Run 3 Selenium Tests against local environment. | feature/msp-10 |
| CI Server Setup | Prepare Jenkins Server | MSP-11 | | Prepare Jenkins Server for CI/CD Pipeline. | feature/msp-11 |
| CI Server Setup | Configure Jenkins Server for Project | MSP-12  | Configure Jenkins Server for Project Setup. | |
| CI Server Setup | Prepare CI Pipeline | MSP-13 | | Prepare CI pipeline (UT only) for all `dev` , `feature` and `bugfix` branches. | feature/msp-13 |
| Registry Setup for Development | Create Docker Registry for Dev Manually | MSP-14  | Create Docker Registry on AWS ECR manually using Jenkins job. | |
| Registry Setup for Development | Prepare Script for Docker Registry| MSP-15  | Prepare a script to create Docker Registry on AWS ECR using Jenkins job. | feature/msp-15 |
| QA Automation Setup for Development | Create a QA Automation Environment | MSP-16  | Create a QA Automation Environment with Docker Swarm. | feature/msp-16 |
| QA Automation Setup for Development | Prepare a QA Automation Pipeline | MSP-17  | Prepare a QA Automation Pipeline on `dev` branch for Nightly Builds. | feature/msp-17 |
| QA Setup for Release | Create a Key Pair for QA Environment | MSP-18-1  | Create a permanent Key Pair for Ansible to be used in QA Environment on Release. | feature/msp-18 |
| QA Setup for Release | Create a QA Infrastructure with AWS Cloudformation | MSP-18-2  | Create a Permanent QA Infrastructure for Docker Swarm with AWS Cloudformation. | feature/msp-18 |
| QA Setup for Release | Create a QA Environment with Docker Swarm | MSP-18-3  | Create a QA Environment for Release with Docker Swarm. | feature/msp-18 |
| QA Setup for Release | Prepare Build Scripts for QA Environment | MSP-19  | Prepare Build Scripts for creating ECR Repo for QA, building QA Docker images, deploying app with Docker Compose. | feature/msp-19 |
| QA Setup for Release | Build and Deploy App on QA Environment Manually | MSP-20  | Build and Deploy App for QA Environment Manually using Jenkins Jobs. | feature/msp-20 | 
| QA Setup for Release | Prepare a QA Pipeline | MSP-21  | Prepare a QA Pipeline using Jenkins on `release` branch for Weekly Builds. | feature/msp-21 |
| Staging and Production Setup | Prepare Petlinic Kubernetes YAML Files | MSP-22  | Prepare Petlinic Kubernetes YAML Files for Staging and Production Pipelines. | feature/msp-22 |
| Staging and Production Setup | Prepare HA RKE Kubernetes Cluster | MSP-23  | Prepare High-availability RKE Kubernetes Cluster on AWS EC2 | feature/msp-23 |
| Staging and Production Setup | Install Rancher App on RKE K8s Cluster | MSP-24  | Install Rancher App on RKE Kubernetes Cluster| |
| Staging and Production Setup | Create Staging and Production Environment with Rancher | MSP-25  | Create Staging and Production Environment with Rancher by creating new cluster for Petclinic | |
| Staging Deployment Setup | Prepare a Staging Pipeline | MSP-26  | Prepare a Staging Pipeline on Jenkins Server | feature/msp-26|
| Production Deployment Setup | Prepare a Production Pipeline | MSP-27  | Prepare a Production Pipeline on Jenkins Server | feature/msp-27|
| Production Deployment Setup | Set Domain Name and TLS for Production | MSP-28  | Set Domain Name and TLS for Production Pipeline with Route 53 | feature/msp-28|
| Production Deployment Setup | Set Monitoring Tools | MSP-29  | Set Monitoring tools, Prometheus and Grafana | |

## MSP 1 - Prepare Development Server Manually on EC2 Instance

* Prepare development server manually on Amazon Linux 2 (t3a.medium) for developers, enabled with `Docker`,  `Docker-Compose`,  `Java 11`,  `Git`.

``` bash
#! /bin/bash
sudo yum update -y
sudo hostnamectl set-hostname petclinic-dev-server
sudo amazon-linux-extras install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user   
newgrp docker
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" \
-o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo yum install git -y
sudo yum install java-11-amazon-corretto -y
```

## MSP 2 - Prepare GitHub Repository for the Project

* Connect to your Development Server via `ssh` and clone the petclinic app from the repository [Spring Petclinic Microservices App](https://github.com/clarusway/petclinic-microservices-with-db.git).

* Root dan cikarak;

``` bash
git clone https://github.com/clarusway/petclinic-microservices-with-db.git
```

* Change your working directory to **petclinic-microservices** and delete the **.git** directory.

```bash
cd petclinic-microservices-with-db
rm -rf .git
```

* Create a new repository on your Github account with the name **test-petclinic-microservices-with-db**.

*  Initiate the cloned repository to make it a git repository and push the local repository to your remote repository.

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://`token`@github.com/yasint23/test-petclinic-microservices-with-db.git
git push origin main
```
* Prepare base branches namely `main`,  `dev`,  `release` for DevOps cycle.

  + Create `dev` base branch.

    ``` bash
    git checkout main
    git branch dev
    git checkout dev
    git push --set-upstream origin dev
    ```

  + Create `release` base branch.

    ```bash
    git checkout main
    git branch release
    git checkout release
    git push --set-upstream origin release
    ```

## MSP 3 - Check the Maven Build Setup on Dev Branch

# Maven lifecylcle bakilabilir bu bolumdeki komutlar ile alakali; https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html

* Switch to `dev` branch.

``` bash
git checkout dev
```

* Test the compiled source code.

``` bash
./mvnw clean test
```
> Note: If you get `permission denied` error, try to give execution permission to **mvnw**.  

    chmod +x mvnw
  

* Take the compiled code and package it in its distributable `JAR` format.

``` bash
./mvnw clean package
```

* Install distributable `JAR`s into local repository.

``` bash
./mvnw clean install
```
## MSP 4 - Prepare Development Server Cloudformation Template

* Create `feature/msp-5` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-5
git checkout feature/msp-5
```

* Create a folder for infrastructure setup with the name of `infrastructure`.

``` bash
mkdir infrastructure
```

* Prepare development server script with [Cloudformation template](./msp-5-dev-server-for-petclinic-app-cfn-template.yml) for developers, enabled with `Docker`,  `Docker-Compose`,  `Java 11`,  `Git` and save it as `dev-server-for-petclinic-app-cfn-template.yml` under `infrastructure` folder.

* Bu yml dosyasini microservis-ci-cd... folder icersinden al, asagidaki sadece userdata kismi.

``` bash
#! /bin/bash
yum update -y
hostnamectl set-hostname petclinic-dev-server
...

* Commit and push the new script to remote repo.

``` bash
git add .
git commit -m 'added cloudformation template for dev server'
git push --set-upstream origin feature/msp-5
git checkout dev
git merge feature/msp-5
git push origin dev
```

## MSP 5 - Prepare Dockerfiles for Microservices

* Create `feature/msp-6` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-6
git checkout feature/msp-6
```

* Prepare a Dockerfile for the `admin-server` microservice with following content and save it under `spring-petclinic-admin-server`.

``` Dockerfile
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=9090
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

* Prepare a Dockerfile for the `api-gateway` microservice with the following content and save it under `spring-petclinic-api-gateway`.

``` Dockerfile
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=8080
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

* Prepare a Dockerfile for the `config-server` microservice with the following content and save it under `spring-petclinic-config-server`.

``` Dockerfile
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=8888
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

* Prepare a Dockerfile for the `customer-service` microservice with the following content and save it under `spring-petclinic-customer-service`.

``` Dockerfile
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=8081
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

* Prepare a Dockerfile for the `discovery-server` microservice with the following content and save it under `spring-petclinic-discovery-server`.

``` Dockerfile
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=8761
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

* Prepare a Dockerfile for the `hystrix-dashboard` microservice with the following content and save it under `spring-petclinic-hystrix-dashboard`.

``` Dockerfile
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=7979
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

* Prepare a Dockerfile for the `vets-service` microservice with the following content and save it under `spring-petclinic-vets-service`.

``` Dockerfile
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=8083
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

* Prepare a Dockerfile for the `visits-service` microservice with the following content and save it under `spring-petclinic-visits-service`.

``` Dockerfile
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=8082
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

* Commit the changes, then push the Dockerfiles to the remote repo.

``` bash
git add .
git commit -m 'added Dockerfiles for microservices'
git push --set-upstream origin feature/msp-6
git checkout dev
git merge feature/msp-6
git push origin dev
```

## MSP 6 - Prepare Script for Building Docker Images

* Create `feature/msp-7` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-7
git checkout feature/msp-7
```

* Prepare a script to build the docker images and save it as `build-dev-docker-images.sh` under `petclinic-microservices-with-db` folder.

``` bash
./mvnw clean package
docker build --force-rm -t "petclinic-admin-server:dev" ./spring-petclinic-admin-server
docker build --force-rm -t "petclinic-api-gateway:dev" ./spring-petclinic-api-gateway
docker build --force-rm -t "petclinic-config-server:dev" ./spring-petclinic-config-server
docker build --force-rm -t "petclinic-customers-service:dev" ./spring-petclinic-customers-service
docker build --force-rm -t "petclinic-discovery-server:dev" ./spring-petclinic-discovery-server
docker build --force-rm -t "petclinic-hystrix-dashboard:dev" ./spring-petclinic-hystrix-dashboard
docker build --force-rm -t "petclinic-vets-service:dev" ./spring-petclinic-vets-service
docker build --force-rm -t "petclinic-visits-service:dev" ./spring-petclinic-visits-service
docker build --force-rm -t "petclinic-grafana-server:dev" ./docker/grafana
docker build --force-rm -t "petclinic-prometheus-server:dev" ./docker/prometheus
```

* Give execution permission to build-dev-docker-images.sh. 

```bash
chmod +x build-dev-docker-images.sh
```

* Build the images.

```bash
./build-dev-docker-images.sh
```

* Commit the changes, then push the new script to the remote repo.

``` bash
git add .
git commit -m 'added script for building docker images'
git push --set-upstream origin feature/msp-7
git checkout dev
git merge feature/msp-7
git push origin dev
```

## MSP 7 - Create Docker Compose File for Local Development

* Create `feature/msp-8` branch from `dev`.

``` bash
git branch feature/msp-8
git checkout feature/msp-8
```

* Prepare docker compose file to deploy the application locally and save it as `docker-compose-local.yml` under `petclinic-microservices-with-db` folder.

``` yaml
version: '2'

services: 
  config-server:
    image: petclinic-config-server:dev
    container_name: config-server
    mem_limit: 512M
    ports: 
      - 8888:8888

  discovery-server:
    image: petclinic-discovery-server:dev
    container_name: discovery-server
    mem_limit: 512M
    ports: 
      - 8761:8761
    depends_on: 
      - config-server
    entrypoint: ["./dockerize", "-wait=tcp://config-server:8888", "-timeout=160s", "--", "java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]

  customers-service:
    image: petclinic-customers-service:dev
    container_name: customers-service
    mem_limit: 512M
    ports:
     - 8081:8081
    depends_on: 
     - config-server
     - discovery-server
    entrypoint: ["./dockerize", "-wait=tcp://discovery-server:8761", "-timeout=160s", "--", "java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar" ]
  
  visits-service:
    image: petclinic-visits-service:dev
    container_name: visits-service
    mem_limit: 512M
    ports:
     - 8082:8082
    depends_on: 
     - config-server
     - discovery-server
    entrypoint: ["./dockerize", "-wait=tcp://discovery-server:8761", "-timeout=160s", "--", "java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar" ]
  
  vets-service:
    image: petclinic-vets-service:dev
    container_name: vets-service
    mem_limit: 512M
    ports:
     - 8083:8083
    depends_on: 
     - config-server
     - discovery-server
    entrypoint: ["./dockerize", "-wait=tcp://discovery-server:8761", "-timeout=160s", "--", "java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar" ]
  
  api-gateway:
    image: petclinic-api-gateway:dev
    container_name: api-gateway
    mem_limit: 512M
    ports:
     - 8080:8080
    depends_on: 
     - config-server
     - discovery-server
    entrypoint: ["./dockerize", "-wait=tcp://discovery-server:8761", "-timeout=160s", "--", "java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar" ]
  
  admin-server:
    image: petclinic-admin-server:dev
    container_name: admin-server
    mem_limit: 512M
    ports:
     - 9090:9090
    depends_on: 
     - config-server
     - discovery-server
    entrypoint: ["./dockerize", "-wait=tcp://discovery-server:8761", "-timeout=160s", "--", "java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar" ]

  hystrix-dashboard:
    image: petclinic-hystrix-dashboard:dev
    container_name: hystrix-dashboard
    mem_limit: 512M
    ports:
     - 7979:7979
    depends_on: 
     - config-server
     - discovery-server
    entrypoint: ["./dockerize", "-wait=tcp://discovery-server:8761", "-timeout=160s", "--", "java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar" ]

  tracing-server:
    image: openzipkin/zipkin
    container_name: tracing-server
    mem_limit: 512M
    environment:
    - JAVA_OPTS=-XX:+UnlockExperimentalVMOptions -Djava.security.egd=file:/dev/./urandom
    ports:
     - 9411:9411 
  
  grafana-server:
    image: petclinic-grafana-server:dev
    container_name: grafana-server
    mem_limit: 256M
    ports:
    - 3000:3000

  prometheus-server:
    image: petclinic-prometheus-server:dev
    container_name: prometheus-server
    mem_limit: 256M
    ports:
    - 9091:9090

  mysql-server:
    image: mysql:5.7.8
    container_name: mysql-server
    environment: 
      MYSQL_ROOT_PASSWORD: petclinic
      MYSQL_DATABASE: petclinic
    mem_limit: 256M
    ports:
    - 3306:3306
```
``` bash
docker-compose -f docker-compose-local.yml up
```
* Check the website running on web browser and open the sec-grp 8080 to everywhere.

* Commit the change, then push the docker compose file to the remote repo.

``` bash
git add .
git commit -m 'added docker-compose file'
git push --set-upstream origin feature/msp-8
git checkout dev
git merge feature/msp-8
git push origin dev
```

## MSP 8 - Setup Unit Tests and Configure Code Coverage Report

* Create `feature/msp-9` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-9
git checkout feature/msp-9
```

* Create following unit tests for `Pet.java` under `customer-service` microservice using the following `PetTest` class and save it as `PetTest.java` under `./spring-petclinic-customers-service/src/test/java/org/springframework/samples/petclinic/customers/model/` folder.

* Create "model" folder under customers and create "PetTest.java" file. (/src/main/java/model seklinde olan ayni dizini test altinda da olusturarak unit testlerimizi yapiyoruz)

``` java
package org.springframework.samples.petclinic.customers.model;

import static org.junit.jupiter.api.Assertions.assertEquals;

import java.util.Date;

import org.junit.jupiter.api.Test;
public class PetTest {
    @Test
    public void testGetName(){
        //Arrange
        Pet pet = new Pet();
        //Act
        pet.setName("Fluffy");
        //Assert
        assertEquals("Fluffy", pet.getName());
    }
    @Test
    public void testGetOwner(){
        //Arrange
        Pet pet = new Pet();
        Owner owner = new Owner();
        owner.setFirstName("Call");
        //Act
        pet.setOwner(owner);
        //Assert
        assertEquals("Call", pet.getOwner().getFirstName());
    }
    @Test
    public void testBirthDate(){
        //Arrange
        Pet pet = new Pet();
        Date bd = new Date();
        //Act
        pet.setBirthDate(bd);
        //Assert
        assertEquals(bd,pet.getBirthDate());
    }
}
```

* Implement unit tests with maven wrapper for only `customer-service` microservice locally on `Dev Server`. Execute the following command under the `spring-petclinic-customers-service folder`.

``` bash
../mvnw clean test
```
## iki nokta yazmamizin sebebi mvnw dosyasi bir ust klasordeki mwnw dosyasini calistir diyoruz. Customer service icinde run ediyoruz aksi halde butun test file lari test edecekti, biz sadece customer altindaki 4 unit testi calistiriyoruz. Ancak mvnw ust klasorde. 

* Commit the change, then push the changes to the remote repo.

``` bash
git add .
git commit -m 'added 3 UTs for customer-service'
git push --set-upstream origin feature/msp-9
```

* Update POM file at root folder for Code Coverage Report using `Jacoco` tool plugin. 
# testler gercek uygulamanin % kacini kapsadigini kiyaslamak icin bu tool kullaniliyor.

* Copy the below to the 'pom' file under root folder,
``` 
                        </configuration>
                    </plugin>
                   # Copy to here.... 
                </plugins>
            </build> 
```
``` xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.2</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <!-- attached to Maven test phase -->
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

* Create code coverage report for only `customer-service` microservice locally on `Dev Server`. Execute the following command under the `spring-petclinic-customers-service folder`.

``` bash
../mvnw test

* Commit the change, then push the changes to the remote repo.

``` bash
git add .
git commit -m 'updated POM with Jacoco plugin'
git push
git checkout dev
git merge feature/msp-9
git push origin dev
```

* Deploy code coverage report (located under relative path `target/site/jacoco` of the microservice) on Simple HTTP Server for only `customer-service` microservice on `Dev Server`.

* '/site/jacoco/' folder created under /...customer service/target and if you run the command which will run, you can check from the browser (http://localhost:8080) the test details.

``` bash
python -m SimpleHTTPServer # for python 2.7
python3 -m http.server # for python 3+
```

## MSP 10 - Prepare and Implement Selenium Tests

* Create `feature/msp-10` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-10
git checkout feature/msp-10
```

* Create a folder for Selenium jobs with the name of `selenium-jobs` under 'petclinic-microservis-with-db'

``` bash
mkdir selenium-jobs
```

* Create Selenium job (`QA Automation` test) for testing `Owners >> All` page and save it as `test_owners_all_headless.py` under `selenium-jobs` folder.

``` python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from time import sleep
import os

# Set chrome options for working with headless mode (no screen)
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("headless")
chrome_options.add_argument("no-sandbox")
chrome_options.add_argument("disable-dev-shm-usage")

# Update webdriver instance of chrome-driver with adding chrome options
driver = webdriver.Chrome(options=chrome_options)
# driver = webdriver.Chrome("/Users/home/Desktop/chromedriver")
# Connect to the application
APP_IP = os.environ['MASTER_PUBLIC_IP']
url = "http://"+APP_IP.strip()+":8080/"
# url = "http://localhost:8080"
print(url)
driver.get(url)
sleep(3)
owners_link = driver.find_element_by_link_text("OWNERS")
owners_link.click()
sleep(2)
all_link = driver.find_element_by_link_text("ALL")
all_link.click()
sleep(2)

# Verify that table loaded
sleep(1)
verify_table = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.TAG_NAME, "table")))

print("Table loaded")

driver.quit()
```

* Create Selenium job (`QA Automation` test) for testing `Owners >> Register` page and save it as `test_owners_register_headless.py` under `selenium-jobs` folder.

``` python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from time import sleep
import random
import os
# Set chrome options for working with headless mode (no screen)
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("headless")
chrome_options.add_argument("no-sandbox")
chrome_options.add_argument("disable-dev-shm-usage")

# Update webdriver instance of chrome-driver with adding chrome options
driver = webdriver.Chrome(options=chrome_options)

# Connect to the application
APP_IP = os.environ['MASTER_PUBLIC_IP']
url = "http://"+APP_IP.strip()+":8080/"
print(url)
driver.get(url)
owners_link = driver.find_element_by_link_text("OWNERS")
owners_link.click()
sleep(2)
all_link = driver.find_element_by_link_text("REGISTER")
all_link.click()
sleep(2)
# Register new Owner to Petclinic App
fn_field = driver.find_element_by_name('firstName')
fn = 'Callahan' + str(random.randint(0, 100))
fn_field.send_keys(fn)
sleep(1)
fn_field = driver.find_element_by_name('lastName')
fn_field.send_keys('Clarusway')
sleep(1)
fn_field = driver.find_element_by_name('address')
fn_field.send_keys('Ridge Corp. Street')
sleep(1)
fn_field = driver.find_element_by_name('city')
fn_field.send_keys('McLean')
sleep(1)
fn_field = driver.find_element_by_name('telephone')
fn_field.send_keys('+1230576803')
sleep(1)
fn_field.send_keys(Keys.ENTER)

# Wait 10 seconds to get updated Owner List
sleep(10)
# Verify that new user is added to Owner List
if fn in driver.page_source:
    print(fn, 'is added and found in the Owners Table')
    print("Test Passed")
else:
    print(fn, 'is not found in the Owners Table')
    print("Test Failed")
driver.quit()
```

* Create Selenium job (`QA Automation` test) for testing `Veterinarians` page and save it as `test_veterinarians_headless.py` under `selenium-jobs` folder.

``` python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from time import sleep
import os

# Set chrome options for working with headless mode (no screen)
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("headless")
chrome_options.add_argument("no-sandbox")
chrome_options.add_argument("disable-dev-shm-usage")

# Update webdriver instance of chrome-driver with adding chrome options
driver = webdriver.Chrome(options=chrome_options)

# Connect to the application
APP_IP = os.environ['MASTER_PUBLIC_IP']
url = "http://"+APP_IP.strip()+":8080/"
print(url)
driver.get(url)
sleep(3)
vet_link = driver.find_element_by_link_text("VETERINARIANS")
vet_link.click()

# Verify that table loaded
sleep(5)
verify_table = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.TAG_NAME, "table")))

print("Table loaded")

driver.quit()
```

* Commit the change, then push the selenium jobs to the remote repo.

``` bash
git add .
git commit -m 'added selenium jobs written in python'
git push --set-upstream origin feature/msp-10
git checkout dev
git merge feature/msp-10
git push origin dev
```

## MSP 11 - Prepare Jenkins Server for CI/CD Pipeline

* Create `feature/msp-11` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-11
git checkout feature/msp-11
```

* Set up a Jenkins Server and enable it with `Git`,  `Docker`,  `Docker Compose`,  `AWS CLI v2`, `Python`,  `Ansible` and `Boto3`.  To do so, prepare a [Cloudformation template for Jenkins Server](./msp-11-jenkins-server-cfn-template.yml) with following script and save it as `jenkins-server-cfn-template.yml` under `infrastructure` folder.

* Get it from repo.
``` bash
#! /bin/bash
# update os
yum update -y
# set server hostname as jenkins-server

```
* Grant permissions to Jenkins Server within Cloudformation template to create Cloudformation Stack and create ECR Registry, push or pull Docker images to ECR Repo.

* Commit the change, then push the cloudformation file to the remote repo.

``` bash
git add .
git commit -m 'added jenkins server cfn template'
git push --set-upstream origin feature/msp-11
git checkout dev
git merge feature/msp-11
git push origin dev
```

## MSP 12 - Configure Jenkins Server for Project

* Get the initial administrative password.

``` bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

* Enter the temporary password to unlock the Jenkins.

* Install suggested plugins.

* Create first admin user.

* Open your Jenkins dashboard and navigate to `Manage Jenkins` >> `Manage Plugins` >> `Available` tab

* Search and select `GitHub Integration`,  `Docker Plugin`,  `Docker Pipeline`, and `Jacoco` plugins, then click `Install without restart`. Note: No need to install the other `Git plugin` which is already installed can be seen under `Installed` tab.

# Jenkins de yuksek CPU gerektiren uygulamalar jenkins server yormasin diye slave node lari agent olarak kullanabiliyorduk. Burada da jenkins server 2375 portundan docker deamon dinleyecek ve jenkins jab lari docker container icerisinde kullanabilmis olacagiz.
# Bu ayari jenkins cfn template deki, sed -i 's/^ExecStart=.*/ExecStart=\/usr\/bin\/doc -H tcp:\/\/127.0.0.1:2375... komutu ile yaptik. Simdi de Jenkins server uzerinde docker cloud agent olarak atayacagiz.

* Configure Docker as `cloud agent` by navigating to `Manage Jenkins` >> `Manage Nodes and Clouds` >> `Configure Clouds`  and using `tcp://localhost:2375` as Docker Host URI.  

* If you want to check how it is work from this link 'https://www.jenkins.io/doc/book/pipeline/docker/' copy the Jenkinsfile (Declarative Pipeline) then "New Item" give name "test docker agent' --> freestyle and paste to the "shell" --> apply/save --> build now.

# Simdi onceki serverdaki belgeleri guthub dan jenkins server'a cekerek onceki server kapatacagiz gereksiz ucret yazmasin, artik jenjkins server ile devam edecegiz.
- Github hesabindan repoyu jenkins server'a clone yapcagiz.
```bash
git clone https://github.com/yasint23/test-petclinic-microservices-with-db.git
```


## MSP 13 - Prepare Continuous Integration (CI) Pipeline

* Create `feature/msp-13` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-13
git checkout feature/msp-13
```

* Create a folder, named `jenkins`, to keep `Jenkinsfiles` and `Jenkins jobs` of the project.

``` bash
mkdir jenkins
```
* Create a Jenkins job with the name of `petclinic-ci-job`: 
  * Select `Freestyle project` and click `OK`
  * Select github project and write the url to your repository's page into `Project url` (https://github.com/[your-github-account]/petclinic-microservices)
  * Under the `Source Code Management` select `Git` 
  * Write the url of your repository into the `Repository URL` (https://github.com/[your-github-account]/petclinic-microservices.git)
  * Add `*/dev`, `*/feature**` and `*/bugfix**` branches to `Branches to build`
  * Select `GitHub hook trigger for GITScm polling` under  `Build triggers`
  * Select `Add timestamps to the Console Output` under `Build Environment`
  * Click `Add build step` under `Build` and select `Execute Shell`
  * Write below script into the `Command`
    ```bash
    echo 'Running Unit Tests on Petclinic Application'
    docker run --rm -v $HOME/.m2:/root/.m2 -v `pwd`:/app -w /app maven:3.6-openjdk-11 mvn clean test
    ```
  * Click `Add post-build action` under `Post-build Actions` and select `Record jacoco coverage report`
  * Click `Save`
  
* Jenkins `CI Job` should be triggered to run on each commit of `feature**` and `bugfix**` branches and on each `PR` merge to `dev` branch.

* Prepare a script for Jenkins CI job (covering Unit Test only) and save it as `jenkins-petclinic-ci-job.sh` under `jenkins` folder.

``` bash
echo 'Running Unit Tests on Petclinic Application'
docker run --rm -v $HOME/.m2:/root/.m2 -v `pwd`:/app -w /app maven:3.8-openjdk-11 mvn clean test
```
* Create a webhook for Jenkins CI Job; 

  + Go to the project repository page and click on `Settings`.

  + Click on the `Webhooks` on the left hand menu, and then click on `Add webhook`.

  + Copy the Jenkins URL, paste it into `Payload URL` field, add `/github-webhook/` at the end of URL, and click on `Add webhook`.
  
  ``` text
  http://[jenkins-server-hostname]:8080/github-webhook/
  ```

* Commit the change, then push the Jenkinsfile to the remote repo.

``` bash
git add .
git commit -m 'added Jenkins Job for CI pipeline'
git push --set-upstream origin feature/msp-13
git checkout dev
git merge feature/msp-13
git push origin dev
```
## MSP 13 - Prepare Continuous Integration (CI) Pipeline

* Create `feature/msp-13` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-13
git checkout feature/msp-13
```

* Create a folder, named `jenkins`, to keep `Jenkinsfiles` and `Jenkins jobs` of the project.

``` bash
mkdir jenkins
```
* Create a Jenkins job with the name of `petclinic-ci-job`:  (developer lar jenkins uzerinde unit testlerini yapiyorlar bu adimda)
  * Select `Freestyle project` and click `OK`
  * Select github project and write the url to your repository's page into `Project url` (https://github.com/yasint23/test-petclinic-microservices-with-db)
  * Under the `Source Code Management` select `Git` 
  * Write the url of your repository into the `Repository URL` (https://github.com/[your-github-account]/petclinic-microservices.git)
  * Add `*/dev`, `*/feature**` and `*/bugfix**` branches to `Branches to build`
  * Select `GitHub hook trigger for GITScm polling` under  `Build triggers`
  * Select `Add timestamps to the Console Output` under `Build Environment`
  * Click `Add build step` under `Build` and select `Execute Shell`
  * Write below script into the `Command`
    ```bash
    echo 'Running Unit Tests on Petclinic Application'
    docker run --rm -v $HOME/.m2:/root/.m2 -v `pwd`:/app -w /app maven:3.8-openjdk-11 mvn clean test
  ```
  * Click `Add post-build action` under `Post-build Actions` and select `Record jacoco coverage report`
  * Click `Save`
  
  ######
  * Bu komutun aciklamasi: 
  - Maven calisinca home directorysinde root altinda `/.m2` local repo olusturur ve butun dependencyleri buraya koyar, daha sonra tekrar mvn calistirinca ayni dependencyleri buradan alir tekrar yuklemez. 
  - $HOME/.m2:/root/.m2 -v (Maven calistiginda root user altinda .m2 klasorunu herdefasinda klasoru olusturmasin diye bizim home directorymizdeki .m2 klasorunu, konteynir icerisindeki /root/.m2 volume ile root altindaki /.m2 klasorune bagliyoruz. 
  - Jenkins uzerinde mvn calistirmak icin dockerhub dan maven-image(maven:3.6-openjdk-11) kullaniyoruz. Ancak pom file gormesi gerekiyor docker run komutunda, bu yuzden volume(-v) bagliyoruz, bu volumde her calistiginda olusan gereksiz dosyalari tutmasin diye --rm yaziyoruz.
  - `pwd`:/app -w /app (Bulundugum klasordeki dosyalari /app icerisine koyduk ancak calismasi icin -w /app `workspace`(jenkins home directory) deki /app pathini tanimliyoruz)
  
  - Sonuc olarak bu komut sayesinde jenkins server'a maven yuklemeden konteynir $HOME/.m2 dan gerekli dosya dependencyleri alarak calisacak ve her seferinde unit testleri yapabilecegiz. 

  Ek Bilgiler:
  -"$HOME/.m2" jenkins serverde "var/lib/jenkins/worksopace/petclinic-ci-job" oluyor. 
  $ cd /var/lib/jenkins
  $ sudo su - jenkins    (Jenkins root girmis olduk, ls -a yazarsak `workspace` `/.m2` klasorlerini gorecegiz)
  - workspace icerisinde her calistirdigimiz job file'lar olusur `petclinic-ci-job` diye bir file olusmus ve yukaridaki komut sayesinde
  projenin butun file lari buraya kopyalanmis oldu)
  $ sudo usermod -s /bin/bash jenkins (Jenkins kullanicisina shell ekleme, burda jenkins server otomotize ediyoruz)
#####
  
* Jenkins `CI Job` should be triggered to run on each commit of `feature**` and `bugfix**` branches and on each `PR` merge to `dev` branch.

* Prepare a script for Jenkins CI job (covering Unit Test only) and save it as `jenkins-petclinic-ci-job.sh` under `jenkins` folder.

``` bash
echo 'Running Unit Tests on Petclinic Application'
docker run --rm -v $HOME/.m2:/root/.m2 -v `pwd`:/app -w /app maven:3.8-openjdk-11 mvn clean test
```
* Create a webhook for Jenkins CI Job; 

  + Go to the Github project repository page and click on `Settings`.

  + Click on the `Webhooks` on the left hand menu, and then click on `Add webhook`.

  + Copy the Jenkins URL, paste it into `Payload URL` field, add `/github-webhook/` at the end of URL, and click on `Add webhook`.
  
  ``` text
  http://[jenkins-server-hostname]:8080/github-webhook/
  ```

* Commit the change, then push the Jenkinsfile to the remote repo.
# Asagidaki islemleri yaptiktan sonra `petclinic-ci-job` jenkins pipeline de trigger olacak, cunku webhook yaptik, job da `*/Feature**` branch belirttik, her degisklik de github job'u trigger edecek. 

``` bash
git add .
git commit -m 'added Jenkins Job for CI pipeline'
git push --set-upstream origin feature/msp-13
git checkout dev
git merge feature/msp-13
git push origin dev
```
# "petclinic-nightly" (Geceleri yapilacak functional testler icin) section a geldik. Onceki "petclinic-ci-job" da unit testleri yaptik.
# Burada gercek hayatta kubernetes ile yapilabiliyor ancak biz docker swarm ve ansible kullanacagiz.
# Neden burada k8s kullanmadik; Maliyeti dusurmek, kucuk bir proje k8s kurulum asamalari ile ugrasmamak, ansible kullanmak icin vs.
# Bunun icin; 
- CFN ile ile 5 adet ec2-instance ayaga kaldiracagiz tabi burda keypair otomotize etmemiz gerekiyor, 
- imageleri AWS-ECR dan docker registry den alacagiz
- Ansible ile instance lari yonetecegiz. Bundan soraki asamalar bunun icin.
#####

## MSP 14 - Create Docker Registry for Dev Manually

* Create a Jenkins Freestyle Job and name it as `create-ecr-docker-registry-for-dev` to create Docker Registry for `dev` on AWS ECR manually.

# Copy the command to the "Execute shell"
# petclininc folder da "aws" command '/usr/path/bin' altinda o nedenle calismasi icin path belirtmemiz gerekiyor.
# Jenkins server'a role verdigimiz icin aws config yapmamiz gerekmiyor. Jenkins server ile ECR kurduk manual yapabilirdik.

``` bash
PATH="$PATH:/usr/local/bin"
APP_REPO_NAME="yasin-test-repo/petclinic-app-dev"
AWS_REGION="us-east-1"

aws ecr create-repository \
  --repository-name ${APP_REPO_NAME} \
  --image-scanning-configuration scanOnPush=false \
  --image-tag-mutability MUTABLE \
  --region ${AWS_REGION}
```

## MSP 15 - Prepare Script for Development Docker Registry

* Create `feature/msp-15` branch from `dev`.

``` bash
git checkout dev
git branch feature/msp-15
git checkout feature/msp-15
```

* Prepare a script to create Docker Registry for `dev` on AWS ECR and save it as `create-ecr-docker-registry-for-dev.sh` under `infrastructure` folder.

``` bash
PATH="$PATH:/usr/local/bin"
APP_REPO_NAME="yasin-test-repo/petclinic-app-dev"
AWS_REGION="us-east-1"

aws ecr create-repository \
  --repository-name ${APP_REPO_NAME} \
  --image-scanning-configuration scanOnPush=false \
  --image-tag-mutability MUTABLE \
  --region ${AWS_REGION}
```

* Commit the change, then push the script to the remote repo.

``` bash
git add .
git commit -m 'added script for creating ECR registry for dev'
git push --set-upstream origin feature/msp-15
git checkout dev
git merge feature/msp-15
git push origin dev
```

## MSP 16 - Create a QA Automation Environment with Docker Swarm

- Create `feature/msp-16` branch from `dev`.

```bash
git checkout dev
git branch feature/msp-16
git checkout feature/msp-16
```

- Prepare a Cloudformation template for Docker Swarm Infrastructure consisting of 3 Managers, 2 Worker Instances and save it as `dev-docker-swarm-infrastructure-cfn-template.yml` under `infrastructure` folder.
## Bu cfn dosyasinda docker image olan 5 ec2 var ve bunlara ansible tarafindan yonetmek icin "tag" ler verildi. Managerlar dan biri grand-master, ileride ansible da dynamic inventory bu tagler ile kullanilacak. Bunu proje klasorunden aldik, daha once hazirlanmis.

- Grant permissions to Docker Machines within Cloudformation template to create ECR Registry, push or pull Docker images to/from ECR Repo.

- Commit the change, then push the cloudformation template to the remote repo.

```bash
git add .
git commit -m 'added cloudformation template for Docker Swarm infrastructure'
git push --set-upstream origin feature/msp-16
```

- Create a Jenkins Job and name it as `test-creating-qa-automation-infrastructure` to test `bash` scripts creating QA Automation Infrastructure for `dev` manually.
  * Select `Freestyle project` and click `OK`
  * Select github project and write the url to your repository's page into `Project url` (https://github.com/[your-github-account]/petclinic-microservices)
  * Under the `Source Code Management` select `Git` 
  * Write the url of your repository into the `Repository URL` (https://github.com/[your-github-account]/petclinic-microservices.git)
  * Add `*/feature/msp-16`branch to `Branches to build`
  * Select `Add timestamps to the Console Output` under `Build Environment`
  * Click `Add build step` under `Build` and select `Execute Shell`
  * Write below script into the `Command` for checking the environment tools and versions with following script.

```bash
echo $PATH
whoami
PATH="$PATH:/usr/local/bin"
python3 --version
pip3 --version
ansible --version
aws --version
```
  * Click `Save`

- After running the job above, replace the script with the one below in order to test creating key pair for `ansible`.
# Jenkins de `Project test-creating-qa-automation-infrastructure` gelip `configure` tiklayip `Execute shell'i` update ediyoruz. 

```bash
PATH="$PATH:/usr/local/bin"
CFN_KEYPAIR="yasin-ansible-test-dev.key"
AWS_REGION="us-east-1"
aws ec2 create-key-pair --region ${AWS_REGION} --key-name ${CFN_KEYPAIR} --query "KeyMaterial" --output text > ${CFN_KEYPAIR}
chmod 400 ${CFN_KEYPAIR}
```

- After running the job above, replace the script with the one below in order to test creating Docker Swarm infrastructure with AWS Cloudformation. 
# Cloudformation jenkins server uzerinden olusturuyoruz, Console da manual olarak stack olusturmadan.

```bash
PATH="$PATH:/usr/local/bin"
APP_NAME="Petclinic"
APP_STACK_NAME="yasin-test-$APP_NAME-App-${BUILD_NUMBER}"
CFN_KEYPAIR="yasin-ansible-test-dev.key"
CFN_TEMPLATE="./infrastructure/dev-docker-swarm-infrastructure-cfn-template.yml"
AWS_REGION="us-east-1"
aws cloudformation create-stack --region ${AWS_REGION} --stack-name ${APP_STACK_NAME} --capabilities CAPABILITY_IAM --template-body file://${CFN_TEMPLATE} --parameters ParameterKey=KeyPairName,ParameterValue=${CFN_KEYPAIR}
```

- After running the job above, replace the script with the one below in order to test SSH connection with one of the docker instance.(Get private Ip of one of the instance)

```bash
CFN_KEYPAIR="yasin-ansible-test-dev.key"
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i ${WORKSPACE}/${CFN_KEYPAIR} ec2-user@172.31.81.222  hostname
```
# var/lib/jenkins/.ssh klasoru altinda known_hosts file var, bu dosya butun ec2 larin bilgilerini icersinde tutuyor. Cok lu ec2 ortamlarinda her gece test icin kosan ec2 larin bilgilerini burda degilde /dev/null da tutsun diye `UserKnownHostsFile=/dev/null` yazdik.

- Prepare static inventory file with name of `hosts.ini` for Ansible under `ansible/inventory` folder using Docker machines private IP addresses.
# test-petclinic-microservices-with-db/ansible/inventory folder olsuturuyoruz icersinde `hosts.ini` file olsuturacagiz. bunu terminal den; `mkdir -p ansible/inventory` ile yapabiliriz.

```ini
172.31.91.243   ansible_user=ec2-user  
172.31.87.143   ansible_user=ec2-user
172.31.90.30    ansible_user=ec2-user
172.31.92.190   ansible_user=ec2-user
172.31.88.8     ansible_user=ec2-user
```

- Commit the change, then push the cloudformation template to the remote repo.

```bash
git add .
git commit -m 'added ansible static inventory host.ini for testing'
git push
```

- Configure `test-creating-qa-automation-infrastructure` job and replace the existing script with the one below in order to test ansible by pinging static hosts.
## Test yapiyoruz 5 ansible instance ile connection kurabiliyor mu, "ping" komutu karsiligi "pong" donmesi gerek her biri icin. Asagidaki komutu jenkins de cofiguration dan exec shell kopyalayip run yaptikdan sonra `console output` da 5 adet ping and pong gormemiz gerekiyor ki connectiuon saglikli oldugunu bilelim.

```bash
PATH="$PATH:/usr/local/bin"
CFN_KEYPAIR="yasin-ansible-test-dev.key"
export ANSIBLE_INVENTORY="${WORKSPACE}/ansible/inventory/hosts.ini"
export ANSIBLE_PRIVATE_KEY_FILE="${WORKSPACE}/${CFN_KEYPAIR}"
export ANSIBLE_HOST_KEY_CHECKING=False
ansible all -m ping
```

- Prepare dynamic inventory file with name of `dev_stack_dynamic_inventory_aws_ec2.yaml` for Ansible under `ansible/inventory` folder using Docker machines private IP addresses.

# Dort tane yaml file hazirliyoruz, aslinda 1. yml file yeterli (best practice) diger 3'u neyi nasil yaptigimizi gostermek icin. 
# Ansible sayfasindan aws plugin yazinca content cikiyor, alip burada update ediyoruz. Resimde yaml dosyasindaki tagler sutun olarak gorunuyor. (https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html)

![Ansible Tags Diagram](./ansible_tags.png)

```yaml
plugin: aws_ec2
regions:
  - "us-east-1"
filters:
  tag:app-stack-name: APP_STACK_NAME
  tag:environment: dev
keyed_groups:
  - key: tags['app-stack-name']
    prefix: 'app_stack_'
    separator: ''
  - key: tags['swarm-role']
    prefix: 'role_'
    separator: ''
  - key: tags['environment']
    prefix: 'env_'
    separator: ''
  - key: tags['server']
    separator: ''
hostnames:
  - "private-ip-address"
compose:
  ansible_user: "'ec2-user'"
```

- Prepare dynamic inventory file with name of `dev_stack_swarm_grand_master_aws_ec2.yaml` for Ansible under `ansible/inventory` folder using Docker machines private IP addresses.

```yaml
plugin: aws_ec2
regions:
  - "us-east-1"
filters:
  tag:app-stack-name: APP_STACK_NAME
  tag:environment: dev
  tag:swarm-role: grand-master
hostnames:
  - "private-ip-address"
compose:
  ansible_user: "'ec2-user'"
```

- Prepare dynamic inventory file with name of `dev_stack_swarm_managers_aws_ec2.yaml` for Ansible under `ansible/inventory` folder using Docker machines private IP addresses.

```yaml
plugin: aws_ec2
regions:
  - "us-east-1"
filters:
  tag:app-stack-name: APP_STACK_NAME
  tag:environment: dev
  tag:swarm-role: manager
hostnames:
  - "private-ip-address"
compose:
  ansible_user: "'ec2-user'"
```

- Prepare dynamic inventory file with name of `dev_stack_swarm_workers_aws_ec2.yaml` for Ansible under `ansible/inventory` folder using Docker machines private IP addresses.

```yaml
plugin: aws_ec2
regions:
  - "us-east-1"
filters:
  tag:app-stack-name: APP_STACK_NAME
  tag:environment: dev
  tag:swarm-role: worker
hostnames:
  - "private-ip-address"
compose:
  ansible_user: "'ec2-user'"
```

- Commit the change, then push the cloudformation template to the remote repo.

```bash
git add .
git commit -m 'added ansible dynamic inventory files for dev environment'
git push
```

- Configure `test-creating-qa-automation-infrastructure` job and replace the existing script with the one below in order to check the Ansible dynamic inventory for `dev` environment.

```bash
APP_NAME="Petclinic"
CFN_KEYPAIR="yasin-ansible-test-dev.key"
PATH="$PATH:/usr/local/bin"
export ANSIBLE_PRIVATE_KEY_FILE="${WORKSPACE}/${CFN_KEYPAIR}"
export ANSIBLE_HOST_KEY_CHECKING=False
export APP_STACK_NAME="yasin-test-$APP_NAME-App-${BUILD_NUMBER}" # Jenkins shell de bunu "yasin-test-$APP_NAME-App-6" seklinde degistir -cloudformation dan version number aliyoruz.  
# Dev Stack
sed -i "s/APP_STACK_NAME/$APP_STACK_NAME/" ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml
cat ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml
ansible-inventory -v -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml --graph
# Dev Stack Grand Master
sed -i "s/APP_STACK_NAME/$APP_STACK_NAME/" ./ansible/inventory/dev_stack_swarm_grand_master_aws_ec2.yaml
cat ./ansible/inventory/dev_stack_swarm_grand_master_aws_ec2.yaml
ansible-inventory -v -i ./ansible/inventory/dev_stack_swarm_grand_master_aws_ec2.yaml --graph
# Dev Stack Managers
sed -i "s/APP_STACK_NAME/$APP_STACK_NAME/" ./ansible/inventory/dev_stack_swarm_managers_aws_ec2.yaml
cat ./ansible/inventory/dev_stack_swarm_managers_aws_ec2.yaml
ansible-inventory -v -i ./ansible/inventory/dev_stack_swarm_managers_aws_ec2.yaml --graph
# Dev Stack Workers
sed -i "s/APP_STACK_NAME/$APP_STACK_NAME/" ./ansible/inventory/dev_stack_swarm_workers_aws_ec2.yaml
cat ./ansible/inventory/dev_stack_swarm_workers_aws_ec2.yaml
ansible-inventory -v -i ./ansible/inventory/dev_stack_swarm_workers_aws_ec2.yaml --graph
```
# sed -i komutu; substitution komutu, bir seyin yerine digerini yaz. -i ile bu degisiklik kalici oluyor. 
- After running the job above, replace the script with the one below in order to test all instances within dev dynamic inventory by pinging static hosts.
# Bu defa dynamic inventory ile ping atiyoruz. Eskisini silip asagidakini kopyaliyoruz.

```bash
# Test dev dynamic inventory by pinging
APP_NAME="Petclinic"
CFN_KEYPAIR="yasin-ansible-test-dev.key"
PATH="$PATH:/usr/local/bin"
export ANSIBLE_PRIVATE_KEY_FILE="${WORKSPACE}/${CFN_KEYPAIR}"
export ANSIBLE_HOST_KEY_CHECKING=False
export APP_STACK_NAME="yasin-test-$APP_NAME-App-${BUILD_NUMBER}" # Build number cloudformation bakarak manual yaz.
sed -i "s/APP_STACK_NAME/$APP_STACK_NAME/" ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml
ansible -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml all -m ping
```

- Create an ansible playbook to install and configure tools (`Docker`, `Docker-Compose`, `AWS CLI V2`) needed for all Docker Swarm nodes (instances) and save it as `pb_setup_for_all_docker_swarm_instances.yaml` under `ansible/playbooks` folder.


```yaml
- hosts: all
  tasks:
  - name: update os
    yum:
      name: '*'
      state: present
  - name: install docker
    command: amazon-linux-extras install docker=latest -y
  - name: start docker
    service:
      name: docker
      state: started
      enabled: yes
  - name: add ec2-user to docker group
    shell: "usermod -a -G docker ec2-user"
  - name: install docker compose.
    get_url:
      url: https://github.com/docker/compose/releases/download/1.26.2/docker-compose-Linux-x86_64
      dest: /usr/local/bin/docker-compose
      mode: 0755
  - name: uninstall aws cli v1
    file:
      path: /bin/aws
      state: absent
  - name: download awscliv2 installer
    unarchive:
      src: https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip
      dest: /tmp
      remote_src: yes
      creates: /tmp/aws
      mode: 0755
  - name: run the installer
    command:
    args:
      cmd: "/tmp/aws/install"
      creates: /usr/local/bin/aws
```

- Create an ansible playbook to initialize the Docker Swarm and configure tools on `Grand Master` instance of Docker Swarm and save it as `pb_initialize_docker_swarm.yaml` under `ansible/playbooks` folder.

```yaml
- hosts: role_grand_master
  tasks:
  - name: initialize docker swarm
    shell: docker swarm init
  - name: install git
    yum:
      name: git
      state: present
  - name: run the visualizer app for docker swarm
    shell: |
      docker service create \
        --name=viz \
        --publish=8088:8080/tcp \
        --constraint=node.role==manager \
        --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
        dockersamples/visualizer
```

- Create an ansible playbook to join the Docker manager nodes to the Swarm and save it as `pb_join_docker_swarm_managers.yaml` under `ansible/playbooks` folder.

```yaml
- hosts: role_grand_master
  tasks:
  - name: Get swarm join-token for managers
    shell: docker swarm join-token manager | grep -i 'docker'
    register: join_command_for_managers

  - debug: msg='{{ join_command_for_managers.stdout.strip() }}'
  
  - name: register grand_master with variable
    add_host:
      name: "grand_master"
      manager_join: "{{ join_command_for_managers.stdout.strip() }}"

- hosts: role_manager
  tasks:
  - name: Join managers to swarm
    shell: "{{ hostvars['grand_master']['manager_join'] }}"
    register: result_of_joining

  - debug: msg='{{ result_of_joining.stdout }}'
```

- Create an ansible playbook to join the Docker worker nodes to the Swarm and save it as `pb_join_docker_swarm_workers.yaml` under `ansible/playbooks` folder.

```yaml
- hosts: role_grand_master
  tasks:
  - name: Get swarm join-token for workers
    shell: docker swarm join-token worker | grep -i 'docker'
    register: join_command_for_workers

  - debug: msg='{{ join_command_for_workers.stdout.strip() }}'
  
  - name: register grand_master with variable
    add_host:
      name: "grand_master"
      worker_join: "{{ join_command_for_workers.stdout.strip() }}"

- hosts: role_worker
  tasks:
  - name: Join workers to swarm
    shell: "{{ hostvars['grand_master']['worker_join'] }}"
    register: result_of_joining

  - debug: msg='{{ result_of_joining.stdout }}'
```

- Commit the change, then push the ansible playbooks to the remote repo.

```bash
git add .
git commit -m 'added ansible playbooks for dev environment'
git push
```

- Configure `test-creating-qa-automation-infrastructure` job and replace the existing script with the one below in order to test the playbooks to create a Docker Swarm on Cloudformation Stack.

```bash
APP_NAME="Petclinic"
CFN_KEYPAIR="yasin-ansible-test-dev.key"
PATH="$PATH:/usr/local/bin"
export ANSIBLE_PRIVATE_KEY_FILE="${WORKSPACE}/${CFN_KEYPAIR}"
export ANSIBLE_HOST_KEY_CHECKING=False
export APP_STACK_NAME="Call-$APP_NAME-App-${BUILD_NUMBER}"
sed -i "s/APP_STACK_NAME/$APP_STACK_NAME/" ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml
# Swarm Setup for all nodes (instances)
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_setup_for_all_docker_swarm_instances.yaml
# Swarm Setup for Grand Master node
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_initialize_docker_swarm.yaml
# Swarm Setup for Other Managers nodes
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_managers.yaml
# Swarm Setup for Workers nodes
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_workers.yaml
```

- After running the job above, replace the script with the one below in order to test tearing down the Docker Swarm infrastructure using AWS CLI with following script.

```bash
PATH="$PATH:/usr/local/bin"
APP_NAME="Petclinic"
AWS_STACK_NAME="yasin-test-$APP_NAME-App-${BUILD_NUMBER}"
AWS_REGION="us-east-1"
aws cloudformation delete-stack --region ${AWS_REGION} --stack-name ${AWS_STACK_NAME}
```

- After running the job above, replace the script with the one below in order to test deleting existing key pair using AWS CLI with following script.

```bash
PATH="$PATH:/usr/local/bin"
CFN_KEYPAIR="yasin-ansible-test-dev.key"
AWS_REGION="us-east-1"
aws ec2 delete-key-pair --region ${AWS_REGION} --key-name ${CFN_KEYPAIR}
rm -rf ${CFN_KEYPAIR}
```

- Create a script to create QA Automation infrastructure and save it as `create-qa-automation-environment.sh` under `infrastructure` folder. (This script shouldn't be used in one time. It should be applied step by step like above)

```bash
# Environment variables
PATH="$PATH:/usr/local/bin"
APP_NAME="Petclinic"
CFN_KEYPAIR="yasin-ansible-test-dev-.key"
CFN_TEMPLATE="./infrastructure/dev-docker-swarm-infrastructure-cfn-template.yml"
AWS_REGION="us-east-1"
export ANSIBLE_PRIVATE_KEY_FILE="${WORKSPACE}/${CFN_KEYPAIR}"
export ANSIBLE_HOST_KEY_CHECKING=False
export APP_STACK_NAME="-$APP_NAME-App-${BUILD_NUMBER}"
# Create key pair for Ansible
aws ec2 create-key-pair --region ${AWS_REGION} --key-name ${CFN_KEYPAIR} --query "KeyMaterial" --output text > ${CFN_KEYPAIR}
chmod 400 ${CFN_KEYPAIR}
# Create infrastructure for Docker Swarm
aws cloudformation create-stack --region ${AWS_REGION} --stack-name ${APP_STACK_NAME} --capabilities CAPABILITY_IAM --template-body file://${CFN_TEMPLATE} --parameters ParameterKey=KeyPairName,ParameterValue=${CFN_KEYPAIR}

# Install Docker Swarm environment on the infrastructure
# Update dynamic inventory (hosts/docker nodes)
sed -i "s/APP_STACK_NAME/$APP_STACK_NAME/" ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml
# Install common tools on all instances/nodes
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_setup_for_all_docker_swarm_instances.yaml
# Initialize Docker Swarm on Grand Master
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_initialize_docker_swarm.yaml
# Join the manager instances to the Swarm
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_managers.yaml
# Join the worker instances to the Swarm
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_workers.yaml

# Build, Deploy, Test the application

# Tear down the Docker Swarm infrastructure
aws cloudformation delete-stack --region ${AWS_REGION} --stack-name ${AWS_STACK_NAME}
# Delete key pair
aws ec2 delete-key-pair --region ${AWS_REGION} --key-name ${CFN_KEYPAIR}
rm -rf ${CFN_KEYPAIR}
```

- Commit the change, then push the script to the remote repo.

```bash
git add .
git commit -m 'added scripts for qa automation environment'
git push
git checkout dev
git merge feature/msp-16
git push origin dev
```

## MSP 17 - Prepare a QA Automation Pipeline for Nightly Builds

![Functional Test Diagram](./functional_test.png)
# For the functional tests; 
- We need running application and for this we need package and install. Maven for package and we need docker images.
- These images will be store in `ECR` and we will pull/push with using `tagging` 
- Create Infrastructure; Docker Swarm (5 EC2) and Configure/manage by Ansible
- Functional test will be done on `Selenium`

- Create `feature/msp-17` branch from `dev`.

```bash
git checkout dev
git branch feature/msp-17
git checkout feature/msp-17
```

- Prepare a script to package the app with maven Docker container and save it as `package-with-maven-container.sh` and save it under `jenkins` folder.

```bash
docker run --rm -v $HOME/.m2:/root/.m2 -v $WORKSPACE:/app -w /app maven:3.8-openjdk-11 mvn clean package
```
# Bu kodun  aciklamasi; Attaching local .m2 repository to the image repository and app will run on the workspace (repo of jenkins) server. 

- Prepare a script to create ECR tags for the dev docker images and save it as `prepare-tags-ecr-for-dev-docker-images.sh` and save it under `jenkins` folder.

```bash
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-admin-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_ADMIN_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:admin-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-api-gateway/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_API_GATEWAY="${ECR_REGISTRY}/${APP_REPO_NAME}:api-gateway-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-config-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_CONFIG_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:config-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-customers-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_CUSTOMERS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:customers-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-discovery-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_DISCOVERY_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:discovery-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-hystrix-dashboard/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_HYSTRIX_DASHBOARD="${ECR_REGISTRY}/${APP_REPO_NAME}:hystrix-dashboard-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-vets-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_VETS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:vets-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-visits-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_VISITS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:visits-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
export IMAGE_TAG_GRAFANA_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:grafana-service"
export IMAGE_TAG_PROMETHEUS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:prometheus-service"
```

- Prepare a script to build the dev docker images tagged for ECR registry and save it as `build-dev-docker-images-for-ecr.sh` and save it under `jenkins` folder.

```bash
docker build --force-rm -t "${IMAGE_TAG_ADMIN_SERVER}" "${WORKSPACE}/spring-petclinic-admin-server"
docker build --force-rm -t "${IMAGE_TAG_API_GATEWAY}" "${WORKSPACE}/spring-petclinic-api-gateway"
docker build --force-rm -t "${IMAGE_TAG_CONFIG_SERVER}" "${WORKSPACE}/spring-petclinic-config-server"
docker build --force-rm -t "${IMAGE_TAG_CUSTOMERS_SERVICE}" "${WORKSPACE}/spring-petclinic-customers-service"
docker build --force-rm -t "${IMAGE_TAG_DISCOVERY_SERVER}" "${WORKSPACE}/spring-petclinic-discovery-server"
docker build --force-rm -t "${IMAGE_TAG_HYSTRIX_DASHBOARD}" "${WORKSPACE}/spring-petclinic-hystrix-dashboard"
docker build --force-rm -t "${IMAGE_TAG_VETS_SERVICE}" "${WORKSPACE}/spring-petclinic-vets-service"
docker build --force-rm -t "${IMAGE_TAG_VISITS_SERVICE}" "${WORKSPACE}/spring-petclinic-visits-service"
docker build --force-rm -t "${IMAGE_TAG_GRAFANA_SERVICE}" "${WORKSPACE}/docker/grafana"
docker build --force-rm -t "${IMAGE_TAG_PROMETHEUS_SERVICE}" "${WORKSPACE}/docker/prometheus"
```

- Prepare a script to push the dev docker images to the ECR repo and save it as `push-dev-docker-images-to-ecr.sh` and save it under `jenkins` folder.

```bash
# Provide credentials for Docker to login the AWS ECR and push the images
aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY} 
docker push "${IMAGE_TAG_ADMIN_SERVER}"
docker push "${IMAGE_TAG_API_GATEWAY}"
docker push "${IMAGE_TAG_CONFIG_SERVER}"
docker push "${IMAGE_TAG_CUSTOMERS_SERVICE}"
docker push "${IMAGE_TAG_DISCOVERY_SERVER}"
docker push "${IMAGE_TAG_HYSTRIX_DASHBOARD}"
docker push "${IMAGE_TAG_VETS_SERVICE}"
docker push "${IMAGE_TAG_VISITS_SERVICE}"
docker push "${IMAGE_TAG_GRAFANA_SERVICE}"
docker push "${IMAGE_TAG_PROMETHEUS_SERVICE}"
```

- Commit the change, then push the scripts to the remote repo.

```bash
git add .
git commit -m 'added scripts for qa automation environment'
git push --set-upstream origin feature/msp-17
```

  - OPTIONAL: Create a Jenkins job with the name of `test-msp-17-scripts` to test the scripts:   
      * Select `Freestyle project` and click `OK`
      * Select github project and write the url to your repository's page into `Project url` (https://github.com/[your-github-account]/petclinic-microservices)
      * Under the `Source Code Management` select `Git` 
      * Write the url of your repository into the `Repository URL` (https://github.com/[your-github-account]/petclinic-microservices.git)
      * Add `*/feature/msp-17` branch to `Branches to build`
      * Click `Add build step` under `Build` and select `Execute Shell`
      * Write below script into the `Command`
        ```bash
        PATH="$PATH:/usr/local/bin"
        APP_REPO_NAME="yasin-repo/petclinic-app-dev" # Write your own repo name
        AWS_REGION="us-east-1" #Update this line if you work on another region
        ECR_REGISTRY="046402772087.dkr.ecr.us-east-1.amazonaws.com" # Replace this line with your ECR name
        aws ecr create-repository \
            --repository-name ${APP_REPO_NAME} \
            --image-scanning-configuration scanOnPush=false \
            --image-tag-mutability MUTABLE \
            --region ${AWS_REGION}
        . ./jenkins/package-with-maven-container.sh
        . ./jenkins/prepare-tags-ecr-for-dev-docker-images.sh
        . ./jenkins/build-dev-docker-images-for-ecr.sh
        . ./jenkins/push-dev-docker-images-to-ecr.sh
        ```
      * Click `Save`
      * Click `Build now` to manually start the job.

- Prepare a docker compose file for swarm deployment and save it as `docker-compose-swarm-dev.yml` under the project root.

```yaml
version: '3.8'

services:
  config-server:
    image: "${IMAGE_TAG_CONFIG_SERVER}"
    networks:
      - clarusnet
    ports:
      - 8888:8888

  discovery-server:
    image: "${IMAGE_TAG_DISCOVERY_SERVER}"
    depends_on:
      - config-server
    entrypoint: ["./dockerize","-wait=tcp://config-server:8888","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
      - 8761:8761

  customers-service:
    image: "${IMAGE_TAG_CUSTOMERS_SERVICE}"
    deploy:
      replicas: 3
      update_config:
          parallelism: 2
          delay: 5s
          order: start-first
    depends_on:
      - config-server
      - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
      - 8081:8081

  visits-service:
    image: "${IMAGE_TAG_VISITS_SERVICE}"
    deploy:
      replicas: 3
      update_config:
          parallelism: 2
          delay: 5s
          order: start-first
    depends_on:
      - config-server
      - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
      - 8082:8082

  vets-service:
    image: "${IMAGE_TAG_VETS_SERVICE}"
    deploy:
      replicas: 3
      update_config:
          parallelism: 2
          delay: 5s
          order: start-first
    depends_on:
      - config-server
      - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
      - 8083:8083

  api-gateway:
    image: "${IMAGE_TAG_API_GATEWAY}"
    deploy:
      replicas: 3
      update_config:
          parallelism: 2
          delay: 5s
          order: start-first
    depends_on:
      - config-server
      - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
      - 8080:8080

  tracing-server:
    image: openzipkin/zipkin
    environment:
      - JAVA_OPTS=-XX:+UnlockExperimentalVMOptions -Djava.security.egd=file:/dev/./urandom
    networks:
      - clarusnet
    ports:
      - 9411:9411

  admin-server:
    image: "${IMAGE_TAG_ADMIN_SERVER}"
    depends_on:
      - config-server
      - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
      - 9090:9090

  hystrix-dashboard:
    image: "${IMAGE_TAG_HYSTRIX_DASHBOARD}"
    depends_on:
      - config-server
      - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
      - 7979:7979

  ## Grafana / Prometheus

  grafana-server:
    image: "${IMAGE_TAG_GRAFANA_SERVICE}"
    networks:
      - clarusnet
    ports:
      - 3000:3000

  prometheus-server:
    image: "${IMAGE_TAG_PROMETHEUS_SERVICE}"
    networks:
      - clarusnet
    ports:
      - 9091:9090
    
  mysql-server:
    image: mysql:5.7.8
    environment: 
      MYSQL_ROOT_PASSWORD: petclinic
      MYSQL_DATABASE: petclinic
    networks:
      - clarusnet
    ports:
      - 3306:3306

networks:
  clarusnet:
    driver: overlay
```

- Create Ansible playbook for deploying app on Docker swarm using docker compose file and save it as `pb_deploy_app_on_docker_swarm.yaml` under `ansible/playbooks` folder.

```yaml
- hosts: role_grand_master
  tasks:
  - name: Copy docker compose file to grand master
    copy:
      src: "{{ workspace }}/docker-compose-swarm-dev-tagged.yml"
      dest: /home/ec2-user/docker-compose-swarm-dev-tagged.yml

  - name: get login credentials for ecr
    shell: "export PATH=$PATH:/usr/local/bin/ && aws ecr get-login-password --region {{ aws_region }} | docker login --username AWS --password-stdin {{ ecr_registry }}"

  - name: deploy the app stack on swarm
    shell: "docker stack deploy --with-registry-auth -c /home/ec2-user/docker-compose-swarm-dev-tagged.yml {{ app_name }}"
    register: output

  - debug: msg="{{ output.stdout }}"
```

- Prepare a script to deploy the application on docker swarm and save it as `deploy_app_on_docker_swarm.sh` under `ansible/scripts` folder.

```bash
PATH="$PATH:/usr/local/bin"
APP_NAME="petclinic"
envsubst < docker-compose-swarm-dev.yml > docker-compose-swarm-dev-tagged.yml 
ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b --extra-vars "workspace=${WORKSPACE} app_name=${APP_NAME} aws_region=${AWS_REGION} ecr_registry=${ECR_REGISTRY}" ./ansible/playbooks/pb_deploy_app_on_docker_swarm.yaml
```
# tagging script will create env variable like $admin-server-tag available inside the jenkins server and with `envsubst` command these env variable will be inside of grand-master server as well. 
# docker-compose-swarm-dev.yml will change to docker-compose-swarm-dev-tagged.yml (we use this in grand master deploy file in playbook) with variables actual values.

- Create Selenium dummy test with name of `dummy_selenium_test_headless.py` with following content to check the setup for the Selenium jobs and save it under `selenium-jobs` folder under the project root.
# Before running our pipeline we want to check with dummy python file test if it is running.

```python
from selenium import webdriver

chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("headless")
chrome_options.add_argument("no-sandbox")
chrome_options.add_argument("disable-dev-shm-usage")
driver = webdriver.Chrome(options=chrome_options)

base_url = "https://www.google.com/"
driver.get(base_url)
source = driver.page_source

if "I'm Feeling Lucky" in source:
  print("Test passed")
else:
  print("Test failed")
driver.close()
```

- Create Ansible playbook for running dummy selenium job and save it as `pb_run_dummy_selenium_job.yaml` under `ansible/playbooks` folder.

```yaml
- hosts: all
  tasks:
  - name: run dummy selenium job
    shell: "docker run --rm -v {{ workspace }}:{{ workspace }} -w {{ workspace }} callahanclarus/selenium-py-chrome:latest python {{ item }}"
    with_fileglob: "{{ workspace }}/selenium-jobs/dummy*.py"
    register: output
  
  - name: show results
    debug: msg="{{ item.stdout }}"
    with_items: "{{ output.results }}"
```

- Prepare a script to run the playbook for dummy selenium job on Jenkins Server (localhost) and save it as `run_dummy_selenium_job.sh` under `ansible/scripts` folder.

```bash
PATH="$PATH:/usr/local/bin"
ansible-playbook --connection=local --inventory 127.0.0.1, --extra-vars "workspace=${WORKSPACE}" ./ansible/playbooks/pb_run_dummy_selenium_job.yaml
```

- Commit the change, then push the scripts for dummy selenium job to the remote repo.

```bash
git add .
git commit -m 'added scripts for running dummy selenium job'
git push --set-upstream origin feature/msp-17
```

- Create a Jenkins freestyle job with name of `test-running-dummy-selenium-job` to check the setup for selenium tests by running dummy selenium job on `feature/msp-17` branch.
- Source code management - github url - branch: feature/msp-17 - execute shell - copy the script above.

- Create Ansible playbook for running all selenium jobs under `selenium-jobs` folder and save it as `pb_run_selenium_jobs.yaml` under `ansible/playbooks` folder.

```yaml
- hosts: all
  tasks:
  - name: run all selenium jobs
    shell: "docker run --rm --env MASTER_PUBLIC_IP={{ master_public_ip }} -v {{ workspace }}:{{ workspace }} -w {{ workspace }} callahanclarus/selenium-py-chrome:latest python {{ item }}"
    register: output
    with_fileglob: "{{ workspace }}/selenium-jobs/test*.py"
  
  - name: show results
    debug: msg="{{ item.stdout }}"
    with_items: "{{ output.results }}"
```

- Prepare a script to run the playbook for all selenium jobs on Jenkins Server (localhost) and save it as `run_selenium_jobs.sh` under `ansible/scripts` folder.

```bash
PATH="$PATH:/usr/local/bin"
ansible-playbook -vvv --connection=local --inventory 127.0.0.1, --extra-vars "workspace=${WORKSPACE} master_public_ip=${GRAND_MASTER_PUBLIC_IP}" ./ansible/playbooks/pb_run_selenium_jobs.yaml
```
- Prepare a Jenkinsfile for `petclinic-nightly` builds and save it as `jenkinsfile-petclinic-nightly` under `jenkins` folder.

```groovy
pipeline {
    agent any
    environment {
        PATH=sh(script:"echo $PATH:/usr/local/bin", returnStdout:true).trim()
        APP_NAME="petclinic"
        APP_STACK_NAME="Callet-${APP_NAME}-app-${BUILD_NUMBER}"
        APP_REPO_NAME="claruswayset-repo/${APP_NAME}-app-dev"
        AWS_ACCOUNT_ID=sh(script:'export PATH="$PATH:/usr/local/bin" && aws sts get-caller-identity --query Account --output text', returnStdout:true).trim()
        AWS_REGION="us-east-1"
        ECR_REGISTRY="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
        CFN_KEYPAIR="call-${APP_NAME}-dev-${BUILD_NUMBER}.key"
        CFN_TEMPLATE="./infrastructure/dev-docker-swarm-infrastructure-cfn-template.yml"
        ANSIBLE_PRIVATE_KEY_FILE="${WORKSPACE}/${CFN_KEYPAIR}"
        ANSIBLE_HOST_KEY_CHECKING="False"
    }
    stages {
        stage('Create ECR Repo') {
            steps {
                echo "Creating ECR Repo for ${APP_NAME} app"
                sh """
                aws ecr create-repository \
                    --repository-name ${APP_REPO_NAME} \
                    --image-scanning-configuration scanOnPush=false \
                    --image-tag-mutability MUTABLE \
                    --region ${AWS_REGION}
                """
            }
        }
        stage('Package Application') {
            steps {
                echo 'Packaging the app into jars with maven'
                sh ". ./jenkins/package-with-maven-container.sh"
            }
        }
        stage('Prepare Tags for Docker Images') {
            steps {
                echo 'Preparing Tags for Docker Images'
                script {
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-admin-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_ADMIN_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:admin-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-api-gateway/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_API_GATEWAY="${ECR_REGISTRY}/${APP_REPO_NAME}:api-gateway-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-config-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_CONFIG_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:config-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-customers-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_CUSTOMERS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:customers-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-discovery-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_DISCOVERY_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:discovery-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-hystrix-dashboard/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_HYSTRIX_DASHBOARD="${ECR_REGISTRY}/${APP_REPO_NAME}:hystrix-dashboard-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-vets-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_VETS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:vets-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-visits-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_VISITS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:visits-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    env.IMAGE_TAG_GRAFANA_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:grafana-service"
                    env.IMAGE_TAG_PROMETHEUS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:prometheus-service"
                }
            }
        }
        stage('Build App Docker Images') {
            steps {
                echo 'Building App Dev Images'
                sh ". ./jenkins/build-dev-docker-images-for-ecr.sh"
                sh 'docker image ls'
            }
        }
        stage('Push Images to ECR Repo') {
            steps {
                echo "Pushing ${APP_NAME} App Images to ECR Repo"
                sh ". ./jenkins/push-dev-docker-images-to-ecr.sh"
            }
        }
        stage('Create Key Pair for Ansible') {
            steps {
                echo "Creating Key Pair for ${APP_NAME} App"
                sh "aws ec2 create-key-pair --region ${AWS_REGION} --key-name ${CFN_KEYPAIR} --query KeyMaterial --output text > ${CFN_KEYPAIR}"
                sh "chmod 400 ${CFN_KEYPAIR}"
            }
        }
        stage('Create QA Automation Infrastructure') {
            steps {
                echo 'Creating QA Automation Infrastructure for Dev Environment with Cloudfomation'
                sh "aws cloudformation create-stack --region ${AWS_REGION} --stack-name ${APP_STACK_NAME} --capabilities CAPABILITY_IAM --template-body file://${CFN_TEMPLATE} --parameters ParameterKey=KeyPairName,ParameterValue=${CFN_KEYPAIR}"

                script {
                    while(true) {
                        echo "Docker Grand Master is not UP and running yet. Will try to reach again after 10 seconds..."
                        sleep(10)

                        ip = sh(script:"aws ec2 describe-instances --region ${AWS_REGION} --filters Name=tag-value,Values=grand-master Name=tag-value,Values=${APP_STACK_NAME} --query Reservations[*].Instances[*].[PublicIpAddress] --output text", returnStdout:true).trim()

                        if (ip.length() >= 7) {
                            echo "Docker Grand Master Public Ip Address Found: $ip"
                            env.GRAND_MASTER_PUBLIC_IP = "$ip"
                            break
                        }
                    }
                    while(true) {
                        try{
                            sh "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i ${WORKSPACE}/${CFN_KEYPAIR} ec2-user@${GRAND_MASTER_PUBLIC_IP} hostname"
                            echo "Docker Grand Master is reachable with SSH."
                            break
                        }
                        catch(Exception){
                            echo "Could not connect to Docker Grand Master with SSH, I will try again in 10 seconds"
                            sleep(10)
                        }
                    }
                }
            }
        }

        stage('Create Docker Swarm for QA Automation Build') {
            steps {
                echo "Setup Docker Swarm for QA Automation Build for ${APP_NAME} App"
                echo "Update dynamic environment"
                sh "sed -i 's/APP_STACK_NAME/${APP_STACK_NAME}/' ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml"
                echo "Swarm Setup for all nodes (instances)"
                sh "ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_setup_for_all_docker_swarm_instances.yaml"
                echo "Swarm Setup for Grand Master node"
                sh "ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_initialize_docker_swarm.yaml"
                echo "Swarm Setup for Other Managers nodes"
                sh "ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_managers.yaml"
                echo "Swarm Setup for Workers nodes"
                sh "ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_workers.yaml"
            }
        }

        stage('Deploy App on Docker Swarm'){
            steps {
                echo 'Deploying App on Swarm'
                sh 'envsubst < docker-compose-swarm-dev.yml > docker-compose-swarm-dev-tagged.yml'
                sh 'ansible-playbook -i ./ansible/inventory/dev_stack_dynamic_inventory_aws_ec2.yaml -b --extra-vars "workspace=${WORKSPACE} app_name=${APP_NAME} aws_region=${AWS_REGION} ecr_registry=${ECR_REGISTRY}" ./ansible/playbooks/pb_deploy_app_on_docker_swarm.yaml'
            }
        }

        stage('Test the Application Deployment'){
            steps {
                echo "Check if the ${APP_NAME} app is ready or not"
                script {

                    while(true) {
                        try{
                            sh "curl -s ${GRAND_MASTER_PUBLIC_IP}:8080"
                            echo "${APP_NAME} app is successfully deployed."
                            break
                        }
                        catch(Exception){
                            echo "Could not connect to ${APP_NAME} app"
                            sleep(5)
                        }
                    }
                }
            }
        }

        stage('Run QA Automation Tests'){
            steps {
                echo "Run the Selenium Functional Test on QA Environment"
                sh 'ansible-playbook -vvv --connection=local --inventory 127.0.0.1, --extra-vars "workspace=${WORKSPACE} master_public_ip=${GRAND_MASTER_PUBLIC_IP}" ./ansible/playbooks/pb_run_selenium_jobs.yaml'
            }
        }
    }

    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
            echo 'Delete the Image Repository on ECR'
            sh """
                aws ecr delete-repository \
                  --repository-name ${APP_REPO_NAME} \
                  --region ${AWS_REGION}\
                  --force
                """
            echo 'Tear down the Docker Swarm infrastructure using AWS CLI'
            sh "aws cloudformation delete-stack --region ${AWS_REGION} --stack-name ${APP_STACK_NAME}"
            echo "Delete existing key pair using AWS CLI"
            sh "aws ec2 delete-key-pair --region ${AWS_REGION} --key-name ${CFN_KEYPAIR}"
            sh "rm -rf ${CFN_KEYPAIR}"
        }
    }
}
```

- Create a Jenkins pipeline with name of `petclinic-nightly` with following script to run QA automation tests and configure a `cron job` to trigger the pipeline every night at midnight (`0 0 * * *`) on `dev` branch. Petclinic nightly build pipeline should be built on temporary QA automation environment.

- Commit the change, then push the script to the remote repo.

```bash
git add .
git commit -m 'added qa automation pipeline for dev'
git push
git checkout dev
git merge feature/msp-17
git push origin dev
```

## MSP 18 - Create a QA Environment on Docker Swarm with Cloudformation and Ansible

- Create `feature/msp-18` branch from `dev`.

```bash
git checkout dev
git branch feature/msp-18
git checkout feature/msp-18
```

- Prepare a Cloudformation template for `QA` Docker Swarm Infrastructure consisting of 3 Managers, 2 Worker Instances and save it as `qa-docker-swarm-infrastructure-cfn-template.yml` under `infrastructure` folder.

- Grant permissions to Docker Machines within Cloudformation template to create ECR Registry, push or pull Docker images to/from ECR Repo.

- Create a Jenkins Job with the name of `create-permanent-key-pair-for-petclinic-qa-env` for Ansible key pair to be used in QA environment using following script, and save the script as `create-permanent-key-pair-for-qa-environment.sh` under `jenkins` folder.

```bash
PATH="$PATH:/usr/local/bin"
APP_NAME="petclinic"
CFN_KEYPAIR="matt-${APP_NAME}-qa.key"
AWS_REGION="us-east-1"
aws ec2 create-key-pair --region ${AWS_REGION} --key-name ${CFN_KEYPAIR} --query "KeyMaterial" --output text > ${CFN_KEYPAIR}
chmod 400 ${CFN_KEYPAIR}
mkdir -p ${JENKINS_HOME}/.ssh
mv ${CFN_KEYPAIR} ${JENKINS_HOME}/.ssh/${CFN_KEYPAIR}
ls -al ${JENKINS_HOME}/.ssh
```

- Prepare a script with the name of `create-qa-infrastructure-cfn.sh` for a Permanent QA Infrastructure with AWS Cloudformation using AWS CLI under `infrastructure` folder.

```bash
PATH="$PATH:/usr/local/bin"
APP_NAME="petclinic"
APP_STACK_NAME="Matt-$APP_NAME-App-QA-${BUILD_NUMBER}"
CFN_KEYPAIR="matt-${APP_NAME}-qa.key"
CFN_TEMPLATE="./infrastructure/qa-docker-swarm-infrastructure-cfn-template.yml"
AWS_REGION="us-east-1"
aws cloudformation create-stack --region ${AWS_REGION} --stack-name ${APP_STACK_NAME} --capabilities CAPABILITY_IAM --template-body file://${CFN_TEMPLATE} --parameters ParameterKey=KeyPairName,ParameterValue=${CFN_KEYPAIR}
```

- Prepare dynamic inventory file with name of `qa_stack_dynamic_inventory_aws_ec2.yaml` for Ansible under `ansible/inventory` folder using Docker machines private IP addresses.

```yaml
plugin: aws_ec2
regions:
  - "us-east-1"
filters:
  tag:app-stack-name: APP_STACK_NAME
  tag:environment: qa
keyed_groups:
  - key: tags['app-stack-name']
    prefix: 'app_stack_'
    separator: ''
  - key: tags['swarm-role']
    prefix: 'role_'
    separator: ''
  - key: tags['environment']
    prefix: 'env_'
    separator: ''
  - key: tags['server']
    separator: ''
hostnames:
  - "private-ip-address"
compose:
  ansible_user: "'ec2-user'"
```

- Prepare script with the name of `qa_build_deploy_environment.sh` to create a `QA` Environment for Release on Docker Swarm using the same playbooks created for `Dev` environment under `ansible/scripts` folder.

```bash
PATH="$PATH:/usr/local/bin"
APP_NAME="petclinic"
CFN_KEYPAIR="matt-${APP_NAME}-qa.key"
APP_STACK_NAME="Matt-$APP_NAME-App-QA-${BUILD_NUMBER}"
export ANSIBLE_PRIVATE_KEY_FILE="${JENKINS_HOME}/.ssh/${CFN_KEYPAIR}"
export ANSIBLE_HOST_KEY_CHECKING=False
sed -i "s/APP_STACK_NAME/$APP_STACK_NAME/" ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml
# Swarm Setup for all nodes (instances)
ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_setup_for_all_docker_swarm_instances.yaml
# Swarm Setup for Grand Master node
ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_initialize_docker_swarm.yaml
# Swarm Setup for Other Managers nodes
ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_managers.yaml
# Swarm Setup for Workers nodes
ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_workers.yaml
```

- Prepare a Jenkinsfile to create a QA Environment on Docker Swarm manually and save it as `jenkinsfile-create-qa-environment-on-docker-swarm` under `jenkins` folder.

```groovy
pipeline {
    agent any
    environment {
        PATH=sh(script:"echo $PATH:/usr/local/bin", returnStdout:true).trim()
        APP_NAME="petclinic"
        APP_STACK_NAME="Matt-$APP_NAME-App-QA-${BUILD_NUMBER}"
        AWS_REGION="us-east-1"
        CFN_KEYPAIR="matt-${APP_NAME}-qa.key"
        CFN_TEMPLATE="./infrastructure/qa-docker-swarm-infrastructure-cfn-template.yml"
        ANSIBLE_PRIVATE_KEY_FILE="${JENKINS_HOME}/.ssh/${CFN_KEYPAIR}"
        ANSIBLE_HOST_KEY_CHECKING="False"
    }
    stages {
        stage('Create QA Environment Infrastructure') {
            steps {
                echo 'Creating Infrastructure for QA Environment with Cloudfomation'
                sh "aws cloudformation create-stack --region ${AWS_REGION} --stack-name ${APP_STACK_NAME} --capabilities CAPABILITY_IAM --template-body file://${CFN_TEMPLATE} --parameters ParameterKey=KeyPairName,ParameterValue=${CFN_KEYPAIR}"

                script {
                    while(true) {
                        echo "Docker Grand Master is not UP and running yet. Will try to reach again after 10 seconds..."
                        sleep(10)

                        ip = sh(script:"aws ec2 describe-instances --region ${AWS_REGION} --filters Name=tag-value,Values=grand-master Name=tag-value,Values=${APP_STACK_NAME} --query Reservations[*].Instances[*].[PublicIpAddress] --output text", returnStdout:true).trim()

                        if (ip.length() >= 7) {
                            echo "Docker Grand Master Public Ip Address Found: $ip"
                            env.GRAND_MASTER_PUBLIC_IP = "$ip"
                            break
                        }
                    }
                    while(true) {
                        try{
                            sh "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i ${JENKINS_HOME}/.ssh/${CFN_KEYPAIR} ec2-user@${GRAND_MASTER_PUBLIC_IP} hostname"
                            echo "Docker Grand Master is reachable with SSH."
                            break
                        }
                        catch(Exception){
                            echo "Could not connect to Docker Grand Master with SSH, I will try again in 10 seconds"
                            sleep(10)
                        }
                    }
                }
            }
        }

        stage('Create Docker Swarm for QA Environment') {
            steps {
                echo "Setup Docker Swarm for QA Environment for ${APP_NAME} App"
                echo "Update dynamic environment"
                sh "sed -i 's/APP_STACK_NAME/${APP_STACK_NAME}/' ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml"
                echo "Swarm Setup for all nodes (instances)"
                sh "ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_setup_for_all_docker_swarm_instances.yaml"
                echo "Swarm Setup for Grand Master node"
                sh "ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_initialize_docker_swarm.yaml"
                echo "Swarm Setup for Other Managers nodes"
                sh "ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_managers.yaml"
                echo "Swarm Setup for Workers nodes"
                sh "ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b ./ansible/playbooks/pb_join_docker_swarm_workers.yaml"
            }
        }
    }
    post {
        failure {
            echo 'Tear down the Docker Swarm infrastructure using AWS CLI'
            sh "aws cloudformation delete-stack --region ${AWS_REGION} --stack-name ${APP_STACK_NAME}"
        }
    }
}
```
- Commit the change, then push the scripts to the remote repo.

```bash
git add .
git commit -m "added jenkinsfile for creating manual qa environment"
git push --set-upstream origin feature/msp-18
git checkout dev
git merge feature/msp-18
git push origin dev
```
- Create a pipeline on Jenkins Server with name of `create-qa-environment-on-docker-swarm` and create QA environment manually on `dev` branch.

## MSP 19 - Prepare Build Scripts for QA Environment

- Create `feature/msp-19` branch from `dev`.

```bash
git checkout dev
git branch feature/msp-19
git checkout feature/msp-19
```

- Create a Jenkins Job and name it as `create-ecr-docker-registry-for-petclinic-qa` to create Docker Registry for `QA` manually on AWS ECR.

```bash
PATH="$PATH:/usr/local/bin"
APP_REPO_NAME="clarusway-repo/petclinic-app-qa"
AWS_REGION="us-east-1"

aws ecr create-repository \
  --repository-name ${APP_REPO_NAME} \
  --image-scanning-configuration scanOnPush=false \
  --image-tag-mutability MUTABLE \
  --region ${AWS_REGION}
```

- Prepare a script to create ECR tags for the docker images and save it as `prepare-tags-ecr-for-qa-docker-images.sh` and save it under `jenkins` folder.

```bash
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-admin-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_ADMIN_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:admin-server-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-api-gateway/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_API_GATEWAY="${ECR_REGISTRY}/${APP_REPO_NAME}:api-gateway-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-config-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_CONFIG_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:config-server-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-customers-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_CUSTOMERS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:customers-service-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-discovery-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_DISCOVERY_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:discovery-server-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-hystrix-dashboard/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_HYSTRIX_DASHBOARD="${ECR_REGISTRY}/${APP_REPO_NAME}:hystrix-dashboard-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-vets-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_VETS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:vets-service-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-visits-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_VISITS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:visits-service-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
export IMAGE_TAG_GRAFANA_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:grafana-service"
export IMAGE_TAG_PROMETHEUS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:prometheus-service"
```

- Prepare a script to build the dev docker images tagged for ECR registry and save it as `build-qa-docker-images-for-ecr.sh` and save it under `jenkins` folder.

```bash
docker build --force-rm -t "${IMAGE_TAG_ADMIN_SERVER}" "${WORKSPACE}/spring-petclinic-admin-server"
docker build --force-rm -t "${IMAGE_TAG_API_GATEWAY}" "${WORKSPACE}/spring-petclinic-api-gateway"
docker build --force-rm -t "${IMAGE_TAG_CONFIG_SERVER}" "${WORKSPACE}/spring-petclinic-config-server"
docker build --force-rm -t "${IMAGE_TAG_CUSTOMERS_SERVICE}" "${WORKSPACE}/spring-petclinic-customers-service"
docker build --force-rm -t "${IMAGE_TAG_DISCOVERY_SERVER}" "${WORKSPACE}/spring-petclinic-discovery-server"
docker build --force-rm -t "${IMAGE_TAG_HYSTRIX_DASHBOARD}" "${WORKSPACE}/spring-petclinic-hystrix-dashboard"
docker build --force-rm -t "${IMAGE_TAG_VETS_SERVICE}" "${WORKSPACE}/spring-petclinic-vets-service"
docker build --force-rm -t "${IMAGE_TAG_VISITS_SERVICE}" "${WORKSPACE}/spring-petclinic-visits-service"
docker build --force-rm -t "${IMAGE_TAG_GRAFANA_SERVICE}" "${WORKSPACE}/docker/grafana"
docker build --force-rm -t "${IMAGE_TAG_PROMETHEUS_SERVICE}" "${WORKSPACE}/docker/prometheus"
```

- Prepare a script to push the dev docker images to the ECR repo and save it as `push-qa-docker-images-to-ecr.sh` and save it under `jenkins` folder.

```bash
aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}
docker push "${IMAGE_TAG_ADMIN_SERVER}"
docker push "${IMAGE_TAG_API_GATEWAY}"
docker push "${IMAGE_TAG_CONFIG_SERVER}"
docker push "${IMAGE_TAG_CUSTOMERS_SERVICE}"
docker push "${IMAGE_TAG_DISCOVERY_SERVER}"
docker push "${IMAGE_TAG_HYSTRIX_DASHBOARD}"
docker push "${IMAGE_TAG_VETS_SERVICE}"
docker push "${IMAGE_TAG_VISITS_SERVICE}"
docker push "${IMAGE_TAG_GRAFANA_SERVICE}"
docker push "${IMAGE_TAG_PROMETHEUS_SERVICE}"
```

- Prepare a docker compose file for swarm deployment on QA environment and save it as `docker-compose-swarm-qa.yml`.

```yaml
version: '3.8'

services:
  config-server:
    image: "${IMAGE_TAG_CONFIG_SERVER}"
    deploy:
      resources:
        limits:
          memory: 512M
    networks:
      - clarusnet
    ports:
     - 8888:8888

  discovery-server:
    image: "${IMAGE_TAG_DISCOVERY_SERVER}"
    deploy:
      resources:
        limits:
          memory: 512M
    depends_on:
      - config-server
    entrypoint: ["./dockerize","-wait=tcp://config-server:8888","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
     - 8761:8761

  customers-service:
    image: "${IMAGE_TAG_CUSTOMERS_SERVICE}"
    deploy:
      resources:
        limits:
          memory: 512M
      replicas: 3
      update_config:
          parallelism: 2
          delay: 5s
          order: start-first
    depends_on:
     - config-server
     - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
    - 8081:8081

  visits-service:
    image: "${IMAGE_TAG_VISITS_SERVICE}"
    deploy:
      resources:
        limits:
          memory: 512M
      replicas: 3
      update_config:
          parallelism: 2
          delay: 5s
          order: start-first
    depends_on:
     - config-server
     - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
     - 8082:8082

  vets-service:
    image: "${IMAGE_TAG_VETS_SERVICE}"
    deploy:
      resources:
        limits:
          memory: 512M
      replicas: 3
      update_config:
          parallelism: 2
          delay: 5s
          order: start-first
    depends_on:
     - config-server
     - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
     - 8083:8083

  api-gateway:
    image: "${IMAGE_TAG_API_GATEWAY}"
    deploy:
      resources:
        limits:
          memory: 512M
      replicas: 5
      update_config:
          parallelism: 2
          delay: 5s
          order: start-first
    depends_on:
     - config-server
     - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
     - 8080:8080

  tracing-server:
    image: openzipkin/zipkin
    deploy:
      resources:
        limits:
          memory: 512M
    environment:
    - JAVA_OPTS=-XX:+UnlockExperimentalVMOptions -Djava.security.egd=file:/dev/./urandom
    networks:
      - clarusnet
    ports:
     - 9411:9411

  admin-server:
    image: "${IMAGE_TAG_ADMIN_SERVER}"
    deploy:
      resources:
        limits:
          memory: 512M
    depends_on:
     - config-server
     - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
     - 9090:9090

  hystrix-dashboard:
    image: "${IMAGE_TAG_HYSTRIX_DASHBOARD}"
    deploy:
      resources:
        limits:
          memory: 512M
    depends_on:
     - config-server
     - discovery-server
    entrypoint: ["./dockerize","-wait=tcp://discovery-server:8761","-timeout=60s","--","java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    networks:
      - clarusnet
    ports:
     - 7979:7979

  ## Grafana / Prometheus

  grafana-server:
    image: "${IMAGE_TAG_GRAFANA_SERVICE}"
    deploy:
      resources:
        limits:
          memory: 256M
    networks:
      - clarusnet
    ports:
    - 3000:3000

  prometheus-server:
    image: "${IMAGE_TAG_PROMETHEUS_SERVICE}"
    deploy:
      resources:
        limits:
          memory: 256M
    networks:
      - clarusnet
    ports:
    - 9091:9090
  
  mysql-server:
    image: mysql:5.7.8
    environment: 
      MYSQL_ROOT_PASSWORD: petclinic
      MYSQL_DATABASE: petclinic
    networks:
      - clarusnet
    ports:
      - 3306:3306

networks:
  clarusnet:
    driver: overlay
```

- Create Ansible playbook for deploying app on QA environment using docker compose file and save it as `pb_deploy_app_on_qa_environment.yaml` under `ansible/playbooks` folder.

```yaml
---
- hosts: role_grand_master
  tasks:
  - name: Copy docker compose file to grand master
    copy:
      src: "{{ workspace }}/docker-compose-swarm-qa-tagged.yml"
      dest: /home/ec2-user/docker-compose-swarm-qa-tagged.yml

  - name: get login credentials for ecr
    shell: "export PATH=$PATH:/usr/local/bin/ && aws ecr get-login-password --region {{ aws_region }} | docker login --username AWS --password-stdin {{ ecr_registry }}"
    register: output

  - name: deploy the app stack on swarm
    shell: "docker stack deploy --with-registry-auth -c /home/ec2-user/docker-compose-swarm-qa-tagged.yml {{ app_name }}"
    register: output

  - debug: msg="{{ output.stdout }}"
```

- Prepare a script to deploy the application on QA environment and save it as `deploy_app_on_qa_environment.sh` under `ansible/scripts` folder.

```bash
PATH="$PATH:/usr/local/bin"
APP_NAME="petclinic"
sed -i "s/APP_STACK_NAME/${APP_STACK_NAME}/" ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml
envsubst < docker-compose-swarm-qa.yml > docker-compose-swarm-qa-tagged.yml
ansible-playbook -i ./ansible/inventory/qa_stack_dynamic_inventory_aws_ec2.yaml -b --extra-vars "workspace=${WORKSPACE} app_name=${APP_NAME} aws_region=${AWS_REGION} ecr_registry=${ECR_REGISTRY}" ./ansible/playbooks/pb_deploy_app_on_qa_environment.yaml
```

- Commit the change, then push the script to the remote repo.

```bash
git add .
git commit -m 'added build scripts for QA Environment'
git push --set-upstream origin feature/msp-19
git checkout dev
git merge feature/msp-19
git push origin dev
```

## MSP 20 - Build and Deploy App on QA Environment Manually

- Create `feature/msp-20` branch from `dev`.

```bash
git checkout dev
git branch feature/msp-20
git checkout feature/msp-20
```

- Create a Jenkins Job with name of `build-and-deploy-petclinic-on-qa-env` to build and deploy the app on `QA environment` manually on `release` branch using following script, and save the script as `build-and-deploy-petclinic-on-qa-env-manually.sh` under `jenkins` folder.

```bash
PATH="$PATH:/usr/local/bin"
APP_NAME="petclinic"
APP_REPO_NAME="clarusway-repo/petclinic-app-qa"
APP_STACK_NAME="Call-petclinic-App-QA-1"
CFN_KEYPAIR="call-petclinic-qa.key"
AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
AWS_REGION="us-east-1"
ECR_REGISTRY="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
export ANSIBLE_PRIVATE_KEY_FILE="${JENKINS_HOME}/.ssh/${CFN_KEYPAIR}"
export ANSIBLE_HOST_KEY_CHECKING="False"
echo 'Packaging the App into Jars with Maven'
. ./jenkins/package-with-maven-container.sh
echo 'Preparing QA Tags for Docker Images'
. ./jenkins/prepare-tags-ecr-for-qa-docker-images.sh
echo 'Building App QA Images'
. ./jenkins/build-qa-docker-images-for-ecr.sh
echo "Pushing App QA Images to ECR Repo"
. ./jenkins/push-qa-docker-images-to-ecr.sh
echo 'Deploying App on Swarm'
. ./ansible/scripts/deploy_app_on_qa_environment.sh
echo 'Deleting all local images'
docker image prune -af
```

- Commit the change, then push the script to the remote repo.

```bash
git add .
git commit -m 'added script for jenkins job to build and deploy app on QA environment'
git push --set-upstream origin feature/msp-20
git checkout dev
git merge feature/msp-20
git push origin dev
```

- Merge `dev` into `release` branch, then run `build-and-deploy-petclinic-on-qa-env` job to build and deploy the app on `QA environment` manually.

```bash
git checkout release
git merge dev
git push origin release
```

## MSP 21 - Prepare a QA Pipeline

- Create `feature/msp-21` branch from `dev`.

```bash
git checkout dev
git branch feature/msp-21
git checkout feature/msp-21
```

- Create a QA Pipeline on Jenkins with name of `petclinic-weekly-qa` with following script and configure a `cron job` to trigger the pipeline every Sundays at midnight (`59 23 * * 0`) on `release` branch. Petclinic weekly build pipeline should be built on permanent QA environment.

- Prepare a Jenkinsfile for `petclinic-weekly-qa` builds and save it as `jenkinsfile-petclinic-weekly-qa` under `jenkins` folder.

```groovy
pipeline {
    agent any
    environment {
        PATH=sh(script:"echo $PATH:/usr/local/bin", returnStdout:true).trim()
        APP_NAME="petclinic"
        APP_REPO_NAME="clarusway-repo/petclinic-app-qa"
        APP_STACK_NAME="Matt-petclinic-App-QA-1"
        CFN_KEYPAIR="matt-petclinic-qa.key"
        AWS_ACCOUNT_ID=sh(script:'export PATH="$PATH:/usr/local/bin" && aws sts get-caller-identity --query Account --output text', returnStdout:true).trim()
        AWS_REGION="us-east-1"
        ECR_REGISTRY="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
        ANSIBLE_PRIVATE_KEY_FILE="${JENKINS_HOME}/.ssh/${CFN_KEYPAIR}"
        ANSIBLE_HOST_KEY_CHECKING="False"
    }
    stages {
        stage('Package Application') {
            steps {
                echo 'Packaging the app into jars with maven'
                sh ". ./jenkins/package-with-maven-container.sh"
            }
        }
        stage('Prepare Tags for Docker Images') {
            steps {
                echo 'Preparing Tags for Docker Images'
                script {
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-admin-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_ADMIN_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:admin-server-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-api-gateway/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_API_GATEWAY="${ECR_REGISTRY}/${APP_REPO_NAME}:api-gateway-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-config-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_CONFIG_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:config-server-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-customers-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_CUSTOMERS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:customers-service-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-discovery-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_DISCOVERY_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:discovery-server-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-hystrix-dashboard/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_HYSTRIX_DASHBOARD="${ECR_REGISTRY}/${APP_REPO_NAME}:hystrix-dashboard-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-vets-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_VETS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:vets-service-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-visits-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_VISITS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:visits-service-qa-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    env.IMAGE_TAG_GRAFANA_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:grafana-service"
                    env.IMAGE_TAG_PROMETHEUS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:prometheus-service"
                }
            }
        }
        stage('Build App Docker Images') {
            steps {
                echo 'Building App Dev Images'
                sh ". ./jenkins/build-qa-docker-images-for-ecr.sh"
                sh 'docker image ls'
            }
        }
        stage('Push Images to ECR Repo') {
            steps {
                echo "Pushing ${APP_NAME} App Images to ECR Repo"
                sh ". ./jenkins/push-qa-docker-images-to-ecr.sh"
            }
        }
        stage('Deploy App on Docker Swarm'){
            steps {
                echo 'Deploying App on Swarm'
                sh '. ./ansible/scripts/deploy_app_on_qa_environment.sh'
            }
        }
    }
    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}
```

- Commit the change, then push the script to the remote repo.

```bash
git add .
git commit -m 'added jenkinsfile petclinic-weekly-qa for release branch'
git push --set-upstream origin feature/msp-21
git checkout dev
git merge feature/msp-21
git push origin dev
```

- Merge `dev` into `release` branch to build and deploy the app on `QA environment` with pipeline.

```bash
git checkout release
git merge dev
git push origin release
```
## MSP 22 - Prepare High-availability RKE Kubernetes Cluster on AWS EC2

* Create `feature/msp-22` branch from `release`.

``` bash
git checkout release
git branch feature/msp-22
git checkout feature/msp-22
```

* Explain [Rancher Container Management Tool](https://rancher.com/docs/rancher/v2.x/en/overview/architecture/).

* Create an IAM Policy with name of `call-rke-controlplane-policy.json` and also save it under `infrastructure` for `Control Plane` node to enable Rancher to create or remove EC2 resources.

``` json
{
"Version": "2012-10-17",
"Statement": [
  {
    "Effect": "Allow",
    "Action": [
      "autoscaling:DescribeAutoScalingGroups",
      "autoscaling:DescribeLaunchConfigurations",
      "autoscaling:DescribeTags",
      "ec2:DescribeInstances",
      "ec2:DescribeRegions",
      "ec2:DescribeRouteTables",
      "ec2:DescribeSecurityGroups",
      "ec2:DescribeSubnets",
      "ec2:DescribeVolumes",
      "ec2:CreateSecurityGroup",
      "ec2:CreateTags",
      "ec2:CreateVolume",
      "ec2:ModifyInstanceAttribute",
      "ec2:ModifyVolume",
      "ec2:AttachVolume",
      "ec2:AuthorizeSecurityGroupIngress",
      "ec2:CreateRoute",
      "ec2:DeleteRoute",
      "ec2:DeleteSecurityGroup",
      "ec2:DeleteVolume",
      "ec2:DetachVolume",
      "ec2:RevokeSecurityGroupIngress",
      "ec2:DescribeVpcs",
      "elasticloadbalancing:AddTags",
      "elasticloadbalancing:AttachLoadBalancerToSubnets",
      "elasticloadbalancing:ApplySecurityGroupsToLoadBalancer",
      "elasticloadbalancing:CreateLoadBalancer",
      "elasticloadbalancing:CreateLoadBalancerPolicy",
      "elasticloadbalancing:CreateLoadBalancerListeners",
      "elasticloadbalancing:ConfigureHealthCheck",
      "elasticloadbalancing:DeleteLoadBalancer",
      "elasticloadbalancing:DeleteLoadBalancerListeners",
      "elasticloadbalancing:DescribeLoadBalancers",
      "elasticloadbalancing:DescribeLoadBalancerAttributes",
      "elasticloadbalancing:DetachLoadBalancerFromSubnets",
      "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
      "elasticloadbalancing:ModifyLoadBalancerAttributes",
      "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
      "elasticloadbalancing:SetLoadBalancerPoliciesForBackendServer",
      "elasticloadbalancing:AddTags",
      "elasticloadbalancing:CreateListener",
      "elasticloadbalancing:CreateTargetGroup",
      "elasticloadbalancing:DeleteListener",
      "elasticloadbalancing:DeleteTargetGroup",
      "elasticloadbalancing:DescribeListeners",
      "elasticloadbalancing:DescribeLoadBalancerPolicies",
      "elasticloadbalancing:DescribeTargetGroups",
      "elasticloadbalancing:DescribeTargetHealth",
      "elasticloadbalancing:ModifyListener",
      "elasticloadbalancing:ModifyTargetGroup",
      "elasticloadbalancing:RegisterTargets",
      "elasticloadbalancing:SetLoadBalancerPoliciesOfListener",
      "iam:CreateServiceLinkedRole",
      "kms:DescribeKey"
    ],
    "Resource": [
      "*"
    ]
  }
]
}
```

* Create an IAM Policy with name of `call-rke-etcd-worker-policy.json` and also save it under `infrastructure` for `etcd` or `worker` nodes to enable Rancher to get information from EC2 resources.

```json
{
"Version": "2012-10-17",
"Statement": [
    {
        "Effect": "Allow",
        "Action": [
            "ec2:DescribeInstances",
            "ec2:DescribeRegions",
            "ecr:GetAuthorizationToken",
            "ecr:BatchCheckLayerAvailability",
            "ecr:GetDownloadUrlForLayer",
            "ecr:GetRepositoryPolicy",
            "ecr:DescribeRepositories",
            "ecr:ListImages",
            "ecr:BatchGetImage"
        ],
        "Resource": "*"
    }
]
}
```

* Create an IAM Role with name of `rke-role` to attach RKE nodes (instances) using `rke-controlplane-policy` and `rke-etcd-worker-policy`.

* Create a security group for External Application Load Balancer of Rancher with name of `rke-alb-sg` and allow HTTP (Port 80) and HTTPS (Port 443) connections from anywhere.
  
* Create a security group for RKE Kubernetes Cluster with name of `rke-cluster-sg` and define following inbound and outbound rules.

  * Inbound rules;

    * Allow HTTP protocol (TCP on port 80) from Application Load Balancer.

    * Allow HTTPS protocol (TCP on port 443) from any source that needs to use Rancher UI or API.

    * Allow TCP on port 6443 from any source that needs to use Kubernetes API server(ex. Jenkins Server).
  
    * Allow SSH on port 22 to any node IP that installs Docker (ex. Jenkins Server).

  * Outbound rules; (aslinda burada all traffic deseydik asagidaki poirtlari acmaya gerek kalmazdi ancak rancher dokumantasyonda yazdigi icin bu sekilde yapiyoruz ancak ileride kurulum asamasinda buraya gelip all traffic diyecegiz!!! Simdiden all traffic diyebiliriz)

    * Allow SSH protocol (TCP on port 22) to any node IP from a node created using Node Driver.

    * Allow HTTP protocol (TCP on port 80) to all IP for getting updates.
    
    * Allow HTTPS protocol (TCP on port 443) to `35.160.43.145/32`, `35.167.242.46/32`, `52.33.59.17/32` for catalogs of `git.rancher.io`.

    * Allow TCP on port 2376 to any node IP from a node created using Node Driver for Docker machine TLS port.
    # Create sec grup diyoruz. Sonra tekrar bu sec grubunun hem inbound hemde outbound kismina kendisini(secgrp id) ekliyoruz, alttaki satirda yazan communication islemini saglamak icin.
  * Allow all protocol on all port from `rke-cluster-sg` for self communication between Rancher `controlplane`, `etcd`, `worker` nodes.

* Log into Jenkins Server and create `rancher.pem` key-pair for Rancher Server using AWS CLI.
# (Cluster'a jenkins server ile baglanacagimiz icin bu islemi yapiyoruz aksi halde gerek yoktu)
  
```bash
aws ec2 create-key-pair --region us-east-1 --key-name rancher.pem --query KeyMaterial --output text > ~/.ssh/rancher.pem
chmod 400 ~/.ssh/rancher.pem
```

* Launch an EC2 instance using `Ubuntu Server 20.04 LTS (HVM) (64-bit x86)` with `t2.medium` type, 16 GB root volume,  `rke-cluster-sg` security group, `rke-role` IAM Role, `Name:Rancher-Cluster-Instance` tag and `rancher.pem` key-pair. Take note of `subnet id` of EC2. 

* Attach a tag to the `nodes (intances)`, `subnets` and `security group` for Rancher with `Key = kubernetes.io/cluster/Rancher` and `Value = owned`. 
# Rancher Cluster Instance-Cluster security gruba ve instance -->'Networking' --> subnet yukaridaki tag ekliyoruz)
  
* Install `kubectl` on Jenkins Server. [Install and Set up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl)

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.9/2020-11-02/bin/linux/amd64/kubectl
sudo mv kubectl /usr/local/bin/kubectl
chmod +x /usr/local/bin/kubectl
kubectl version --short --client
```  
  
* Log into `Rancher-Cluster-Instance` from Jenkins Server (Bastion host) and install Docker using the following script.
# Jenkins server terminaline Rancher instance'in connection bilgisini girecegiz.

cd .ssh
ssh -i "rancher.pem" ubuntu@ec2-3-239-227-112.compute-1.amazonaws.com

```bash
# Set hostname of instance
sudo hostnamectl set-hostname rancher-instance-1
# Update OS 
sudo apt-get update -y
sudo apt-get upgrade -y
# Install and start Docker on Ubuntu 19.03
# Update the apt package index and install packages to allow apt to use a repository over HTTPS
sudo apt-get install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg \
  lsb-release
# Add Docker’s official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
# Burada donup kalabilir cunku sec grupda outbound all traffic izin vermedik. Olusan ..keyring.gpg file sudo rm komutu ile silerek, sec grupda Outbound--> All traffic--> Everywhre yaptik dan sonra bu komutu calistiralim.

# Use the following command to set up the stable repository 
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# Update packages
sudo apt-get update

# Install and start Docker
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo systemctl enable docker

# Add ubuntu user to docker group
sudo usermod -aG docker ubuntu
newgrp docker
```

* Create a target groups with name of `rancher-http-80-tg` with following setup and add the `rancher instances` to it.

```bash
Target type         : instance
Protocol            : HTTP
Port                : 80

<!-- Health Checks Settings -->
Protocol            : HTTP
Path                : /healthz
Port                : traffic port
Healthy threshold   : 3
Unhealthy threshold : 3
Timeout             : 5 seconds
Interval            : 10 seoconds
Success             : 200
```

* Create Application Load Balancer with name of `rancher-alb` using `rke-alb-sg` security group with following settings and add `rancher-http-80-tg` target group to it.

```text
Scheme              : internet-facing
IP address type     : ipv4

<!-- Listeners-->
Protocol            : HTTPS/HTTP
Port                : 443/80
Availability Zones  : Select AZs of RKE instances
Target group        : `rancher-http-80-tg` target group 
```

* Configure ALB --> Listener (edit) of HTTP on `Port 80` to redirect traffic to HTTPS on `Port 443`. (Sadece secure porttan talepler kabul edilsin) 
# Target grubu, load balancer'a register yapmak onemli, kontrol et!!!

# Create DNS A record for `rancher.devopsyasin.com` and attach the `yasin-rancher-alb` application load balancer to it.
# Route53 --> hosted zone --> devopsyasin.com --> create record--> simple routing--> Define simple record--> 
# --> 'rancher'devopsyasin.com--> record type:A --> value:Alias the application loadbalacer--> region-->dual.simplerecord-->create-->create record

* Install RKE, the Rancher Kubernetes Engine, [Kubernetes distribution and command-line tool](https://rancher.com/docs/rke/latest/en/installation/)) on Jenkins Server.
- Come to jenkins server on terminal (by typing exit from rancher instance)
```bash
curl -SsL "https://github.com/rancher/rke/releases/download/v1.3.7/rke_linux-amd64" -o "rke_linux-amd64"
sudo mv rke_linux-amd64 /usr/local/bin/rke
chmod +x /usr/local/bin/rke
rke --version
```
# Suana kadar rancher-ec2'ya IAM role, key create, tags verdik, load balancer olusturduk. Son olarak jenkins server'a rke install ettik.
# Rancer clusterlari yonetiyor ancak kendisi de bir cluster icerisinde calisiyor. Bunun icin rancher-ec2 icinde cluster kuracagiz bunuda jenkins server daki 'rke' ile 'helm' yardimi ile yapacagiz.

"https://www.rancher.com/docs/rke/latest/en/config-options/" for more informtaion and to see yaml file below.

* Create `rancher-cluster.yml` with following content to configure RKE Kubernetes Cluster and save it under `infrastructure` folder.

```yaml
nodes:
  - address: 172.31.82.64            #private ip of rancher server
    internal_address: 172.31.82.64   #private ip of rancher server
    user: ubuntu
    role:
      - controlplane
      - etcd
      - worker

# ignore_docker_version: true

services:
  etcd:
    snapshot: true
    creation: 6h
    retention: 24h

ssh_key_path: ~/.ssh/rancher.pem

# Required for external TLS termination with
# ingress-nginx v0.22+
ingress:
  provider: nginx
  options:
    use-forwarded-headers: "true"
```

* Run `rke` command to setup RKE Kubernetes cluster on EC2 Rancher instance *`Warning:` You should add rule to cluster sec group for Jenkins Server using its `IP/32` from SSH (22) and TCP(6443) before running `rke` command, because it is giving connection error.*
- Bu kisma gerek yok biz cunku Jenkinse her yone actik. 

```bash
cd petclinic-microservices-with-db/infrastructure
rke up --config ./rancher-cluster.yml
```

* Check if the RKE Kubernetes Cluster created successfully.

```bash
mkdir -p ~/.kube
mv ./kube_config_rancher-cluster.yml $HOME/.kube/config
chmod 400 ~/.kube/config
kubectl get nodes
kubectl get pods --all-namespaces
```
- Suana kadar EC2-rancher icersine kubernetes cluster kurduk. Bi sonraki adim da buna rancher uygulamasini deploy edecegiZ.
* Commit the change, then push the script to the remote repo.

``` bash
git add .
git commit -m 'added rancher setup files'
git push --set-upstream origin feature/msp-22
git checkout release
git merge feature/msp-22
git push origin release
```

## MSP 23 - Install Rancher App on RKE Kubernetes Cluster

* Install Helm [version 3+](https://github.com/helm/helm/releases) on Jenkins Server. [Introduction to Helm](https://helm.sh/docs/intro/). [Helm Installation](https://helm.sh/docs/intro/install/).

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
helm version
```

* Add helm chart repositories of Rancher. 
# Rancheri clustera kurarken helm den aliyoruz chartlari, servis deploy vs onlarca yml yerine direk helm repodan aliyoruz

```bash
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
helm repo list
```

* Create a namespace for Rancher.

```bash
kubectl create namespace cattle-system
```

* Install Rancher on RKE Kubernetes Cluster using Helm.

```bash
helm install rancher rancher-latest/rancher \     # komutlari tek tek gir
  --namespace cattle-system \
  --set hostname=rancher.devopsyasin.com \        # root53 dns name aldik       
  --set tls=external \
  --set replicas=1
```

* Check if the Rancher Server is deployed successfully.
  
```bash
kubectl -n cattle-system get deploy rancher
kubectl -n cattle-system get pods
```
# Rancher calisdigini browser dan gormek icin; "rancher.devopsyasin.com" Eger 'temproraly not available" uyarisi alirsan;
# Target group içerisinde bulunun rancher instance'ın healthy olduğundan emin olun, load balancer'ı ve listener'ları kontrol edin.

## MSP 24 - Prepare Petlinic Kubernetes YAML Files

* Create `feature/msp-24` branch from `release`.

``` bash
git checkout release
git branch feature/msp-24
git checkout feature/msp-24
```
# 'Docker kompose' hazir olan docker compose yaml file larini kubernetes yaml file'na ceviren bir tool.
# Biz burada elimizde olan docker yaml files'lari paketleyip bir helm chart'a cevirecegiz bu chartlari S3 bucket da saklayacagiz sonrada bunu k8s yaml file'ina donusturerek uygulamyi deploy edecegiz.
# Biz en basta test asamalarin da docker swarm yerine k8s kullanmis olsaydik buna gerek kalmayacakti. Bu cevirme islemini yapmasak tek tek k8s yaml file lar hazirlayacaktik, daha kolay olsun diye ceviriyor.
# Kompose gibi Kustomize da var, K8s files lari customize etmek icin kullaniliyor.(https://kustomize.io/) 

* Create a folder with name of `k8s` for keeping the deployment files of Petclinic App on Kubernetes cluster.

* Create a `docker-compose.yml` under `k8s` folder with the following content as to be used in conversion the k8s files.

# Bu docker-compose.yml file'ini, k8s yaml files larina cevirecegiz. Icerikdeki herseyi cevirmiyor (mesela depends on) bunlar icinde labell lar ile ek bilgi giriliyor, bu yuzden burada sadece cevirilecek olanlari yazdik. (https://kompose.io/) buradan bakilabilir.
# Daha onceki docker-compose.yml file'ini kullanmiyoruz cunku burada 'Values' variable'i ile imageleri helm repodan alcak.

```yaml
version: '3'
services:
  config-server:
    image: "{{ .Values.IMAGE_TAG_CONFIG_SERVER }}"
    ports:
     - 8888:8888
    labels:
      kompose.image-pull-secret: "regcred"
  discovery-server:
    image: "{{ .Values.IMAGE_TAG_DISCOVERY_SERVER }}"
    ports:
     - 8761:8761
    labels:
      kompose.image-pull-secret: "regcred"
  customers-service:
    image: "{{ .Values.IMAGE_TAG_CUSTOMERS_SERVICE }}"
    deploy:
      replicas: 2
    ports:
    - 8081:8081
    labels:
      kompose.image-pull-secret: "regcred"
  visits-service:
    image: "{{ .Values.IMAGE_TAG_VISITS_SERVICE }}"
    deploy:
      replicas: 2
    ports:
     - 8082:8082
    labels:
      kompose.image-pull-secret: "regcred"
  vets-service:
    image: "{{ .Values.IMAGE_TAG_VETS_SERVICE }}"
    deploy:
      replicas: 2
    ports:
     - 8083:8083
    labels:
      kompose.image-pull-secret: "regcred"
  api-gateway:
    image: "{{ .Values.IMAGE_TAG_API_GATEWAY }}"
    deploy:
      replicas: 1
    ports:
     - 8080:8080
    labels:
      kompose.image-pull-secret: "regcred"
      kompose.service.expose: "{{ .Values.DNS_NAME }}"
      kompose.service.type: "nodeport"
  tracing-server:
    image: openzipkin/zipkin
    environment:
    - JAVA_OPTS=-XX:+UnlockExperimentalVMOptions -Djava.security.egd=file:/dev/./urandom
    ports:
     - 9411:9411
  admin-server:
    image: "{{ .Values.IMAGE_TAG_ADMIN_SERVER }}"
    ports:
     - 9090:9090
    labels:
      kompose.image-pull-secret: "regcred"
  hystrix-dashboard:
    image: "{{ .Values.IMAGE_TAG_HYSTRIX_DASHBOARD }}"
    ports:
     - 7979:7979
    labels:
      kompose.image-pull-secret: "regcred"
  grafana-server:
    image: "{{ .Values.IMAGE_TAG_GRAFANA_SERVICE }}"
    ports:
    - 3000:3000
    labels:
      kompose.image-pull-secret: "regcred"
  prometheus-server:
    image: "{{ .Values.IMAGE_TAG_PROMETHEUS_SERVICE }}"
    ports:
    - 9091:9090
    labels:
      kompose.image-pull-secret: "regcred"

  mysql-server:
    image: mysql:5.7.8
    environment: 
      MYSQL_ROOT_PASSWORD: petclinic
      MYSQL_DATABASE: petclinic
    ports:
    - 3306:3306
```

* Install [conversion tool](https://kompose.io/installation/) named `Kompose` on your Jenkins Server. [User Guide](https://kompose.io/user-guide/#user-guide)

```bash
curl -L https://github.com/kubernetes/kompose/releases/download/v1.26.1/kompose-linux-amd64 -o kompose
chmod +x kompose
sudo mv ./kompose /usr/local/bin/kompose
kompose version
```

* Install Helm [version 3+](https://github.com/helm/helm/releases) on Jenkins Server. [Introduction to Helm](https://helm.sh/docs/intro/). [Helm Installation](https://helm.sh/docs/intro/install/).

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
helm version
```

* Create an helm chart named `petclinic_chart` under `k8s` folder.

```bash
cd k8s
helm create petclinic_chart
```

* Remove all files under the petclinic_chart/templates folder.

```bash
rm -r petclinic_chart/templates/*
```
  
* Convert the `docker-compose.yml` into k8s/petclinic_chart/templates objects and save under `k8s/petclinic_chart` folder.

```bash
kompose convert -f docker-compose.yml -o petclinic_chart/templates
```

* Update deployment files with `init-containers` to launch microservices in sequence. See [Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/).

# Daha once uygulamamizi servisler (Config-server discovery-server Api-gateway customer ve visit) ile ayaga kaldiriyorduk, kubernetes de initcontainers ile yapacagiz. 
# Copy to below 'resources: {}' in ...deployment.yml files and 'initContainers' must be same line (indentation) with 'containers..'
```yaml
# for discovery server deployment files
      initContainers:
      - name: init-config-server
        image: busybox
        command: ['sh', '-c', 'until nc -z config-server:8888; do echo waiting for config-server; sleep 2; done;']
# for all other microservices deployment except config-server and discovery-server
      initContainers:
      - name: init-discovery-server
        image: busybox
        command: ['sh', '-c', 'until nc -z discovery-server:8761; do echo waiting for discovery-server; sleep 2; done;']
``` 
* Update `spec.rules.host` field of `api-gateway-ingress.yaml` file as below.

```yaml
'{{ .Values.DNS_NAME }}'
```

* Add `k8s/petclinic_chart/values-template.yaml` file as below.
# Daha once jenkins server da tag ler olusturmustuk, imagelerin taglerini oradan alacak.

```yaml
IMAGE_TAG_CONFIG_SERVER: "${IMAGE_TAG_CONFIG_SERVER}"
IMAGE_TAG_DISCOVERY_SERVER: "${IMAGE_TAG_DISCOVERY_SERVER}"
IMAGE_TAG_CUSTOMERS_SERVICE: "${IMAGE_TAG_CUSTOMERS_SERVICE}"
IMAGE_TAG_VISITS_SERVICE: "${IMAGE_TAG_VISITS_SERVICE}"
IMAGE_TAG_VETS_SERVICE: "${IMAGE_TAG_VETS_SERVICE}"
IMAGE_TAG_API_GATEWAY: "${IMAGE_TAG_API_GATEWAY}"
IMAGE_TAG_ADMIN_SERVER: "${IMAGE_TAG_ADMIN_SERVER}"
IMAGE_TAG_HYSTRIX_DASHBOARD: "${IMAGE_TAG_HYSTRIX_DASHBOARD}"
IMAGE_TAG_GRAFANA_SERVICE: "${IMAGE_TAG_GRAFANA_SERVICE}"
IMAGE_TAG_PROMETHEUS_SERVICE: "${IMAGE_TAG_PROMETHEUS_SERVICE}"
DNS_NAME: <"DNS Name of your application">                         #Enter your dns name not "rancher dns"
```

* Check if the petclinic_chart working as expected. (Test etmek icin yapiyoruz)

```bash
export IMAGE_TAG_CONFIG_SERVER="testing-image-1"    
export IMAGE_TAG_DISCOVERY_SERVER="testing-image-2" 
export IMAGE_TAG_CUSTOMERS_SERVICE="testing-image-3"
export IMAGE_TAG_VISITS_SERVICE="testing-image-4"   
export IMAGE_TAG_VETS_SERVICE="testing-image-5"     
export IMAGE_TAG_API_GATEWAY="testing-image-6"      
export IMAGE_TAG_ADMIN_SERVER="testing-image-7"     
export IMAGE_TAG_HYSTRIX_DASHBOARD="testing-image-8"
export IMAGE_TAG_GRAFANA_SERVICE="testing-image-9"
export IMAGE_TAG_PROMETHEUS_SERVICE="testing-image-10"

# create values.yaml file from template by updating with environments variables(run inside 'petclinic-micro...' if you're inside k8s cd..)
envsubst < k8s/petclinic_chart/values-template.yaml > k8s/petclinic_chart/values.yaml
# test petclinic_chart (Bu komut bize chart olusturacak ve dry run ile cikti olarak konsolda gorecegiz buda test amacli)
helm install ptest k8s/petclinic_chart/ --namespace dev --debug --dry-run
```

### Set up a Helm v3 chart repository in Amazon S3

* This pattern helps you to manage Helm v3 charts efficiently by integrating the Helm v3 repository into Amazon Simple Storage Service (Amazon S3) on the Amazon Web Services (AWS) Cloud. (https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/set-up-a-helm-v3-chart-repository-in-amazon-s3.html)

* Create an S3 bucket for Helm charts. In the bucket, create a folder called stable/myapp. The example in this pattern uses s3://petclinic-helm-charts/stable/myapp as the target chart repository.

* Install the helm-s3 plugin for Amazon S3.

```bash
helm plugin install https://github.com/hypnoglow/helm-s3.git
```

* Initialize the Amazon S3 Helm repository.

```bash
AWS_REGION=us-east-1 helm s3 init s3://yasin-petclinic-helm-charts/stable/myapp 
```

* The command creates an index.yaml file in the target to track all the chart information that is stored at that location.

* Verify that the index.yaml file was created.

```bash
aws s3 ls s3://yasin-petclinic-helm-charts/stable/myapp/
```

* Add the Amazon S3 repository to Helm on the client machine. 

```bash
helm repo list   # rancher-latest  https://releases.rancher.com/server-charts/latest repoyu goruyoruz.

AWS_REGION=us-east-1 helm repo add stable-petclinicapp s3://yasin-petclinic-helm-charts/stable/myapp/
```
* Simdi stable-petclinicapp chart'ini repoya ekledik.

* Update `version` field of `k8s/petclinic_chart/Chart.yaml` file as below for testing.

```yaml
version: 1.1.1
```

* Package the local Helm chart.

```bash
cd k8s
helm package petclinic_chart/   #petclinic_chart-1.1.1.tgz zipli dosya olustu k8s icerisinde kontrol edebilirsin.  
```

* Store the local package in the Amazon S3 Helm repository.

```bash
HELM_S3_MODE=3 AWS_REGION=us-east-1 helm s3 push ./petclinic_chart-1.1.1.tgz stable-petclinicapp
```
# petclinic_chart-1.1.1.tgz chartini s3 'e ekledik

* Search for the Helm chart.

```bash
helm search repo stable-petclinicapp
```

* You get an output as below.

```bash
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                
stable-petclinicapp/petclinic_chart     1.1.1           0.1.0           A Helm chart for Kubernetes
```

* In Chart.yaml, set the `version` value to `1.1.2` in Chart.yaml, and then package the chart, this time changing the version in Chart.yaml to 1.1.2. Version control is ideally achieved through automation by using tools like GitVersion or Jenkins build numbers in a CI/CD pipeline. 

```bash
helm package petclinic_chart/
```

* Push the new version to the Helm repository in Amazon S3.

```bash
HELM_S3_MODE=3 AWS_REGION=us-east-1 helm s3 push ./petclinic_chart-1.1.2.tgz stable-petclinicapp
```

* Verify the updated Helm chart.

```bash
helm repo update
helm search repo stable-petclinicapp
```

* You get an output as below.

```bash
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                
stable-petclinicapp/petclinic_chart     1.1.2           0.1.0           A Helm chart for Kubernetes
```

* To view all the available versions of a chart execute following command.

```bash
helm search repo stable-petclinicapp --versions
```

* Output:

```bash
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                
stable-petclinicapp/petclinic_chart     1.1.2           0.1.0           A Helm chart for Kubernetes
stable-petclinicapp/petclinic_chart     1.1.1           0.1.0           A Helm chart for Kubernetes
```

* Test to installing the chart from the Amazon S3 Helm repository.

```bash
AWS_REGION=us-east-1 helm upgrade --install petclinic-release stable-petclinicapp/petclinic_chart  --version 1.1.2 --namespace dev --debug --dry-run
```

* In Chart.yaml, set the `version` value to `HELM_VERSION` in Chart.yaml for automation in jenkins pipeline.

* Commit the change, then push the script to the remote repo.

``` bash
git add .
git commit -m 'added Configuration YAML Files for Kubernetes Deployment'
git push --set-upstream origin feature/msp-24
git checkout release
git merge feature/msp-24
git push origin release
```

## MSP 25 - Create Staging and Production Environment with Rancher

# Bundan sonraki islemleri rancher sayfasinda yapiyoruz.
# For a Helm installation, run komutunu burada calistirarak password olusturup, rancher sayfasina girerek devam ediyoruz. 
# rancher şifremi unuttum:
KUBECONFIG=~/.kube/config
kubectl --kubeconfig $KUBECONFIG -n cattle-system exec $(kubectl --kubeconfig $KUBECONFIG -n cattle-system get pods -l app=rancher | grep '1/1' | head -1 | awk '{ print $1 }') -- reset-password

- Click 'local' --> Click 'Download Kubeconfig' (local cluster config dosyasi iniyor) Bu dosyanin icerigini terminalde .kube/config dosyasina rewrite yapistiralim. "overwrite onaylamak gerekiyor save icin"

- "Terminalde 'kubectl get nodes' ile controlplane,etcd,worker gormemiz gerekiyor"

* To provide access of Rancher to the cloud resources, create a `Cloud Credentials` for AWS on Rancher and name it as `Yasin-AWS-Training-Account`.
* Rancher da AWS servisleri ile ilgili islem yapabilmek icin.
- Rancher sayfasinda, Home(uc cizgi)--> Cluster management--> Cloud Credentials --> create--> aws --> any name and credentials. 

* Create a `Node Template` on Rancher with following configuration for to be used while launching the EC2 instances and name it as `Yasin-AWS-RancherOs-Template`.

# Bundan sonra ayni islemleri yaparken kolaylik olsun diye template olsuturuyoruz.
Home--> Cluster Managament--> RKE1 Configuration --> Node Templates --> Add Template --> AWS --> Default VPC --> Asagidaki degerler. 
```text
Region            : us-east-1
Security group    : create new sg (rancher-nodes)
Instance Type     : t2.medium
Root Disk Size    : 16 GB
AMI (RancherOS)   : ami-0e8a3347e4c5959bd
SSH User          : rancher
Label             : os=rancheros
```

## MSP 26 - Prepare a Staging Pipeline

* Create `feature/msp-26` branch from `release`.

``` bash
git checkout release
git branch feature/msp-26
git checkout feature/msp-26
```

* Create a Kubernetes cluster using Rancher page with RKE and new nodes in AWS  and name it as `petclinic-cluster-staging`.

```text
Cluster Type      : Amazon EC2
Name Prefix       : petclinic-k8s-instance
Count             : 3
etcd              : checked
Control Plane     : checked
Worker            : checked
```
# it will take some time to get ready refresh the page to see the cluster.
* Create `petclinic-staging-ns` namespace on `petclinic-cluster-staging` with Rancher.
# Cluster--> Project/namespaces--> Create namespaces (on default)--> name:petclinic-staging-ns -->default--> create

* Create a Jenkins Job (freestyle job) and name it as `create-ecr-docker-registry-for-petclinic-staging` to create Docker Registry for `Staging` manually on AWS ECR.

``` bash
PATH="$PATH:/usr/local/bin"
APP_REPO_NAME="yasin-repo/petclinic-app-staging"
AWS_REGION="us-east-1"

aws ecr create-repository \
  --repository-name ${APP_REPO_NAME} \
  --image-scanning-configuration scanOnPush=false \
  --image-tag-mutability MUTABLE \
  --region ${AWS_REGION}
```

* Prepare a script to create ECR tags for the staging docker images and name it as `prepare-tags-ecr-for-staging-docker-images.sh` and save it under `jenkins` folder.
# Onceki scriptler gibi ancak burada 'release' yerine 'staging' yaziyoruz.

``` bash
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-admin-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_ADMIN_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:admin-server-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-api-gateway/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_API_GATEWAY="${ECR_REGISTRY}/${APP_REPO_NAME}:api-gateway-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-config-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_CONFIG_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:config-server-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-customers-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_CUSTOMERS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:customers-service-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-discovery-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_DISCOVERY_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:discovery-server-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-hystrix-dashboard/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_HYSTRIX_DASHBOARD="${ECR_REGISTRY}/${APP_REPO_NAME}:hystrix-dashboard-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-vets-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_VETS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:vets-service-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-visits-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_VISITS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:visits-service-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
export IMAGE_TAG_GRAFANA_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:grafana-service"
export IMAGE_TAG_PROMETHEUS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:prometheus-service"
```

* Prepare a script to build the staging docker images tagged for ECR registry and name it as `build-staging-docker-images-for-ecr.sh` and save it under `jenkins` folder.

``` bash
docker build --force-rm -t "${IMAGE_TAG_ADMIN_SERVER}" "${WORKSPACE}/spring-petclinic-admin-server"
docker build --force-rm -t "${IMAGE_TAG_API_GATEWAY}" "${WORKSPACE}/spring-petclinic-api-gateway"
docker build --force-rm -t "${IMAGE_TAG_CONFIG_SERVER}" "${WORKSPACE}/spring-petclinic-config-server"
docker build --force-rm -t "${IMAGE_TAG_CUSTOMERS_SERVICE}" "${WORKSPACE}/spring-petclinic-customers-service"
docker build --force-rm -t "${IMAGE_TAG_DISCOVERY_SERVER}" "${WORKSPACE}/spring-petclinic-discovery-server"
docker build --force-rm -t "${IMAGE_TAG_HYSTRIX_DASHBOARD}" "${WORKSPACE}/spring-petclinic-hystrix-dashboard"
docker build --force-rm -t "${IMAGE_TAG_VETS_SERVICE}" "${WORKSPACE}/spring-petclinic-vets-service"
docker build --force-rm -t "${IMAGE_TAG_VISITS_SERVICE}" "${WORKSPACE}/spring-petclinic-visits-service"
docker build --force-rm -t "${IMAGE_TAG_GRAFANA_SERVICE}" "${WORKSPACE}/docker/grafana"
docker build --force-rm -t "${IMAGE_TAG_PROMETHEUS_SERVICE}" "${WORKSPACE}/docker/prometheus"
```

* Prepare a script to push the staging docker images to the ECR repo and name it as `push-staging-docker-images-to-ecr.sh` and save it under `jenkins` folder.

``` bash
aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}  #Ecr'a push yapma komutu
docker push "${IMAGE_TAG_ADMIN_SERVER}"
docker push "${IMAGE_TAG_API_GATEWAY}"
docker push "${IMAGE_TAG_CONFIG_SERVER}"
docker push "${IMAGE_TAG_CUSTOMERS_SERVICE}"
docker push "${IMAGE_TAG_DISCOVERY_SERVER}"
docker push "${IMAGE_TAG_HYSTRIX_DASHBOARD}"
docker push "${IMAGE_TAG_VETS_SERVICE}"
docker push "${IMAGE_TAG_VISITS_SERVICE}"
docker push "${IMAGE_TAG_GRAFANA_SERVICE}"
docker push "${IMAGE_TAG_PROMETHEUS_SERVICE}"
```

* Install `Rancher CLI` on Jenkins Server.
# Rancher CLI will be use inside the jenkins pipeline to deploy application inside cluster created by rancher.

```bash (run on home directory:ec2-user@jenkins-server)
curl -SsL "https://github.com/rancher/cli/releases/download/v2.4.13/rancher-linux-amd64-v2.4.13.tar.gz" -o "rancher-cli.tar.gz"
tar -zxvf rancher-cli.tar.gz
sudo mv ./rancher-v2.4.13/rancher /usr/local/bin/rancher
chmod +x /usr/local/bin/rancher
rancher --version
```
  
* Create Rancher API Key [Rancher API Key](https://rancher.com/docs/rancher/v2.x/en/user-settings/api-keys/#creating-an-api-key) to enable access to the `Rancher` server. Take note, `Access Key (username)` and `Secret Key (password)`.

# We need to create credentials for jenkins to connect rancher instance
# Rancher page--> Click Avatar--> Account&API keys--> Create API key--> (scope & A month from now )--> create *Don't close page copy information somewhere you will not see the page again. Then go to jenkins page.

* Create a credentials with kind of `Username with password` on Jenkins Server using the `Rancher API Key`.

  * On jenkins server, select Manage Jenkins --> Manage Credentials --> Jenkins -->   Global credentials (unrestricted) --> Add Credentials.

  * Paste `Access Key (username)` to Username field and `Secret Key (password)` to Password field.

  * Define an id like `rancher-petclinic-credentials`.

* On some systems we need to install Helm S3 plugin as Jenkins user to be able to use S3 with pipeline script. 

``` bash
sudo su -s /bin/bash jenkins
export PATH=$PATH:/usr/local/bin
helm version
helm plugin install https://github.com/hypnoglow/helm-s3.git
exit from jenkins user

* Create a Staging Pipeline on Jenkins with name of `petclinic-staging` with following script and configure a `cron job` to trigger the pipeline every Sundays at midnight (`59 23 * * 0`) on `release` branch. `Petclinic staging pipeline` should be deployed on permanent staging-environment on `petclinic-cluster` Kubernetes cluster under `petclinic-staging-ns` namespace.

* Prepare a Jenkinsfile for `petclinic-staging` pipeline and save it as `jenkinsfile-petclinic-staging` under `jenkins` folder.

``` groovy
pipeline {
    agent any
    environment {
        PATH=sh(script:"echo $PATH:/usr/local/bin", returnStdout:true).trim()
        APP_NAME="petclinic"
        APP_REPO_NAME="yasin-repo/petclinic-app-staging"
        AWS_ACCOUNT_ID=sh(script:'export PATH="$PATH:/usr/local/bin" && aws sts get-caller-identity --query Account --output text', returnStdout:true).trim()
        AWS_REGION="us-east-1"
        ECR_REGISTRY="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
        RANCHER_URL="https://rancher.clarusway.us" # rancher url https://rancher.devopsyasin.com/
        // Get the project-id from Rancher UI (petclinic-cluster-staging namespace, View in API, copy projectId )
        RANCHER_CONTEXT="petclinic-cluster:project-id"    # Project/Namespaces-->click ... on petclinic staging ns-->edit yaml--> copy "c-rn2r8:p-skwz6" here
       //First part of projectID
        CLUSTERID="petclinic-cluster"   # paste first part of id "c-rn2r8"
        RANCHER_CREDS=credentials('rancher-petclinic-credentials') # From jenkins credential which name we gave
    }
    stages {
        stage('Package Application') {
            steps {
                echo 'Packaging the app into jars with maven'
                sh ". ./jenkins/package-with-maven-container.sh"
            }
        }
        stage('Prepare Tags for Staging Docker Images') {
            steps {
                echo 'Preparing Tags for Staging Docker Images'
                script {
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-admin-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_ADMIN_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:admin-server-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-api-gateway/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_API_GATEWAY="${ECR_REGISTRY}/${APP_REPO_NAME}:api-gateway-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-config-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_CONFIG_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:config-server-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-customers-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_CUSTOMERS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:customers-service-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-discovery-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_DISCOVERY_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:discovery-server-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-hystrix-dashboard/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_HYSTRIX_DASHBOARD="${ECR_REGISTRY}/${APP_REPO_NAME}:hystrix-dashboard-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-vets-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_VETS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:vets-service-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-visits-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_VISITS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:visits-service-staging-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    env.IMAGE_TAG_GRAFANA_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:grafana-service"
                    env.IMAGE_TAG_PROMETHEUS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:prometheus-service"
                }
            }
        }
        stage('Build App Staging Docker Images') {
            steps {
                echo 'Building App Staging Images'
                sh ". ./jenkins/build-staging-docker-images-for-ecr.sh"
                sh 'docker image ls'
            }
        }
        stage('Push Images to ECR Repo') {
            steps {
                echo "Pushing ${APP_NAME} App Images to ECR Repo"
                sh ". ./jenkins/push-staging-docker-images-to-ecr.sh"
            }
        }
        stage('Deploy App on Petclinic Kubernetes Cluster'){
            steps {
                echo 'Deploying App on K8s Cluster'
                sh "rancher login $RANCHER_URL --context $RANCHER_CONTEXT --token $RANCHER_CREDS_USR:$RANCHER_CREDS_PSW"
                sh "envsubst < k8s/petclinic_chart/values-template.yaml > k8s/petclinic_chart/values.yaml"
                sh "sed -i s/HELM_VERSION/${BUILD_NUMBER}/ k8s/petclinic_chart/Chart.yaml"
                sh "rancher kubectl delete secret regcred -n petclinic-staging-ns || true"
                sh """          (Bu kisim "https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/")
                rancher kubectl create secret generic regcred -n petclinic-staging-ns \  
                --from-file=.dockerconfigjson=$JENKINS_HOME/.docker/config.json \
                --type=kubernetes.io/dockerconfigjson
                """
                sh "rm -f k8s/config"
                sh "rancher cluster kf $CLUSTERID > k8s/config"
                sh "chmod 400 k8s/config"
                sh "helm repo add stable-petclinic s3://petclinic-helm-charts/stable/myapp/"
                sh "helm package k8s/petclinic_chart"
                sh "helm s3 push petclinic_chart-${BUILD_NUMBER}.tgz stable-petclinic"
                sh "helm repo update"
                sh "AWS_REGION=$AWS_REGION helm upgrade --install petclinic-app-release stable-petclinic/petclinic_chart --version ${BUILD_NUMBER} --namespace petclinic-staging-ns --kubeconfig k8s/config"
            }
        }
    }
    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}
```

* Create an `A` record of `staging-petclinic.devopsyasin.com` in your hosted zone using AWS Route 53 domain registrar and bind it to your `petclinic cluster`.
# route53--> Quick create record--> staging-petclinic--> A routes.. --> Value:paste Public IP one of the cluster node--> create record
* Commit the change, then push the script to the remote repo.

``` bash
git add .
git commit -m 'added jenkinsfile petclinic-staging for release branch'
git push --set-upstream origin feature/msp-26
git checkout release
git merge feature/msp-26
git push origin release
```

## MSP 27 - Prepare a Production Pipeline

* Create `feature/msp-27` branch from `release`.

``` bash
git checkout release
git branch feature/msp-27
git checkout feature/msp-27
```

* Create a Kubernetes cluster using Rancher with RKE and new nodes in AWS (on one EC2 instance only) and name it as `petclinic-cluster`.
# Cluster management--> create--> Enter information below --> create
```text
Cluster Type      : Amazon EC2
Name Prefix       : petclinic-k8s-instance
Count             : 3
etcd              : checked
Control Plane     : checked
Worker            : checked
```

* Create a Jenkins Job and name it as `create-ecr-docker-registry-for-petclinic-prod` to create Docker Registry for `Production` manually on AWS ECR.

``` bash 
PATH="$PATH:/usr/local/bin"
APP_REPO_NAME="yasin-repo/petclinic-app-prod"
AWS_REGION="us-east-1"

aws ecr create-repository \
  --repository-name ${APP_REPO_NAME} \
  --image-scanning-configuration scanOnPush=false \
  --image-tag-mutability MUTABLE \
  --region ${AWS_REGION}
```

* Prepare a script to create ECR tags for the production docker images and name it as `prepare-tags-ecr-for-prod-docker-images.sh` and save it under `jenkins` folder.

``` bash
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-admin-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_ADMIN_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:admin-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-api-gateway/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_API_GATEWAY="${ECR_REGISTRY}/${APP_REPO_NAME}:api-gateway-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-config-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_CONFIG_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:config-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-customers-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_CUSTOMERS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:customers-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-discovery-server/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_DISCOVERY_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:discovery-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-hystrix-dashboard/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_HYSTRIX_DASHBOARD="${ECR_REGISTRY}/${APP_REPO_NAME}:hystrix-dashboard-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-vets-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_VETS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:vets-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
MVN_VERSION=$(. ${WORKSPACE}/spring-petclinic-visits-service/target/maven-archiver/pom.properties && echo $version)
export IMAGE_TAG_VISITS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:visits-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
export IMAGE_TAG_GRAFANA_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:grafana-service"
export IMAGE_TAG_PROMETHEUS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:prometheus-service"
```

* Prepare a script to build the production docker images tagged for ECR registry and name it as `build-prod-docker-images-for-ecr.sh` and save it under `jenkins` folder.

``` bash
docker build --force-rm -t "${IMAGE_TAG_ADMIN_SERVER}" "${WORKSPACE}/spring-petclinic-admin-server"
docker build --force-rm -t "${IMAGE_TAG_API_GATEWAY}" "${WORKSPACE}/spring-petclinic-api-gateway"
docker build --force-rm -t "${IMAGE_TAG_CONFIG_SERVER}" "${WORKSPACE}/spring-petclinic-config-server"
docker build --force-rm -t "${IMAGE_TAG_CUSTOMERS_SERVICE}" "${WORKSPACE}/spring-petclinic-customers-service"
docker build --force-rm -t "${IMAGE_TAG_DISCOVERY_SERVER}" "${WORKSPACE}/spring-petclinic-discovery-server"
docker build --force-rm -t "${IMAGE_TAG_HYSTRIX_DASHBOARD}" "${WORKSPACE}/spring-petclinic-hystrix-dashboard"
docker build --force-rm -t "${IMAGE_TAG_VETS_SERVICE}" "${WORKSPACE}/spring-petclinic-vets-service"
docker build --force-rm -t "${IMAGE_TAG_VISITS_SERVICE}" "${WORKSPACE}/spring-petclinic-visits-service"
docker build --force-rm -t "${IMAGE_TAG_GRAFANA_SERVICE}" "${WORKSPACE}/docker/grafana"
docker build --force-rm -t "${IMAGE_TAG_PROMETHEUS_SERVICE}" "${WORKSPACE}/docker/prometheus"
```

* Prepare a script to push the production docker images to the ECR repo and name it as `push-prod-docker-images-to-ecr.sh` and save it under `jenkins` folder.

``` bash
aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}
docker push "${IMAGE_TAG_ADMIN_SERVER}"
docker push "${IMAGE_TAG_API_GATEWAY}"
docker push "${IMAGE_TAG_CONFIG_SERVER}"
docker push "${IMAGE_TAG_CUSTOMERS_SERVICE}"
docker push "${IMAGE_TAG_DISCOVERY_SERVER}"
docker push "${IMAGE_TAG_HYSTRIX_DASHBOARD}"
docker push "${IMAGE_TAG_VETS_SERVICE}"
docker push "${IMAGE_TAG_VISITS_SERVICE}"
docker push "${IMAGE_TAG_GRAFANA_SERVICE}"
docker push "${IMAGE_TAG_PROMETHEUS_SERVICE}"
```

- At this stage, we will use Amazon RDS instead of mysql pod and service. Create a mysql database on AWS RDS.
# Create database on AWS Console--> Standart Create--> Enter inf. below
  - Engine options: MySQL
  - Version : 5.7.30
  - Templates: Free tier
  - DB instance identifier: petclinic
  - Master username: root              # Normalde production ortamda root olmaz burada egitim amacli, islemleri yapabilmek adina
  - Master password: petclinic
  - Public access: Yes                 
  - Additional Configuration --> Initial database name: petclinic
  
- Delete mysql-server-deployment.yaml file from k8s/petclinic_chart/templates folder. (Dursun lazim olabilir diye silmek yerine k8s folderina tasiyabilirsin)

- Update k8s/petclinic_chart/templates/mysql-server-service.yaml as below. 

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    kompose.cmd: kompose convert -f docker-compose-local-db.yml
    kompose.version: 1.26.1 (a9d05d509)
  labels:
    io.kompose.service: mysql-server
  name: mysql-server
spec:
  type: ExternalName
  externalName: petclinic.cbanmzptkrzf.us-east-1.rds.amazonaws.com # Change this line with the endpoint of your RDS.
```
# Jenkins server, rancher ile namespace ile iletisim kuracak. Bu yuzden rancher da cluster uzerinde namespace oluturalim.
# Rancher--> petclinic cluster-->Projects/Namespace-->Create Namespace(default)--> Name: petclinic-prod-ns-->create
# Click '...' beside petclinic --> edit yaml --> copy Project ID (you will use this information below Jenkins-petclinic-prod)

* Prepare a Jenkinsfile for `petclinic-prod` pipeline and save it as `jenkinsfile-petclinic-prod` under `jenkins` folder.

* Create `petclinic-prod-ns` namespace on `petclinic-cluster` with Rancher.
# Update "url, context, ıd, s3" in jenkins file. 

``` groovy (Bu dokumani buradan kopyalama, aciklamalari ekleyince format degisti hata veriyor)
pipeline {
    agent any                        #Jenkins server rancher uzerinden clusteri yonetiyor. gerekli bilgi ve credentials'lar veriyoruz.
    environment {
        PATH=sh(script:"echo $PATH:/usr/local/bin", returnStdout:true).trim()
        APP_NAME="petclinic"
        APP_REPO_NAME="yasin-repo/petclinic-app-prod"
        AWS_ACCOUNT_ID=sh(script:'export PATH="$PATH:/usr/local/bin" && aws sts get-caller-identity --query Account --output text', returnStdout:true).trim()
        AWS_REGION="us-east-1"
        ECR_REGISTRY="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
        RANCHER_URL="https://rancher.devopsyasin.com"
        // Get the project-id from Rancher UI (petclinic-cluster-staging namespace, View in API, copy projectId )
        RANCHER_CONTEXT="c-nkvt9:p-dhq7k" 
       //First part of projectID
        CLUSTERID="c-nkvt9"
        RANCHER_CREDS=credentials('rancher-petclinic-credentials')
    }
    stages {
        stage('Package Application') {
            steps {
                echo 'Packaging the app into jars with maven'
                sh ". ./jenkins/package-with-maven-container.sh"
            }
        }
        stage('Prepare Tags for Production Docker Images') {
            steps {
                echo 'Preparing Tags for Production Docker Images'
                script {
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-admin-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_ADMIN_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:admin-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-api-gateway/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_API_GATEWAY="${ECR_REGISTRY}/${APP_REPO_NAME}:api-gateway-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-config-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_CONFIG_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:config-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-customers-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_CUSTOMERS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:customers-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-discovery-server/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_DISCOVERY_SERVER="${ECR_REGISTRY}/${APP_REPO_NAME}:discovery-server-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-hystrix-dashboard/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_HYSTRIX_DASHBOARD="${ECR_REGISTRY}/${APP_REPO_NAME}:hystrix-dashboard-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-vets-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_VETS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:vets-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    MVN_VERSION=sh(script:'. ${WORKSPACE}/spring-petclinic-visits-service/target/maven-archiver/pom.properties && echo $version', returnStdout:true).trim()
                    env.IMAGE_TAG_VISITS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:visits-service-v${MVN_VERSION}-b${BUILD_NUMBER}"
                    env.IMAGE_TAG_GRAFANA_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:grafana-service"
                    env.IMAGE_TAG_PROMETHEUS_SERVICE="${ECR_REGISTRY}/${APP_REPO_NAME}:prometheus-service"
                }
            }
        }
        stage('Build App Production Docker Images') {
            steps {
                echo 'Building App Production Images'
                sh ". ./jenkins/build-prod-docker-images-for-ecr.sh"
                sh 'docker image ls'
            }
        }
        stage('Push Images to ECR Repo') {
            steps {
                echo "Pushing ${APP_NAME} App Images to ECR Repo"
                sh ". ./jenkins/push-prod-docker-images-to-ecr.sh"
            }
        }
        stage('Deploy App on Petclinic Kubernetes Cluster'){
            steps {
                echo 'Deploying App on K8s Cluster'
                sh "rancher login $RANCHER_URL --context $RANCHER_CONTEXT --token $RANCHER_CREDS_USR:$RANCHER_CREDS_PSW"
                sh "envsubst < k8s/petclinic_chart/values-template.yaml > k8s/petclinic_chart/values.yaml"
                sh "sed -i s/HELM_VERSION/${BUILD_NUMBER}/ k8s/petclinic_chart/Chart.yaml"
                sh "rancher kubectl delete secret regcred -n petclinic-prod-ns || true"
                sh """
                rancher kubectl create secret generic regcred -n petclinic-prod-ns \
                --from-file=.dockerconfigjson=$JENKINS_HOME/.docker/config.json \
                --type=kubernetes.io/dockerconfigjson
                """
                sh "rm -f k8s/config"
                sh "rancher cluster kf $CLUSTERID > k8s/config"
                sh "chmod 400 k8s/config"
                sh "helm repo add stable-petclinic s3://yasin-petclinic-helm-charts/stable/myapp/"
                sh "helm package k8s/petclinic_chart"
                sh "helm s3 push --force petclinic_chart-${BUILD_NUMBER}.tgz stable-petclinic"
                sh "helm repo update"
                sh "AWS_REGION=$AWS_REGION helm upgrade --install petclinic-app-release stable-petclinic/petclinic_chart --version ${BUILD_NUMBER} --namespace petclinic-prod-ns --kubeconfig k8s/config"
            }
        }
    }
    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}
```

* Commit the change, then push the script to the remote repo.

``` bash
git add .
git commit -m 'added jenkinsfile petclinic-production for main branch'
git push --set-upstream origin feature/msp-27
git checkout release
git merge feature/msp-27
git push origin release
```

* Merge `release` into `main` branch to build and deploy the app on `Production environment` with pipeline.

```bash
git checkout main
git merge release
git push origin main
```
* Create a `Production Pipeline` on Jenkins with name of `petclinic-prod` with following script and configure a `github-webhook`(bunu yapmadik) to trigger the pipeline every `commit` on `main` branch. `Petclinic production pipeline` should be deployed on permanent prod-environment on `petclinic-cluster` Kubernetes cluster under `petclinic-prod-ns` namespace. 

# New item--> Name:petclinic-prod:pipeline--> pipeline script from scm:git--> Githubrepo url-->main-->path:./jenkins/jenkinsfile-petclinic-prod--> Save
# Kodlar calisiyorsa Rancher da Workload da servisleri gormus olmamiz gerekiyor


## MSP 28 - Setting Domain Name and TLS for Production Pipeline with Route 53

* Create `feature/msp-28` branch from `main`.

``` bash
git checkout main
git branch feature/msp-28
git checkout feature/msp-28
```

* Create an `A` record of `petclinic-prod.devopsyasin.com`(/k8s/petclinic/template/values template.yml da ne yaziyorsa onunla ayni olmali) in your hosted zone using AWS Route 53 domain registrar and bind it to your `petclinic cluster`.
# route53--> create record--> simple record--> A class--> herhangi bir instance PublicIP kopyala--> create

* Configure TLS(SSL) certificate for `petclinic-prod.devopsyasin.com` using `cert-manager` on petclinic K8s cluster with the following steps.

* Log into Jenkins Server and configure the `kubectl` to connect to petclinic cluster by getting the `Kubeconfig` file from Rancher and save it as `$HOME/.kube/config` or set `KUBECONFIG` environment variable.

```bash
#create petclinic-config file under home folder(/home/ec2-user/.kube).
nano petclinic-config
# paste the content of kubeconfig file from rancher page and save it.
chmod 400 petclinic-config
export KUBECONFIG=/home/ec2-user/.kube/petclinic-config
# test the kubectl with petclinic namespaces
kubectl get ns
```

* Install the `cert-manager` on petclinic cluster. See [Cert-Manager info](https://cert-manager.io/docs/). 
# Sertifikayi manual olusturmayip agent sayesinde yapacagiz.

  * Create the namespace for cert-manager 

  ```bash(/home/ec2-user)
    kubectl get ns                              
    kubectl create namespace cert-manager   #Default namespaces kullanilabilirdi, butun sertifika islemleri icin ayri bir ns olusturacagiz
  ```

  * Add the Jetstack Helm repository.

  ```bash
  helm repo add jetstack https://charts.jetstack.io
  ```

  * Update your local Helm chart repository.

  ```bash
  helm repo update
  ```

  * Install the `Custom Resource Definition` resources separately

  ```bash
  kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.7.1/cert-manager.crds.yaml   
  # ilave resources olusturduk -certificate request- bunlardan biri
  ```

  * Install the cert-manager Helm chart 

  ```bash
  helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.7.1
  ```

  * Verify that the cert-manager is deployed correctly.

  ```bash
  kubectl get pods --namespace cert-manager -o wide  
  ```
  # 3 adet cert-manager pods gormek gerekiyor.

* Create `ClusterIssuer` with name of `tls-cluster-issuer-prod.yml` for the production certificate through `Let's Encrypt ACME` (Automated Certificate Management Environment) with following content by importing YAML file on Ranhcer and save it under `k8s` folder. *Note that certificate will only be created after annotating and updating the `Ingress` resource.*
# /k8s folder altinda `tls-cluster-issuer-prod.yml` adin da file olusturuyoruz.  

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
  namespace: cert-manager
spec:
  acme:
    # The ACME server URL
    server: https://acme-v02.api.letsencrypt.org/directory
    # Email address used for ACME registration
    email: pakyasin23@gmail.com
    # Name of a secret used to store the ACME account private key
    privateKeySecretRef:
      name: letsencrypt-prod
    # Enable the HTTP-01 challenge provider
    solvers:
    - http01:
        ingress:
          class: nginx
```

* Check if `ClusterIssuer` resource is created.

```bash (cd petclinic-microservis..)
kubectl apply -f k8s/tls-cluster-issuer-prod.yml
kubectl get clusterissuers letsencrypt-prod -n cert-manager -o wide
```
### Doğrulama işlemi nasıl çalışır.
1. server admin Let's Encrypt'e der ki:
Bu benim DNS'im (http://example.com). Buna bir certificate ver.
2. Let's Encrypt de der ki:
Senin bu sitenin sahibi olduğunu nereden bileyim. Sana bazı görevler vereceğim. (challange)
    1. görevin.
	Sana bir kere kullancağın bir key vereceğim(nonce). Bunu private key pair'in ile imzala. Böylelikle key pairini senin kontrol ettiğini anlayabileyim.
    2. Bir file'ı (ed98) https://example.com/8303'e koy.
3. server admin görevleri yapar.
4. Let's Encrypt kontrol eder. Doğruysa sertifika verir. (validation)
###

# Rancher-->Service Discovery--> Ingress-->Burada route53 deki adres olmali-->...Edit yaml--> Asagidaki annotations birde spec kismini buraya yapistir.

* Issue production Let’s Encrypt Certificate by annotating and adding the `api-gateway` ingress resource with following through Rancher.
```yaml
metadata:
  name: api-gateway
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"             # Bu satiri kopyala  
spec:                           
  tls:                                                             # Burdan en sona kadar butun satirlar kopyala   
  - hosts:
    - petclinic-prod.devopsyasin.com    
    secretName: petclinic-tls
```
# Olusan ingress linkini tiklayinca sifre cikacak bu su anlamina geliyor challange oldu ve link kendiliginden kaybolacak sertfika olusacak. `https://yasin-petclinic.. linkini tiklayinca petclinic sayfasina https ile guvenli sertifikali baglanilmis olacak.

# Biz bu yaml dosyasinda ki bilgileri pipeline dan once k8s/petc../templates altinda api-gateway-ingress.yaml dosyasina kopyalasaydik burada bu isleme gerek kalmaz otomatik olarak olrudu ancak egitim amacli manual nasil sertifika olusturuluyor gorduk.

* Check and verify that the TLS(SSL) certificate created and successfully issued to `yasin-petclinic.devopsyasin.com` by checking URL of `https://yasin-petclinic..`

* Commit the change, then push the tls script to the remote repo.

``` bash
git add .
git commit -m 'added tls scripts for petclinic-production'
git push --set-upstream origin feature/msp-28
git checkout main
git merge feature/msp-28
git push origin main
```

* Run the `Production Pipeline` `petclinic-prod` on Jenkins manually to examine the petclinic application.


## MSP 29 - Monitoring with Prometheus and Grafana

# Rancher Page-->Serives Discovery-->Services: "Grafana server--> '...' edit yaml. type=NodePort yapip save dedik.
# Herhangi bir instance public ip:(port:grafana sever da yaziyor) Grafana sayfasi acilacak. 
- Grafana--> Data Sources--> Prometheus--> Save test (Error: daha once docker compose file da 9090 yapmistik) 
- Ayni sayfa da url karsisinda 9090--> 9091 diye degistir: Data source is working gorduk,

# Ayni islemleri Prometheus Service 'NodePort' yapiyoruz. Herhangi bir Public Ip:port numarasi,prometheus sayfasi acildi.

# petclinic-micro.../docker/icerisinde prometheus grafana docker filelari gorebilirsin bu sekilde kullaniyoruz. 
# Burada prometheus.yml file'da "metrics_path: /actuator/prometheus" kismindan petclinic url'mize eklersek matric leri goruruz(normalde /metrics dir ancak burada bu sekilde ayarlama yapildi).
# (https://petclinic-prod.devopsyasin.com/actuator/prometheus)

# Grafana--> dashboards--> browse-- petclinic metrics tiklayinca grafikler cikiyor.


* Change the port of Prometheus Service to `9090`, so that Grafana can scrape the data.

