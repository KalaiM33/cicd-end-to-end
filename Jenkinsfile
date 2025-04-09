pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
               git branch: 'main', credentialsId: 'kalaim33', url: 'https://github.com/KalaiM33/cicd-end-to-end'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t kalaim33/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }
       stage('Login') {
            steps {
                script{
                    sh "docker login -u kalaim33 -p Praptika@2020"
                }
            }
    
        }
        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push kalaim33/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git branch: 'main', credentialsId: 'kalaim33', url: 'https://github.com/KalaiM33/cicd-demo-manifests-repo'
            }
        }
        
         stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                  withCredentials([gitUsernamePassword(credentialsId: 'KalaiGits', gitToolName: 'Default')]) {
                        sh '''
                        git config --global user.name "Kalaim33"
                        git config --global user.password "Praptika@2015"
                        cat deploy.yaml
                        sed -i "s/10/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/KalaiM33/cicd-demo-manifests-repo.git
                        '''                        
                    }
                }
            }
        }
    }
}
