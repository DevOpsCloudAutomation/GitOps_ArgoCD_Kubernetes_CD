pipeline
{
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
                sh "sed -i 's/Build_Tag/${Build_Number}/g' Deployment.yaml"
                sh "cat Deployment.yaml"
            }
        }

        stage("Push the changed deployment file to Git")
        {
            steps
            {
                sh """
                   git config --global user.name "DevOpsCloudAutomation"
                   git config --global user.email "Pavankumarkj347@Gmail.com"
                   git add Deployment.yaml
                   git commit -m "Updated Kubernetes Deployment File"
                """

                withCredentials([gitUsernamePassword(credentialsId: 'GitHub_Token', gitToolName: 'Default')])
                {
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