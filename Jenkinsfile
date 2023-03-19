pipeline {
    agent {
        docker { image 'node:16.13.1-alpine' }
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t my-app .'
                sh 'docker tag hello-world raypal4/hello-world:latest'
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u ${USERNAME} -p ${PASSWORD} raypal4'
                }
                sh 'docker push raypal4/hello-world:latest'
            }
        }

        // stage('Deploy') {
        //     steps {
        //         withKubeConfig([credentialsId: 'kube-config']) {
        //             kubernetesDeploy(
        //                 configs: 'path/to/k8s/deployment.yaml',
        //                 enableConfigSubstitution: true,
        //                 kubeconfigId: 'kube-config',
        //                 kubeconfigContextName: 'my-cluster',
        //                 recreateMode: 'RollingUpdate',
        //                 waitUntilDeployed: true
        //             )
        //         }
        //     }
        // }
    }
}
