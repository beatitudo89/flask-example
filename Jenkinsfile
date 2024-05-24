node {
     stage('Clone repository') {
         checkout scm
     }
     stage('Build image') {
         app = docker.build("admin/flask-example")
         
     }
     stage('Push image') {
         docker.withRegistry('https://127.0.0.1/', 'harbor_cred') {
             app.push("${env.BUILD_NUMBER}")
             app.push("latest")
         }
     }
     stage('SonarQubeScanner') {
         def scannerHome = tool 'SonarQubeScanner';
         withSonarQubeEnv('SonarQubeScanner') {
             sh """${scannerHome}/bin/sonar-scanner \
                             -Dsonar.projectKey=demo-project \
                             -Dsonar.sources=. \
                             -Dsonar.host.url=http://10.0.2.15:9000 \
                             -Dsonar.login=squ_06592d081a6a8a840c7d09ea87d0782ba055ea9f
             """
        }
     }
}
