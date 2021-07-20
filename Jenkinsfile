pipeline{
    agent any
    environment {
        IMAGE_REPO = "asia.gcr.io/abstract-stream-316904"
        IMAGE_NAME = 'java-cobrand-demo'
        IMAGE_TAG = 'jenkins'
    }

    stages {

        stage('git scm') {
            steps {
                git 'https://github.com/sreelalvu/jenkins-multibranching'
            }
        }

        stage('Maven build') {
            steps {
                sh "mvn -Dmaven.test.skip=true clean package"
            }
        }

        stage('Build Image') {
            steps {
              sh 'docker build -t "my-image:${IMAGE_TAG}" .'
            }
        }

        stage('Push Image') {
            steps {
                withDockerRegistry([ credentialsId: "gcr:abstract-stream-316904", url: "https://asia.gcr.io/abstract-stream-316904" ]) {
                sh 'docker tag my-image:${IMAGE_TAG} ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}'
                sh 'docker push ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}'
                }
                }
            }
        // stage('Remove Unused docker image') {
        // steps{
        //     sh "docker rmi my-image:${IMAGE_TAG}"
        // }
        // }
        stage('Deploy Production') {
            steps{
                git url: 'https://github.com/sreelalvu/jenkins-multibranching'
                step([$class: 'KubernetesEngineBuilder', 
                        projectId: "abstract-stream-316904",
                        clusterName: "marriot-demo",
                        zone: "asia-south1-a",
                        manifestPattern: 'k8s/',
                        credentialsId: "sreelalvu",
                        verifyDeployments: false])
            }
        }
        stage('cat README') {
            when {
                branch "multi1"
            }
            steps {
              sh '''
                cat README.md
              '''
            }
        }
    }
}       
