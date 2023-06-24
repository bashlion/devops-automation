pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
        dockerTool 'docker'
    }
    stages{
        stage('Scan base image to be used to buil application'){
            steps{
                neuvector nameOfVulnerabilityToExemptFour: '', 
                nameOfVulnerabilityToExemptOne: '', 
                nameOfVulnerabilityToExemptThree: '', 
                nameOfVulnerabilityToExemptTwo: '', 
                nameOfVulnerabilityToFailFour: '', 
                nameOfVulnerabilityToFailOne: '', 
                nameOfVulnerabilityToFailThree: '', 
                nameOfVulnerabilityToFailTwo: '', 
                numberOfHighSeverityToFail: '', 
                numberOfMediumSeverityToFail: '', 
                registrySelection: 'docker-pub', 
                repository: 'openjdk', 
                scanLayers: true, 
                scanTimeout: 10, 
                tag: '8'
            }
        }
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/bashlion/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image and push it to local registry for scanning'){
            steps{
                script{
                    sh 'docker build -t local-reg:5000/tapasn47/devops-integration:1.0 .'
                }
            }
        }
        stage('Scan built image'){
            steps {
                neuvector nameOfVulnerabilityToExemptFour: '', 
                nameOfVulnerabilityToExemptOne: '', 
                nameOfVulnerabilityToExemptThree: '', 
                nameOfVulnerabilityToExemptTwo: '', 
                nameOfVulnerabilityToFailFour: '', 
                nameOfVulnerabilityToFailOne: '', 
                nameOfVulnerabilityToFailThree: '', 
                nameOfVulnerabilityToFailTwo: '', 
                numberOfHighSeverityToFail: '', 
                numberOfMediumSeverityToFail: '', 
                registrySelection: 'local', 
                repository: 'local-reg:5000/tapasn47/devops-integration', 
                scanLayers: true, 
                scanTimeout: 10, 
                standaloneScanner: true, 
                tag: '1.0'
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u tapasn47 -p ${dockerhubpwd}'

}
                   sh 'docker tag local-reg:5000/tapasn47/devops-integration:1.0 tapasn47/devops-integration:2.0'
                   sh 'docker push tapasn47/devops-integration:2.0'
                }
            }
        }
        stage('Scan pushed image'){
            steps {
                neuvector nameOfVulnerabilityToExemptFour: '', 
                nameOfVulnerabilityToExemptOne: '', 
                nameOfVulnerabilityToExemptThree: '', 
                nameOfVulnerabilityToExemptTwo: '', 
                nameOfVulnerabilityToFailFour: '', 
                nameOfVulnerabilityToFailOne: '', 
                nameOfVulnerabilityToFailThree: '', 
                nameOfVulnerabilityToFailTwo: '', 
                numberOfHighSeverityToFail: '', 
                numberOfMediumSeverityToFail: '', 
                registrySelection: 'docker-pri', 
                repository: 'tapasn47/devops-integration', 
                scanLayers: true, 
                scanTimeout: 10, 
                tag: '2.0'
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubeconfig(credentialsId: 'k8s-config-2', serverUrl: 'https://192.168.211.204:6443') {
                        sh 'kubectl create ns devops-deploy-3'
                        sh 'kubectl apply -f deploymentservice.yaml -n devops-deploy-3'
}
                }
            }
        }
    }
}
