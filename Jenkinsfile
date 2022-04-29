pipeline {
    agent any

    parameters{
        string(name: "PLATFORM", description: 'Do you want to deploy the app to android or IOS')
        
        choice(name: "ENVIROMENT", choices: ['master','development', 'QA'], description: "for what enviroment you want to deploy")
        
        booleanParam(name: "START", description: 'are you sure?')
        
    }
    
    
    stages {
        stage('ENVS') {
            steps {
                sh "echo '${PLATFORM}'"
                sh "echo '${ENVIROMENT}'"
                sh "echo '${START}'"
            }
        }
        stage('SCM') {
            steps {
                checkout([$class: 'GitSCM',
                branches: [[name: 'development']],
                extensions: [],
                userRemoteConfigs: [[credentialsId: 'd95e380e-6b13-4326-854d-4e083ef271fa',
                url: 'https://github.com/MathMedrado/ASP.net-sample-app.git']]])
            }
        }
        stage('build') {
            steps {
                sh "cd ${WORKSPACE}/devopswebapp && docker build . -t matheusmedrado2020/asp-netapp:${BUILD_NUMBER} "
            }
        }
        stage('deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'c5485004-483b-48fc-ac9b-c3ab56c2a330', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh "docker login -u '${USERNAME}' -p '${PASSWORD}'"
                    sh "docker push  matheusmedrado2020/asp-netapp:${BUILD_NUMBER}"
                }
            }
        
        }
        
    }
    post {
        always {
          sh 'docker logout'
        }
    }
    
}
