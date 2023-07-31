## Demo app - With Express & Mongo 

This demo app shows a simple user profile app set up using 
- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based

### With Docker

#### To start the application

Step 1: Create docker network

    docker network create mongo-network 

Step 2: start mongodb 

    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    

Step 3: start mongo-express
    
    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express   

_NOTE: creating docker-network is optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_

Step 4: open mongo-express from browser

    http://localhost:8081

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your nodejs application locally - go to `app` directory of project 

    cd app
    npm install 
    node server.js
    
Step 7: Access you nodejs application UI from browser

    http://localhost:3000

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

    docker-compose -f docker-compose.yaml up
    docker-compose -f docker-compose.yaml down

_You can access the mongo-express under localhost:8080 from your browser_
    
Step 2: in mongo-express UI - create a new database "my-db"

Step 3: in mongo-express UI - create a new collection "users" in the database "my-db"       
    
Step 4: start node server 

    cd app
    npm install
    node server.js
    
Step 5: access the nodejs application from browser 

    http://localhost:3000

#### Build a docker image from the application

    docker build -t my-app:1.0 .   
    docker run my-app:1.0    
    
The dot "." at the end of the command denotes location of the Dockerfile.

#### Deploying our containerized application on server
Create private Docker Registry on Amazon ECR
Log into private registry (Copy command from ECR)
Tag Docker Image using -> ECRregistryDomain/imageName:tag 
```
docker tag my-app:1.0 ECRregistryDomain/my-app:1.0
```
Push Docker Image to AWS ECR repository
```
docker push ECRregistryDomain/my-app:1.0
```
Add our example application to Dockerfile
Change mongodb server url from localhost to mongodb service name in Node Code
Start docker containers with docker-compose
```
docker-compose -f docker-compose.yaml up
```