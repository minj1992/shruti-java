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
        stage('Testing') {
            steps {
             sh 'mvn test -Dtest=FaresTest,SimpleTest'
            }
        }
                 stage('Building') {
            steps {
                sh 'mvn clean package'
            
            }
            post {
  success {
    // One or more steps need to be included within each condition's block.
    archiveArtifacts artifacts: 'target/*.war, *.sql', followSymlinks: false
  }
}

            
        }
        stage("deployement"){
            steps{
                echo "====++++executing deployement..."
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://34.170.124.154:8080/')], contextPath: null, war: '**/*.war'
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++A executed successfully++++===="
                }
                failure{
                    echo "====++++A execution failed++++===="
                }
        
            }
        }
        
    }

}

