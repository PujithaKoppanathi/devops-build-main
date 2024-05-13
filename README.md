# Dockerizing the Application
## Dockerfile:
Create a Dockerfile in the root of your project:
Stage 1 - Build React Application
FROM node:14-alpine as build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install

COPY . .
RUN npm run build

Stage 2 - Serve React Application using Nginx
FROM nginx:alpine

COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]



## docker-compose.yml:
Create a docker-compose.yml file in the root of your project:

version: '3'

services:
  app:
    build: .
    ports:
      - "80:80"




## Bash Scripting
Also

version: '3'

services:
  app:
    build: .
    ports:
      - "80:80"
## Version Control
Make sure to add a .gitignore file to ignore node_modules and other generated files. Also, add a .dockerignore file to exclude unnecessary files from the Docker build context.
## Docker Hub
docker login
docker tag saipuja1996/dev:latest
docker push saipuja1996/dev:latest

# For prod, make sure to tag and push as well
docker tag saipuja1996/prod:latest
docker push saipuja1996/prod:latest
<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/1addaaa9-6e6e-4ce3-adcf-c2843dffeb5f">
<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/18aa8db1-5ded-4906-bdd6-d38a3fc4d009">

## Jenkins

Install Jenkins and configure build steps to build Docker images, push them to Docker Hub, and deploy them to AWS. Configure Jenkins to trigger builds on both dev and master branches.


Jenkins

pipeline {
    agent any
    
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'bash build.sh'
            }
        }
        stage('Push Docker Image to Dev') {
            when {
                branch 'dev'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'saipuja1996') {
                        docker.image(saipuja1996/dev:latest').push()
                    }
                }
            }
        }
        stage('Push Docker Image to Prod') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'saipuja1996') {
                        docker.image(saipuja1996/prod:latest').push()
                    }
                }
            }
        }
    }
}

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/0e6e6efe-a7ca-4f6d-846e-7b1867daa1cb">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/d6a58c01-8351-4dbb-80ee-84980d1859ac">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/4e9ab666-cda9-46eb-8868-bff332e44e1a">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/747e4d97-d3a9-4f4f-9202-7dc14d2b582c">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/6f739f7c-d1e1-49ee-b1b9-d53b81473f71">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/9a6d633a-fb76-4e49-a3b9-60299f26cf1e">



# AWS

Launch an EC2 instance (t2.micro) and configure Security Groups to allow access to port 80 from all IPs, but restrict SSH access to only your IP.



<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/e50c9e6d-1a6e-483c-88b5-98e154fe2c9d">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/9fb5a2c7-73b0-4f7e-865f-b9baf438af01">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/99fe24c5-cf78-4178-adc6-d35d3e83ed80">

# Monitoring


Set up monitoring using an open-source tool like Prometheus. Configure alerts to notify when the application goes down.


# prometheus.yml

alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - alertmanager:9093

rule_files:
  - alerts.yml




# prometheus.yml

alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - alertmanager:9093

rule_files:
  - alerts.yml

global:
 resolve_timeout: 1m

route:
 receiver: 'email-notifications'

receivers:
- name: 'email-notifications'
  email_configs:
  - to: saipuja_1996@gmail.com
    from: saipuja#1996@gmail.com
    smarthost: smtp.gmail.com:587
    auth_username: saipuja#1996@gmail.com
    auth_identity: saipuja#1996@gmail.com
    auth_password: saipuja#1996
    send_resolved: true



<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/d4d6305b-f3b2-4410-bedb-d80e56fa411a">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/c36e91a3-8c65-4583-af2f-0776e45678b6">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/7ee5aa59-7946-48fa-b4f0-b335ff5da84d">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/aed4c767-1fcf-4bc0-8a9b-80df5ad41210">

<img width="468" alt="image" src="https://github.com/PujithaKoppanathi/devops-build-main/assets/169693994/5943c887-e6c6-4fc4-bc05-aa6645906d46">







