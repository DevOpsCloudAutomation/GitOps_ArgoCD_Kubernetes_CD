pipeline
{
    agent any

    environment
    {
        application_Name = "webapplication-argocd"
    }

    stages
    {
        stage('Git Checkout')
        {
            steps()
            {
                git branch: 'main', url: 'https://github.com/DevOpsCloudAutomation/GitOps_ArgoCD_Kubernetes_CD.git'
            }
        }

        stage('Update Docker Image Tag in Kubernetes Manifest')
        {
            steps()
            {
                sh "sed -i 's/${application_Name}.*/${application_Name}:${buildNumber}/g' Deployment.yaml"
            }
        }

        stage("Push Updated Kubernetes Deployment File to GitHub")
        {
            steps()
            {
                sh """
                    git config --global user.name "DevOpsCloudAutomation"
                    git config --global user.email "Pavankumarkj347@gmail.com"
                    git add Deployment.yaml
                    git commit -m "Updated Build Number in Deployment Manifest File"
                """
                
                withCredentials([gitUsernamePassword(credentialsId: 'Jenkins_ArgoCD_Token', gitToolName: 'Git')])
                {
                    sh "git push https://github.com/DevOpsCloudAutomation/GitOps_ArgoCD_Kubernetes_CD main"
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

        success
        {
            slackSend channel: 'awsdevopscloudautomation',
            color: 'good',
            message: "${currentBuild.currentResult} ✅\n Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n Application is Successfully Deployed to Kubernetes Cluster using ArgoCD\n More Information Available at: ${env.BUILD_URL}"
        }
        
        failure
        {
            slackSend channel: 'awsdevopscloudautomation',
            color: 'good',
            message: "${currentBuild.currentResult} ⛔️\n Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n Application Deployment is Failed to Kubernetes Cluster using ArgoCD\n More Information Available at: ${env.BUILD_URL}"
        }
    }
}