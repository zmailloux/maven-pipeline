pipeline {
    agent any
    tools {
        maven 'maven-3.5.4'
        jdk 'jdk8'
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
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
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