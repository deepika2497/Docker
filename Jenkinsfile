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
        stage('Upload to S3') {
    steps {
        script {
            def s3Bucket = 'my-automation1997-bucket'
            def artifactName = 'vprofile-v2.war'
            sh "aws s3 cp vprofile-v2.war s3://${s3Bucket}/${artifactName}"
        }
    }
}

        stage('Deploy') {
    steps {
        script {
            withCredentials([sshUserPrivateKey(credentialsId: 'ec2-creds', keyFileVariable: 'SSH_KEY')]) {
                sh 'scp -o StrictHostKeyChecking=no -i $SSH_KEY target/vprofile-v2.war ubuntu@16.171.227.178:/home/ubuntu/'
                sh 'ssh -i $SSH_KEY ubuntu@16.171.227.178 "sudo cp -rf /home/ubuntu/vprofile-v2.war /var/lib/tomcat9/webapps"'
                sh 'ssh -i $SSH_KEY ubuntu@16.171.227.178 "sudo systemctl restart tomcat9"'
            }
        }
    }
}


}
}
