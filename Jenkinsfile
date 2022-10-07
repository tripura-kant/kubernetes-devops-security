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
                 sh 'sudo usermod -a -G docker jenkins'
                 sh "docker build -t tripurakant/numeric-app:""$GIT_COMMIT"" ."
                 sh 'docker push tripurakant/numeric-app:""$GIT_COMMIT""'
              
            }
    }
    
  }
  }
