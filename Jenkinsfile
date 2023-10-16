pipeline {
    agent any
    tools {
        maven 'maven3'
    }

    stages {
        stage('Build Artifact') {
            steps {
                script{
                  sh 'mvn clean package -DskipTests'
                }
                
            }
        }

        stage('Test') {
            steps {
                script{
                  sh 'mvn test'
                }
            }
        }
        stage('Deploy') {
    steps {
        script {
            def ec2Creds = credentials('ec2-creds') // Replace with your SSH credential ID
            sshagent(credentials: [ec2Creds]) {
                sh 'scp -o StrictHostKeyChecking=no target/vprofile-v2.war ubuntu@16.171.227.178:/home/ubuntu/'
                sh 'ssh ubuntu@16.171.227.178 "sudo cp -rf /home/ubuntu/vprofile-v2.war /var/lib/tomcat9/webapps"'
                sh 'ssh ubuntu@16.171.227.178 "sudo systemctl restart tomcat9"'
            }
        }
    }
}

}
}
