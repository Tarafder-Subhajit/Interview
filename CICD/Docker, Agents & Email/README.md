# How to setup Docker server?
- ### PLUGIN
  Go to Manage Jenkins -> Plugins -> search for "docker"  
  There are two major plugins "Docker" & "Docker Piepeline"  
  Docker Plugin: Docker container becomes the Jenkins agent.  
  Docker Pipeline Plugin: Jenkins agent runs your pipeline steps inside Docker containers.  
  
  Now go to Manage Jenkins -> tools , Search for Docker so you can see Docker installation being available and you can use automatically for the installation part.  
  <img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/db88eaf8-e351-4ce8-830c-1592cdf2eaf3" />

- ### Installing on SERVER
  - Create a AWS EC2 VM
  - Install Docker
  - Enable docker & containerd service
  - Start the docker service

# Jenkinsfile with Docker
Github Repo - https://github.com/DevopsWorking/addressbook
```
pipeline{
  agent any
  tools{
    maven 'Maven'
  }
  stages{
    stage('Checkout'){
      steps{
        <generate the github repo checkout code from snippet generator>
      }
    }
    stage('Maven build'){
      steps{
        sh 'mvn package'
      }
    }
    stage('Docker build'){
      steps{
        sh 'docker build -t addressbook .'
      }
    }
    stage('Docker list images'){
      steps{
        sh 'docker images'
      }
    }
  }
}
```
