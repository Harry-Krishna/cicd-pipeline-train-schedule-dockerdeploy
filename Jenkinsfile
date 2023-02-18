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
                app.inside{
                    sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Push Docker Image'){
            when{
            branch 'master'
            }
            steps{
                script{
                    withDockerRegistry(credentialsId: 'Docker_hub', url:  'https://registry.hub.docker.com') {
                    app.push("${env.v1}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
