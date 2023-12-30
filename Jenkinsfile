pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
                script {
                    if (isUnix()) {
                        git branch: 'main', url: 'https://github.com/Omar-Eidaros/course3-jenkins-gs-spring-petclinic'
                    } else {
                        checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/Omar-Eidaros/course3-jenkins-gs-spring-petclinic']]])
                    }
                }
            }
        }
        stage("build") {
            steps {
                script {
                    if (isUnix()) {
                        sh "./mvnw build"
                    } else {
                        bat ".\\mvnw.cmd package"
                    }
                }
            }
        }
        stage("capture") {
            steps {
                script {
                    archiveArtifacts artifacts: 'target/*.jar'
                    jacoco()
                    junit 'target/surefire-reports/TEST*.xml'
                }
            }
        }
    }
    
    post {
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
                recipientProviders: [developers()],
                subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                to: 'alldevelopers@eda.com'
        }
    }
}
