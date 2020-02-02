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
  }
}
