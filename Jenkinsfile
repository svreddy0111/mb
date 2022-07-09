pipeline{
	agent any
	stages{
		
		stage("ImageBuild"){
		   steps{
			    sh """
					docker info
					pwd
					ls -l
					cd microservices
                    cp ../app/* .
					ls -l
					docker build -t docker.io/sheshivardhanp28/nbapp:v${BUILD_NUMBER} .
					docker images
				"""
		   }
		
		}
		stage("ImagePush"){
		   steps{
			  withCredentials([usernamePassword(credentialsId: 'DOCKERREG', passwordVariable: 'dpasswd', usernameVariable: 'duser')]) {
			  sh '''
			      docker login -u ${duser} -p ${dpasswd}
				  
				
			   '''
			}
		   }
		}
		stage("Deploy2k8s"){
			steps{
			  withCredentials([file(credentialsId: 'K8sLogin', variable: 'MyKey')]) {
				sh '''
			        cd microservices
					ls -l
					pwd
					cat pod.yml
					sed -i "s/IMAGEREP/docker.io\\/sheshivardhanp28\\/nbapp:v${BUILD_NUMBER}/g" pod.yml
					cat pod.yml
					scp -i ${MyKey} -o StrictHostKeyChecking=no pod.yml ubuntu@10.0.0.112:/home/ubuntu 
					
				'''
				}		
			}
		}
	}
}
