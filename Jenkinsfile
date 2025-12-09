pipeline {
    agent any
   
    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/gopiseshu/javajenkins.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn -B -DskipTests=false clean package'
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
               sh """
                  scp -o StrictHostKeyChecking=no target/myapp-1.0-SNAPSHOT.war ec2-user@44.220.62.122:/opt/tomcat/webapps/ROOT.war

                  ssh -o StrictHostKeyChecking=no ec2-user@44.220.62.122 "/opt/tomcat/bin/shutdown.sh || true && sleep 2 && /opt/tomcat/bin/startup.sh"
                  """
           }
       }

    }

    post {
        success { echo 'Deployment SUCCESS' }
        failure { echo 'Deployment FAILED' }
    }
}
