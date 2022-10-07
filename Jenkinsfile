pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded late
            }
        }   
    
    stage('Unit Tests & Jacoco') {
            steps {
              sh "mvn test"
            }
         post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
    
    stage('Docker push ') {
            steps {
              
                 sh "docker build -t docker-registry:5000/java-app:latest ."
                 sh 'docker push docker-registry:5000/java-app:latest'
              
            }
    }
    
  }
  }
