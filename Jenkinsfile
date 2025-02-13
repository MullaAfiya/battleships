pipeline {
    agent any
    environment {
        NEXUS_URL = 'http://13.201.188.247:8081/'
        NEXUS_CREDENTIAL_ID = 'Nexus-Creds'
       
    }
    
    stages {
        stage('Record Trigger Branch') {
            steps {
                script {
                    // Record the name of the branch that triggered the pipeline
                    def triggerBranch = env.BRANCH_NAME ?: 'master'
                    echo "Triggered by branch: ${triggerBranch}"
                    
                    // You can save this information to a file, send it to an external system,
                    // or use it for any other purpose as needed in your pipeline
                    writeFile file: 'trigger_branch.txt', text: "${triggerBranch}"
                }
            }
        }
        
        stage('Checkout') {
            steps {
                // Lightweight checkout support not available, falling back to full checkout
                checkout scm
            }
        }
        
        stage('Print Workspace') {
            steps {
                echo "Workspace contents:"
                sh "ls -la"
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Install npm dependencies
                sh 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                echo "Building on ${params.ENVIRONMENT} environment"
                sh "echo ENVIRONMENT: ${params.ENVIRONMENT}"
                    
                // Run Webpack to bundle the application
                sh 'webpack --config webpack.config.js'
            }
        }
        
        stage('Test') {
            steps {
                // Run tests if applicable
                sh 'npm test'
            }
        }
        
        stage('Build Image through Kaniko') {
            steps {
                echo "Build Image through Kaniko"
                // Add Kaniko build steps here
            }
        }
        
        stage('Push Artifact to Nexus Repo') {
            steps {
                echo "Push Artifact to Nexus Repo"
               steps {
                script {
                    // Retrieve Nexus credentials from Jenkins credentials store
                    def credentials = credentials(NEXUS_CREDENTIAL_ID)
                    if (credentials == null) {
                        error "Failed to retrieve Nexus credentials with ID: ${NEXUS_CREDENTIAL_ID}"
                    }
                    
                    // Publish Node.js artifacts to Nexus using Nexus API
                    sh "curl -v -u ${credentials.username}:${credentials.password} --upload-file ${NPM_PACKAGE_PATH} ${NEXUS_URL}/repository/npm-private"
                }
            }
        }
    }




                
            }
        }
        
        stage('Deployment in EKS through Helm') {
            steps {
                echo "Current branch name: ${env.BRANCH_NAME}"
                echo "Deployment in EKS through Helm"
                // Add deployment steps here
            }
        }
    }
}
