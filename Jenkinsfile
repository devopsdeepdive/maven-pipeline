pipeline {
  agent any

stages {
  stage('Checkout') {
    steps {
checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-user', url: 'https://github.com/devopsdeepdive/maven-pipeline.git']]])    }
  }
  
  stage('Validate') {
    steps {
      sh 'mvn validate'
    }
  }
  stage('Compile') {
    steps {
      sh 'mvn compile'
    }
  }
  stage('Test') {
    steps {
      sh 'mvn test'
    }
  }
  stage('Package') {
    steps {
      sh 'mvn package'
    }
  }
  
  stage('Deploy') {
            steps {
      sshagent(['jenkins-slave']) {
    sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/maven-deploy-pipeline/target/maven-web-application.war ubuntu@ec2-174-129-57-188.compute-1.amazonaws.com:/opt/apache-tomcat-8.5.82/webapps/"
}

        }
    }
   stage('Notify') {
    steps {
      sh slackSend channel: '#prod_notfications', color: '#439FE0', iconEmoji: ':)', message: 'My jenkins build successfully deployed ', teamDomain: 'devopsdeepdivebatch', tokenCredentialId: 'slack-integration'
    }
  }
  

}
}
