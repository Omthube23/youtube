pipeline {
    agent any 

    stages {
        stage('01.Clone Repo') {
            steps {
                echo "Git Cloning"
                git branch: 'main', credentialsId: 'Omthube23', url: 'git@github.com:Omthube23/youtube.git'
            }
        }

        stage('02.Clean') {
            steps {
                echo "Cleaning"
                sh 'mvn clean'
            }
        }

        stage('03.Package') { // Corrected stage name
            steps {
                echo "Packaging"
                sh 'mvn package'
            }
        }

        stage('04.Deploy to Nexus') {
            steps {
                echo "Deploy to Nexus"
                sh '''
                    curl -v -u admin:admin \
                    --upload-file target/youtube-0.0.1-SNAPSHOT.jar \
                    http://localhost:8081/repository/omm/com/google/youtube/0.0.1/youtube-0.0.1-SNAPSHOT.jar
                '''
            }
        }
    }
}
