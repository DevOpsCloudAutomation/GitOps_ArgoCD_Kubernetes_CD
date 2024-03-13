pipeline
{
    agent any

    stages
    {
        stage('Git Checkout')
        {
            steps()
            {
                git branch: 'main', url: 'https://github.com/DevOpsCloudAutomation/Kubernetes_GitOps_ArgoCD.git'
            }
        }

        stage('Update Docker Image Tag in Kubernetes Manifest')
        {
            steps()
            {
                sh "cat Deployment.yaml"
                sh "sed -i 's/Build_Tag/${buildNumber}/g' Deployment.yaml"
                sh "cat Deployment.yaml"
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "DevOpsCloudAutomation"
                   git config --global user.email "Pavankumarkj347@gmail.com"
                   git add Deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'Jenkins_ArgoCD_Token', gitToolName: 'Git')]) {
                  sh "git push https://github.com/DevOpsCloudAutomation/Kubernetes_GitOps_ArgoCD main"
                }
            }
        }
    }

    post
    {
        always
        {
            cleanWs()
        }
    }
}
