pipeline {
    agent {label 'agent1'}
    environment {
        GIT_REPOSITORY = "${GIT_URL.tokenize('/')[-1].tokenize('.')[0]}"
    }
    stages {
        stage("Clean Workspace") {
            steps {
                deleteDir()
            }
        }
        stage("Checkout") {
            steps {
                checkout scm
                sh('env')
            }
        }
        stage('Run Sonar scanner') {
            environment {
                PROJECT_KEY = "scan-${env.GIT_REPOSITORY}"
                PROJECT_NAME = "scan-${env.GIT_REPOSITORY}"
                PROJECT_VERSION = '1.0'
                SONAR_OPTS = ''
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh('''
                    sonar-scanner \\
                            -Dsonar.projectKey="${PROJECT_KEY}" \\
                            -Dsonar.projectName="${PROJECT_NAME}" \\
                            -Dsonar.projectVersion="${PROJECT_VERSION}" \\
                            -Dsonar.sources=./sonarqube-scanner/src \\
                            -Dsonar.analysis.mode=publish \\
                            ${SONAR_OPTS}
                    ''')
                }
            }
        }
        /*
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        */
    }
}
