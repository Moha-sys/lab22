pipeline{
    agent any
    
     environment {
        IMAGE_NAME='hello_backend'
    }

    stages{
        stage("checkout task3 branch"){
            steps{

                sh '''
                git checkout task3
                '''
            }

        }
        stage("login"){
            steps{
                
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) 

                {
                     sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                                            -u "$DOCKER_USERNAME" \
                                            --password-stdin                    
                                            
                        '''
                }

               
            }
         
        }

        stage("build image"){
            steps{
            sh '''
                docker build -t mostafaosmanfathi/${IMAGE_NAME}:${BUILD_NUMBER} .

            '''
            }
           

        }
        stage("push image"){
            steps{
            sh '''
                docker push mostafaosmanfathi/${IMAGE_NAME}:${BUILD_NUMBER}

            '''
            } 

        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
