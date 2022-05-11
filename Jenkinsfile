pipeline {
    agent any
    environment {
        dockercImage='client'
        dockersImage='server'
        registry = "sagarshirke2674/spe"
        registryCredential = 'docker-login'
    }

    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/sagarshirke2674/interestwin.git',
				branch: 'master',
                credentialsId: 'git-access-token'
            }
        }
         stage('Build') {
            steps {
                sh 'npm install'
                
            }
        }
         stage('Test') {
            steps {
                sh 'npm run test'
            }
        }
        //  stage('Closing Ports') {
        //     steps {
        //         sh 'fuser -n tcp -k 3000'
        //         sh 'fuser -n tcp -k 5000'
        //     }
        // }
        
        
        stage('Building Client Image') {
            steps{
                dir('client'){
                script {
                    dockercImage = docker.build registry + ":$dockercImage"
                }
                }
            }
        }
        stage('Building Server Image') {
            steps{
                
                script {
                    dockersImage = docker.build registry + ":$dockersImage"
                }
            }
        }
        stage('Deploy client image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockercImage.push()  
                    }
                }
            }
        }
        stage('Deploy Server Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockersImage.push()  
                    }
                }
            }
        }
        stage('Clean Images') {
            steps {
                sh 'docker rmi -f sagarshirke2674/spe:client'
                sh 'docker rmi -f sagarshirke2674/spe:server'
                
            }
        }
        stage('Deploy and Run Images'){
            steps {
                ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'deploy.yml', sudoUser: null
            }
        }
       
    }
}
