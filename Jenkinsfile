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
        stage('Build docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("cwlau-pronto/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Push docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('http://registry.hub.docker.com','docker_hub_login'){
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }        
    }
}
