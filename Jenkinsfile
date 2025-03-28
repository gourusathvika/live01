pipeline {
    agent {
        label 'dev'
    }

    stages {
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/gourusathvika/live01.git'
            }
        }
        stage('maven') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('sonarqube') {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonarqube') {
                    sh 'mvn  sonar:sonar'
                }
            }
        }
        stage('nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'vamsi', classifier: '', file: 'target/live.war', type: 'war']], credentialsId: 'admin', groupId: 'vamsi.maven.com', nexusUrl: '54.147.17.206:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '0.0.1-SNAPSHOT'
            }
        }
         stage('tomcat') {
            steps {
               deploy adapters: [tomcat9(credentialsId: 'tomcat1', path: '', url: 'http://54.174.2.240:80')], contextPath: '/live', war: 'target/live.war'
            }
        }
    }
}
