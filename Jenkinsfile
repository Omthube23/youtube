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

        stage('03.Package') {
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

        stage('05.Verify Deployment') {
            steps {
                echo "Verifying Deployment"
                script {
                    def nexusURL = "http://localhost:8081/repository/shubham/com/msoffice/msexcel/0.0.1/msexcel-0.0.1-SNAPSHOT.jar"
                    def response = httpRequest(url: nexusURL, validResponseCodes: '200')
                    echo "Deployment verification response: ${response.content}"
                }
            }
        }

        stage('06.SCP to Docker Server') {
            steps {
                echo "SCP to Docker Server"
                script {
                    sshagent(credentials: ['id_rsa']) { // Ensure the ssh-agent plugin is installed
                        try {
                            def result = sh(
                                script: '''
                                    scp -v -i /home/ubuntu/.ssh/id_rsa \
                                    target/msexcel-0.0.1-SNAPSHOT.jar \
                                    user@docker-server:/root/Java-CBCICD/target/
                                ''', 
                                returnStdout: true
                            )
                            echo "SCP command output: ${result}"
                        } catch (Exception e) {
                            echo "SCP failed: ${e.getMessage()}"
                        }
                    }
                }
            }
        }
    }
}
