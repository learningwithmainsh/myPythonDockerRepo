def buildNumber = BUILD_NUMBER
pipeline {
     
    agent any

    environment {
        registry = "482834251634.dkr.ecr.ap-south-1.amazonaws.com/my-python-docker-repo"
    }
    

stages
{
        stage('Cloning Git') {
            steps{
        git 'https://github.com/learningwithmainsh/myPythonDockerRepo.git'
    }
    }

    
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }



stage('Pushing to ECR') {
     steps{  
         script {
          dockerImage = docker.build registry
                sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 482834251634.dkr.ecr.ap-south-1.amazonaws.com/'
                sh 'docker push 482834251634.dkr.ecr.ap-south-1.amazonaws.com/my-python-docker-repo'
         }
        }
      }
    
    
       // Stopping Docker containers for cleaner Docker run
     stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }

stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 482834251634.dkr.ecr.ap-south-1.amazonaws.com/my-python-docker-repo'
            }
      }
    }


    } 

//Sending email
     post{
    
always{
    
    emailext attachLog: true, to: 'mnshkmrpnd@gmail.com,manishkumarpndey144@gmail.com,manishkumarpandeyballia@gmail.com',
             subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
             body: "${JOB_NAME}   is over .. Build number  # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
             replyTo: 'manishkumarpandey144@gmail.com@gmail.com'
   }

}


    }
    
