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
                app = docker.build("harry0521/trainschedule")
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
                    withDockerRegistry(credentialsId: 'Docker_hub', url: 'https://login.docker.com/u/login/identifier?state=hKFo2SBBQWJ3MFUtSnFuVmxVSDB4ZDhaNW56ekl4c2hwTm5pOaFur3VuaXZlcnNhbC1sb2dpbqN0aWTZIEw0MndUc25ucHJJdl9DTzAzTUVrQjYwM1FWN1ZZZkpoo2NpZNkgbHZlOUdHbDhKdFNVcm5lUTFFVnVDMGxiakhkaTluYjk') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
