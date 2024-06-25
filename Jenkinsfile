pipeline {
    agent any
    stages{
        stage("checkout"){
            steps{
            sh 'ls'
            git branch:'main', url: 'https://github.com/yassineKhouaja/course3-jenkins-gs-spring-petclinic'
            sh 'ls'   
            }

        }
        stage("build"){
            steps{        
                sh "./mvnw compile"
                sh "./mvnw package"
            }
        }
        stage("capture"){
            steps{
                sh "ls target"
                archiveArtifacts '**/target/*.jar'
                jacoco()
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
    }
    post {
        always{ // regression
            emailext body: "${env.BUILD_URL} \n${currentBuild.absoluteUrl}",
                to: 'yassinekhouaja@gmail.com', recipientProviders: [previous()],
                subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"   
        }
    }
}