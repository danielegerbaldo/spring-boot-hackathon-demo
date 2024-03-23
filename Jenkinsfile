pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                checkout scm
            }
        }
    stage('Build') {
            steps {
                withGradle {
                // Gradle build step
                sh './gradlew publishToMavenLocal'
                }
            }
     }
    stage('SonarQube Code Analysis') {
                steps {
                    dir("${WORKSPACE}"){
                    // Run SonarQube analysis for Angular
                    script {
                        def scannerHome = tool name: 'springboot-demo', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                        withSonarQubeEnv('sonar') {
                            sh "echo $pwd"
                            sh "${scannerHome}/bin/sonar-scanner"
                        }
                    }
                }
                }
           }
           stage("SonarQube Quality Gate Check") {
                steps {
                    script {
                    def qualityGate = waitForQualityGate()
                        
                        if (qualityGate.status != 'OK') {
                            echo "${qualityGate.status}"
                            error "Quality Gate failed: ${qualityGateStatus}"
                        }
                        else {
                            echo "${qualityGate.status}"
                            echo "SonarQube Quality Gates Passed"
                        }
                    }
                }
            }
        }
}
