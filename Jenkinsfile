pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage ('BUild Docker Image'){
            when {
                branch 'master'
            }
            steps{
                script{
                app = docker.build("harry0521/trainscheduledeployement")
                sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        stage('Push Docker Image'){
            when{
            branch 'master'
            }
            steps{
                script{
                    docker.withRegistry(' https://registry.hub.docker.com', 'Docker_Hub_login'){
                    sh "docker push harry0521/trainscheduledeployement:latest "
                    }
                }
            }
        }
    }
}
