# Dealership Reviews Portal
  
Course: [Full Stack Application Development Capstone Project](https://www.coursera.org/learn/ibm-cloud-native-full-stack-development-capstone?specialization=ibm-full-stack-cloud-developer) - [IBM Full Stack Software Developer](https://www.coursera.org/professional-certificates/ibm-full-stack-cloud-developer)


## Table of contents

- [Overview](#overview)
- [Features](#features)
- [Demo](#demo)
- [Architecture Overview](#architecture-overview)
- [Technologies Used](#technologies-used)
- [Setup and Commands](#setup-and-commands)
- [Acknowledgements](#acknowledgements)
- [License](#license)


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

**Enhancements**
  1. Front-End Enhancements
    - Searchable Dropdown for States: Transformed the "States" dropdown on the "Dealerships" page into a searchable textbox. Allows users to filter dealerships by entering search strings.
    - Improved Color Scheme: Enhanced the color scheme of the Navbar and Dealerships button on the home page. Refined the colors of the Review panel and review icons on the "Dealerships Review" page.
    - Dealer Review Panel Adjustments: Fine-tuned visual elements such as font size and alignment to improve readability.
     
  3. Car Inventory Back-End Service
    - Microservice for Car Inventory: Developed a back-end microservice using MongoDB and Node.js to manage car inventory details. Integrated this microservice with the existing Django back end.
    
  5. Front-End Development for Car Inventory Service
    - Car Inventory Front-End Component: Created and integrated a front-end component to interact with the car inventory back-end microservice. Users can filter cars based on Make, Model, Year, Mileage, Price.

## Demo
![Demo](./demo.gif)

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

<p>&nbsp;</p>

**1. The project skeleton can be accessed by cloning this [repository](https://github.com/ibm-developer-skills-network/xrwvm-fullstack_developer_capstone).**

**2. Frontend Setup**
   
- Switch to the client directory.
    ``` bash
    cd /home/project/fullstack_developer_capstone/server/frontend
    ```
    
- Install all required packages.
    ``` bash
    npm install
    ```
    
- Run the following command to build the client.
    ``` bash
    npm run build
    ```

**3. Django Setup:**
   
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
    ```
    
- Create a superuser by running the following command.
    ``` bash
    python3 manage.py createsuperuser
    ```
    
- Run the server by executing the following command.
    ``` bash
    python3 manage.py runserver
    ```

**4. Run the Mongo Server**
   
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
    
- Open `djangoapp/.env` and replace the backend url
  
**5. Deploy sentiment analysis on Code Engine**
   
- In the code engine CLI, change to `server/djangoapp/microservices` directory.
    ``` bash
    cd fullstack_developer_capstone/server/djangoapp/microservices
    ```
    
- Run the following command to docker build the sentiment analyzer app
    ``` bash
    docker build . -t us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer
    ```
    
- Push the docker image by running the following command.
    ``` bash
    docker push us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer
    ```
    
- Deploy the senti_analyzer application on code engine.
    ``` bash
    ibmcloud ce application create --name sentianalyzer --image us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer --registry-secret icr-secret --port 5000
    ```
    
- Open `djangoapp/.env` and replace the sentiment analyzer url

**6. Run the Mongo Server on the carsInventory directory to obtain the searchcars url**

- Change to `server/carsInventory` directory.
    ``` bash
    cd fullstack_developer_capstone/server/carsInventory
    ```
- Install the dependencies by executing the command:
    ``` bash
    npm install
    ```
- Build the Docker application.
    ``` bash
    docker build . -t nodeapp
    ```
- Execute the below command to launch the server:
    ``` bash
    docker-compose up
    ```

## Acknowledgements
This project was developed as part of the IBM Full Stack Software Developer Specialization on Coursera.

## License
This project is open-source and available under the Apache 2.0 License.
