paipeline {
    agent {
        label'master'
    }
    parameters {             
        string(name: 'image_tag', defaultValue: '', description: 'jenkins image build tag',)
        //gitParameter(branch: '', branchFilter: 'origin/(.*)', defaultValue: '', description: '', name: 'BRANCH', type: 'PT_BRANCH',) 
    }    
    stages {        
        stage('checkout scm') {            
            steps {                
                git branch: 'master',                    
                credentialsId: 'github_jenkins',                        
                url: 'https://github.com/oscarose/dockertest.git'           
            }        
       }
       stage('copy file from s3 bucket') {
           steps {
              sh """
              cd ${WORKSPACE}
              aws s3 cp s3://quinta/jenkins.war .
              aws s3 cp s3://quinta/apache-tomcat-8.5.50.tar.gz .
              aws s3 cp s3://quinta/jdk-8u241-linux-x64.tar.gz .
              """
           }
       }
       stage('docker build image and push to docker hub') {                 
           docker.withRegistry('https://index.docker.io/v1/', 'myrepodocker') {
               def app = docker.build("belushi/jenkins:${params.image_tag}", '.').push()
           }
       }
    }
}
