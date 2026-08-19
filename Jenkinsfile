pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/injamurikumar410-create/spring-boot-mongo-docker-kkfunda-kk.git'
            }
        }

        stage('Setup KubeConfig') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-eks-cred']]) {
                    sh '''
                        aws eks update-kubeconfig --region ap-southeast-2 --name my-cluster
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
    steps {
        sh '''
            pwd
            ls -la
            find . -name "springBootMongo.yaml"
            kubectl apply -f springBootMongo.yaml --validate=false
        '''
            }
        }

        stage('Verify Pods and Services') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-eks-cred']]) {
                    sh '''
                        kubectl get pods
                        kubectl get svc
                    '''
                }
            }
        }
    }
}
