# My DCA Calculator

The goal of this application is to create a DCA Calculator Website in React for showing my competence in React and in continuous 
deployment.

I will deploy this website on a OVH VPS.

# Branching Strategy

- ##### Master Branch (master)

Deployment branch, here is the version who is uploaded on the server. This branch is 
automaticelly connected to the server and deploy the version on the server with the github action

- ##### Develop Branch (develop)

All the modification are here, on this branch, all the tests have to pass. This branch concatenate
all the new features. When we want to deploy this version, create a merge request on the master branch


- ##### Feature Branch (X-feature)

This branch contain all the new feature, when we want to develop a new feature, we have to create a new branch from
the develop branch, and when this feature is finish, create a merge request to push this on the feature branch. This feature branch will
be deleted when the feature is implemented in the develop branch.


# How to launch the development code

##### Build the docker image from the Dockerfile inside our `.` repository
```
docker build -t my-dca-calculator-react-image .  
```

##### Watch if our image is in our docker 
```
docker image ls   
```

##### Run the docker image
```
docker run -d -v $(pwd)/src:/app/src:ro  -p 3000:3000 --name my-dca-calculator-react-app my-dca-calculator-react-image
```

Parameters :
- `-v $(pwd)/src:/app/src:ro` : Create the binding between our current code (in our local repository), and the code in the app. That mean that when we will do some change in the code in our local repository, the code will be automaticely change in our docker image repository. The running is in the Read Only `:ro` Mode, like that, the edited files can only go in the way our computer -> docker image, and not in the way dowker image -> our computer
- `-p 3000:3000` : Our browser send the request on the port 3000 (first) of our computer, our computer call the port 3000 (second) of the docker image

##### Remove the app from the running images 
```
docker rm my-dca-calculator-react-app -f 
docker ps  
```

##### Launch a terminal inside our running image
```
docker exec -it  my-dca-calculator-react-app bash
```

##### Add a variable environment to the application

Dockerfile
```
ENV REACT_APP_NAME=MyDCACalculator
```

.js File
```
{`${process.env.REACT_APP_NAME}`}
```

⚠️ Or create a .env file and put all the ENV variable inside and :
```
docker run -d --env-file ./.env -v $(pwd)/src:/app/src:ro  -p 3000:3000 --name my-dca-calculator-react-app my-dca-calculator-react-image
```

# Docker compose

Sometimes, we have a lot of big command, and it is hard to remember each. And sometimes, we have to launch 
several images in the same time, and it's too complicated to remember all this commands. So the docker compose 
is a file that include all this command, like that, we'll have only to launch docker compose

##### For launching the services :
```
docker-compose up -d --build
```
##### For stopping the services :
```
docker-compose down
```

##### For watching the running services :
```
docker ps
```

##### For watching the images :
```
docker image ls
```


# Multi-stage build

##### For building an image with the path of the Dockerfile
```
docker build -f Dockerfile.dev .
```

##### Launch dev-mode : 
```
docker-compose -f docker-compose.yml -f docker-compose-dev.yml up -d --build
// For watching the running services
docker ps  
// For stopping the services :  
docker-compose -f docker-compose.yml -f docker-compose-dev.yml down   
```


##### Launch prod-mode :
```
docker-compose -f docker-compose.yml -f docker-compose-prod.yml up -d --build
// For watching the running services
docker ps  
// For stopping the services :  
docker-compose -f docker-compose.yml -f docker-compose-prod.yml down   
```
