node
{
   stage('SCM Checkout'){
git credentialsId: '7f6edce2-bc0a-4b7e-9ed7-cc2a462ae9b3', url: 'https://github.com/Yphanikumar5/gradle-web-app.git'
    }
    stage('Gradle Clean Build'){
        sh 'gradle clean build'
    }
    stage('Build Docker Image'){
        sh 'docker build -t yphanikumar5/gradle-web-app .'
    }
    stage('Push Docker Image'){
      
withCredentials([string(credentialsId: 'df3ba3ae-53e6-4f72-9cf7-cd38459034d1', variable: 'Dockerpwd')]) {

  sh "docker login -u yphanikumar5 -p ${Dockerpwd}"
        }
         sh 'docker push yphanikumar5/gradle-web-app'
    }
    
     stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name gradle-web-app yphanikumar5/gradle-web-app'
         
        sshagent(['4e54ae87-8e9a-46c9-9ba7-bc98545f216b']) {
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.29.150 docker stop gradle-web-app || true'
          sh 'ssh  ubuntu@172.31.29.150 docker rm gradle-web-app || true'
          sh 'ssh  ubuntu@172.31.29.150 docker rmi -f  $(docker images -q) || true'
		  sh "ssh  ubuntu@172.31.29.150 ${dockerRun}"
    // some block
}

    }

}
