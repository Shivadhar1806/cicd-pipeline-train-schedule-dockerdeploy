agentName = "server-slave"

pipeline {
  agent none
  stages {
     stage('Prep') {
            steps {
                script {
                    agentName = "server-slave"
                }
            }
        }
     stage ('Build') {
        agent { label agentName }
        steps {
          echo 'Running build stage'
          sh './gradlew build --no-daemon'
          archiveArtifacts artifacts: 'dist/trainSchedule.zip'
       }
    }
     stage('Build Docker Image') {
            when {
                branch 'master'
            }
            agent { label agentName }
            steps {
                script {
                    app = docker.build("shdh/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            agent { label agentName }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
