@Library("Shared") _
pipeline {
    agent {label "Jarvis"}

    stages {
        stage("Initialize"){
            steps{
                script{
                    hello()
                }
            }
        }
       stage("Code"){
        steps{
            script{
                clone("https://github.com/dev-anurag264/django-notes-app.git", "main")    
            }
        }
    }
    stage("Build"){
        steps{
               echo "Building code"
               docker_build("notes-app","latest","anurag17s")
        }
    }
    stage("Push to dockerhub"){
        steps{
            echo "Pushing the image to docker hub"
            withCredentials([usernamePassword('credentialsId':"DockerhubCred",passwordVariable:"DockerhubPass",usernameVariable:"DockerhubUser")]){
                sh "docker login -u ${env.DockerhubUser} -p ${DockerhubPass}"
                sh "docker image tag notes-app:latest ${env.DockerhubUser}/notes-app:latest"
                sh "docker push ${env.DockerhubUser}/notes-app:latest"  
            }
      
        }
    }
    stage("Deploy"){
        steps{
               echo "Deploying code"
               sh "docker compose up -d --build"
        }
    }
    }
}
