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
                git branch: 'main', url: 'https://github.com/DevOpsCloudAutomation/Kubernetes_GitOps_ArgoCD.git'
            }
        }

        stage('Update Docker Image Tag in Kubernetes Manifest')
        {
            steps()
            {
                sh "sed -i 's/${application_Name}.*/${application_Name}:${buildNumber}/g' Deployment.yaml"
            }
        }

        stage("Push the changed deployment file to Git")
        {
            steps()
            {
                sh """
                    git config --global user.name "DevOpsCloudAutomation"
                    git config --global user.email "Pavankumarkj347@gmail.com"
                    git add Deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                
                withCredentials([gitUsernamePassword(credentialsId: 'Jenkins_ArgoCD_Token', gitToolName: 'Git')])
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