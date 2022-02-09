pipeline {
    agent any
   
    
    tools{
        maven 'MY_MAVEN'

    }

    stages {
        stage('Git_clone') {
            steps {
                git credentialsId: 'GIT_HUB_CREDENTIAL_ID', url: 'https://github.com/ashokbawge/hello-world.git'
            }
        }
    
        stage ('Maven_build'){
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage ('Docker_Build_image'){
            steps {
                sh ' docker build -t ashokbawge/jenkins_pipeline_demo:${BUILD_NUMBER} . '
            }
        }
        
        stage ('Docker push image') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_HUB_CREDENTIAL_ID', variable: 'docker_hub_credential_id')]) {
                    sh ' docker login -u ashokbawge -p ${docker_hub_credential_id}'
                        }
                
                
                sh ' docker push ashokbawge/jenkins_pipeline_demo:${BUILD_NUMBER}  '
                sh ' docker rmi ashokbawge/jenkins_pipeline_demo:${BUILD_NUMBER} '
            }
        }
        
     stage ('Docker container creation'){
             steps {
                            sshagent(['jenkins']) {
                             
                            sh ' ssh   -o StrictHostKeyChecking=no vagrant@100.0.0.50 docker stop javawebapp '
                             sh ' ssh   -o StrictHostKeyChecking=no vagrant@100.0.0.50 docker rm javawebapp ' 
                            sh 'ssh -o StrictHostKeyChecking=no vagrant@100.0.0.50 docker run -itd --name javawebapp -p 8080:8080 ashokbawge/jenkins_pipeline_demo:${BUILD_NUMBER}'
                           }
             }
            
        }
        
    
        
        
    }
    
}
