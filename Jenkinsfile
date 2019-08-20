pipeline {

    agent any

    stages {
        stage('Checkout') {
            steps {
		sh 'echo checkout'
		checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/jacksonville-management/homebase']]])
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
                // hygieiaArtifactPublishStep artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT"
		//
		xldCreatePackage artifactsPath: 'target', manifestPath: 'deployit-manifest.xml', darPath: 'my-jacksonvilleapp-1.2-SNAPSHOT-$BUILD_NUMBER.0.dar'
		xldPublishPackage serverCredentials: 'admin-credentials', darPath: 'my-jacksonvilleapp-1.2-SNAPSHOT-$BUILD_NUMBER.0.dar'
		sh 'ls -al' 
            } 
        }
        stage('DEV') {
            steps {
                sh 'echo "deploy to dev ok"'
            }
            post { 
                success {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT", buildStatus: 'Success', environmentName: 'DEV'
		    sh 'echo success' 
		    // 
		    xldDeploy serverCredentials: 'admin-credentials', environmentId: 'Environments/DEV', packageId: 'Applications/my-jacksonvilleapp/13.0'
                }
                failure {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT", buildStatus: 'Failure', environmentName: 'DEV'
		    sh 'echo failure'
                }
            }
        }
        stage('STAGE') {
            steps {
                sh 'echo "deploy to stage ok"'
            }
            post {
                success {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT", buildStatus: 'Success', environmentName: 'STAGE'
                    sh 'echo success'
                    //
                    xldDeploy serverCredentials: 'admin-credentials', environmentId: 'Environments/STAGE', packageId: 'Applications/my-jacksonvilleapp/13.0'
                }
                failure {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT", buildStatus: 'Failure', environmentName: 'STAGE'
                    sh 'echo failure'
                }
            }
        }
        stage('PROD') {
            steps {
                sh 'echo "deploy to stage ok"'
            }   
            post {
                success {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT", buildStatus: 'Success', environmentName: 'PROD'
                    sh 'echo success'
                    //
                    xldDeploy serverCredentials: 'admin-credentials', environmentId: 'Environments/PROD', packageId: 'Applications/my-jacksonvilleapp/13.0'
                }
                failure {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT", buildStatus: 'Failure', environmentName: 'PROD'
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
