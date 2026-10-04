pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        DOCKERHUB_USER = 'ranimnaffouti'
    }

    stages {
        stage('Build images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Push images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                 usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                    sh '''
                        echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                        for svc in backend frontend; do
                            docker tag $DOCKERHUB_USER/projets-$svc:latest $DOCKERHUB_USER/projets-$svc:$BUILD_NUMBER
                            docker push $DOCKERHUB_USER/projets-$svc:latest
                            docker push $DOCKERHUB_USER/projets-$svc:$BUILD_NUMBER
                        done
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
