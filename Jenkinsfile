pipeline{
    agent any
    tools{
        jdk 'jdk-17'
        nodejs 'Node18'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'master', url: 'https://github.com/rajneesh-kumar-sharma/StarBucks-Projects.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh "npm install"                
            }
        }        
        /*
        stage("Docker Build & Push"){
            steps{
                steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"DOCKERHUB_PASSWORD",usernameVariable:"DOCKERHUB_USERNAME")]){
                sh "docker build -t ${env.DOCKERHUB_USERNAME}/starbucks ."
                sh "docker login -u ${env.DOCKERHUB_USERNAME} -p ${env.DOCKERHUB_PASSWORD}"
                sh "docker tag ${env.DOCKERHUB_USERNAME}/starbucks ${env.DOCKERHUB_USERNAME}/starbucks:latest"
                sh "docker push ${env.DOCKERHUB_USERNAME}/starbucks:latest"
                }
            }
            }
        }
        
        stage('App Deploy to Docker container'){
            steps{
                sh 'docker run -d --name starbucks -p 3000:3000 coolrajnish/starbucks:latest'
            }
        }*/

    }
    post {
    always {
        script {
            def buildStatus = currentBuild.currentResult
            def buildUser = currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')[0]?.userId ?: 'Github User'
            
            emailext (
                subject: "Pipeline ${buildStatus}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <p>This is a Jenkins starbucks CICD pipeline status.</p>
                    <p>Project: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p>Build Status: ${buildStatus}</p>
                    <p>Started by: ${buildUser}</p>
                    <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: 'coolrajnish.sharma@gmail.com',
                from: 'coolrajnish.sharma@gmail.com',
                replyTo: 'coolrajnish.sharma@gmail.com',
                mimeType: 'text/html',
                attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
            )
           }
       }

    }

}
