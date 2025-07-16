pipeline {
    agent any
    
    environment {
        // Define any environment variables if needed
        GIT_REPO = 'git@github.com:hashicorp/terraform.git'
        GIT_BRANCH = 'future/jenkins'
        SSH_CREDENTIAL_ID = 'sshagent'
    }
    
    stages {
        stage('Git Clone') {
            steps {
                // Clean workspace before cloning
                cleanWs()
                
                // Clone the repository using SSH
                sshagent(credentials: ["${SSH_CREDENTIAL_ID}"]) {
                    sh '''
                        git clone -b ${GIT_BRANCH} ${GIT_REPO} .
                        echo "Successfully cloned ${GIT_REPO} on branch ${GIT_BRANCH}"
                        git log --oneline -5
                    '''
                }
            }
        }
        
        stage('Build') {
            steps {
                echo 'Add your build steps here'
                // Example build commands:
                // sh 'make build'
                // sh 'go build .'
                // sh 'terraform init'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Add your test steps here'
                // Example test commands:
                // sh 'make test'
                // sh 'go test ./...'
                // sh 'terraform validate'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Add your deployment steps here'
                // Example deployment commands:
                // sh 'terraform plan'
                // sh 'terraform apply -auto-approve'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed'
            // Clean up workspace
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}