node('linux') {
  
  stage('Create Test Stack') {
    git credentialsId: '60b8f86c-7d7b-431f-80f8-c64706b98a3a', url: 'https://github.com/UST-SEIS665/seis665-02-fall-2018-final-QianyueLi.git'
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'f87900c7-5108-430b-88f3-b960d132bc27', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
	    sh 'aws cloudformation create-stack --template-body file://./docker-single-server.json --stack-name final-test --tags Key=Name,Value=Stack --region us-east-1 --parameters ParameterKey=KeyName,ParameterValue=SEIS665W5
	    sh 'aws cloudformation describe-stacks --region us-east-1 --stack-name final-test'
	    sh 'aws cloudformation describe-stacks --stack-name final-test --region us-east-1 --query Stacks[].Outputs[].[OutputValue] --output text' 
	 
  }
  

	
  stage('Deploy Redis Standalone') {
	 sshagent(['cf99fa89-255b-417f-a499-d889d8f13fa6']) {
		 sh 'ssh -o StrictHostKeyChecking=no ubuntu@$dockerip docker run -d --name redis -p 6379:6379 redis:latest'
	  }	
	  
    
  }

  stage('Test Redis Standalone') {
	  sshagent(['cf99fa89-255b-417f-a499-d889d8f13fa6']) {
		  sh 'ssh -o StrictHostKeyChecking=no ubuntu@$dockerip redis-cli set hello world'
		  sh 'ssh -o StrictHostKeyChecking=no ubuntu@$dockerip redis-cli get hello'
	  }
	 
  }
  
  stage('Delete Test Stack') {
	  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'f87900c7-5108-430b-88f3-b960d132bc27', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
		  sh 'aws cloudformation delete-stack --region us-east-1 --stack-name final-test'
  
	  }
	  
	  
  }
  
  
}
