
pipeline {
    environment{
    registry="achrefh/achrefh"
    registryCredential='6e848f2e-ffcb-4703-bc0b-c4e1db1bea7e'
    dokerImage="CICD"
}
    agent any
    stages {
        stage('Cloning the project') {
            steps {
                // clone from git and test trigger
                git branch: 'main', url: 'https://github.com/hammami-achref/ProjectCI.git'
                echo "-------------------Clone Stage Done ------------------------------- "
            }
        }
        stage("clean"){
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true clean"
                }
            }
        }
        stage("compile") {
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true compile"
                }
            }
        }
        stage("test") {
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true test"
                }
            }
        }
        stage("build") {
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true clean package"
                }
            }
        }
        stage("docker build") {
                       steps{
                         script {
                            dockerImage = docker.build registry +":$BUILD_NUMBER"
                       }
                 }
       }
        
        stage("docker push") {
              steps{
                 script {
                 withDockerRegistry(credentialsId: registryCredential) {
                  dockerImage.push()
                  }
                }
           }
        }
        stage('Cleaning up') {
             steps{
             sh "docker rmi $registry:$BUILD_NUMBER"
        }
    }
    stage('running containers'){
        steps{
            sh 'docker-compose up -d'
        }
    }
        
    }    

}
pipeline {
    agent any
    stages {
        stage('Cloning project') {
            steps {
                // clone project from git 
                git branch: 'main' , url: 'https://github.com/hammami-achref/ProjectCI.git'
                echo "-------------------Clone Stage Done ------------------------------- "
            }
        }
        stage("maven clean") {
            steps {
                script {
                    sh "mvn -X -Dmaven.test.failure.ignore=true clean"
                }
            }
        }
        stage("compile project") {
            steps {
                script {
                    sh "mvn -X -Dmaven.test.failure.ignore=true compile"
                }
            }
        }
        stage("Maven test") {
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true test"
                }
            }
        }
        stage("maven build") {
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true clean package"
                }
            }
        }
        stage('deploy to nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'tpAchatProject', classifier: '', file: 'target/tpAchatProject-1.0.jar', type: 'jar']], credentialsId: '40b847b0-8db8-4f34-b768-0f1179cc6f3e', groupId: 'com.esprit.examen', nexusUrl: '192.168.1.152:8081/repository/maven-releases/', nexusVersion: 'nexus3', protocol: 'http', repository: 'nexusdeploymentrepo', version: '1.0'
            }
        }
        
    }    

}
