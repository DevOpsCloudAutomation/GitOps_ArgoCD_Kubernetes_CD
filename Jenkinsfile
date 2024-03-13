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
                sh "sed -i 's/Build_Tag/${Build_Number}/g' Deployment.yaml"
            }
        }

        stage('Deploy Application to EKS Kubernetes Cluster')
        {
            steps()
            {
                sh 'kubectl delete deployment webpage-deployment -n test || true'
                sh 'kubectl apply -f Deployment.yaml'
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
            message: "${currentBuild.currentResult} ✅\n Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n Application is Successfully Deployed to Production Environment\n More Information Available at: ${env.BUILD_URL}"
        }
        
        failure
        {
            slackSend channel: 'awsdevopscloudautomation',
            color: 'good',
            message: "${currentBuild.currentResult} ⛔️\n Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n Application Deployment is Failed to Production Environment\n More Information Available at: ${env.BUILD_URL}"
        }
    }
}