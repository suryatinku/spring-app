node {
  def image
   stage ('checkout') {
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/suryatinku/spring-app.git']]])      
        }
   
   stage ('Build') {
         def mvnHome = tool name: 'maven', type: 'maven'
         def mvnCMD = "${mvnHome}/bin/mvn "
         sh "${mvnCMD} clean package"           
        }
    stage('Build image') {
  
       app = docker.build("javademo:${BUILD_NUMBER}")
    }

    stage('Push image to ecr') {
        docker.withRegistry('https://098324025508.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:aws') {
            app.push("${env.BUILD_NUMBER}")
        }
    }       
}
