pipeline {
    agent any

    stages{ 
        stage('Build') {
            steps {
                withGradle {
                    sh './gradlew assemble'
                }
            }
        }

     /*   stage('Test') {
            steps {
                withGradle {
                    sh './gradlew clean test'

                    configFileProvider([configFile(
                    fileId:'hello-grails-gradle.properties',
                    targetlocation: 'gradle.properties')]) {
                        sh './gradlew iT'
                    }
                    
                    //sh './gradlew -Dgeb.env=firefoxHeadless iT'
                    sh './gradlew codenarcTest'
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    publishHTML (target : [allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'build/reports/tests/',
                        reportFiles: 'index.html',
                        reportName: 'My Reports',
                        reportTitles: 'The Report'])
                }
            }
        }*/

        stage('QA') {
            steps {
                withGradle {
                    sh './gradlew check'

                    withSonarQubeEnv(
                    credentialsId: 'ba6547cc-5244-48a3-b921-0a4361ec46a4',
                    installationName: 'local') { 
                            sh './gradlew sonarqube'
                    }
                }
            }
        }
    }
}
