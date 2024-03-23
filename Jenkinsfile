pipeline {
    agent any

    stages {
        withMaven(globalMavenSettingsConfig: '', jdk: '', maven: 'maven', mavenSettingsConfig: '', traceability: true) {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                checkout scm
            }
        }
    stage('Build') {
            steps {
                // Maven build step
                sh 'mvn clean package'
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
}
