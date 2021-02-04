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

        stage('Test') {
            steps {
                withGradle {
                    sh './gradlew clean test'

                    configFileProvider([configFile(
                    fileId:'hello-grails-gradle.properties',
                    variable: 'systemProp.geb.env')]) {
                        sh './gradlew -Deb.env=$systemProp.geb.env iT'
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
                        reportDir: 'build/reports',
                        reportFiles: 'myreport.html',
                        reportName: 'My Reports',
                        reportTitles: 'The Report'])
                }
            }
        }
    }
}
