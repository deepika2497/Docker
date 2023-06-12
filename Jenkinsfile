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
        stage("Upload Artifact s3") {
            steps {
                script {
                    sh "aws s3 cp target/vprofile-v1.war s3://automation999/vprofile-artifact/vprofile-v1.war"
                }
            }
        }


    }
}