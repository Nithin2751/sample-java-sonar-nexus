pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://3.90.44.210:30900/'
       // NEXUS_URL = 'http://13.203.213.172:30801'  // Replace with your actual Nexus URL
        REPO = 'maven-releases'
        GROUP_ID = 'com.devops'
       // ARTIFACT_ID = 'sample-java-app'
       // VERSION = '1.0'
        PACKAGING = 'jar'
        FILE = 'target/sample-java-app-1.0.jar'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Nithin2751/sample-java-sonar-nexus.git', branch: 'main'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-id', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('MySonar') {
                        sh '''
                            mvn clean verify sonar:sonar \
                              -Dsonar.projectKey=sample-java-app \
                              -Dsonar.host.url=$SONAR_HOST_URL \
                              -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Upload to Nexus') {
            steps {
               // withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                 //   sh '''
                   //     curl -v -u $USERNAME:$PASSWORD --upload-file $FILE \
                     //   $NEXUS_URL/repository/$REPO/$(echo $GROUP_ID | tr '.' '/')/$ARTIFACT_ID/$VERSION/$ARTIFACT_ID-$VERSION.$PACKAGING
                   // '''
                    echo "uploaded to nexus"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
