node {
    def app

     stage('Clone repository') {
         checkout scm
     }
     stage('Build image') {
         app = docker.build("wnstn385/payments")
     }
     stage('Push image') {
            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
                sleep 3
                sh "docker rmi registry.hub.docker.com/wnstn385/payments:${env.BUILD_NUMBER}"
            }
         
     }

     stage('Trigger Manifest') {
         build job: "payments-infra", parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
         }        

 }