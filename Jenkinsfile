pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded lat
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
  stage('Dependency check ') {
            steps {
                 sh 'mvn dependency-check:check'
                 
            }
    }
   
    stage('Docker push ') {
            steps {
                 sh 'sudo usermod -a -G docker jenkins'
                 sh 'docker build -t tripurakant/numeric-app:""$GIT_COMMIT"" .'
            }
    }
        stage('K8s Deployment') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                sh "sed -i 's#REPLACE_ME#tripurakant/numeric-app:latest#g' k8s_deployment_service.yaml"
                sh "kubectl apply -f k8s_deployment_service.yaml"
                }
            }
    } 
  }
}
