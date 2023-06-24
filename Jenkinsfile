pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
        dockerTool 'docker'
    }
    stages{
        stage('Scan base image to be used to buil application'){
            steps{
                neuvector nameOfVulnerabilityToExemptFour: '', nameOfVulnerabilityToExemptOne: '', nameOfVulnerabilityToExemptThree: '', nameOfVulnerabilityToExemptTwo: '', nameOfVulnerabilityToFailFour: '', nameOfVulnerabilityToFailOne: '', nameOfVulnerabilityToFailThree: '', nameOfVulnerabilityToFailTwo: '', numberOfHighSeverityToFail: '200', numberOfMediumSeverityToFail: '200', registrySelection: 'docker-pub', repository: 'openjdk', scanLayers: true, scanTimeout: 10, tag: '8'
            }
        }
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/bashlion/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t tapasn47/devops-integration:1.0 .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u tapasn47 -p ${dockerhubpwd}'

}
                   sh 'docker push tapasn47/devops-integration'
                }
            }
        }
        stage('Scan pushed image'){
            steps {
                neuvector nameOfVulnerabilityToExemptFour: '', nameOfVulnerabilityToExemptOne: '', nameOfVulnerabilityToExemptThree: '', nameOfVulnerabilityToExemptTwo: '', nameOfVulnerabilityToFailFour: '', nameOfVulnerabilityToFailOne: '', nameOfVulnerabilityToFailThree: '', nameOfVulnerabilityToFailTwo: '', numberOfHighSeverityToFail: '', numberOfMediumSeverityToFail: '', registrySelection: 'docker-pri', repository: 'tapasn47/devops-integration', scanLayers: true, scanTimeout: 10, tag: '1.0'
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubeconfig(credentialsId: 'k8s-config-2', serverUrl: 'https://192.168.211.204:6443') {
                        sh 'kubectl create ns devops-deploy-1'
                        sh 'kubectl apply -f deploymentservice.yaml -n devops-deploy-1 '
}
                }
            }
        }
    }
}
