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
}
  
   stage('sonarQube ') {
            steps {
                 sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=numerc-2 -Dsonar.host.url=http://35.226.175.104:9000 -Dsonar.login=sqp_bb04a8a3989f79440dd36f9101eeee6a27d82cc7'
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
  
