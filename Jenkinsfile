node(){
    
    stage("checkout from SCM"){
        
      git 'https://github.com/Premvikash/java-maven-sample-war.git'  
    }
    stage("Maven build"){
        def mvnHome = tool name: 'M2_HOME', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean package"
    }
    stage("Build docker image"){
        
        sh "docker build -t neelakanta03/myimage:1.0 ."
    }
    stage("Push image"){
        withCredentials([string(credentialsId: 'DOCKERHUB_CREDENTIALS', variable: 'DOCKERHUB_CREDENTIALS')]) {
          sh "docker login -u neelakanta03 -p ${DOCKERHUB_CREDENTIALS}"
      }
      sh "docker push neelakanta03/myimage:1.0"
    }

     stage("Remove existing container"){
 	def removeContainer = "docker rm -f MyContainer"
          try{
	sshagent(['ec2-user-pem']) {
   	 sh "ssh -o StrictHostKeyChecking=no ec2-user@13.127.66.53 ${removeContainer}"
	}
          }catch(error){
		sh "echo No contianer exists"
	}

	}
    
    stage("Create container"){
        def runContainer = "docker run -itd --name MyContainer -p 8080:8080 kneelakanta03/myimage:1.0"
	sshagent(['ec2-user-pem']) {
   	 sh "ssh -o StrictHostKeyChecking=no ec2-user@13.127.66.53 ${runContainer}"
	}

     }
    
}
