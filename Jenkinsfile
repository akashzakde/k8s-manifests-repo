pipeline{
    agent any
    tools{
        maven 'MVN'
        jdk 'JDK11'
        git 'git'
    }
    environment{
        APP_NAME = "myapp"
    }
    stages{
        stage('Fetch Code'){
            steps{
                git branch: 'main', url: 'https://github.com/akashzakde/k8s-manifests-repo.git'
            }
        }
        stage('Updating Kubernetes deployment file'){
            steps {
                sh "cat Kubernetes-manifests.yaml"
                sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' Kubernetes-manifests.yaml"
                sh "cat Kubernetes-manifests.yaml"
            }
        }
        stage('Push the changed deployment file to Github'){
            steps {
                script{
                    sh """
                    git config --global user.name "Akash Z"
                    git config --global user.email "akashz@gmail.com"
                    git add Kubernetes-manifests.yaml
                    git commit -m 'Updated the deployment file by build number ${IMAGE_TAG} of CI pipeline' """
                    withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        sh "git push https://$user:$pass@github.com/akashzakde/k8s-manifests-repo.git main"
                    }
                }
            }
        }
    }
}
