pipeline {
    agent any
    tools {
        maven 'maven-3.5.4'
        jdk 'jdk-8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn release:update-versions -DdevelopmentVersion=1.0.${BUILD_NUMBER}-SNAPSHOT'
                sh 'mvn -B clean verify -DskipMunitTests'
            }
            post {
                success {
                    publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'target/munit-reports/coverage',
                        reportFiles: 'summary.html',
                        reportName: 'MUnit Report'
                    ]
                    publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: 'JUnit Report'
                    ]
                }
            }
        }


        stage ('Acceptance Testing') {
            steps {
                sh '''
                    echo "Here we would do some acceptance tests"
                '''
            }
        }

    }

    post{
      success {
          script {
              sh 'echo Sending JAR to artifactory...'
              def server = Artifactory.newServer url: 'http://35.192.53.59/artifactory', username: 'admin', password: 'Turtle00'
              // Artifactory pro
              def uploadSpec = """{
                "files": [
                  {
                    "pattern": "target/*.jar",
                    "target": "maven-app/builds/${GIT_BRANCH}/"
                  }
               ]
              }"""
              def buildInfo = Artifactory.newBuildInfo()
              server.upload spec: uploadSpec, buildInfo: buildInfo
              buildInfo.env.collect()
              server.publishBuildInfo buildInfo

          }
      }
    }
}
