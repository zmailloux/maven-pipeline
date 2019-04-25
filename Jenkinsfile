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
		                echo "Working"
                    //junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
    }
}
