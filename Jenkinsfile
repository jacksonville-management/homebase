pipeline {

    agent any

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
		//
		xldCreatePackage artifactsPath: 'target', manifestPath: 'deployit-manifest.xml', darPath: 'my-jacksonvilleapp-1.0-SNAPSHOT-$BUILD_NUMBER.0.dar'
		xldPublishPackage serverCredentials: 'admin-credentials', darPath: 'my-jacksonvilleapp-1.0-SNAPSHOT-$BUILD_NUMBER.0.dar'
		sh 'ls -al' 
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
		    // 
		    // xldDeploy serverCredentials: 'admin-credentials', environmentId: 'Environments/DEV', packageId: 'Applications/my-jacksonvilleapp/5.0'
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
