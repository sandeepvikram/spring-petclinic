pipeline {
    agent { label 'SPC'}
        
    triggers {
        pollSCM ('* * * * *')
    }
    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'git branch')
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        retry(2)       
    }
    stages {
        stage('scm') {
            environment {
                SOURCECM = 'GITHUB'
            }
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/sandeepvikram/spring-petclinic.git'
                echo env.SOURCECM
            }
        }
        stage('build') {
            environment {
                buildd = 'MAVEN'
            }
            steps {
                echo env.MAVEN
                timeout(time: 10, unit: 'MINUTES') {
                    sh "cd spring-petclinic"
                    sh "./mvnw ${params.GOAL}"
            } 
        }
    post {
        success {
            archive '**/*.war'
            junit '**/TEST-*.xml'
            echo 'The build is successfull'
        }
        failure {
            echo 'The build has failed'
        }
        always {
            echo 'This is good'
        }

    }
}
