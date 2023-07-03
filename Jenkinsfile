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

                
        stage("CI: Edit Configs Repo"){
            
         container('deployer'){
             
            stage("Clone Git Repository") {
          
                git(
                    url: "https://github.com/moh-amer/graduation_config",
                    branch: "main",
                    changelog: true,
                    poll: true
                )
                
            
             }
        
        stage("Change Deployment Configs") {
            
                sh "git config --global --add safe.directory '/home/jenkins/agent/workspace/${env.JOB_BASE_NAME}'"
                sh "git config --global user.email 'medodeth666@gmail.com' "
                sh "git config --global user.name 'moh-amer' "
                sh "sed -i 's/\\(image: docker\\.nexus\\.local\\.com\\/nodejs-app:\\).*/\\1${BUILD_NUMBER}/' deployment.yaml"

                sh "git add . "
                sh "git commit -m 'Editing TAG to ${BUILD_NUMBER}'"
                 
             }
             
             stage("Push to Git Repository") {
                withCredentials([gitUsernamePassword(credentialsId: 'git-hub-moh_amer', gitToolName: 'Default')]) {
                    sh "git push -u origin main"
                }
   
              }
        }
    }
        
    }
