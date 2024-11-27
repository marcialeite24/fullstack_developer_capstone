# Dealership Reviews Portal
  
Course: [Full Stack Application Development Capstone Project](https://www.coursera.org/learn/ibm-cloud-native-full-stack-development-capstone?specialization=ibm-full-stack-cloud-developer) - [IBM Full Stack Software Developer](https://www.coursera.org/professional-certificates/ibm-full-stack-cloud-developer)


## Table of contents

- [Overview](#overview)
- [Features](#features)
- [Architecture Overview](#architecture-overview)
- [Technologies Used](#technologies-used)
- [Setup and Commands](#setup-and-commands)
  - [Build and Deploy](#build-and-deploy)
  - [Autoscaling](#autoscaling)
  - [Rolling Updates and Rollbacks](#rolling-updates-and-rollbacks)


## Overview
The Dealership Reviews Portal is a web application developed as the capstone project for the IBM Full Stack Software Developer Professional Certificate. The portal aims to provide a central database of dealership reviews across the United States, enabling customers to look up dealerships, filter by state, and read or write reviews for specific branches. The project incorporates modern full-stack technologies and follows best practices in software development.

## Features
**Anonymous Users**
  - View the About Us and Contact Us pages.
  - View a list of dealerships across the country.
  - Filter dealerships by state and view reviews for a selected dealership.
  - Log in to access additional features.

**Authorized Users**
  - Perform all actions available to anonymous users.
  - Submit reviews for any dealership, including details like:
    - Purchase information (date, make, model, and year of the car).
    - Sentiment analysis of the review text (positive, neutral, negative).

**Admin Users**
  - Log in to manage the system.
  - Add and manage car attributes (make, model, etc.) via the admin panel.

## Architecture Overview
![Architecture Overview](./project-architecture.png)

## Technologies Used
- **Frontend:** React for dynamic user interaction.
- **Backend:** Django for web services and Express.js for dealer/review microservices.
- **Database:** SQLite for car data and MongoDB for dealer/review data.
- **Sentiment Analysis:** IBM Cloud Code Engine hosting a sentiment analyzer service.
- **Containerization:** Docker for deploying microservices.
- **CI/CD:** Continuous integration and deployment for code linting and application updates.
- **Deployment:** Kubernetes for scalable hosting.

## Setup and Commands
Prerequisites
  - Docker installed on your machine.
  - Kubernetes cluster set up.
  - Node.js and Python environments configured.

1. Clone this repository to your local machine. (The project skeleton can be accessed by cloning this [repository](https://github.com/ibm-developer-skills-network/xrwvm-fullstack_developer_capstone))
2. Django Setup:
  - Run the following to set up the django environment.
    ``` bash
    cd /home/project/fullstack_developer_capstone/server
    pip install virtualenv
    virtualenv djangoenv
    source djangoenv/bin/activate
    ```
  - Install the required packages by running the following command.
    ``` bash
    python3 -m pip install -U -r requirements.txt
    ```
  - Run the following command to perform models migration.
    ``` bash
    python3 manage.py makemigrations
    python3 manage.py migrate
    python3 manage.py runserver
    ```

2. Run the Mongo Server
  - Change to the database directory.
    ``` bash
    cd /home/project/fullstack_developer_capstone/server/database
    ```
  - Build the nodeapp.
    ``` bash
    docker build . -t nodeapp
    ```
  - Run the following command to start the server.
    ``` bash
    docker-compose up
    ```




   
2. Autoscaling
  - Apply the Horizontal Pod Autoscaler
    ``` bash
    kubectl autoscale deployment guestbook --cpu-percent=5 --min=1 --max=10
    ```
  - Check Autoscaler Status
    ``` bash
    kubectl get hpa guestbook
    ```
  - Generate Load to Observe Autoscaling
    ``` bash
    kubectl run -i --tty load-generator --rm --image=busybox:1.36.0 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- <your app URL>; done"
    ```
  - Monitor Replica Scaling
    ``` bash
    kubectl get hpa guestbook --watch
    ```
  - Check the details of the Horizontal Pod Autoscaler
    ``` bash
    kubectl get hpa guestbook
    ```
    
3. Rolling Updates and Rollbacks
  - Make changes to index.html or other components
  - Rebuild the Docker Image
    ``` bash
    docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1 && docker push us.icr.io/$MY_NAMESPACE/guestbook:v1
    ```
  - Update the deployment.yaml file and apply changes
    ``` bash
    kubectl apply -f deployment.yml
    ```
  - Restart the Application
    ``` bash
    kubectl port-forward deployment.apps/guestbook 3000:3000
    ```
  - View Deployment Rollout History
    ``` bash
    kubectl rollout history deployment/guestbook
    ```
  - View Deployment Revision Details
    ``` bash
    kubectl rollout history deployments guestbook --revision=2
    ```
  - Undo the Deployment
    ``` bash
    kubectl rollout undo deployment/guestbook --to-revision=1
    ```
  - Verify the Rollback
    ``` bash
    kubectl get rs
    ```
