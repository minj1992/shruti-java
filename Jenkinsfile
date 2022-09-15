pipeline {
    agent any

    stages {
        stage('Gitcheckout') {
            steps {
                echo 'Cloning-Code..'
                git credentialsId: '17a69dcf-0d88-4e97-90b2-16ac5e581d99', poll: false, url: 'https://github.com/minj1992/shruti-java.git'
              //  slackSend channel: 'realtime-java-automation', message: 'Building........'
                slackSend channel: 'realtime-java-automation', color: '#439FE0', message: 'Cloning-Code........'
            }
                post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
             slackSend channel: 'realtime-java-automation', message: 'Success-Cloning-Code..'
        }
        failure{
            echo "========pipeline execution failed========"
             slackSend channel: 'realtime-java-automation', message: 'Job Failed'
        }
    }
        }
      stage("build & SonarQube analysis") {
            
            steps {
              withSonarQubeEnv('sonar') {
                 sh 'mvn clean package sonar:sonar'
              }
            }
          }
                
    }
}

