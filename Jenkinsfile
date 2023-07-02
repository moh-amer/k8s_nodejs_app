node('kube-agent') {


        stage('CI : Build Image') {
            container('kaniko') {
                stage('Build and push to docker hub') {
                  
                        script{
                            sh '''
                            
                            /kaniko/executor --git branch=k8s_task --context git://github.com/moh-amer/k8s_nodejs_app \
                            --insecure --skip-tls-verify --destination nexus-service.tools.svc.cluster.local/nodejs-app:${BUILD_NUMBER}
                            
                            '''
                        }
                    
                }
            }
        }
        
    }
