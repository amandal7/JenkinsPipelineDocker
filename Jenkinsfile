pipeline {
    agent any

    stages {
        stage('Building Project') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'MyGitHub', url: 'https://github.com/amandal7/JenkinsPipelineDocker']]])
            }
        }
		stage('Building Docker Image') {
            steps {
                script{
				 sh 'docker build -t devopsank/firstPipelineDockerImageWithJenkins .'
				}
            }
        }
        
        stage('Pushing Image To DockerHub') {
            steps {
                script{
				withCredentials([string(credentialsId: 'dockercreds', variable: 'dockercreds')]) {
						sh 'docker login -u devopsank -p ${dockercreds}'
						sh 'docker push devopsank/firstPipelineDockerImageWithJenkins'
              }
				}
            }
        }
        
        stage('Running The Container') {
            steps {
                script{
						sh 'docker container run -d -p 9999:80 --name JenkinsPipelineDockerContainer2 devopsank/firstPipelineDockerImageWithJenkins'
				}
            }
        }
		
    }
}
