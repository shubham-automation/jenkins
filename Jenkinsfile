pipeline {
    agent any

    stages {        
        stage('Run Automation Test Cases') {
            steps {
                script {
                    sh "pip3 install -r requirements.txt"
                    sh "python3 test.py"
                }
            }
                post {
                  always {
                    junit 'test-reports/*.xml'
                  }
                }    
        }

        stage('Docker Build') {
            steps {
                script {
                  sh "docker build -t v1 ."
                  sh "docker tag v1:latest chaudharishubham2911/cicd-demo1:v1"
                  sh "docker push chaudharishubham2911/cicd-demo1:v1"
                }
            }
        }

        stage('Image Scanning') {
            steps {
                script {
                  curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl
                  trivy image --format template --template "@html.tpl" --output trivy_report.html --exit-code 1 --severity HIGH,CRITICAL chaudharishubham2911/cicd-demo1:v1                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                  sh "docker build -t v1 ."
                        sh "docker tag v1:latest chaudharishubham2911/cicd-demo1:v1"
                        sh "docker push chaudharishubham2911/cicd-demo1:v1"
                }
            }
        }

        stage('Docker build') {
            steps {
                script {
                        def registryCredentials = [
                        credentialsId: 'docker-creds'
                        ]
                        withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                        sh "docker build -t v1 ."
                        sh "docker tag v1:latest chaudharishubham2911/cicd-demo1:v1"
                        sh "docker push chaudharishubham2911/cicd-demo1:v1"
                    }
                }
            }
        }






    }
}
