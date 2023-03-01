
# Spring Boot Kubernetes and MySQL
Sample project to test and deploy spring boot application with mysql database in kubernetes using JKube maven plugin 

## Overview
This repository contains the project code for a simple RESTful API. The objective is to design a system API that will handle Orders data. The API provides CRUD functionality to place orders.

We have an orders table that has the following fields:

``` 
    id    : integer 
    name  : string 
    qty   : integer
    price : double
```

## Technologies Used

- Docker with kubernetes enabled
- Kubernetes command-line tool(kubectl)
- JDK 8
- Maven
## Getting Started :
## Docker 
The project includes a dockerfile to build the image of the springboot application.Below docker file is used in building a docker image of our spring-boot application.

```
FROM openjdk:8
EXPOSE 8080
ADD target/springboot-crud-k8s.jar springboot-crud-k8s.jar
ENTRYPOINT ["java","-jar","/springboot-crud-k8s.jar"]
```
- Go to the root directory and use below command to create an image:

   ```docker build -t springboot-project1 .``` 
- Tagging image to your repository:

   ```docker -t springboot-project1:latest joshann/springboot-project1```

- Push image to docker hub:

   ```docker joshann/springboot-project1 push```
For mysql database, mysql-5.7 image is pulled from the docker hub.

## Kubernetes
### 1) Setting up mysql database
In this application mysql 5.7 is used, and it is defined in the mysql-deployment.yml file and the image is directly pulled from the docker hub during deployment. Follow below steps to set up mysql database using secrets and confiugurations.The mysql pod is exposed at port 3306, using headless service we are exposing the pod to  port 3306
which is used to communicate with other pods.

Go to mysql-k8s directory and open cmd

- Create a Secret for MySQL Login Credentials :

    ``` kubectl apply -f mysql-secret.yaml ```

- Create a ConfigMap for MySQL Configuration :

    ``` kubectl apply -f mysql-config.yml ```
-  Create a MySQL Deployment :
  
    ``` kubectl apply -f mysql-deployment.yml ```

- Test MySQL Connectivity :

   ``` kubectl get pods ```

   ``` kubectl exec -it <podName> bash ```

   ``` mysql -u username -p password ```

   ``` show databaes; ```

   the above command shows the availabe databases

### 2) Setting up Spring Boot in Kubernetes using Ingress

In this sringboot application we are using our custom image ie. joshann/springboot-project1 which we will be defining in the template of app-deployment.yml.The pod containing the springboot application is open to port 8080. and using cluster Ip service it is exposed to 4330.
 
Go to spring-boot-k8s director and open cmd


- Create springboot deployment and service at once:

    ``` kubectl apply -f app-deployment.yml```
- Configuring Ingress:

    ``` kubectl apply -f Ingress.yml ```

## Test the Application

- Use this command to get list of orders :

   ```curl -X GET http://hello-world.info/orders```
  
- Use below url to view data from the database in postman:

   ```GET http://hello-world.info/orders```
- Use below url to post data to database using postman:

   ```POST http://hello-world.info/orders```


## Contact

        Dasireddy Sai Joshan (joshan.dasireddy@argano.com)
