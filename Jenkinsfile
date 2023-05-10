pipeline {
    agent { label 'jenkins-slave' }
    stages {
        stage('build') {
            steps {
                script {
                   withCredentials([usernamePassword(credentialsId: 'dockerhubaccount', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) 
                    {
                          sh """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t monasamir/server:a${BUILD_NUMBER} $WORKSPACE/badreads-backend/ 
                            docker push monasamir/server:a${BUILD_NUMBER} 
                       """
                   
                       sh """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t monasamir/client:a${BUILD_NUMBER} $WORKSPACE/badreads-frontend/
                            docker push monasamir/client:a${BUILD_NUMBER}
                       """
                   }
                }
            }
        }
        stage('deploy') {
            steps {
                      sh """
                         echo "Running Helm"
                         helm install vois${BUILD_NUMBER} onboard-task
                        """                                    
                  }
            }
    }
}
