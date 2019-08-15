pipeline {
    agent {
        none

    stages {
        stage('Checkout') {
            steps {
		sh 'echo checkout'
            }
        }    
        stage('Clean') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
		sh 'mvn compile'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Static Analysis for Sonar') {
            steps {
                // withSonarQubeEnv('YSBSonarqube') {
                //     sh 'mvn clean verify sonar:sonar'
                // }
		sh 'echo Sonar'
            }
        }
        stage('Sonar Gate') {
            steps {
                sh 'echo "sonar gate ok"'
            } 
        }
        stage('Build & Publish') {
            steps {
                sh 'mvn package'
		sh 'mvn install'
                sh 'echo "publish to artifactory ok"'
                // hygieiaArtifactPublishStep artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.0-SNAPSHOT"
            } 
        }
        stage('Deploy to DEV') {
            steps {
                sh 'echo "deploy to dev ok"'
            }
            post { 
                success {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.0-SNAPSHOT", buildStatus: 'Success', environmentName: 'DEV'
		    sh 'echo success' 
                }
                failure {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.0-SNAPSHOT", buildStatus: 'Failure', environmentName: 'DEV'
		    sh 'echo failure'
                }
            }
        }
        stage('Hygieia') {
            steps {
                sh 'echo "Update Hygieia"'
            }
	}
    }
}
