node{
      def buildNumber = BUILD_NUMBER
   stage("Git Clone"){
        git url: ' https://github.com/sandeepdurai/java-web-app-docker.git',branch: 'master'
    }
       stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven" 
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
    } 
    stage("Build Docker Image") {
         sh "docker build -t praveen1988/java-web-app-docker:${buildNumber} ."
    }
    stage("Docker login and Push"){
        withCredentials([string(credentialsId: 'docker_hub_pwd', variable: 'docker_hub_pwd')]) {
        sh "docker login -u praveen1988 -p ${docker_hub_pwd} " 
    }
        sh "docker push praveen1988/java-web-app-docker:${buildNumber}"
    }
    stage("Deploy to dockercontinor in docker deployer"){
        sshagent(['docker_dev_server']) {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@54.169.241.207 docker rm -f cloudcandy || true"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@54.169.241.207 docker run -d -p 8080:8080 --name cloudcandy praveen1988/java-web-app-docker:${buildNumber}"           
    }  
}
}
