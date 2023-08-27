# 311renpengassignment

Instructions:

Create a new Node.js application using the Express framework.
Set up automatic testing for the application using a testing framework such as Jest or Mocha.
Create a code repository for the application on a version control system such as Git.
Create a Dockerfile for the application that includes all necessary dependencies.
Set up a CI/CD pipeline using a tool such as Github Action.
Configure the pipeline to automatically build the Docker image and run the tests whenever changes are pushed to the code repository.
Push the Docker image to a container registry such as Docker Hub or AWS ECR.
Set up a task definition and service in Amazon Elastic Container Service (ECS) to deploy the containerized application.
Configure the pipeline to automatically deploy the containerized application to ECS whenever changes are pushed to the code repository and all tests pass.
Write a documentation explaining the steps you have done, including the tools and services used.
Deliverables:

The Node.js application with automatic testing set up
Code repository with the CI/CD pipeline configured
Dockerfile and configuration files for ECS
Documentation explaining the steps taken to set up the pipeline.



Report:

1)	Create a new Node.js application using the Express framework.
npm install express
npm init
npm install express when notice the express note installed
npm install jest supertest --save-dev
express my-app to create the app

2)	Set up automatic testing for the application using a testing framework such as Jest or Mocha.
npm install --save-dev jest
create a new file called app.test.js in the my-app directory with the following content:
const request = require('supertest');
const app = require('./app');

describe('GET /', () => {
  it('responds with "Hello World!"', (done) => {
    request(app)
      .get('/')
      .expect(200)
      .expect('Hello World!', done);
  });
});
3)	Create a Dockerfile for the application that includes all necessary dependencies
docker build -t my-app -f Dockerfile .
docker push 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/renpeng:latest

aws ecr get-login-password  #update password
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/renpeng
docker push 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/renpeng:latest



4)	Create a code repository for the application on a version control system such as Git.
git status
git add .
git commit -m "update workflows and dockerfile"
git push

5)	Set up a CI/CD pipeline using a tool such as Github Action. Configure the pipeline to automatically build the Docker image and run the tests whenever changes are pushed to the code repository.
name: My Workflow
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
        # Add your build steps here
        - name: Build
          run: echo "Building..."
    
        # Build the Docker image
        - name: Build Docker image
          run: docker build -t index.js -f Dockerfile .
  
        # Tag the Docker image
        - name: Tag Docker image
          run: docker tag renpeng 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/renpeng:latest
  
        # Log in to the container registry
        - name: Log in to container registry
          run: echo ${{ secrets.REGISTRY_PASSWORD }} | docker login 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/renpeng:latest -u ${{ secrets.REGISTRY_USERNAME }} --password-stdin
  
        # Push the Docker image to the container registry
        - name: Push Docker image to container registry
          run: docker push 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/renpeng:latest
  


6)	Push the Docker image to a container registry such as Docker Hub or AWS ECR.
7)	Set up a task definition and service in Amazon Elastic Container Service (ECS) to deploy the containerized application.
The URL is as below: 
255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/renpeng:latest
8)	Configure the pipeline to automatically deploy the containerized application to ECS whenever changes are pushed to the code repository and all tests pass.
Have manually build docker and pushed to the RCS repository, but I still got trouble make it works in CICD pipeline, may need further verification on the code. So far I got error, run: docker build -t index.js -f Dockerfile .probably due to the repository created yesterday have been removed from aws account.



