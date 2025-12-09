pipeline {
    agent any
    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }

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
                # copy WAR to Tomcat webapps as ROOT.war (overwrite)
                scp -o StrictHostKeyChecking=no target/*.war ec2-user@44.220.62.122:/opt/tomcat/webapps/ROOT.war

                # restart Tomcat gracefully
                ssh -o StrictHostKeyChecking=no ec2-user@44.220.62.122"/opt/tomcat/bin/shutdown.sh || true && sleep 2 && /opt/tomcat/bin/startup.sh"
                """
            }
        }
    }

    post {
        success { echo 'Deployment SUCCESS' }
        failure { echo 'Deployment FAILED' }
    }
}
