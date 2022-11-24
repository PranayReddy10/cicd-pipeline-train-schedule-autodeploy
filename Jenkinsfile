pipeline {
    agent {
        label 'agent01'
    }
    environment { 
           DOCKER_IMAGE_NAME = "pranayreddy10/train-schedule"
           DOCKERHUB_CREDENTIALS = credentials('dckr_pat_bT5b05gIEcaxrxwG8ajx55Tk3xc')
    }  
    stages {
        stage('Clone MS-Repo') {
            steps {
                script 
                {
checkout([$class: 'GitSCM', branches: [[name: 'master']],  userRemoteConfigs: [[url: 'https://github.com/PranayReddy10/cicd-pipeline-train-schedule-autodeploy.gits']]] 
}
            }				
        }
        stage('Build') {
            steps { 
                script {
                	sh '''
                	echo 'Gradle Build Started'
                	./gradlew build --no-daemo
              		'''
                }   
            }
        }
        stage('Containerize') {
            steps{
            sh '''
            echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
            docker build -t $DOCKER_IMAGE_NAME cicd-pipeline-train-schedule-autodeploy/Dockerfile .
            docker push $DOCKER_IMAGE_NAME
             '''
            }
        }
        stage ('K8S Deploy'){
            steps{
                sh ''' 
                  kubectl get pods
                  kubectl apply -f .cicd-pipeline-train-schedule-autodeploy/train-schedule-kube.yml
                  '''

            } 
        }
    }
}  
