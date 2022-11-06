pipeline {
    agent any
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t nayeem0987/nodeapp:v1 "
            }
        }
		stage('Push Image to Docker Hub'){
			steps{
				withCredentials([string(credentialsId: 'dockerhubPWD', variable: 'dockerhubPWD')]) {
					sh "docker login -u nayeem0987 -p ${dockerhubPWD}"
					sh "docker push nayeem0987/nodeapp:v1"
				}
			}
		}
	    
	    
		stage('Login to staging server') {
            steps {
                sshagent(['login-to-minikube']) {		
		     sh 'eval $(minikube docker-env) && scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/for-java/pods.yml services.yml nayeem@192.168.122.160:/home/nayeem/'
			 script{
				try{
				        
					sh "ssh nayeem@192.168.122.160 kubectl apply -f ."
				}catch(error){
					sh "ssh nayeem@192.168.122.160 kubectl create -f ."
				}
			 }
		     
                    }
            }
        }
		stage('Login again to minikube server'){
			steps{
				sshagent(['login-to-minikube']) {
		     sh 'ssh -o StrictHostKeyChecking=no nayeem@192.168.122.160'
            }
	}
	}
   }
}
